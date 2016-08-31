---
layout: post
title:  "BioDigital Summer Intern Review 3 - Cross Section"
comments: true
categories: Blog
tag: WebGL
img: /assets/blog-img/biodigital/cross-section.png
---


Cross section is a feature that can be very useful and heavily used in 
WebGL app for medical and architecture usage. 

Basically it's when given a set of clipping planes, we should clip 
the geometries that fall outside of the clipping planes, and draw the 
clipping caps. In addition, we would prefer a dynamic real-time solution 
which doesn't need any predefined knowledge of the geometries.   

<!--more-->
One easy solution is simply discarding any fragment that is on the other side 
of each clipping planes. The job can be done in a fragment shader (derived from SceneJS): 

```GLSL
uniform vec4 u_ClippingPlanes;  // xyz: normal vector, w: distance from origin
// ...
void main() {
    // ...
    dist += clamp(dot(a_position.xyz, u_ClippingPlanes.xyz) - u_ClippingPlanes.w, 0.0, 1000.0);
    // ...
    if (dist > 0.0) {
        discard;
    }
}
```

It's easy, straight-forward, fast, no need for predefined geometry info, and works fine if we only want to do the clipping. 
But when it turns to drawing the caps, there's problems. Cuz we don't have any geometry info of the clipping caps. 

However there's still one hack to draw the caps in some simple situations. 
That is: we color the exposing back face with a solid color. 
So we will create an illusion of a cap being there. 

![](/assets/blog-img/biodigital/naive-clipping.png)


But there's a lot limitation. First the illusion will break if we have multiple clipping planes: 
we will see through the holes of the clipping planes on the other side. 

![](/assets/blog-img/biodigital/naive-clipping-multiple.png)

Next, we can only color the "cap" (It's actually the backfaces) with a solid color, or 
it doesn't look like a cap anymore. 

So is there any alternative methods that can give us a better cross section feature? 

Finally I find [this brilliant repo](https://github.com/daign/clipping-with-caps) 
and mostly I do is porting it from three.js to sceneJS. And I successfully reduce one draw pass. 

Basically what it does is using a stencil Buffer to tell us where shall we draw 
a fragment for the clipping caps. The whole pipeline consists of three passes. 

* First pass: draw the scene with a cheap shader with **only clipped by clipping planes on the same side with the camera**, which is used to write the stencil Buffer 
* Second pass: draw the actual scene clipped by **all clipping planes** 
* Third pass: draw the capping geometry (a box in our case) with a stencil Function where only fragments passing the stencil test will be drawn

We've already gone through how to clip within a fragment shader so we should be comfortable with 
the second pass. Let's focus on the tricky first and third pass.

### First pass

This is the most important and most tricky part. 
What we are actually doing here is using `gl.stencilOp` to modify the stencil Buffer.

Generally speaking, for each backface fragment, we add one to the stencil Value. 
For each frontface fragment, we subtract one. Thus finally places where is clipped, i.e.
where the backfaces are exposing, where be labeled with a greater number.  

TODO 




![](/assets/blog-img/biodigital/human-clipping-cap-vs.png)
