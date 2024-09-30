---
title: 'Visual Odometry on Mars'
date: 2016-02-19
permalink: /posts/2016/02/visual-odometry/
tags:
  - visual odometry
  - mars exploration
---

Creating a visual localization system for robot localization on Mars.

### Description:

Autonomy is essential for flying on Mars because it's impossible to remotely operate a flying vehicle due to the communications delay between planets. At a high level, the vehicle must calculate its change in pose over time at a high framerate ("odometry"). Over the summer I integrated a state-of-the-art visual odometry technique into NASA's Mars exploration UAV prototype. This technique, [Semi-Direct Visual Odometry](https://ieeexplore.ieee.org/iel7/6895053/6906581/06906584.pdf?casa_token=zgsATVirdE8AAAAA:IsjHRdt1nxnLhCYR5u3_umdRHMZf8eVvuLDP29YR-cNX4xM_Z87FwOfM2BBStD2lOm6fVaVT6gJk){:target="_blank"} by Forster et al, allows for the vehicle to be localized with centimeter accuracy using only information from a camera facing the surface of the planet. Solving the localization problem paves the way for autonomous navigation and exploration. In the video above, I briefly overview the technologies I integrated to achieve camera-only localization in a simulated Mars environment. In [this video](http://www.youtube.com/watch?v=Pl9OGwPpl3k){:target="_blank"}, I explain the project in detail during a technical talk at NASA. This [PDF presentation](/content/Visual_Odometry_Talk.pdf){:target="_blank"} contains the latest version of the slides.

### Teaser Image:

![](/content/mars_flyer.jpg)
