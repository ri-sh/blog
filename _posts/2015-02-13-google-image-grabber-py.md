---
layout: post
title: "Google  Image  Grabber.py"
date: 2015-02-13 13:27:00
tags: ["Google Image", "Python"]
thumbnail: /assets/img/migrated/google-image-grabber-py/img0.png
---

_Originally posted on my old blog on 2015-02-13._

We all do download images and wallpapers .It is easy if you manually download few images but while building a large data set of images containing more than 200 images is very difficult . In order to automate this task of downloading the images using a simple script which downloads the full sized images from Google search is the shown in this blog post . 

  


Let's start with the quick 'n dirty for collecting data: Google Search.  


_A brief non sequitur_...Google Image search has gotten incredibly cool. The feature to filter results on image content is likely powered by some computer vision algorithms, though I don't know to what extent that's true. Check out this search for Vladimir Putin.  


[![](/assets/img/migrated/google-image-grabber-py/img0.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgUqsXVzBm26FXTTX2pjVIt-KSGcc0-0oVtYoDnVURNVa810IQh6dJhs1uos5GQTKcw9GADP1uxBhvNWlv18QUrW-7JHTcrGlQLYlWjJuFgUNuoLWvp6HXO1ZYN0yHNC_mA9XAQW_fjyek/s1600/google_img_putin_badass.png)

  
Amusing Google Image search for "vladimir Putin" + badass **UPDATE:** _some details from[Google Research](http://research.google.com/ "google research homepage") on their blog [here](http://googleresearch.blogspot.com/2013/06/improving-photo-search-step-across.html "improving photo search - google research")._

####  Search API

  
  
  
So, I searched for "Best wallPapers" and "Hd Images" and got a sense for what's out there. Then I wrote a script to make image searching & downloading the image faster and repeatable.  
  
The program grabs the images on the first page of the search, and downloads them. I ran the script 5-6 times for each category with a different search query each time for simplicity and to avoid dealing with pagination.
