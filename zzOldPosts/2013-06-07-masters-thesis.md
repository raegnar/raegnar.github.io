---
layout: page
title: Master's Thesis
date: 2013-06-07 13:04
author: randallr
comments: true
categories: []
---
<span style="font-size:1.5em;">Hybrid Computational Voxelization Using the Graphics Pipeline</span>
<table style="border:none;" width="100%" cellspacing="0">
<tbody>
<tr valign="top">
<td style="border:none;" width="200"><a href="http://randallr.files.wordpress.com/2013/03/voxeldragon.png"><img class="alignnone size-full wp-image-405" src="http://randallr.files.wordpress.com/2013/03/voxeldragon.png" alt="voxelDragon" width="274" height="257" /></a></td>
<td style="border:none;" width="10px"></td>
<td style="border:none;">
<table style="border:none;" cellspacing="0">
<tbody>
<tr valign="top">
<td style="border:none;" width="50px"><b>Authors:</b></td>
<td style="border:none;"><b style="color:#000000;">Randall Rauwendaal</b>
<i>Oregon State University</i></td>
</tr>
<tr>
<td style="border:none;"></td>
<td style="border:none;"></td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>
<h2>Abstract</h2>
<div>This thesis presents an efficient computational voxelization approach that utilizes the graphics pipeline. Our approach is hybrid in that it performs a precise gap-free computational voxelization, employs fixed-function components of the GPU, and utilizes the stages of the graphics pipeline to improve parallelism. This approach makes use of the latest features of OpenGL and fully supports both conservative and thin voxelization. In contrast to other computational voxelization approaches, this approach is implemented entirely in OpenGL, and achieves both triangle and fragment parallelism through its use of both the geometry and fragment shaders. A novel approach utilizing the graphics pipeline to complement geometric triangle intersection computations is presented. By exploiting features of the existing graphics pipeline we are able to rapidly compute accurate scene voxelization in a manner that integrates well with existing OpenGL applications, is robust across many different models, and eschews the need for complex work/load-balancing schemes.</div>
&nbsp;
<h2>Links</h2>
<a href="http://hdl.handle.net/1957/35463">ScholarsArchive@OSU</a>
<h2>Citation</h2>
<b style="color:#000000;">R. Rauwendaal</b>, Hybrid Computational Voxelization Using the Graphics Pipeline, <i>Master's thesis</i>, Oregon State University, 2012. Available online: <a href="http://hdl.handle.net/1957/35463">http://hdl.handle.net/1957/35463</a>
<h2>Bibtex</h2>
<pre style="word-wrap:break-word;white-space:pre-wrap;">@MastersThesis{Rauwendaal2012Masters,
  author = {Randall Rauwendaal},
  title  = {Hybrid Computational Voxelization Using the Graphics Pipeline},
  school = {Oregon State University},
  year   = {2012},
  month  = {11},
  day    = {29},
  url    = {http://hdl.handle.net/1957/35463}
}</pre>
<h2>Note</h2>
This is functionally the same material presented in the <a href="http://jcgt.org/published/0002/01/02/">JCGT paper</a> of the same name. I would recommend reading the <a href="http://jcgt.org/published/0002/01/02/">JCGT paper</a> over the thesis as it incorporates the excellent suggestions from their reviewers.
