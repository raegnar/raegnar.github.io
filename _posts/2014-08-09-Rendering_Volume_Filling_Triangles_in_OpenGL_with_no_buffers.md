---
layout: post
title: "Rendering Volume Filling Triangles in OpenGL (with no buffers)"
comments: true
tags: [OpenGL]
---

This is the promised follow-up to [Rendering a Screen Covering Triangle in OpenGL (with no buffers)](2014-06-14-Rendering a Screen Covering Triangle in OpenGL (with no buffers).md), except this time the goal is to write a shader that accesses every location in a 3d texture (volume).  We use the same screen covering trick as before to draw a triangle to cover a viewport match to the X and Y dimensions of the volume, and we use instanced rendering to draw repeated triangles for each layer in the Z-dimension.

The vertex shader looks the same as before with the addition of the ```instanceID```.

~~~glsl
flat out int instanceID;
 
void main()
{
    float x = -1.0 + float((gl_VertexID & 1) << 2);
    float y = -1.0 + float((gl_VertexID & 2) << 1);
    instanceID  = gl_InstanceID;
    gl_Position = vec4(x, y, 0, 1);
}
~~~

The fragment shader can then recover the voxel coordinates from ```gl_FragCoord``` and the ```instanceID```.

~~~glsl
flat in int instanceID;
 
void main()
{
    ivec3 voxelCoord = ivec3(gl_FragCoord.xy, instanceID);
    voxelOperation(voxelCoord);
}
~~~

Very similar to drawing the single screen covering triangle, we set our viewport to the XY-dimensions of the volume, bind a junk VAO to make certain graphics drivers happy, and call ```glDrawArraysInstanced``` with the Z-dimension of the volume, so that we draw a triangle per-slice of the volume.

~~~glsl
glViewport(0, 0, width, height);
glBindVertexArray(junkVAO);
glDrawArraysInstanced(GL_TRIANGLE_STRIP, 0, 3, depth);
~~~

Which would look sort of like the following:

![glScreenSpaceTriangle](/assets/img/volumefillingtriangles.png){:height="50%" width="50%"}

This can be useful for quickly processing a volume. Initially, I used this as an OpenGL 4.2 fallback (instead of compute shaders) so that I could still use the NSight debugger, until I realized this approach was actually outperforming the compute shader. Of course, when to use compute shaders, and how to use them effectively deserves a post of its own.