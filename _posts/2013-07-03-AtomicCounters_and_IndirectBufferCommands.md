---
layout: post
title: "AtomicCounters & IndirectBufferCommands"
date: 2013-07-03 10:27
comments: true
categories: [OpenGL, GLSL]
tags: [OpenGL, GLSL]
---

I’ve made use of Atomic Counters and Indirect Buffers in the past, but always in the most straightforward manner. I.e. create a dedicated buffer for the atomic counter, and another for the Indirect Command Buffer, increment the counter in a shader then write the Atomic Counter value into the Indirect Command Buffer using the [Image API](https://www.khronos.org/registry/OpenGL/extensions/ARB/ARB_shader_image_load_store.txt), ending up with a shader that looks something like below.

~~~glsl
#version 420
 
layout(location = 0) in ivec3 inputBuffer;
 
layout(r32ui, binding = 0) uniform uimageBuffer outputBuffer;
layout(r32ui, binding = 1) uniform uimageBuffer indirectArrayCommand;
layout(       binding = 0) uniform atomic_uint  atomicCounter;
 
void main()
{
    // ...
    // do some stuff
    // ...
 
    if(someCondition == true)
    {
        //increment counter
        int index = int(atomicCounterIncrement(atomicCounter));
 
        //store stuff in output buffer
        imageStore(outputBuffer, index, uvec4(someStuff)));
    }
 
    memoryBarrier();
 
    //Store the atomicCounter value to the count (the first element) of the DrawArraysIndirect command
    imageStore(indirectArrayCommand, 0, uvec4(atomicCounter(atomicCounter)));
}
~~~

This works fine, but one annoying thing about this approach is that it consumes an extra image unit (of the max 8 available). Fortunately, it turns out that it is unnecessary to create an extra atomic counter and perform the synchronization with the indirect draw command. It is possible to simply bind the appropriate element of the indirect draw buffer directly to the atomic counter.

~~~glsl
// This binds the count element of the Indirect Array Command Buffer directly as an atomic counter in the shader
// (no need for copy from dedicated atomic counter)
glBindBufferRange(GL_ATOMIC_COUNTER_BUFFER,        // Target buffer is the atomic counter
                  0,                               // Binding point, must match the shader
                  IndirectArrayCommandBuffer_id,   // Source buffer is the Indirect Draw Command Buffer
                  0,                               // Offset, 0 for count, 1 for primCount (instances), etc...
                  sizeof(GLuint));
~~~

This allows us to get rid of Indirect Buffers image unit binding, which simplifies the shader as shown below. The main reason I’ve found to do this is reduce the number of image units required by the shader, as its very easy to hit the limit of 8.

~~~glsl
#version 420
 
layout(location = 0) in ivec3 inputBuffer;
 
layout(r32ui, binding = 0) uniform uimageBuffer outputBuffer;
layout(       binding = 0) uniform atomic_uint  atomicCounter;
 
void main()
{
    // ...
    // do some stuff
    // ...
 
    if(someCondition == true)
    {
        //increment counter
        int index = int(atomicCounterIncrement(atomicCounter));
 
        //store stuff in output buffer
        imageStore(outputBuffer, index, uvec4(someStuff)));
    }
}
~~~