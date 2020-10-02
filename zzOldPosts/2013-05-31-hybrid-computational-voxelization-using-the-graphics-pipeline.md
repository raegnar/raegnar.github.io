---
layout: page
title: Hybrid Computational Voxelization Using the Graphics Pipeline
date: 2013-05-31 14:22
author: randallr
comments: true
categories: []
---
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
<tr valign="top">
<td style="border:none;" width="50px"></td>
<td style="border:none;">Mike Bailey
<i>Oregon State University</i></td>
</tr>
<tr>
<td style="border:none;vertical-align:text-top;" width="50px"><b>Journal</b></td>
<td style="border:none;"><a href="http://jcgt.org/">Journal of Computer Graphics Techniques</a></td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>
<h2>Abstract</h2>
<div>This paper presents an efficient computational voxelization approach that utilizes the graphics pipeline. Our approach is hybrid in that it performs a precise gap-free computational voxelization, employs fixed-function components of the GPU, and utilizes the stages of the graphics pipeline to improve parallelism. This approach makes use of the latest features of OpenGL and fully supports both conservative and thin-surface voxelization. In contrast to other computational voxelization approaches, our approach is implemented entirely in OpenGL and achieves both triangle and fragment parallelism through its use of geometry and fragment shaders. By exploiting features of the existing graphics pipeline, we are able to rapidly compute accurate scene voxelizations in a manner that integrates well with existing OpenGL applications, is robust across many different models, and eschews the need for complex work/load-balancing schemes.</div>
<h2>Downloads</h2>
<div><span style="margin-right:20px;"><a href="https://randallr.files.wordpress.com/2015/12/hybridvoxelization.pdf"><img src="http://randallr.files.wordpress.com/2013/06/pdf_icon-32x32.png" alt="" width="32px" border="0" /> Full-Text PDF</a> (6.4 MB)</span>
<span style="margin-right:20px;"><a href="http://jcgt.org/published/0002/01/02/code.zip"><img src="http://randallr.files.wordpress.com/2013/06/zip_icon-32x322.png" alt="" width="32px" border="0" /> GLSL Code</a> (11 kB)</span></div>
<h2>Links</h2>
<a href="http://jcgt.org/published/0002/01/02/">Journal of Computer Graphics Techniques page</a>
<h2>Citation</h2>
<b style="color:#000000;">R. Rauwendaal</b> and M. Bailey, Hybrid Computational Voxelization Using the Graphics Pipeline, <i>Journal of Computer Graphics Techniques (JCGT)</i>, vol. 2, no. 1, 15-37, 2013. Available online <a href="http://jcgt.org/published/0002/01/02/">http://jcgt.org/published/0002/01/02/</a>.
<h2>Bibtex</h2>
<pre style="word-wrap:break-word;white-space:pre-wrap;">@article{Rauwendaal2013Voxel,
  author  = {Randall Rauwendaal and Mike Bailey},
  title   = {Hybrid Computational Voxelization Using the Graphics Pipeline},  
  year    = {2013},
  month   = {March},
  day     = {18},
  journal = {Journal of Computer Graphics Techniques (JCGT)},
  volume  = {2},
  number  = {1},
  pages   = {15--37},
  url     = {http://jcgt.org/published/0002/01/02/}
}</pre>
