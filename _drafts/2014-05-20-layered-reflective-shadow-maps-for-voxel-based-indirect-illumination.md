---
layout: post
title: Layered Reflective Shadow Maps for Voxel-based Indirect Illumination
date: 2014-05-20 08:55
author: randallr
comments: true
categories: [Research]
---
So, a lot has happened. I completed my <a href="http://rauwendaal.net/phd-thesis/">Doctorate</a>, almost moved to Norway, but then ended up moving to Canada instead (Victoria, BC). I now work for the Advanced Technology Group at Intel, where I was very fortunate enough  to have the opportunity to assist a new colleague of mine, Masamichi Sugihara (<a href="https://twitter.com/masasugihara">@masasugihara</a>), with his publication "<a title="Layered Reflective Shadow Maps for Voxel-based Indirect Illumination" href="http://rauwendaal.net/layered-reflective-shadow-maps-for-voxel-based-indirect-illumination/">Layered Reflective Shadow Maps for Voxel-based Indirect Illumination</a>," which has been accepted to <a href="http://www.highperformancegraphics.org/2014/program/">HPG 2014</a>.

<a href="https://randallr.files.wordpress.com/2014/05/layered_reflective_shadow_maps_for_voxel-based_indirect.jpg"><img class="alignnone wp-image-741 size-full" src="https://randallr.files.wordpress.com/2014/05/layered_reflective_shadow_maps_for_voxel-based_indirect.jpg" alt="" width="800" height="150" /></a>
Check out the preprint <a href="https://software.intel.com/sites/default/files/Layered_Reflective_Shadow_Maps_for_Voxel-based_Indirect.pdf">here</a>
<blockquote>We introduce a novel voxel-based algorithm that interactively simulates both diffuse and glossy single-bounce indirect illumination. Our algorithm generates high quality images similar to the reference solution while using only a fraction of the memory of previous methods. The key idea in our work is to decouple occlusion data, stored in voxels, from lighting and geometric data, encoded in a new per-light data structure called layered reflective shadow maps (LRSMs). We use voxel cone tracing for visibility determination and integrate outgoing radiance by performing lookups in a pre-filtered LRSM. Finally we demonstrate that our simple data structures are easy to implement and can be rebuilt every frame to support both dynamic lights and scenes.</blockquote>
<h3><strong>Hire Masamichi!</strong></h3>
Due to some rather shortsighted reorganization, Masasmichi is currently pursuing employment opportunities that will either; allow him to stay in Canada, or return to Japan. If you are interested in hiring a top-notch graphics coder, please get in touch.
