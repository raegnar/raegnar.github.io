---
layout: post
title: "CUDA 5: Enabling Dynamic Parallelism"
date: 2013-03-23 11:16
comments: true
categories: [CUDA, Dynamic Parallelism]
tags: [CUDA, Dynamic Parallelism]
---

I finally got a GPU capable of dynamic parallelism, so I decided to mess around with CUDA 5. But I discovered a couple of configuration options that are required if you want to enable dynamic parallelism. You'll know you haven't configured things correctly if you attempt to call a kernel from the device and you get the following error message:

>ptxas : fatal error : Unresolved extern function 'cudaGetParameterBuffer'

Note: this assume you have already selected the appropriate CUDA 5 build customizations for your project

Open the project project properties
* <span style="line-height:13px;">Make sure to set "Generate Relocatable Device Code" to **"Yes (-rdc=true)"**<a href="http://randallr.files.wordpress.com/2013/03/yes.png"><img class="alignnone size-full wp-image-350" alt="yes" src="http://randallr.files.wordpress.com/2013/03/yes.png" width="630" height="116" /></a></span></li>
* Set "code generation" to **"compute_35,sm_3"**<a href="http://randallr.files.wordpress.com/2013/03/compute1.png"><img class="alignnone size-full wp-image-352" alt="compute" src="http://randallr.files.wordpress.com/2013/03/compute1.png" width="630" height="69" /></a></li>
* Finally add **"cudadevrt.lib"** to the CUDA Linker's "Additional Dependencies"<a href="http://randallr.files.wordpress.com/2013/03/cudadevrt.png"><img class="alignnone size-full wp-image-353" alt="cudadevrt" src="http://randallr.files.wordpress.com/2013/03/cudadevrt.png" width="609" height="160" /></a></li>
