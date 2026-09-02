---
layout: post
title: "rewriting Hough Line Detection algorithm In Opencv"
date: 2016-09-02 18:19:00
thumbnail: /assets/img/migrated/rewriting-hough-line-detection/img0.jpg
---

_Originally posted on my old blog on 2016-09-02._

Hough Transform is a popular technique to detect any shape, if you can represent that shape in mathematical form. It can detect the shape even if it is broken or distorted a little bit.  
  
  
  
  
  
The linear Hough transform algorithm uses a two-dimensional array, called an accumulator, to detect the existence of a line described by   
![r=x\\cos \\theta +y\\sin \\theta ](/assets/img/migrated/rewriting-hough-line-detection/img0.jpg).  
The dimension of the accumulator equals the number of unknown parameters, i.e., two, considering quantized values of r and θ in the pair (r,θ).   
For each pixel at _(x,y)_ and its neighborhood, the Hough transform algorithm determines if there is enough evidence of a straight line at that pixel. If so, it will calculate the parameters (r,θ) of that line, and then look for the accumulator's bin that the parameters fall into, and increment the value of that bin. By finding the bins with the highest values, typically by looking for local maxima in the accumulator space, the most likely lines can be extracted  
  
  
The algorithm for detecting straight lines can be divided into the following steps:  
  
Basic Hough Transform for lines Algo --  
  
Initialize h[d,theta]=0  
for each edge pixel in E(x,y) in Image :  
for theta = 0 to 180  
h[d,theta]+=1  
  
find values of (d,theta) where H(d,theta ) is maximum  
  
  
Performing Hough transform on shapes.png  
  
[![](/assets/img/migrated/rewriting-hough-line-detection/img1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQ3akP3JvJdt8SWQntDyTuiZh9WVerHffq3SnqpwzjXruQcSbaD-bfCONtAf_9XO2O3qTPbN9U53PkIpq2yBSdDW92eoQsh-wpx5FV1Yml5SYE6XAEaSdPWm8bYKdmVaPpUMCHToDRo58/s1600/shapes.png)  
---  
shapes.png  
  
Final result  
  
[![](/assets/img/migrated/rewriting-hough-line-detection/img2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiI5lHFBLdHYQ3bCNa8_LLBwy7R_ruc9SItu6MycjBqb0qPIHHnPf5q-WTY-ojwiaV3AtJTRo8-hZ4X2UJ4k2x3fSTeP2PlZHgsTJx65Ztf7gif9TPomwEm5gOLpPdImA5LpascLXqm9ac/s1600/houghlines.png)  
---  
Houghlines |   
|   
|   
|   
|   
|   
|   
|   
|   
|   
|   
|   
|   
|   
|   
|   
|   
|
