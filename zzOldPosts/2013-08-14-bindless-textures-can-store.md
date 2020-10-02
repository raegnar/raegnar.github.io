---
layout: post
title: Bindless textures can "store"
date: 2013-08-14 09:46
author: randallr
comments: true
categories: [OpenGL]
---
I don't know how I missed this when Nvidia released <a href="http://www.opengl.org/registry/specs/NV/bindless_texture.txt"><code>NV_bindless_texture</code></a>, I guess because all the samples I saw used bindless textures to demonstrate a ridiculous number of texture reads. But I realized when reading the recently released <a href="http://www.opengl.org/registry/specs/ARB/bindless_texture.txt"><code>ARB_bindless_texture</code></a> extension that they can also be used to "store," or write, to a very large number of textures (using <a href="http://www.opengl.org/registry/specs/ARB/shader_image_load_store.txt"><code>ARB_shader_image_load_store</code></a> functionality). Which finally gets rid of that extremely pesky <code>MAX_IMAGE_UNITS</code> limitation I've been complaining about. The only downside is that I can no longer run my program at home on my GTX 480.
