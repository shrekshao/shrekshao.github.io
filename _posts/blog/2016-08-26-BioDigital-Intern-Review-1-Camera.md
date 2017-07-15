---
layout: post
title:  "BioDigital Summer Intern Review 1 - Camera tricks - Cylindrical coordinates"
comments: true
categories: Blog
tag: WebGL
---

I've literally spent my whole summer vacation at BioDigital for a complete 3-month internship. 
At this time I would like to make a review and introduce some tricks that may be generally useful in WebGL app development. 
In this first review, I will introduce some tricks that can help make the camera behaves more user-friendly for a display-stage style 3D app. 

<!--more-->

A display-stage style scene is a very common use case for WebGL 3D app. 
(Some Examples: [BioDigital Human](https://human.biodigital.com/), [Sketchfab](https://sketchfab.com/models/popular), [Car Visualizer](http://carvisualizer.plus360degrees.com/threejs/))
Basically we have a 3D object in the center of the scene, and a camera focusing at it, usually with orbiting and panning enabled. Sometimes it also allows the user to pick certain part 
of the object, and then focus on it, with a smooth camera navigation applied. 

Is there any improvement, or existing issues, with the camera system here? 



`Cylindrical coordinates` can be really beneficial in this display stage style scene. It fits our intuition of how the camera is supposed to navigate in this scene, 
compared to `Cartesian coordinates`.

![Cylindrical coordinates](/assets/blog-img/biodigital/CylindricalCoordinates_1001.gif)

`Cylindrical coordinates`: `(radius, theta, z)`. Their linear interpolation will create smooth camera animation of `(eye to look at distance, orbiting, vertical panning)`, which are 
three main camera movements we are expecting for such a scene.

We will find `Cylindrical coordinates` even more useful when we are trying to animate a camera navigating and focusing from one part of the object to another, 
without flying into the model and any predefined info, like this: 

![](/assets/blog-img/biodigital/camera.gif)

And this pic below (top view) shows how it is working. We are making the most of the feature of `Cylindrical coordinates`. 

![](/assets/blog-img/biodigital/cylindrical-cam-nav.png)

As shown in the picture, we know that the camera position will be on the line formed by center axis and object that we are looking at. 
The distance to the look at object can be decided by other elements (say, the length of the diagnal of the object bounding box).  
To make a curved smooth animation, 
all we have to do is linear interpolating the three coordinates values. 

That's pretty neat isn't it. 

Some edge cases do need certain treatment, one of which is when the object is very close to the center axis. 
To avoid some crazy orbiting from front to back, 
I use two ways to solve this: either interpolate the result with a linear translation in `Cartesian coordinates`, or get the rotation of the target camera position with current camera position 
taken into account. 
