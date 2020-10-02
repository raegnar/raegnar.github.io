---
layout: page
title: Layered Reflective Shadow Maps for Voxel-based Indirect Illumination
date: 2014-05-30 09:40
author: randallr
comments: true
categories: []
---
<table style="border:none;" width="100%" cellspacing="0">
<tbody>
<tr valign="top">
<td style="border:none;" width="200"><a href="http://randallr.files.wordpress.com/2014/05/lrsmdragons.png"><img class="alignnone size-full wp-image-405" src="http://randallr.files.wordpress.com/2014/05/lrsmdragons.png" alt="LRSMDragons" width="274" height="257" /></a></td>
<td style="border:none;" width="10px"></td>
<td style="border:none;">
<table style="border:none;" cellspacing="0">
<tbody>
<tr valign="top">
<td style="border:none;" width="50px"><b>Authors:</b></td>
<td style="border:none;"><a style="color:#000000;" href="https://software.intel.com/en-us/intel-rendering-technologies#masamichi">Masamichi Sugihara</a>
<i><a href="https://software.intel.com/en-us/intel-rendering-technologies">Advanced Rendering Technology</a>, Intel</i></td>
</tr>
<tr valign="top">
<td style="border:none;" width="50px"></td>
<td style="border:none;"><b style="color:#000000;">Randall Rauwendaal</b>
<i>Advanced Technology Group, Intel</i></td>
</tr>
<tr valign="top">
<td style="border:none;" width="50px"></td>
<td style="border:none;"><a style="color:#000000;" href="https://software.intel.com/en-us/intel-rendering-technologies#marco">Marco Salvi</a>
<i><a href="https://software.intel.com/en-us/intel-rendering-technologies">Advanced Rendering Technology</a>, Intel</i></td>
</tr>
<tr>
<td style="border:none;vertical-align:text-top;" width="50px"><b>Conference</b></td>
<td style="border:none;"><a href="http://www.highperformancegraphics.org/2014/">High-Performance Graphics 2014</a></td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>
<h2>Abstract</h2>
<div>We introduce a novel voxel-based algorithm that interactively simulates both diffuse and glossy single-bounce indirect illumination. Our algorithm generates high quality images similar to the reference solution while using only a fraction of the memory of previous methods. The key idea in our work is to decouple occlusion data, stored in voxels, from lighting and geometric data, encoded in a new per-light data structure called layered reflective shadow maps (LRSMs). We use voxel cone tracing for visibility determination and integrate outgoing radiance by performing lookups in a pre-filtered LRSM. Finally we demonstrate that our simple data structures are easy to implement and can be rebuilt every frame to support both dynamic lights and scenes.</div>
&nbsp;
<h2>Links</h2>
<a href="https://software.intel.com/en-us/articles/layered-reflective-shadow-maps-for-voxel-based-indirect-illumination">Intel Developer Zone page</a>
<h2>Citation</h2>
M. Sugihara, <b style="color:#000000;">R. Rauwendaal</b>, and M. Salvi, Layered Reflective Shadow Maps for Voxel-based Indirect Illumination, In <i>Proceedings of High-Performance Graphics (2014)</i>.
<h2>Bibtex</h2>
<pre style="word-wrap:break-word;white-space:pre-wrap;">@inproceedings{Sugihara2014,
  author  = {Masamichi Sugihara and Randall Rauwendaal and Marco Salvi},
  title   = {Layered Reflective Shadow Maps for Voxel-based Indirect Illumination},  
  pages   = {117--125}
  year    = {2014},
  DOI     = {10.2312/hpg.20141100},
  journal = {Proceedings of High Performance Graphics 2014},
}

</pre>
<h2></h2>
