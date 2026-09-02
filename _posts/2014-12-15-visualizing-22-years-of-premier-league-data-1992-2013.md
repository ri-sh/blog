---
layout: post
title: "Visualizing 22 Years of Premier League Data (1992-2013)"
date: 2014-12-15 02:17:00
tags: ["DataAnalsis", "scraping", "Plotly", "BeautifulSoup", "visualization", "DataMining"]
thumbnail: /assets/img/migrated/visualizing-22-years-of-premier-league-data-1992-2013/img0.PNG
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2014-12-15._

## (Python) Visualizing 22 years of Premier League data scraped from Wikipedia

[![](/assets/img/migrated/visualizing-22-years-of-premier-league-data-1992-2013/img0.PNG)](/assets/img/migrated/visualizing-22-years-of-premier-league-data-1992-2013/img0.PNG)

This post focuses on visualizing the data of the major Premier League clubs -- 12 clubs, represented above by colored lines. I scraped the data from Wikipedia because I was unable to find raw and reliable data containing match data of 22 years in one place. It's also difficult to get match details like the top goal scorer and goals scored from different sources.

Inspired by Anna Powell-Smith's work at [thestoryoftheseason.com](http://www.thestoryoftheseason.com/) and her timeline-of-Premier-League-seasons visualization, I came up with the idea of drawing this graph. Being a noob at d3.js, I was more comfortable using Plotly, which has a Python library for plotting graphs on the web.

I wrote this script to scrape the data from Wikipedia's tables:

```python
from bs4 import BeautifulSoup
import numpy as np
import pandas as pd
import urllib2

soup = ''

def getPremLeague(wiki):
    """
    input takes the wiki url of football club season list url in the
    form <"http://en.wikipedia.org/wiki/List_of_"+" Football team name"+"_F.C._seasons">
    returns a dictionary containing
    {
        'Years': a numpy array of the years of the Premier League match starting from 1992 to 2013
        'Wins':  a numpy array of year-wise number of matches won
        'Draw':  a numpy array of year-wise number of matches drawn
        'Lose':  a numpy array of year-wise number of matches lost
        'Position': a list containing the rank of the team in that year
        'Topscorer': a list containing the top goal scorer
        'Topgoals':  a list containing the goals scored by the top scorer
    }
    """
    header = {'User-Agent': 'Mozilla/5.0'}  # needed to prevent 403 error on Wikipedia
    req = urllib2.Request(wiki, headers=header)
    page = urllib2.urlopen(req)
    soup = BeautifulSoup(page)
    tabletype = ""
    table = soup.find_all("table", {"class": "wikitable sortable"})
    if (len(table) == 0):
        table = soup.find_all("table", {"class": "wikitable plainrowheaders"})

    if (len(table) == 0):
        table = soup.find_all("table", {"class": "wikitable plainrowheaders sortable"})

    if (len(table) == 0):
        table = soup.find_all("table", {"class": "wikitable"})
        tabletype = "wikitable"

    print len(table)

    won = []
    draw = []
    lose = []
    years = []
    position = []
    TopScorer = []
    Topgoal = []

    for tables in table:
        for row in tables.findAll("tr"):
            cells = row.findAll("td")

            if len(cells) > 4:
                if (len(cells[2].text) <= 2 and len(cells[0].text.encode('ascii', 'ignore')) > 1):
                    leag = cells[0].text.encode('ascii', 'xmlcharrefreplace')
                    if ("Prem" in leag or ("PL" in leag)):
                        yrs = BeautifulSoup(str(row.findAll("th")))
                        if (yrs.text[1:5] != '2014'):
                            years.append(int(yrs.text[1:5]))
                            won.append(int((cells[2].text[:2])))
                            draw.append(int(cells[3].text[:2]))
                            lose.append(int(cells[3].text[:2]))
                            position.append(str(cells[8].text[-3:]))
                            # getting the list of top scorers in that year
                            top = cells[len(cells) - 2].findAll("span")
                            topscorer = ""
                            if (len(top) >= 1):
                                scorer = top[0].text.split(",")
                                topscorer = scorer[1] + ' ' + scorer[0]  # splitting and joining
                            else:
                                topscorer = cells[len(cells) - 2].text

                            TopScorer.append(topscorer.strip())
                            Topgoal.append(int(cells[len(cells) - 1].text[:2]))
                    elif ("Premier" in cells[1].text.encode('ascii', 'xmlcharrefreplace') and tabletype == "wikitable"):
                        # for some teams on Wikipedia the "Premier" name is on cells[1] instead
                        yrs = cells[0].text.encode('ascii', 'xmlcharrefreplace')

                        if (yrs[1:5] != '2014'):
                            years.append(int(yrs[:4]))
                            won.append(int((cells[4].text[:2])))
                            draw.append(int(cells[5].text[:2]))
                            lose.append(int(cells[6].text[:2]))
                            position.append(str(cells[9].text[-3:]))
                            topscorer = cells[len(cells) - 2].text
                            TopScorer.append(topscorer.strip())
                            Topgoal.append(int(cells[len(cells) - 1].text[:2]))

    Team = {
        'Years': np.array(years),
        'Wins': np.array(won),
        'Draws': np.array(draw),
        'Lose': np.array(lose),
        'Position': position,
        'Topscorer': TopScorer,
        'Topgoal': np.array(Topgoal),
    }
    return Team
```

I'm planning to write a more detailed post on the scraping part of the project at some point. The script is entirely in Python, using Beautiful Soup to scrape the Wikipedia pages. This [GitHub repo](https://github.com/rishabhsixfeet/wikiScraper) contains all the files I used to create the plots.

The following plot shows the cumulative matches won per year by each team, i.e. the scoring trajectory of each team -- it shows that Manchester United has a constant growth curve, whereas Manchester City has a steep growing curve.

_(The interactive Plotly charts that were embedded here are gone -- Plotly's old chart-hosting URLs [\~rishabh_roy/0.embed](https://plot.ly) and [\~rishabh_roy/1.embed](https://plot.ly) have been permanently deleted (HTTP 410) and there's no archived copy of the rendered charts. The scraper code above still works the same way, so the underlying data and analysis are intact -- just not the original interactive graphs.)_
