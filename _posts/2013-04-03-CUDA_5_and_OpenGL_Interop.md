---
layout: post
title: CUDA 5 and OpenGL Interop and Dynamic Parallelism
date: 2013-04-03 17:42
comments: true
categories: [CUDA, Dynamic Parallelism, OpenGL]
tags: [CUDA, Dynamic Parallelism, OpenGL]
---

I seem to revisit this every time every time Nvidia releases a new version of of CUDA.

## The good news...
The old methods still work, the whole register, map, bind, etc... process I described in my now two year old post <a title="Writing to 3D OpenGL textures in CUDA 4.1 with 3D surface writes" href="http://rauwendaal.net/2011/12/02/writing-to-3d-opengl-textures-in-cuda-4-1-with-3d-surface-writes/">Writing to 3D OpenGL textures in CUDA 4.1 with 3D Surface writes</a> still works.  Ideally, the new version number shouldn't introduce any new problems...

## The bad news...
Unfortunately, if you try to write to a globally scoped CUDA surface from a device-side launched kernel (i.e. a dynamic kernel), nothing will happen.  You'll scratch your head and wonder why code that works perfectly fine when launched from the host-side, fails silently when launched device-side.

I only discovered the reason when I decided to read, word for word, the <a href="http://docs.nvidia.com/cuda/pdf/CUDA_Dynamic_Parallelism_Programming_Guide.pdf">CUDA Dynamic Parallelism Programming Guide</a>. On page 14, in the "Textures & Surfaces" section is this note:

> **NOTE: The device runtime does not support legacy module-scope (i.e. Fermi-style) textures and surfaces within a kernel launched from the device**. Module-scope (legacy) textures may be created from the host and used in device code as for any kernel, but may only be used by a top-level kernel (i.e. the one which is launched from the host).

So now the old way of dealing with textures is considered "Legacy" but apparently not quite deprecated yet.  So don't use them if you plan on using dynamic parallelism.
**Additional Note:** if you so much call a function that attempts to perform a "Fermi-style" surface write you're kernel will fail silently, so I highly recommend removing all "Fermi-style" textures and surfaces if you plan on using dynamic parallelism.

So what's the "New style" of textures and surfaces, well also on page 14 is a footnote saying:

>Dynamically created texture and surface objects are an addition to the CUDA memory model introduced with CUDA 5.0. Please see the CUDA Programming Guide for details.

So I guess they're called "Dynamically created textures and surfaces", which is a mouthful so I'm going to refer to them as "Kepler-style" textures and surfaces. In the actual API they are <code><a href="http://docs.nvidia.com/cuda/cuda-runtime-api/index.html#group__CUDART__TYPES_1g83eb271ebc4cb2817e66d7c752f0c29b">cudaTextureObject_t</a></code> and <code><a href="http://docs.nvidia.com/cuda/cuda-runtime-api/index.html#group__CUDART__TYPES_1gbe57cf2ccbe7f9d696f18808dd634c0a">cudaSurfaceObject_t</a></code>, and you can pass them around as parameters instead of having to declare them at file scope.

## OpenGL Interop
So now we have two distinct methods for dealing with textures and surfaces, "Fermi-style" and "Kepler-style", but we only know how graphics interoperability works with the old, might-as-well-be-deprecated, "Fermi-style" textures and surfaces.

And while there are some samples showing how the new "Kepler-style" textures and surfaces work (see the <a href="http://docs.nvidia.com/cuda/cuda-samples/index.html#bindless-texture">Bindless Texture</a> sample), all the interop information still seems to target the old "Fermi-style" textures and surfaces.  Fortunately, there is some common ground between "Kepler-style" and "Fermi-style" textures and surfaces, and that common ground is the <code>cudaArray</code>.

Really, all we have to do is replace <a href="http://rauwendaal.net/2011/12/02/writing-to-3d-opengl-textures-in-cuda-4-1-with-3d-surface-writes/#step6">Step 6</a>  (binding a <code>cudaArray</code> to a globally scoped surface) from the previous tutorial, with the creation of a <code>cudaSurfaceObject_t</code>. That entails creating a cuda resource description (<code>cudaResourceDesc</code>), and all we have to do is appropriately set the array portion of the <code>cudaResourceDesc</code> to our <code>cudaArray</code>, and then use that <code>cudaResourceDesc</code> to create our <code>cudaSurfaceObject_t</code>, which we can then pass to our kernels, and use to write to our registered and mapped OpenGL textures.

~~~cpp
// Create the cuda resource description
struct cudaResourceDesc resoureDescription;
memset(&resDesc, 0, sizeof(resoureDescription));
resDesc.resType = cudaResourceTypeArray;	// be sure to set the resource type to cudaResourceTypeArray
resDesc.res.array.array = yourCudaArray;	// this is the important bit

// Create the surface object
cudaSurfaceObject_t writableSurfaceObject = 0;
cudaCreateSurfaceObject(&amp;writableSurfaceObject, &amp;resoureDescription);
~~~

And thats it! Here's hoping the API doesn't change again anytime soon.
