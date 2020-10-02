---
layout: post
title: Hybrid Computational Voxelization Using the Graphics Pipeline
date: 2013-03-18 10:58
comments: true
categories: [Research]
tags: [Research]
---
<a href="http://randallr.files.wordpress.com/2013/03/dragonforward.png"><img alt="dragonForward" src="http://randallr.files.wordpress.com/2013/03/dragonforward.png" width="630" height="196" /></a>

Got a paper published in the Journal of Computer Graphics Techniques, see it [here](http://jcgt.org/published/0002/01/02/)

>This paper presents an efficient computational voxelization approach that utilizes the graphics pipeline. Our approach is hybrid in that it performs a precise gap-free computational voxelization, employs fixed-function components of the GPU, and utilizes the stages of the graphics pipeline to improve parallelism. This approach makes use of the latest features of OpenGL and fully supports both conservative and thin-surface voxelization. In contrast to other computational voxelization approaches, our approach is implemented entirely in OpenGL and achieves both triangle and fragment parallelism through its use of geometry and fragment shaders. By exploiting features of the existing graphics pipeline, we are able to rapidly compute accurate scene voxelizations in a manner that integrates well with existing OpenGL applications, is robust across many different models, and eschews the need for complex work/load-balancing schemes.
