---
layout: post
title: "Writing to 3-components buffers using the image API in OpenGL"
date: 2012-08-27 14:07
comments: true
tags: [GLSL, OpenGL]
---

As I've described in detail in another blogpost atomic counters used in conjunction with the image API and indirect draw buffers can be an excellent and highly performant alternative/replacement to the `transformFeedback` mechanism (oh wait, I still haven't published that previous blogpost... and performant is not actually a real word).

Anyway, one place where this atomic counter + image API + indirect buffers approach becomes a little cumbersome, is its slightly less than elegant handling of 3-components buffer texture formats.

In the <a href="http://www.opengl.org/registry/doc/glspec42.core.20120427.withchanges.pdf">OpenGL 4.2 spec</a> the list of supported buffer texture formats is listed in table 3.15, while the list of supported image unit formats is listed in table 3.21.  The takeaway from comparing these tables is that the supported image unit formats generally omit 3 components formats (other than the `GL_R11F_G11F_B10F` format).  So how to deal with this if you have a say a `GL_RGB32F`, or `GL_RGB32UI` internal format? Well, its actually pretty easy; just bind the proxy texture as the one component version of the internal format (`GL_R32F`, or `GL_R32UI`).

~~~cpp
glBindImageTexture(0, buffer_proxy_tex, 0, GL_TRUE, 0, GL_WRITE_ONLY, GL_R32F);
~~~

Then in the shader put a 3-component stride on the atomic counter, then store each component with its own `imageStore` operation.

~~~glsl
layout(binding = 0)         uniform atomic_uint atomicCount;
layout(rgb32f, binding = 0) uniform imageBuffer positionBuffer;

void main()
{
  //Some other code...

  int index = 3*int(atomicCounterIncrement(atomicCount));

  imageStore(positionBuffer, index+0, vec4(x));
  imageStore(positionBuffer, index+1, vec4(y));
  imageStore(positionBuffer, index+2, vec4(z));

  //Some more code...
}
~~~

And that actually works great, in my experience, replacing `transformFeedback` with this approach has been as fast or faster despite the multiple `imageStore` calls.
