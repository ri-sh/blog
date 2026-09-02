---
layout: post
title: "Google Image Grabber.py"
date: 2015-02-13 13:27:00
tags: ["Google Image", "Python"]
thumbnail: /assets/img/migrated/google-image-grabber-py/img0.png
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2015-02-13._

We all do download images and wallpapers. It's easy if you manually download a few images, but building a large dataset of more than 200 images that way is very difficult. To automate this, here's a simple script that downloads full-sized images from Google search.

Let's start with the quick 'n dirty way of collecting data: Google Search.

_A brief non sequitur_... Google Image search has gotten incredibly cool. The feature to filter results on image content is likely powered by some computer vision algorithms, though I don't know to what extent that's true. Check out this search for Vladimir Putin.

[![](/blog/assets/img/migrated/google-image-grabber-py/img0.png)](/blog/assets/img/migrated/google-image-grabber-py/img0.png)

Amusing Google Image search for "Vladimir Putin" + badass. **UPDATE:** _some details from [Google Research](http://research.google.com/ "google research homepage") on their blog [here](http://googleresearch.blogspot.com/2013/06/improving-photo-search-step-across.html "improving photo search - google research")._

#### Search API

So, I searched for "Best wallpapers" and "HD images" and got a sense for what's out there. Then I wrote a script to make image searching and downloading faster and repeatable.

The program grabs the images on the first page of the search results and downloads them. I ran the script 5-6 times for each category with a different search query each time, for simplicity and to avoid dealing with pagination.

```python
import json
import os
import time
import requests
from PIL import Image
from StringIO import StringIO
import socket
from requests.exceptions import ConnectionError
import urllib2

def go(query, path):
  """Download full size images from Google image search.

  Don't print or republish images without permission.
  I used this to train a learning algorithm.
  """
  BASE_URL = 'https://ajax.googleapis.com/ajax/services/search/images?'\
             'v=1.0&q=' + query + '&start=%d'
  header = {'User-Agent': 'Mozilla/5.0'}
  BASE_PATH = os.path.join(path, query)

  if not os.path.exists(BASE_PATH):
    os.makedirs(BASE_PATH)
  print BASE_PATH
  start = 0 # Google's start query string parameter for pagination.
  while start < 60: # Google will only return a max of 56 results.
    r = requests.get(BASE_URL % start)
    for image_info in json.loads(r.text)['responseData']['results']:
      url = image_info['unescapedUrl'].replace("%20",'')
      print url
      try:
        image_r = urllib2.urlopen(url).read()
      except ConnectionError, e:
        print 'could not download %s' % url
        continue
      except urllib2.HTTPError ,e:
        print "HHTp eror}"
        continue
      except  urllib2.URLError,e :
        print "socket error ..........."
        continue
      except UnboundLocalError , e:
        print "UnboundLocalError"
      except:
        import sys
    # prints `type(e), e` where `e` is the last exception
        print sys.exc_info()[:2]

      # Remove file-system path characters from name.
      title = image_info['titleNoFormatting'].replace('/', '').replace('\\', '').replace('.','').encode('utf-8').replace('|','').replace(':','').replace(',','').replace('?','')

      file = open(  BASE_PATH+"\\"+title+'.jpg', 'wb')
      try:
        file.write(image_r)

      #Image.open(StringIO(image_r.content)).save(file, 'JPEG')
      except IOError, e:
        # Throw away some gifs...blegh.
        print 'could not save %s' % url
        continue
      finally:
        file.close()

    print start
    start += 4 # 4 images per page.

    # Be nice to Google and they'll be nice back :)
    time.sleep(1.5)

# Example use
go('check', 'C:\\Pictures\\landscape')
```
