---
layout: post
title: "Top 10 Python Idioms I Wish I'd Learned Earlier"
date: 2015-05-05 13:46:00
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2015-05-05._

#  TOP 10 PYTHON IDIOMS I WISH I'D LEARNED EARLIER 

I've been programming all my life, but never been _a_ programmer. Most of my work was done in Visual Basic because it's what I was most comfortable with, plus a smattering of other languages (R, C, JavaScript, etc.). A couple of years ago, I decided to use Python exclusively so that I could improve my coding. I ended up re-inventing many wheels -- which I didn't mind too much, because I enjoy solving puzzles. But sometimes it's good to have a more efficient, Pythonesque approach, and time after time I found myself having "aha!" moments, realizing I'd been doing things the hard, excessively verbose way for no reason. Here is a list of ten Python idioms that would have made my life much easier if I'd thought to search for them early on.  
Missing from this list are some idioms such as list comprehensions and lambda functions, which are very Pythonesque and very efficient and very cool, but also very difficult to miss because they're mentioned on StackOverflow every other answer! Also ternary x if y else z constructions, decorators and generators, because I don't use them very often.

In [1]:
    
    
    ########################################
    

##  1\. Python 3-style printing in Python 2

One of the things that kept me from concentrating on Python was this whole version 2 - version 3 debacle. Finally I went with Python 2 because all the libraries I wanted were not 3-compatible, and I figured if I needed to, I would laboriously adjust my code later. But really, the biggest differences in everyday programming are printing and division, and now I just import from future.

