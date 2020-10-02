---
layout: post
title: "How to use CUDA 3.0's new Graphics Interoperability API with OpenGL"
date: 2011-02-10 16:13
comments: true
categories: [CUDA, OpenGL]
tags: [CUDA, OpenGL]
---
It always bothered me that whenever I took a look at using CUDA in my graphics applications there didn't seem to be an elegant way to use textures from OpenGL with CUDA without doing potentially expensive copies. But that is finally no longer necessary with CUDA 3.0's new graphics interoperability API.

The only real documentation is the online doxygen generated stuff, the best place to start is at the [Graphics Interoperability][graphicsInterop] page. Unfortunately there is no documentation for the cudaGraphicsResource struct that all these new functions seem to use.  And while there is a API agnostic [`cudaGraphicsUnregisterResource`][cudaGraphicsUnregisterResource] function, there is no function to actually register a resource unless you look in the API specific modules, which you might first assume, as I did, are deprecated, but it's only the modules that say [DEPRECATED] real big across the top that are actually deprecated, the new non-deprecated modules simply have a link to the deprecated modules.  So for OpenGL you simply have to look at the [OpenGL Interoperability][openglInterop] page to find the rest of the functions you'll need, there are similar pages for whatever other API you would like to use.

So basically the process is to register a resource, generally a texture or a buffer via the `cudaGraphicsGLRegisterImage` and `cudaGraphicsGLRegisterBuffer` functions respectively. These functions assign a valid pointer to your `cudaGraphicsResource` pointer.  Then create a CUDA stream with `cudaStreamCreate`, map your graphics resource to the CUDA stream with `cudaGraphicsMapResources`, and at this pointer you can recover a pointer to your texture or buffer data in your CUDA code using the `cudaGraphicsSubResourceGetMappedArray` and `cudaGraphicsResourceGetMappedPointer` functions respectively.

However, if you map a texture to a resource you can can only get a pointer to a `cudaArray`, which is read-only, whereas with a buffer, you can get a pointer to actual data and write to it as well, and since my entire goal in this endeavor was to use CUDA kernels to write to textures as a replacements for my clunky GLSL shaders, thats what I needed to use.

Fortunately there is a workaround called Texture Buffer Objects, which I like to thing of as simply an API to map a Pixel Buffer Object as the data of a Texture. You simply have to remember to create a CUDA stream and map your resources to the CUDA stream before calling any CUDA function that use that resource. So anyway, I'll just post the most relevant bits of code and hopefully it'll help someone.

Test.cpp
~~~cpp
//CUDA graphics resource
cudaGraphicsResource *resources[1];

GLuint pbo;
GLuint tbo_tex;

static GLuint width  = 512;
static GLuint height = 512;

void init_cuda()
{
    //Create your Pixel Buffer Object
    glGenBuffers(1, &pbo);
    glBindBuffer(GL_PIXEL_UNPACK_BUFFER_ARB, pbo);
    glBufferData(GL_PIXEL_UNPACK_BUFFER_ARB, width*height*sizeof(float4), NULL, GL_DYNAMIC_DRAW);
    glBindBuffer(GL_PIXEL_UNPACK_BUFFER_ARB, 0);

    //Create your Texture
    glGenTextures(1, &_tbo_tex);
    glBindTexture(GL_TEXTURE_BUFFER_EXT, tbo_tex); //bind Texture

    //Attach Pixel Buffer Object to the Texture
    glTexBufferEXT(GL_TEXTURE_BUFFER_EXT, GL_RGBA32F_ARB, pbo);

    glBindTexture(GL_TEXTURE_BUFFER_EXT, 0); //unbind Texture

    //Setup CUDA
    cudaSetDevice(cutGetMaxGflopsDeviceId());
    cudaGLSetGLDevice(cutGetMaxGflopsDeviceId());

    //Register Pixel Buffer Object as CUDA graphics resource
    cudaGraphicsGLRegisterBuffer(resources, pbo, cudaGraphicsMapFlagsNone);

    cudaStream_t cuda_stream;

    //Create CUDA stream
    cudaStreamCreate(&cuda_stream);

    //Map the graphics resource to the CUDA stream
    cudaGraphicsMapResources(1, resources, cuda_stream);

    //Call CUDA function
    map_texture(resources[0], width, height);

    //Unmap the CUDA stream
    cudaGraphicsUnmapResources(1, resources, cuda_stream);

    //Destroy the CUDA stream
    cudaStreamDestroy(cuda_stream);
}

