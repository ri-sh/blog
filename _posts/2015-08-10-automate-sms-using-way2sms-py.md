---
layout: post
title: "Automate SMS Using Way2SMS.py"
date: 2015-08-10 04:18:00
tags: ["python", "Python"]
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2015-08-10._

**Python Way2sms.py**

Way2sms offers SMS communication totally cost free to mobile users anywhere in India, at the same time, enables advertisers to reach out to millions of mobile users through the revolutionary concept of 'Mobitisement' (advertisement on mobile phone).

So my question is why can't we utilize this free SMS service to send sms to our friends or sms alerts to ourselves? Of course it is possible. Modern languages like perl and python offer powerful modules like mechanize for pragmatic browsing. (I'm a bit python addicted ;-) )

No need to explain this stuff I think, just go on, it's like a story. Just blind automation. Don't forget to put your way2sms username and password on the script.

```python
#-------------------------------------------------------------------------------
# Name:        Way2sms
# Purpose:   send sms way2sms #
# Author:      Rishabh Roy
#
# Created:     09/08/2015
# Copyright:   (c) Rishabh 2015
# Licence:     <GPL>
#-------------------------------------------------------------------------------
import urllib2
import cookielib
from getpass import getpass
import sys

# login to way2sms.com -- replace your username with your mobile number on way2sms and password here
username = ""
passwd = ''

def Way2sms(message, number):
    message = "+".join(message.split(' '))

    url = 'http://site21.way2sms.com/Login1.action'
    data = 'username=' + username + '&password=' + passwd

    cj = cookielib.CookieJar()
    opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cj))
    opener.addheaders = [('User-Agent', "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/43.0.2357.134 Safari/537.36")]

    try:
        usock = opener.open(url, data)
    except IOError:
        print "cannot connect "
        sys.exit(1)

    jession_id = str(cj).split('~')[1].split(' ')[0]
    print jession_id

    p = opener.open("http://site21.way2sms.com/ebrdg.action?id=" + jession_id)
    send_sms_url = 'http://site21.way2sms.com/smstoss.action'
    send_sms_data = 'ssaction=ss&Token=' + jession_id + '&mobile=' + number + '&message=' + message + '&msgLen=' + str(140 - len(message))
    opener.addheaders = [('Referer', 'http://site21.way2sms.com/sendSms?Token=' + jession_id)]

    try:
        sms_sent_page = opener.open(send_sms_url, send_sms_data)
    except IOError as e:
        print e

    p = opener.open('http://site21.way2sms.com/smscofirm.action?SentMessage=' + message + '&Token=' + jession_id + '&status=0')
```

EDIT: way2sms have modified their website with random IDs in form fields and all. Don't know why, may be to prevent automation. :-D
