---
layout: publication
title: Hybrid Computational Voxelization Using the Graphics Pipeline
---

voxelDragon		
Authors:	Randall Rauwendaal
Oregon State University
Mike Bailey
Oregon State University
[Journal	Journal of Computer Graphics Techniques](http://jcgt.org/)

|   | Authors | Randall Rauwendaal <br>Oregon State University |
|---|---------|------------------------------------------------|
|   |         | Mike Bailey<br>Oregon State University         |
|   | Journal | Journal of Computer Graphics Techniques        |

<table style="border:none;" width="100%" cellspacing="0">
<tbody>
<tr valign="top">
<td style="border:none;" width="200"><a href="https://randallr.files.wordpress.com/2013/03/voxeldragon.png"><img data-attachment-id="405" data-permalink="https://rauwendaal.net/publications/voxeldragon/" data-orig-file="https://randallr.files.wordpress.com/2013/03/voxeldragon.png" data-orig-size="274,257" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;}" data-image-title="voxelDragon" data-image-description="" data-medium-file="https://randallr.files.wordpress.com/2013/03/voxeldragon.png?w=274" data-large-file="https://randallr.files.wordpress.com/2013/03/voxeldragon.png?w=274" class="alignnone size-full wp-image-405" src="https://randallr.files.wordpress.com/2013/03/voxeldragon.png?w=705" alt="voxelDragon" srcset="https://randallr.files.wordpress.com/2013/03/voxeldragon.png 274w, https://randallr.files.wordpress.com/2013/03/voxeldragon.png?w=150 150w" sizes="(max-width: 274px) 100vw, 274px" title="" style=""></a></td>
<td style="border:none;" width="10px"></td>
<td style="border:none;">
<table style="border:none;" cellspacing="0">
<tbody>
<tr valign="top">
<td style="border:none;" width="50px"><b>Authors:</b></td>
<td style="border:none;"><b style="color:#000000;">Randall Rauwendaal</b><br>
<i>Oregon State University</i></td>
</tr>
<tr valign="top">
<td style="border:none;" width="50px"></td>
<td style="border:none;">Mike Bailey<br>
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

## Abstract
This paper presents an efficient computational voxelization approach that utilizes the graphics pipeline. Our approach is hybrid in that it performs a precise gap-free computational voxelization, employs fixed-function components of the GPU, and utilizes the stages of the graphics pipeline to improve parallelism. This approach makes use of the latest features of OpenGL and fully supports both conservative and thin-surface voxelization. In contrast to other computational voxelization approaches, our approach is implemented entirely in OpenGL and achieves both triangle and fragment parallelism through its use of geometry and fragment shaders. By exploiting features of the existing graphics pipeline, we are able to rapidly compute accurate scene voxelizations in a manner that integrates well with existing OpenGL applications, is robust across many different models, and eschews the need for complex work/load-balancing schemes.

## Downloads
[Full-Text PDF]() (6.4 MB)
[GLSL Code]() (11 kB)

## Links
[Journal of Computer Graphics Techniques page](http://jcgt.org/published/0002/01/02/)

## Citation
**R. Rauwendaal** and M. Bailey, Hybrid Computational Voxelization Using the Graphics Pipeline, *Journal of Computer Graphics Techniques (JCGT)*, vol. 2, no. 1, 15-37, 2013. Available online http://jcgt.org/published/0002/01/02/.

## Bibtex
```
@article{Rauwendaal2013Voxel,
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
}
```