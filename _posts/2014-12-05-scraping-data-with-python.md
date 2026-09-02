---
layout: post
title: "Scraping Data with Python"
date: 2014-12-05 00:30:00
tags: ["python", "scraping"]
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2014-12-05._

In a perfect world, all the data you needed would be easily accessible online. We're not quite there yet. In the past couple months I've had to write several scrapers to acquire large datasets and avoid a lot of tedious point/clicking or copy/pasting. (I also scraped some NFL player data to help with my fantasy football picks next year - same concept.)  
  
"Scraping" data basically means to retrieve data from the web, stored in a less convenient format like HTML tables, and copy it into a format you can use such as a CSV file or database. It can be somewhat tedious, but it usually beats the alternative of trying to copy data by hand. Python has some excellent tools for scraping which I will cover here.  
  
If you're scraping data from HTML pages, you're going to need some basic knowledge of HTML, and you'll need to check out the structure of the page you're scraping (right click > View Page Source) to figure out how to get to the content you need. Once you have an idea, the following tools will be useful to parse out what you need.  
  
  
**PyQuery**  
**  
**If you're not familiar with jQuery, it's a JavaScript library that provides easy access to elements within an HTML page.[PyQuery](http://pypi.python.org/pypi/pyquery) ports the same concept to Python, allowing you to use jQuery syntax to find specific elements from an HTML string. Here's an example that finds all links with "special" class attribute in a string of HTML:  
  

    
    
    >>> from pyquery import PyQuery
    >>> p = PyQuery("<a href='abc.html'>Link 1</a> <a href='def.html' class='special'>Link 2</a>")
    >>> p("a.special")
    [<a.special>]
    

  
  
**Basic Automated Browsing**  
**  
**Python has several great libraries for automatically browsing web sites. A good one to start with is[spynner](http://pypi.python.org/pypi/spynner). Spynner builds onto the [mechanize](http://pypi.python.org/pypi/mechanize) library, which in turn is built on the [urllib2](http://docs.python.org/library/urllib2.html) URL-opening library: urllib2 generates HTTP request and response objects, mechanize has a Browser object capable of navigating these requests and responses, and spynner adds some additional features including automatic form-filling and JavaScript support. Instead of working only with HTML, spynner can process jQuery and JavaScript to render pages as your usual browser would. These features and having to render the page do result in a slight performance hit, so if you don't need such an advanced tool, you can drop down to mechanize, which also has a Browser class with a similar interface.  
  
Spynner and PyQuery can be used together. This example loads a page, then uses PyQuery to get a list of all <a> tags within <h3> tags:  
  

    
    
    >>> from spynner import Browser
    >>> from pyquery import PyQuery
    >>> browser = Browser()
    >>> browser.set_html_parser(PyQuery)
    >>> browser.load("https://www.google.com/search?hl=en&q=france")
    True
    >>> browser.soup("h3 a")
    [<a>, <a>, <a>, <a>, <a>, <a>, <a>, <a>, <a>, <a>, <a>, <a>, <a>]
    

  
  
**Example**  
**  
**In this example, we'll build a simple scraper to get a dictionary of state population sizes from the Wikipedia page<http://en.wikipedia.org/wiki/List_of_U.S._states>.  
  
The first step is to take a look at the page's HTML. Notice that the data we want is in a table. There are several tables, but only one with class "wikitable" (you can use PyQuery to confirm this - PyQuery(html)('table.wikitable') should return only a single item.) The state names are in the first column, and the population sizes in the seventh. Note that the state names are also within a <a> link tag and population numbers have commas in them.  
  
After our initial research, the scraper we write will look like this:  
  
  

    
    
    from spynner import Browser
    from pyquery import PyQuery
    
    browser = Browser()
    browser.set_html_parser(PyQuery)
    
    browser.load("http://en.wikipedia.org/wiki/List_of_U.S._states")
    
    # get the table of states
    table = browser.soup("table.wikitable")
    # skip the first row, which contains only column names
    rows = table("tr")[1:]
    
    pop_dict = {}
    for row in rows:
        columns = row.findall("td")
        state_name = columns[0].find('a').text
        population = int(columns[6].text.replace(',', ''))
        pop_dict[state_name] = population
        
    print pop_dict
    

  
  
**  
****Parting Advice**  
**  
**If you're scraping, you should've already exhausted more reasonable channels such as searching the page for more conveniently-formatted data or contacting the people in charge. If so, by definition, you're acquiring data that was not made available for easy download - and there may have been good reason. It's always smart to make sure that what you're doing is legal and that there are no strings attached with use of the data. (My NFL data can only be used for noncommercial purposes, for example.) Just because you can get it doesn't mean it's yours to freely use.  
  
Since scraping can involve a lot of trial and error, it's usually best to save a page of sample data to your own machine and "practice" on that until you're sure the scraper will work. You don't want to be making a lot of needless HTTP requests - this could alert the data owner to what you're doing and cause unnecessary trouble for you, and could even result in your IP address being blocked or the data being taken down. Make sure your scraper behaves reasonably by limiting how often requests are made and how much is downloaded in a given period. This will help not to draw attention and prevent wasting someone else's resources.