In [2]:
    
    
    mynumber = 5
    
    print "Python 2:"
    print "The number is %d" % (mynumber)
    print mynumber / 2,
    print mynumber // 2
    
    from __future__ import print_function
    from __future__ import division
    
    print('\nPython 3:')
    print("The number is {}".format(mynumber))
    print(mynumber / 2, end=' ')
    print(mynumber // 2)
    
    
    
    Python 2:
    The number is 5
    2 2
    
    Python 3:
    The number is 5
    2.5 2
    
    

Oh, and here's an easter egg for C programmers:

In [3]:
    
    
    from __future__ import braces
    
    
    
      File "<ipython-input-3-2aebb3fc8ecf>", line 1
        from __future__ import braces
    SyntaxError: not a chance
    

##  2\. enumerate(list)

It might seem obvious that you should be able to iterate over a list and its index at the same time, but I used counter variables or slices for an embarrassingly long time.

In [4]:
    
    
    mylist = ["It's", 'only', 'a', 'model']
    
    for index, item in enumerate(mylist):
        print(index, item)
    
    
    
    0 It's
    1 only
    2 a
    3 model
    
    

##  3\. Chained comparison operators

Because I was so used to statically typed languages (where this idiom would be ambiguous), it never occurred to me to put two operators in the same expression. In many languages, 4 > 3 > 2 would return as False, because (4 > 3) would be evaluated as a boolean, and then True > 2 would be evaluated as False.

In [5]:
    
    
    mynumber = 3
    
    if 4 > mynumber > 2:
        print("Chained comparison operators work! \n" * 3)
    
    
    
    Chained comparison operators work! 
    Chained comparison operators work! 
    Chained comparison operators work! 
    
    
    

##  4\. collections.Counter

The collections library is, like, the best thing ever. Stackoverflow turned me on to ordered dicts early on, but I kept using a snippet to create dicts to count occurrences of results in my code. One of these days, I'll figure out a use for collections.deque.

In [6]:
    
    
    from collections import Counter
    from random import randrange
    import pprint
    mycounter = Counter()
    for i in range(100):
        random_number = randrange(10)
        mycounter[random_number] += 1
    for i in range(10):
        print(i, mycounter[i])
    
    
    
    0 10
    1 10
    2 13
    3 6
    4 6
    5 11
    6 10
    7 14
    8 12
    9 8
    
    

##  5\. Dict comprehensions

A rite of passage for a Python programmer is understanding list comprehensions, but eventually I realized dict comprehensions are just as useful -- especially for reversing dicts.

In [7]:
    
    
    my_phrase = ["No", "one", "expects", "the", "Spanish", "Inquisition"]
    my_dict = {key: value for value, key in enumerate(my_phrase)}
    print(my_dict)
    reversed_dict = {value: key for key, value in my_dict.items()}
    print(reversed_dict)
    
    
    
    {'Inquisition': 5, 'No': 0, 'expects': 2, 'one': 1, 'Spanish': 4, 'the': 3}
    {0: 'No', 1: 'one', 2: 'expects', 3: 'the', 4: 'Spanish', 5: 'Inquisition'}
    
    

##  6\. Executing shell commands with subprocess

I used to use the os library exclusively to manipulate files; now I can even programmatically call complex command-line tools like ffmpeg for video editing

In [8]:
    
    
    import subprocess
    output = subprocess.check_output('dir', shell=True)
    print(output)
    
    
    
     Volume in drive C is OS
     Volume Serial Number is ECAC-AE50
    
     Directory of C:\Users\David\Documents\Dropbox\IPython_Synced\GitHub\Misc_ipynb
    
    2014-11-26  06:04 AM    <DIR>          .
    2014-11-26  06:04 AM    <DIR>          ..
    2014-11-23  11:47 AM    <DIR>          .git
    2014-11-26  06:06 AM    <DIR>          .ipynb_checkpoints
    2014-11-23  08:59 AM    <DIR>          CCCma
    2014-09-03  06:58 AM            19,450 colorbrewdict.py
    2014-09-03  06:58 AM            92,175 imagecompare.ipynb
    2014-11-23  08:41 AM    <DIR>          Japan_Earthquakes
    2014-09-03  06:58 AM             1,100 LICENSE
    2014-09-03  06:58 AM             5,263 monty_monte.ipynb
    2014-09-03  06:58 AM            31,082 pocket_tumblr_reddit_api.ipynb
    2014-11-26  06:04 AM             3,211 README.md
    2014-11-26  06:14 AM            19,898 top_10_python_idioms.ipynb
    2014-09-03  06:58 AM             5,813 tree_convert_mega_to_gexf.ipynb
    2014-09-03  06:58 AM             5,453 tree_convert_mega_to_json.ipynb
    2014-09-03  06:58 AM             1,211 tree_convert_newick_to_json.py
    2014-09-03  06:58 AM            55,970 weather_ML.ipynb
                  11 File(s)        240,626 bytes
                   6 Dir(s)  180,880,490,496 bytes free
    
    
    

##  7\. dict .get() and .iteritems() methods

Having a default value when a key does not exist has all kinds of uses, and just like enumerate() for lists, you can iterate over key, value tuples in dicts

In [9]:
    
    
    my_dict = {'name': 'Lancelot', 'quest': 'Holy Grail', 'favourite_color': 'blue'}
    
    print(my_dict.get('airspeed velocity of an unladen swallow', 'African or European?\n'))
    
    for key, value in my_dict.iteritems():
        print(key, value, sep=": ")
    
    
    
    African or European?
    
    quest: Holy Grail
    name: Lancelot
    favourite_color: blue
    
    

##  8\. Tuple unpacking for switching variables

Do you know how many times I had to use a third, temporary dummy variable in VB? c = a; a = b; b = c?

In [10]:
    
    
    a = 'Spam'
    b = 'Eggs'
    
    print(a, b)
    
    a, b = b, a
    
    print(a, b)
    
    
    
    Spam Eggs
    Eggs Spam
    
    

##  9\. Introspection with help()

I was aware of dir(), but I had assumed help() would do the same thing as IPython's ? magic command. It does way more. (This post has been updated after some great advice from reddit's /r/python which, indeed, I wish I'd known about before!)

In [1]:
    
    
    my_dict = {'That': 'an ex-parrot!'}
    
    help(my_dict)
    
    
    
    Help on dict object:
    
    class dict(object)
     |  dict() -> new empty dictionary
     |  dict(mapping) -> new dictionary initialized from a mapping object's
     |      (key, value) pairs
     |  dict(iterable) -> new dictionary initialized as if via:
     |      d = {}
     |      for k, v in iterable:
     |          d[k] = v
     |  dict(**kwargs) -> new dictionary initialized with the name=value pairs
     |      in the keyword argument list.  For example:  dict(one=1, two=2)
     |  
     |  Methods defined here:
     |  
     |  __cmp__(...)
     |      x.__cmp__(y) <==> cmp(x,y)
     |  
     |  __contains__(...)
     |      D.__contains__(k) -> True if D has a key k, else False
     |  
     |  __delitem__(...)
     |      x.__delitem__(y) <==> del x[y]
     |  
     |  __eq__(...)
     |      x.__eq__(y) <==> x==y
     |  
     |  __ge__(...)
     |      x.__ge__(y) <==> x>=y
     |  
     |  __getattribute__(...)
     |      x.__getattribute__('name') <==> x.name
     |  
     |  __getitem__(...)
     |      x.__getitem__(y) <==> x[y]
     |  
     |  __gt__(...)
     |      x.__gt__(y) <==> x>y
     |  
     |  __init__(...)
     |      x.__init__(...) initializes x; see help(type(x)) for signature
     |  
     |  __iter__(...)
     |      x.__iter__() <==> iter(x)
     |  
     |  __le__(...)
     |      x.__le__(y) <==> x<=y
     |  
     |  __len__(...)
     |      x.__len__() <==> len(x)
     |  
     |  __lt__(...)
     |      x.__lt__(y) <==> x<y
     |  
     |  __ne__(...)
     |      x.__ne__(y) <==> x!=y
     |  
     |  __repr__(...)
     |      x.__repr__() <==> repr(x)
     |  
     |  __setitem__(...)
     |      x.__setitem__(i, y) <==> x[i]=y
     |  
     |  __sizeof__(...)
     |      D.__sizeof__() -> size of D in memory, in bytes
     |  
     |  clear(...)
     |      D.clear() -> None.  Remove all items from D.
     |  
     |  copy(...)
     |      D.copy() -> a shallow copy of D
     |  
     |  fromkeys(...)
     |      dict.fromkeys(S[,v]) -> New dict with keys from S and values equal to v.
     |      v defaults to None.
     |  
     |  get(...)
     |      D.get(k[,d]) -> D[k] if k in D, else d.  d defaults to None.
     |  
     |  has_key(...)
     |      D.has_key(k) -> True if D has a key k, else False
     |  
     |  items(...)
     |      D.items() -> list of D's (key, value) pairs, as 2-tuples
     |  
     |  iteritems(...)
     |      D.iteritems() -> an iterator over the (key, value) items of D
     |  
     |  iterkeys(...)
     |      D.iterkeys() -> an iterator over the keys of D
     |  
     |  itervalues(...)
     |      D.itervalues() -> an iterator over the values of D
     |  
     |  keys(...)
     |      D.keys() -> list of D's keys
     |  
     |  pop(...)
     |      D.pop(k[,d]) -> v, remove specified key and return the corresponding value.
     |      If key is not found, d is returned if given, otherwise KeyError is raised
     |  
     |  popitem(...)
     |      D.popitem() -> (k, v), remove and return some (key, value) pair as a
     |      2-tuple; but raise KeyError if D is empty.
     |  
     |  setdefault(...)
     |      D.setdefault(k[,d]) -> D.get(k,d), also set D[k]=d if k not in D
     |  
     |  update(...)
     |      D.update([E, ]**F) -> None.  Update D from dict/iterable E and F.
     |      If E present and has a .keys() method, does:     for k in E: D[k] = E[k]
     |      If E present and lacks .keys() method, does:     for (k, v) in E: D[k] = v
     |      In either case, this is followed by: for k in F: D[k] = F[k]
     |  
     |  values(...)
     |      D.values() -> list of D's values
     |  
     |  viewitems(...)
     |      D.viewitems() -> a set-like object providing a view on D's items
     |  
     |  viewkeys(...)
     |      D.viewkeys() -> a set-like object providing a view on D's keys
     |  
     |  viewvalues(...)
     |      D.viewvalues() -> an object providing a view on D's values
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  __hash__ = None
     |  
     |  __new__ = <built-in method __new__ of type object>
     |      T.__new__(S, ...) -> a new object with type S, a subtype of T
    
    
    

#  10\. PEP-8 compliant string chaining

[PEP8](https://www.python.org/dev/peps/pep-0008) is the style guide for Python code. Among other things, it directs that lines not be over 80 characters long and that indenting by consistent over line breaks.  
This can be accomplished with a combination of backslashes \; parentheses () with commas , ; and addition operators +; but every one of these solutions is awkward for multiline strings. There is a multiline string signifier, the triple quote, but it does not allow consistent indenting over line breaks.   
There is a solution: parentheses without commas. I don't know why this works, but I'm glad it does.

In [12]:
    
    
    my_long_text = ("We are no longer the knights who say Ni! "
                    "We are now the knights who say ekki-ekki-"
                    "ekki-p'tang-zoom-boing-z'nourrwringmm!")
    print(my_long_text)
    
    
    
    We are no longer the knights who say Ni! We are now the knights who say ekki-ekki-ekki-p'tang-zoom-boing-z'nourrwringmm!
    
    

In [ ]:
