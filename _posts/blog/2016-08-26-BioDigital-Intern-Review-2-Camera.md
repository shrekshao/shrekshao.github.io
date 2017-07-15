---
layout: post
title:  "BioDigital Summer Intern Review 2 - Camera tricks - Mouse Orbiting issue"
comments: true
categories: Blog
tag: WebGL
---

Continuing our last blog, we are looking at some other issue of camera in WebGL apps with a display stage style scene. 

Another common camera issue in these apps is that after we pan the camera, orbiting becames kind of weird. 
This is because we orbit camera around its look at position, and now look at position is not centered at the object anymore. Lacking of other 
referencing scene objects (like terrain, walls), the orbiting seems to us like floating in the air doesn't follow control nicely.  

<!--more-->

One solution is that we transform the object itself instead of the camera. This comes out more intuitive to us human. 

However, transforming the object might be painful. For example, the object animation might fail. And it doesn't make sense to us programmer. 
Cuz we are indeed correctly transforming the camera. It is not our fault. However, PM and user might only care how they feel when they cannot find the focus of the camera after some crazy panning. 

I came up with a hack that we still transform the camera only, but in a way as if we are actually transforming the object, which is making use of the principle of relativity. 

We know that the transformation matrix goes like `ProjectMatrix * ViewMatrix * ModelMatrix`. What we are actually doing, is treating the matrix in `ModelMatrix` as part of the `ViewMatrix`. 
Say we are orbiting the object, then there must be a rotation matrix R in `ModelMatrix`. We are now treating it as part of the ViewMatrix. So the result won't be changed, but we 
are now transforming only the camera. This is actually using the Associative property of Matrix multiplication. 

We have proved it is doable by only transforming the camera, and the result will be the same as transforming the object. But still, how do we apply the user (say mouse) interaction to the camera transformation?

Again Let's use some visual aid: 

![](/assets/blog-img/biodigital/camera-eye-lookat-orbit.png)

So we found out transforming the object is equivalent to transforming the eye-to-look-at vector! 

Before: 

![](/assets/blog-img/biodigital/camera-lookat-old.gif)

After:

![](/assets/blog-img/biodigital/camera-lookat.gif)