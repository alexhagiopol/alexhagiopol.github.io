---
title: 'Segmentation For Holographic Images'
date: 2020-11-26
permalink: /posts/2020/11/segmentation-for-holographic-images/
tags:
  - computer vision
  - 3D reconstruction
  - deep learning
  - statistical learning
  - semantic segmentation
  - augmented reality
---

Patent I authored while working at Microsoft. Describes an image segmentation technique for producing photorealistic human holograms.

### Introduction:

My [2019 U.S. Patent, "Segmentation for Holographic Images"](https://patents.google.com/patent/US10902608B2)
describes a method of 2D image segmentation that - when used as a precursor to 3D reconstruction algorithms - greatly 
improves resulting hologram photorealism relative to previously established prior art such as [this landmark 2015 paper by Collet et al](https://dl.acm.org/doi/abs/10.1145/2766945). 
The invention enhances augmented reality applications such as using human avatars for communication or preservation purposes. 
This technology was used by Microsoft's [Mixed Reality Capture Studios](https://www.microsoft.com/en-us/mixed-reality/capture-studios).

The following image shows the improvement in human avatar quality achieved with the described approach:

![fig6](/content/patent_fig_6.png)


### Summary:

I summarize the patent here by reviewing the ideas illustrated in the figures:

Figure 1 describes the relationship between volumetric 3D reconstruction and the 2D images: a 3D volume 
can be calculated from many 2D silhouettes. Thus, to accurately reconstruct a 3D volume observed in 2D images, 
many accurate volume silhouettes must be first obtained in 2D.
![fig1](/content/patent_fig_1.png)

Because humans are extremely good at identifying other humans, producing human holograms with
even slight volumetric errors is immersion-breaking for a consumer of augmented reality content. Figure 2
communicates that previous techniques used to obtain silhouettes were not sufficiently accurate for volumetric
hologram applications at the time of this patent's writing. In the absence of extremely well-labeled and 
diverse training data, even neural networks did not produce silhouettes accurate enough for human hologram applications. 
Traditional segmentation techniques like background subtraction are also defeated when the RGB intensities of background
pixels are very similar to the RGB intensities of the target volume.
![fig2](/content/patent_fig_2.png)

Figure 3 describes the approach proposed in this patent: combining (1) foreground-background subtraction,
(2) neural network based semantic segmentation, and (3) statistical learning based postprocessing to produce 
highly refined silouettes for volumetric reconstruction.
![fig3](/content/patent_fig_3.png)

Figure 5 outlines the algorithm pipeline proposed in this patent in text form:
![fig5](/content/patent_fig_5.png)

Figure 6 shows the final payoff of the proposed approach in terms of hologram quality. The top image is an input to the hologram
reconstruction algorithm described [by Collet et al](https://dl.acm.org/doi/abs/10.1145/2766945). The lower left image 
is a view of the hologram that is produced with traditional silhouette calculation approaches. The lower right image
is a view of the hologram that is produced using the approach described in this patent. The improvement in reconstruction
quality is drastic.
![fig6](/content/patent_fig_6.png)