void cuda_test()    //Call this in your draw loop to animate
{
    dim3 blockSize(16, 16);
    dim3 gridSize(width / blockSize.x, height / blockSize.y);

    cudaStream_t cuda_stream;

    //Create CUDA stream
    cudaStreamCreate(&cuda_stream);

    //Map the graphics resource to the CUDA stream
    cudaGraphicsMapResources(1, resources, cuda_stream);

    //Call CUDA function
    test_cuda(width, height, blockSize, gridSize, cuda_stream);

    //Unmap the CUDA stream
    cudaGraphicsUnmapResources(1, resources, cuda_stream);

    //Destroy the CUDA stream
    cudaStreamDestroy(cuda_stream);
}
~~~

Test.cu
~~~cpp
#ifndef _TEST_CU_
#define _TEST_CU_

#include
#include
#include

float4 *cuda_data = NULL;

extern "C" void map_texture(cudaGraphicsResource *resource, int w, int h)
{
    size_t size;
    cudaGraphicsResourceGetMappedPointer((void **)(&cuda_data), &size, resource);
}

__global__ void test_kernel(float4 *cuda_data, int width, int height, int frame_counter)
{
    uint x = __umul24(blockIdx.x, blockDim.x) + threadIdx.x;
    uint y = __umul24(blockIdx.y, blockDim.y) + threadIdx.y;
    uint i = __umul24(y, width) + x;

    if((x < width) && (y < height))
    {
        //Create a checkerboard pattern with 32x32 pixel squares
        cuda_data[i] = ((((x+frame_counter)/32 + (y+frame_counter)/32 ) & (int)(0x1)) == 0) ? make_float4(1.0, 1.0, 1.0, 1.0) : make_float4(0.0, 0.0, 0.0, 1.0);
    }
}

static int frame_counter = 0;

extern "C" void test_cuda(int width, int height, dim3 blockSize, dim3 gridSize, cudaStream_t &cuda_stream)
{
    test_kernel<<>>(cuda_data, width, height, frame_counter);
    frame_counter++;
}

#endif
~~~

Since there is no fixed function functionality for drawing texture buffer objects you must write a shader for displaying your buffer, which is pretty easy to do as seen below.

tbo_shader.glsl
~~~glsl
///////////////////////////////////////////////////////////////////////////////
VERTEX
///////////////////////////////////////////////////////////////////////////////

varying vec2 st;

void main()
{
    st = gl_MultiTexCoord0.xy;
    gl_Position = gl_ModelViewProjectionMatrix * gl_Vertex;
}

///////////////////////////////////////////////////////////////////////////////
FRAGMENT
///////////////////////////////////////////////////////////////////////////////

#version 120
#extension GL_EXT_gpu_shader4 : enable

varying vec2 st;

uniform samplerBuffer buffer;
uniform ivec2 dim;

void main()
{
    int i = int(st.x * float(dim.x));
    int j = int(st.y * float(dim.y));

    gl_FragData[0] = texelFetch(buffer, i+dim.x*j);
}
~~~

Of course, there is no reason to display the buffer if your just doing computations on it, and there is no reason you can't use this technique on Vertex or other buffers. And finally, I don't have much CUDA experience so I can't guarantee that I'm not doing anything suboptimal in the above code. I would also recommend wrapping all the CUDA functions `cutilSafeCall` functions.



[graphicsInterop]: http://developer.download.nvidia.com/compute/cuda/3_0-Beta1/toolkit/docs/online/group__CUDART__INTEROP.html
[cudaGraphicsUnregisterResource]: http://developer.download.nvidia.com/compute/cuda/3_0-Beta1/toolkit/docs/online/group__CUDART__INTEROP_gc47f5f6144307d803da1ba3c8dc26738.html#gc47f5f6144307d803da1ba3c8dc26738
[openglInterop]: http://developer.download.nvidia.com/compute/cuda/3_0-Beta1/toolkit/docs/online/group__CUDART__OPENGL.html