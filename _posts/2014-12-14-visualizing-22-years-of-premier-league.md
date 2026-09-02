---
layout: post
title: "Visualizing 22 years of Premier League data (1992-2013)."
date: 2014-12-14 18:17:00
tags: ["visualization", "BeautifulSoup", "DataAnalsis", "DataMining", "Plotly", "scraping"]
thumbnail: /assets/img/migrated/visualizing-22-years-of-premier-league/img0.PNG
---

_Originally posted on my old blog on 2014-12-14._

##  (Python)Visualizing 22 years of Premier League data scraped from Wikipedia.  
  
[![](/assets/img/migrated/visualizing-22-years-of-premier-league/img0.PNG)](/assets/img/migrated/visualizing-22-years-of-premier-league/img0.PNG)

  


This post work focuses on visualizing the data of Major Premier League clubs 12 clubs which are represented above by colored lines. I Scraped the Data from Wikipedia because i was unable to find raw and reliable data containing match data of 22 years at a place. Also, it is difficult to do get in match details like the Top goal Scorer and top goals scored data from different sources.  


Inspired by the Anna Powell-Smith's work <http://www.thestoryoftheseason.com/>

  


work on timeline of Premier Series.

I came up with idea of drawing this graph using d3.js being a noob at d3js i am was comfortable in using plotly 

I used plotly which has a python library to plot the graph on Web.

  


I wrote this script to scrape data from Wikipedia Tables

  


The script I wrote to scrape the data from Wikipedia can be found  [here](https://gist.github.com/rishabhsixfeet/428d390e1e3e8c444ec3 "Source Code for WikiData Scraping")
    
    
    //Your code here
    //Be sure you escape your tags with < and > when displaying things like HTML.
    

  
  


I am planning to write a detailed blog post on the entire 'scraping' part of the project in near future.

Script is completely in python I am using Beautiful Soup to scrape the Wikipedia pages .

this [Github](https://github.com/rishabhsixfeet/wikiScraper) rep contains all the files i used to create the plots .

  


  


the following plot shows the cumulative matches won per year by the teams

it shows the scoring trajectory of the teams .

the graph below shows that Manchester United has a constant growth curve where as Manchester City has steep growing curve
