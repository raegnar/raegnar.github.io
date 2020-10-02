---
layout: post
title: Writing to 3D OpenGL textures in CUDA 4.1 with 3D surface writes
date: 2011-12-02 12:31
comments: true
categories: [CUDA, OpenGL]
tags: [CUDA, OpenGL]
---
**Edit:** For how this works in CUDA 5 see my new post <a href="http://rauwendaal.net/2013/04/03/cuda-5-and-opengl-interop/" title="CUDA 5 and OpenGL Interop and Dynamic Parallelism">CUDA 5 and OpenGL Interop and Dynamic Parallelism</a>.

CUDA 4.1 has been <a href="http://developer.nvidia.com/cuda-toolkit-41">released</a>, and with it, and they've added support for writing to 3D surfaces. And thanks to some pointers from some very helpful Nvidia engineers (thanks <a href="http://www.mpi-inf.mpg.de/~gziegler/">Gernot</a>!), I was able to write to a 3D OpenGL texture with a CUDA kernel, without having to copy any data between the host and the device.

The new toolkit has an excellent volumeFiltering sample that shows how to write to 3D surfaces, which was very helpful, but there are still a couple of gotchas to watch out for.

## OpenGL interop
The sample uses <code>cudaMalloc3DArray</code> to directly allocate data for the 3D surfaces, so it doesn't show the process for 3D surface writes in which the allocation has occurred by creating an OpenGL texture. Fortunately, that takes just a few extra steps.

## The Steps
* <a href="#step1">Create an OpenGL 3D Texture</a>
* <a href="#step2">Register the texture as an "image" with CUDA</a>
* <a href="#step3">Map the "image" to a CUDA graphics resource</a>
* <a href="#step4">Get a <code>cudaArray</code> pointer from the resource</a>
* <a href="#step5">Pass the <code>cudaArray</code> pointer to the device</a>
* <a href="#step6">Bind the <code>cudaArray</code> to a globally scoped CUDA surface</a>
* <a href="#step7">Call a CUDA kernel</a>
* <a href="#step8">Write to the surface using <code>surf3Dwrite</code></a>
* <a href="#step9">Unmap the resource</a>
* <a href="#step10">Unregister the texture</a>

## Step 1: Create an OpenGL 3D texture {#step1}
Hopefully most people know how to do this, just watch out that you are using a texture format that is CUDA compatible, I'm not entirely sure all which textures are supported, but <a href="http://forums.nvidia.com/index.php?showtopic=164987">this forum post</a> shows a couple that definitely work.

~~~cpp
glGenTextures(1, &texID);
glBindTexture(GL_TEXTURE_3D, texID);
{
	glTexParameteri(GL_TEXTURE_3D, GL_TEXTURE_MIN_FILTER, GL_NEAREST        );
	glTexParameteri(GL_TEXTURE_3D, GL_TEXTURE_MAG_FILTER, GL_NEAREST        );
	glTexParameteri(GL_TEXTURE_3D, GL_TEXTURE_WRAP_S,     GL_CLAMP_TO_BORDER);
	glTexParameteri(GL_TEXTURE_3D, GL_TEXTURE_WRAP_T,     GL_CLAMP_TO_BORDER);
	glTexParameteri(GL_TEXTURE_3D, GL_TEXTURE_WRAP_T,     GL_CLAMP_TO_BORDER);

	glTexImage3D(GL_TEXTURE_3D, 0, GL_RGBA32F, textureDim.x, textureDim.y, textureDim.z, 0, GL_RGBA, GL_FLOAT, NULL);
}
glBindTexture(GL_TEXTURE_3D, 0);
~~~

## Step 2: Register the texture as an "image" with CUDA  {#step2}
This is done with <code><a href="http://developer.download.nvidia.com/compute/cuda/4_0/toolkit/docs/online/group__CUDART__OPENGL_gd7be3ca8a7a739d57f0b558562c5706e.html">cudaGraphicsGLRegisterImage</a></code>, just make sure you specify the <code>cudaGraphicsRegisterFlagsSurfaceLoadStore</code> flag as this tell CUDA that you want to bind this image/texture to a surface reference. If you wrap this in a <code>cutilSafeCall</code> and you used an unsupported texture format, you'll probably get an error message.

~~~cpp
cutilSafeCall(cudaGraphicsGLRegisterImage(&cuda_image_resource, texID, GL_TEXTURE_3D, cudaGraphicsRegisterFlagsSurfaceLoadStore));
~~~

## Step 3: Map the "image" to a CUDA graphics resource  {#step3}
You must map the resource with <code><a href="http://developer.download.nvidia.com/compute/cuda/4_0/toolkit/docs/online/group__CUDART__INTEROP_gb7064fb72e54d89d0666e192b45d35cc.html#gb7064fb72e54d89d0666e192b45d35cc">cudaGraphicsMapResources</a></code> before you can get a <code>cudaArray</code> from it.

~~~cpp
cutilSafeCall(cudaGraphicsMapResources(1, &cuda_image_resource, 0));
~~~

