---
layout: post
title: "Fixed imageAtomicAverageRGBA8"
date: 2013-05-02 10:27
comments: true
tags: [GLSL]
---

So I fixed some issues I had in my previous implementation of `imageAtomicAverageRGBA8`, see the [previous post](/glslrunningaverage/) for an explanation of what I got wrong. Reposting the corrected code here, and sorry to anyone who was trying to use the broken version.

~~~glsl
void imageAtomicAverageRGBA8(layout(r32ui) coherent volatile uimage3D voxels, ivec3 coord, vec3 nextVec3)
{
	uint nextUint = packUnorm4x8(vec4(nextVec3,1.0f/255.0f));
	uint prevUint = 0;
	uint currUint;

	vec4 currVec4;

	vec3 average;
	uint count;

	// "Spin" while threads are trying to change the voxel
	while((currUint = imageAtomicCompSwap(voxels, coord, prevUint, nextUint)) != prevUint)
	{
		prevUint = currUint;					//store packed rgb average and count
		currVec4 = unpackUnorm4x8(currUint);	//unpack stored rgb average and count

		average =      currVec4.rgb;			//extract rgb average
		count   = uint(currVec4.a*255.0f);		//extract count

		//Compute the running average
		average = (average*count + nextVec3) / (count+1);

		//Pack new average and incremented count back into a uint
		nextUint = packUnorm4x8(vec4(average, (count+1)/255.0f));
	}
}
~~~

Anyway, original credit for this technique should go to Cyril Crassin, whose implementation in [<a href="#CG2">Crassin & Greene</a>] deftly avoided the mistakes I made by implementing his own pack/unpack functions. Still not sure why his implementation doesn't work for me though. 

**Note:** I tried to debug these in the Nsight shader debugger and got the message "Not a debuggable shader", so either it doesn't support atomics (unverified), or these "spinlock" type shaders are too clever for the debugger somehow (for now).

### References
[<a name="CG2"></a>Crassin & Greene] Octree-Based Sparse Voxelization Using the GPU Hardware Rasterizer <a href="http://www.seas.upenn.edu/~pcozzi/OpenGLInsights/OpenGLInsights-SparseVoxelization.pdf">http://www.seas.upenn.edu/%7Epcozzi/OpenGLInsights/OpenGLInsights-SparseVoxelization.pdf</a>
