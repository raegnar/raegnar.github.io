---
layout: post
title: OpenCV and OpenCL
# subtitle: Excerpt from Soulshaping by Jeff Brown
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
categories: [OpenGL, OpenCV]
tags: [OpenGL, OpenCV]
---

So I started using OpenCV for my Computer Vision class, but I didn’t want to give up my OpenGL based framework, and since I had such a hard time finding any hints on how to convert OpenCV Images to OpenGL textures, I’m going to post the technique I used here.  What I did eventually find was this, which didn’t immediately work for me as written.

So OpenCV images are stored in these IplImage structs, and they’re actually pretty great because they load just about anything

~~~glsl
IplImage *image = cvLoadImage("filename");
~~~

So after you create you OpenCV Image, how do you get an OpenGL texture.  Well, OpenCV images are stored as unsigned bytes so so you’re going to want your texture type to be `GL_UNSIGNED_BYTE`, and most of the other parameters  to pass to `glTexImage2D` come right out of the `IplImage` struct, the only thing to be wary of is swapping the RGB colors, if you don’t, red will look blue, and blue will look red.  So be sure to set internalFormat to `GL_RGB`, and format to `GL_BGR` like so

~~~glsl
glTexImage2D(GL_TEXTURE_2D,        //target
             0,                    //level
             GL_RGB,               //internalFormat
             image->width,         //width
             image->height,        //height
             0,                    //border
             GL_BGR,               //format
             GL_UNSIGNED_BYTE,     //type
             image->imageData);    //pointer to image data
~~~

Of course, this only works if your Image is color, if your Image is grayscale your going to want to change `GL_BGR` to `GL_LUMINANCE`

~~~glsl
glTexImage2D(GL_TEXTURE_2D,        //target
             0,                    //level
             GL_RGB,               //internalFormat
             image->width,         //width
             image->height,        //height
             0,                    //border
             GL_LUMINANCE,         //format
             GL_UNSIGNED_BYTE,     //type
             image->imageData);    //pointer to image data
~~~

And you could probably change the internal format of the OpenGL texture as well, but I don’t presume to know what you want to do with this. And one more snippet for good measure, this time loading a color image and converting it to a gray scale image all in OpenCV.

~~~glsl
IplImage *color_image = cvLoadImage("filename");
IplImage *grayscale = cvCreateImage(cvGetSize(color_image), 8, 1);
cvCvtColor(color_image, grayscale, CV_BGR2GRAY);
~~~
