---
layout: post
title: "Gesture  Recognition in python"
date: 2014-06-02 19:23:00
thumbnail: /assets/img/migrated/gesture-recognition-in-python/img0.png
---

_Originally posted on my old blog on 2014-06-02._

Below is a flowchart representation of the program  
![](/assets/img/migrated/gesture-recognition-in-python/img0.png)  
The hand tracking is based on color recognition. The program is therefore initialized by sampling color from the hand:  
  
The hand is then extracted from the background by using a threshold using the sampled color profile. Each color in the profile produces a binary image which in turn are all summed together. A nonlinear median filter is then applied to get a smooth and noise free binary representation of the hand.   
![](/assets/img/migrated/gesture-recognition-in-python/img1.png)  
When the binary representation is generated the hand is processed in the following way:  
![](/assets/img/migrated/gesture-recognition-in-python/img2.png)  
The properties determining whether a convexity defect is to be dismissed is the angle between the lines going from the defect to the neighbouring convex polygon vertices  
![](/assets/img/migrated/gesture-recognition-in-python/img3.png)
