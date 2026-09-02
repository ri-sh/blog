---
layout: post
title: "Snakuscules"
date: 2015-01-16 20:37:00
tags: ["matlab", "image", "Algorithm", "computer vision"]
thumbnail: /assets/img/migrated/snakuscules/img0.png
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2015-01-16._

###  Snakuscules 

Recently got familiar with a methodology for contour segmentation used in biomedical image processing. Researchers at the 'Biomedical Imaging Group' of École polytechnique fédérale de Lausanne (EPFL), worked on segmenting approximate circular regions in images using the concepts of snakes, known as Snakuscule.  
  
A snakuscule is a simple active counter that preys upon bright blobs in an image. Here is my implementation:  


  
[![](/assets/img/migrated/snakuscules/img0.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj_AIrGUvH7fS9B9mO7Y5kIEqTiKjP1AmBORronnwgBwE9W_H5xj2tKC410g8b5qGBHi_BS4i0Slqyz7G27y-najC_a8gAX5_MmzFTlzrxroR82c1I8UMVAl6_agGdwG3y1L25ro8PmlZs/s1600/snake_pic.png)  
---  
Snakuscule enveloping a bright blob in an image  
Such active contours move under the influence of energy gradients. For every snakuscule, the energy difference between the outer adjoining annulus and the inner disk is calculated. Such active contours can be programmed to prey upon bright blobs in an image. Hence, they move in the direction of decreasing energy difference.  
  


[![](/assets/img/migrated/snakuscules/img1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjLv5bR1GDgPkx_oP60n8VyL3V-tK5PDwr5X2TUXfy-THIJUhxb5qA6JZ_428Lp00fFNnj3wegSi8Ev-atgJ2epc-xvfsSxKo1p1J1lENCnf-fQPMDc03jfdMkvcu18VmrXzK6Fym5PFS0/s1600/snakuscule_template.png)

  
Energy to be minimized can be given by the equation:  
  


[![](/assets/img/migrated/snakuscules/img2.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh-DizO7MzIQpExJCHgoD-TV7JCHz7KCkcsQbIzUH6oKaUWX76raqaneBo_BXLAy2k6xxV98HASZvXNfiQZelSkzcSC0NhIJBErKwIjdvGUYhtJVoxEEjQPefG3dA7PuMrSHpNlFCMprSE/s1600/snakuscule_eq.PNG)

To minimize energy, and to detect brighter blobs, the snakuscule can move in any of the four directions, or vary its radius. Out of these six possible actions, snakuscule selects the one that maximizes the decrease in energy.  
  
We normalize the energy function by dividing the energy of outer annulus and inner disk by the area constituting them respectively.  


Normalized form:

[![](/assets/img/migrated/snakuscules/img3.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhaj-6Z31jpcdS4fC-s8wujNzbbFsvh9lfZXyoO9EB5mfFgus0wKFy1y38ayCTJE9AbwypnjWQUo2_EsUFrevsnm8eOITrKqdan6BoEzoqvhUT5Sh5uR0SgxLSjXwBqmQKhewBJQwzoPAc/s1600/snakuscule_eq_norm.PNG)

A more rigorous implementation can be seen in the [original paper](http://bigwww.epfl.ch/preprints/thevenaz0701p.pdf) on snakuscules by Philippe Thévenaz and Michael Unser. 

  


Matlab codes for the same can be found at [https://github.com/ri-sh/Snakuscule](https://github.com/ri-sh/Snakuscule)
