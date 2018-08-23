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

<p>&nbsp;</p>
<p>&nbsp;</p>
<div id="page_title" align="center">
<h1>Volumetric Lighting</h1>
</div>
<div id="page_author" align="center">Chris Varanese</div>
<p>&nbsp;</p>
<h2>Project Description</h2>
<p>This project sought first to create shadows using a shadowmap. After that, volumetric lighting, i.e. "God Rays," were created using a similar technology. This lighting technique results in visible light rays, as if the air were cloudy or foggy. As volumetric lighting was the key feature of this project, the shadows and overall scene are not ideal, meaning the shadows are unoptomized, shown by the low resolution edges and a low amount of shadow acne.</p>
<h2>New Features Implemented</h2>
<p>Of course, the new technologies featured in the project are:</p>
<ul>
<li>Shadow Maps
<ul>
<li>Depth Buffer</li>
</ul>
</li>
<li>Volumetric Lighting
<ul>
<li>Ray Tracing</li>
<li>Post Processing
<ul>
<li>Blur</li>
</ul>
</li>
<li>Light Depth Buffer</li>
<li>Scene Depth Buffer</li>
<li>Combined read from two textures</li>
</ul>
</li>
</ul>
<h2>How it works</h2>
<p>The general process was taken from a <a href="http://developer.download.nvidia.com/SDK/10.5/direct3d/Source/VolumeLight/doc/VolumeLight.pdf">Nvidia paper on the subject</a>. First, a shadow map is necessary. Then, a similar depth buffer from the perspective of the camera is needed. Now, by using a new quad rendered to the screen, depths are read in from the scene depth buffer and converted to world space and then to light space. A ray is traced from the light space position of the initial quad to the ending position that was read from the depth buffer. We sample this ray X times, depending on how high of a quality we want there to be, and then each time that the sample is in view of the light, we add to a value. Thus, we get a count for how bright each pixel on the quad should be. A slight optimization on this process is to then blur this texture of the light values so that the edges are less harsh and more natural. Lastly, we take this quad and another prerendered quad of just a view of the scene and we add the pixel values together.</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2>Results</h2>
<p>This project ended incredibly well!</p>
<p><img style="display: block; margin-left: auto; margin-right: auto;" src="https://thumbs.gfycat.com/ForsakenGlisteningHogget-max-1mb.gif" alt="Rotating Dragon" width="300" height="300" /></p>
<p>This is a dragon rotating in the scene so that we can really look at how it ended up. Here is some more of the scene:</p>
<table style="width: 724.1px; margin-left: auto; margin-right: auto;" border="0">
<tbody>
<tr style="height: 119.2px;">
<td style="width: 174px; height: 119.2px;">
<p><img style="display: block; margin-left: auto; margin-right: auto;" src="https://i.imgur.com/xo22tR5.png" alt="" width="209" height="212" /></p>
</td>
<td style="width: 157.1px; height: 119.2px;"><img style="display: block; margin-left: auto; margin-right: auto;" src="https://i.imgur.com/sJgKm8u.png" alt="" width="213" height="216" /></td>
</tr>
<tr style="height: 25px;">
<td style="width: 174px; height: 25px;"><img style="display: block; margin-left: auto; margin-right: auto;" src="https://i.imgur.com/TejZdEp.png" alt="" width="205" height="203" /></td>
<td style="width: 157.1px; height: 25px;"><img style="display: block; margin-left: auto; margin-right: auto;" src="https://i.imgur.com/U12Vx7L.png" alt="" width="213" height="213" /></td>
</tr>
</tbody>
</table>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>
<p>&nbsp;The only problem is that, when the sampling isn't set that high, there are these weird circles, similar to shadow acne. I am not quite sure how to fix this.</p>
<p>&nbsp;</p>
<p><a href="https://www.youtube.com/watch?v=FMcJD0R-rYQ">Video of the scene.</a></p>
<p>&nbsp;</p>
<h2>Sources</h2>

<p><a href="http://developer.download.nvidia.com/SDK/10.5/direct3d/Source/VolumeLight/doc/VolumeLight.pdf">http://developer.download.nvidia.com/SDK/10.5/direct3d/Source/VolumeLight/doc/VolumeLight.pdf</a></p>
