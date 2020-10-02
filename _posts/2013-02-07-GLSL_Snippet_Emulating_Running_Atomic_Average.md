---
layout: post
title: "GLSL Snippet: emulating running atomic average of colors using imageAtomicCompSwap"
date: 2013-02-07 12:12
comments: true
tags: [GLSL, OpenGL]
---

This is basically straight out of the [Crassin & Greene][CG] chapter from the excellent <a href="http://openglinsights.com/">OpenGL Insights</a> book, which calculates a running average for a RGB voxel color and stores it into a RGBA8 texture (using the alpha component as an access count).  But for whatever reason when I dropped their GLSL snippet into my code I couldn't get it to work correctly.  So, I attempted to rewrite it as simply as possible, and basically ended up with almost the same thing except I used the provided GLSL functions <code>packUnorm4x8</code> and the <code>unpackUnorm4x8</code> instead of rolling my own, so it's ever so slightly simpler.

Anyway, I've verified that this <del>(mostly)</del> works on a GTX 480, <del>I still get a small bit of flickering on a few voxels</del>. Flickering has been fixed, and also works on a GTX Titan.

~~~cpp
void imageAtomicAverageRGBA8(layout(r32ui) coherent volatile uimage3D voxels, ivec3 coord, vec3 nextVec3)
{
	uint nextUint = packUnorm4x8(vec4(nextVec3,1.0f/255.0f));
	uint prevUint = 0;
	uint currUint;

	vec4 currVec4;

	vec3 average;
	uint count;

	//&quot;Spin&quot; while threads are trying to change the voxel
	while((currUint = imageAtomicCompSwap(voxels, coord, prevUint, nextUint)) != prevUint)
	{
		prevUint = currUint;					//store packed rgb average and count
		currVec4 = unpackUnorm4x8(currUint);	//unpack stored rgb average and count

		average =      currVec4.rgb;		//extract rgb average
		count   = uint(currVec4.a*255.0f);	//extract count

		//Compute the running average
		average = (average*count + nextVec3) / (count+1);

		//Pack new average and incremented count back into a uint
		nextUint = packUnorm4x8(vec4(average, (count+1)/255.0f));
	}
}
~~~

This works by using the <code>imageAtomicCompSwap</code> function to effectively implement a <a href="http://en.wikipedia.org/wiki/Spinlock">spinlock</a>, which "spins" until all threads trying to access the voxel are done.

Apparently, the compiler can be quite picky about how things like this are written (don't use "<code>break</code>" statements), see this thread <a href="https://devtalk.nvidia.com/default/topic/526793/opengl/glsl-loop-39-break-39-instruction-not-executed/">GLSL loop 'break' instruction not executed</a> for more information, <del>and I can't guarantee this will work on Kepler or any other architectures</del>, and it definitely works fine for both Fermi and Kepler architectures, if anyone can let me know how it works on an AMD architecture I'll add that information here.

<strong>Edit/Update: </strong>So I had a few mistakes in my previous implementation which weren't very noticeable in a sparsely tessellated model (like the Dwarf), but became much more noticeable as triangle density increased (like in the curtains and plants of the Sponza model).  Anyway, it turned out I hadn't considered the effects of the <code>packUnorm4x8</code> and <code>unpackUnorm4x8</code> functions correctly. The <code>packUnorm4x8</code> function clamps input components from 0 to 1, so the count updates were getting discarded, and obviously the average was coming out wrong.  Anyway, the solution was to divide by 255 when "packing" the count, and multiply by 255 when unpacking.  This method should work with up to 255 threads attempting to write to the same voxel location.

### References
[<a name="CG"></a>Crassin & Greene] Octree-Based Sparse Voxelization Using the GPU Hardware Rasterizer <a href="http://www.seas.upenn.edu/~pcozzi/OpenGLInsights/OpenGLInsights-SparseVoxelization.pdf">http://www.seas.upenn.edu/%7Epcozzi/OpenGLInsights/OpenGLInsights-SparseVoxelization.pdf</a>
