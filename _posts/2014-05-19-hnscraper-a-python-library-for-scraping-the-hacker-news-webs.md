---
layout: post
title: "HNScraper - A Python library for scraping the Hacker News website"
date: 2014-05-19 19:48:00
tags: ["scraping", "Python", "hackernews"]
---

_Originally posted on my old blog on 2014-05-19._

I'm a big fan of the Hacker News website, and over the last nine months or so, I've upvoted quite a few stories. I wanted to include links to these on my blog. In this age of RSS, I assumed this would be simple, but it was not to be. HN does not make saved stories available by RSS, and doesn't support adding RSS feeds into sidebars. So I started building a homebrew solution.  
  
The result is that i wrote a hnscraper library, and the posterouscontrib program here.  
<https://github.com/rishabhsixfeet/ScraperHN>  
  
The hnscraper library allows a developer to connect to the HN website, and scrape stories, currently from the front page or from a user's saved stories page.  
The posterouscontrib program builds a static html file, suitable for inclusion in a Posterous sidebar by adding an iframe to a custom template.  
This gives an example of an interactive session using the HNScraper object. The object acts as a wrapper around a mechanize brower. It is stateful - the usage pattern is to log in, navigate where you want and then start pulling stories:  

    
    
    >>> import hnscraper
    >>> h=hnscraper.HNScraper()
    >>> h.login_with_google('USERNAMEGOESHERE','PASSWORDGOESHERE')
    >>> h.nav_saved_stories()
    >>> stories=h.get_current_page_stories()
    >>> list(stories)[0]
    {'score': 254, 'link': u'http://adgrok.com/why-founding-a-three-person-startup-with-zero-revenue-is-better-than-working-for-goldman-sachs', 'user': u'cjg', 'title': u'Founding a startup with zero revenue is better than working for Goldman Sachs', 'ord': 1, 'comhead': u' (adgrok.com) '}
    >>> h.nav_more()
    >>> stories=h.get_current_page_stories()
    >>> list(stories)[0]
    {'score': 61, 'link': u'http://www.aaronstannard.com/post/2010/06/12/The-Myth-of-the-Single-Person-Startup.aspx', 'user': u'Aaronontheweb', 'title': u'The Myth of the Single-Person Startup', 'ord': 31, 'comhead': u' (aaronstannard.com) '}
    

The posterouscontrib program is non-interactive. To use it, make sure you have mako installed, then copy the provided hn-saved.conf.example to hn-saved.conf and edit as appropriate - full path names are recommended. The output file should be made available via a publicly accessible webserver. Once you are happy that the program is operating correctly, it can be included in a crontab for regular execution.   
The commented snippet at the top of the template/output file can be included in your template, with an updated url, to make your Hacker News saved stories available through your sidebar.