## Step 4: Get a `cudaArray` pointer from the resource  {#step4}
Unlike with buffers, we won't get a raw pointer from CUDA, instead we get a mapped <code>cudaArray</code> type by calling <code><a href="http://developer.download.nvidia.com/compute/cuda/4_0/toolkit/docs/online/group__CUDART__INTEROP_g09f772ed8c6e8e03f396a8895fc42050.html#g09f772ed8c6e8e03f396a8895fc42050">cudaGraphicsSubResourceGetMappedArray</a></code>. The <code>cudaArray</code> pointer is only guaranteed valid while "mapped".

~~~cpp
cutilSafeCall(cudaGraphicsSubResourceGetMappedArray(&cuda_array, cuda_image_resource, 0, 0));
~~~

## Step 5: Pass the <code>cudaArray</code> pointer to the device  {#step5}
Getting the <code>cudaArray</code> pointer is pretty much the last thing we do on the host side. Once we have the pointer we pass it over to the device side code (in the .cu file)

~~~cpp
launch_kernel(cuda_image_array, textureDim);
~~~

## Step 6: Bind the <code>cudaArray</code> to a globally scoped CUDA surface  {#step6}
Once we have the cudaArray pointer on the device side we bind it to the surface reference. For some reason the surface reference must be declared in the global scope. There is no <code>cudaUnbindSurface</code>, so don't worry about that.

~~~cpp
cutilSafeCall(cudaBindSurfaceToArray(surfaceWrite, cuda_array));
~~~

## Step 7: Call a CUDA kernel  {#step7}
Now that we have a surface reference to work with we can call our CUDA kernel. Make sure not to use too large of block for your kernel launch, which is pretty easy to do if your specifying the dimension in 3D. I believe the limit is 1024 on current gen hardware. If you exceed the limit the kernel will fail to launch, you can catch this with <code>cutilCheckMsg</code>.

~~~cpp
dim3 block_dim(8, 8, 8);
dim3 grid_dim(texture_dim.x/block_dim.x, texture_dim.y/block_dim.y, texture_dim.z/block_dim.z);

kernel<<< grid_dim, block_dim >>>(texture_dim);

cutilCheckMsg("kernel failed");
~~~

## Step 8: Write to the surface using surf3Dwrite  {#step8}
Now that we've launched our CUDA kernel we can write to the globally scoped surface with <code>surf3Dwrite</code>. I got tripped up at this point because I didn't realize that surface memory uses byte addressing. This means that the x-coordinate used to access a surface element needs to be multiplied by the byte size of the element. This is easy to miss if you're going by the SDK sample, since it uses a 1-byte surface of <code>unsigned char</code>'s.

~~~cpp
__global__
void kernel(dim3 texture_dim)
{
	int x = blockIdx.x*blockDim.x + threadIdx.x;
	int y = blockIdx.y*blockDim.y + threadIdx.y;
	int z = blockIdx.z*blockDim.z + threadIdx.z;

	if(x >= texture_dim.x || y >= texture_dim.y || z >= texture_dim.z)
	{
		return;
	}

	float4 element = make_float4(1.0f, 1.0f, 1.0f, 1.0f);
	surf3Dwrite(element, surfaceWrite, x*sizeof(float4), y, z);
}
~~~

## Step 9: Unmap the resource  {#step9}
Make sure to unmap the resource with <code><a href="http://developer.download.nvidia.com/compute/cuda/4_0/toolkit/docs/online/group__CUDART__INTEROP_gc4dcf300df27f8cf51a89f0287b07861.html#gc4dcf300df27f8cf51a89f0287b07861">cudaGraphicsUnmapResources</a></code> before you try to do anything else with the texture, like use it in OpenGL. If you surface writes were misaligned you'll probably get an "unknown error" when trying to unmap the resource, if it was called with <code>cutilSafeCall</code>.

~~~cpp
cutilSafeCall(cudaGraphicsUnmapResources(1, &cuda_image_resource, 0));
~~~

## Step 10: Unregister the texture  {#step10}
This is just more cleanup, be sure to unregister the texture/image resource with <code><a href="http://developer.download.nvidia.com/compute/cuda/4_0/toolkit/docs/online/group__CUDART__INTEROP_g1d45ac44d1affe17fb356e0b7a0b0560.html#g1d45ac44d1affe17fb356e0b7a0b0560">cudaGraphicsUnregisterResource</a></code>, you probably don't want to do this until you are done with the texture.

~~~cpp
cutilSafeCall(cudaGraphicsUnregisterResource(cuda_image_resource));
~~~

## Conclusion & Source
This is a feature I've been looking forward to for quite awhile, and I'm very glad to see it implemented in the newest CUDA release. Hopefully I've managed to describe to process clearly enough that other people can avoid the mistakes I made. If you still having trouble make sure you've called <code>cudaGLSetGLDevice</code>. I created a very simple source <a href="https://docs.google.com/open?id=0B61Vxw4WozyLYmE4ZjgyNTgtZDExZS00M2E1LTljOTAtMjYxMzg4ODQ0Nzc1">example</a> from an SDK sample, so hopefully it will work/compile if you extract it in your SDK sample directory (C:\ProgramData\NVIDIA Corporation\NVIDIA GPU Computing SDK 4.1\C\src\).
