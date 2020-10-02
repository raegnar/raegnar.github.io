---
layout: post
title: "GLSL sign function"
date: 2012-06-15 15:26
comments: true
tags: [GLSL]
---

The GLSL `sign` function always seems a great way to remove some unnecessary if statements from my shaders, but I never seem to get to use it because I always need to consider zero as either positive or negative, and not its own special value.

Anyway, I just realized you can accomplish the same thing with the `step` function.

~~~glsl
step(0, x)*2 - 1;
~~~

This will return `-1.0` if `x < 0`, and `1.0` if `x >= 0`.

Which is not terribly readable, hence this overly verbose function
~~~glsl
//returns -1.0 if x < 0, and 1.0 if x >= 0
float signGreaterEqualZero(float x)
{
	return step(0, x)*2 - 1;
}
~~~