---
layout: post
title: "Snakuscules"
date: 2015-01-16 12:37:00
tags: ["Algorithm", "computer vision", "image", "matlab"]
---

_Originally posted on my old blog on 2015-01-16._

###  Snakuscules 

Recently got familiar with a methodology for contour segmentation used in biomedical image processing. Researchers at the 'Biomedical Imaging Group' of École polytechnique fédérale de Lausanne (EPFL), worked on segmenting approximate circular regions in images using the concepts of snakes, known as Snakuscule.  
  
A snakuscule is a simple active counter that preys upon bright blobs in an image. Here is my implementation:  


  
[![](/assets/img/migrated/snakuscules/img0.png)](/assets/img/migrated/snakuscules/img0.png)  
---  
Snakuscule enveloping a bright blob in an image  
Such active contours move under the influence of energy gradients. For every snakuscule, the energy difference between the outer adjoining annulus and the inner disk is calculated. Such active contours can be programmed to prey upon bright blobs in an image. Hence, they move in the direction of decreasing energy difference.  
  


[![](/assets/img/migrated/snakuscules/img1.png)](/assets/img/migrated/snakuscules/img1.png)

  
Energy to be minimized can be given by the equation:  
  


[![](/assets/img/migrated/snakuscules/img2.PNG)](/assets/img/migrated/snakuscules/img2.PNG)

To minimize energy, and to detect brighter blobs, the snakuscule can move in any of the four directions, or vary its radius. Out of these six possible actions, snakuscule selects the one that maximizes the decrease in energy.  
  
We normalize the energy function by dividing the energy of outer annulus and inner disk by the area constituting them respectively.  


Normalized form:

[![](/assets/img/migrated/snakuscules/img3.PNG)](/assets/img/migrated/snakuscules/img3.PNG)

A more rigorous implementation can be seen in the [original paper](http://bigwww.epfl.ch/preprints/thevenaz0701p.pdf) on snakuscules by Philippe Thévenaz and Michael Unser. 

  


Matlab codes for the same can be found at [https://github.com/ri-sh/Snakuscule](https://github.com/rishabhsixfeet/Snakuscule)
