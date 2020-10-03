---
layout: post
title: "OpenGL 4.3 released"
date: 2012-08-06 11:44
comments: true
tags: [OpenGL]
---

OpenGL 4.3 has just been released and almost instantly <a href="http://www.g-truc.net/">G-Truc</a> (Christophe Riccio) posted another excellent of his excellent <a href="http://www.g-truc.net/doc/OpenGL4.3review.pdf">OpenGL reviews</a>.  Additionally <a href="http://web.engr.oregonstate.edu/~mjb/WebMjb/mjb.html">Mike Bailey</a> has already made available some <a href="http://web.engr.oregonstate.edu/~mjb/sig12/compute.shader.1pp.pdf">slides</a> on the new Compute shaders.  And as usual nvidia already has <a href="http://www.nvidia.com/content/devzone/opengl-driver-4.3.html">beta drivers</a> available.

With over 20 new extensions this is a rather large update for a point release, and while I'm extremely grateful that people have had a chance preview these extensions and provide an alternative to the pain of reading through the extensions themselves (though I will probably do that anyway), I can't help but wish that this sort of access wasn't so exclusive.  I find the current method of dumping a bunch of new extensions out each SIGGRAPH and shouting "Surprise!" a bit jarring, and more tragically there is no mechanism to allow the OpenGL community at large to provide feedback (unless you are a member of Khronos, I guess).

If you look at the extensions I think they lend themselves perfectly to publication on some sort of official Wiki, revision history would be managed automatically, the OpenGL community could provide feedback on the discussion page, and Khronos members would have permission to make the actual edits to the extension.  I guess what I am saying, in particular regards to the publication of new extensions, is that I wish OpenGL were a little bit more "open."

* <a href="http://www.opengl.org/registry/doc/glspec43.core.20120806.withchanges.pdf">OpenGL 4.3 core specification (with changes)</a></li>
* <a href="http://www.opengl.org/registry/doc/GLSLangSpec.4.30.6.pdf">GLSL 4.3 specification</a></li>
* <a href="http://www.g-truc.net/doc/OpenGL4.3review.pdf">OpenGL 4.3 review</a></li>

<em>Extensions:</em>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_arrays_of_arrays.txt">ARB_arrays_of_arrays</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_clear_buffer_object.txt">ARB_clear_buffer_object</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_copy_image.txt">ARB_copy_image</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_ES3_compatibility.txt">ARB_ES3_compatibility</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_explicit_uniform_location.txt">ARB_explicit_uniform_location</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_fragment_layer_viewport.txt">ARB_fragment_layer_viewport</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_framebuffer_no_attachments.txt">ARB_framebuffer_no_attachments</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_internalformat_query2.txt">ARB_internalformat_query2</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_invalidate_subdata.txt">ARB_invalidate_subdata</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_program_interface_query.txt">ARB_program_interface_query</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_robust_buffer_access_behavior.txt">ARB_robust_buffer_access_behavior</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_stencil_texturing.txt">ARB_stencil_texturing</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_texture_buffer_range.txt">ARB_texture_buffer_range</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_texture_query_levels.txt">ARB_texture_query_levels</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_texture_storage_multisample.txt">ARB_texture_storage_multisample</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_texture_view.txt">ARB_texture_view</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_vertex_attrib_binding.txt">ARB_vertex_attrib_binding</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_KHR_debug.txt">KHR_debug</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_compute_shader.txt">ARB_compute_shader</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_multi_draw_indirect.txt">ARB_multi_draw_indirect</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_shader_image_size.txt">ARB_shader_image_size</a>
* <a href="http://us.download.nvidia.com/opengl/specs/GL_ARB_shader_storage_buffer_object.txt">ARB_shader_storage_buffer_object</a>
