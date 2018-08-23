---
layout: post
title:  "Volumetric Lighting"
date:   2018-03-22
excerpt: "OpenGL implementation of Volumetric Lighting"
project: true
tag:
- volumetric
- lighting
- openGL
- graphics
- c++
- nvidia
- csc471
comments: true
---
## Project Description

This project sought first to create shadows using a shadowmap. After that, volumetric lighting, i.e. "God Rays," were created using a similar technology. This lighting technique results in visible light rays, as if the air were cloudy or foggy. As volumetric lighting was the key feature of this project, the shadows and overall scene are not ideal, meaning the shadows are unoptomized, shown by the low resolution edges and a low amount of shadow acne.

## New Features Implemented
Of course, the new technologies featured in the project are:
* Shadow Maps
* Depth Buffer
* Volumetric Lighting
* Ray Tracing
* Post Processing
* Blur
* Light Depth Buffer
* Scene Depth Buffer
* Combined read from two textures

## How it works

The general process was taken from a [Nvidia paper on the subject](http://developer.download.nvidia.com/SDK/10.5/direct3d/Source/VolumeLight/doc/VolumeLight.pdf). First, a shadow map is necessary. Then, a similar depth buffer from the perspective of the camera is needed. Now, by using a new quad rendered to the screen, depths are read in from the scene depth buffer and converted to world space and then to light space. A ray is traced from the light space position of the initial quad to the ending position that was read from the depth buffer. We sample this ray X times, depending on how high of a quality we want there to be, and then each time that the sample is in view of the light, we add to a value. Thus, we get a count for how bright each pixel on the quad should be. A slight optimization on this process is to then blur this texture of the light values so that the edges are less harsh and more natural. Lastly, we take this quad and another prerendered quad of just a view of the scene and we add the pixel values together.

## Results
This project ended incredibly well!
{% capture images %}
	https://thumbs.gfycat.com/ForsakenGlisteningHogget-max-1mb.gif
{% endcapture %}
{% include gallery images=images cols=1 %}

This is a dragon rotating in the scene so that we can really look at how it ended up. Here is some more of the scene:

{% capture images %}
	https://i.imgur.com/xo22tR5.png
	https://i.imgur.com/sJgKm8u.png
	https://i.imgur.com/TejZdEp.png
    https://i.imgur.com/U12Vx7L.png
{% endcapture %}
{% include gallery images=images caption="Images of the scene" cols=2 %}

The only problem is that, when the sampling isn't set that high, there are these weird circles, similar to shadow acne. I am not quite sure how to fix this.

[Video of the scene.](https://www.youtube.com/watch?v=FMcJD0R-rYQ)

## Sources

* http://developer.download.nvidia.com/SDK/10.5/direct3d/Source/VolumeLight/doc/VolumeLight.pdf
