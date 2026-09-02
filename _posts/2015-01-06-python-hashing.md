---
layout: post
title: "Python Hashing"
date: 2015-01-06 15:26:00
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2015-01-06._

### 

these are some of things i learnt and i wanted to share from Brandon Rhodes, from this [pycon talk](http://pyvideo.org/video/276/the-mighty-dictionary-55).

###  Hashing 

some of the most important data structures used in python is a Dictionary 

a python dictionary is an implementation of list. dictionary gives us an O(1) lookup on average .

i am not going to say /show how to use a dictionary but how the internals of a dictionary in python works its something worth appreciating and how intelligently dictionaries trade space for time on a go to save our cpu time to avoid collisions/overlapping in the dictionaries .

when we implement a dictionary it behind the scenes is creating an 8 element list or table in a continuous block of ram which is a hash table 

the table is of following form 

Index  |  Hash |  key  |  value  
---|---|---|---  
001 |    
|    
|    
  
010 |    
|    
|    
  
011 |    
|    
|    
  
100 |    
|    
|    
  
101 |    
|    
|    
  
110 |    
|    
|    
  
111 |    
|    
|    
  
  
  


in python keys need to be hashed hash is a mathematical algorithm that takes an input like a string or a number and scatter it over zeroes and ones in form of 32 bit binaries when we run an hash on a item it always gives back the same result for example when we do the following code .

>>import hashlib  
>> hash('Goku')  
3644900746342488926  
  
>>print binary(hash ('Goku'))

  
'0b11001010010101010010011110101011010011010011001101101101011110'   
  
Index  |  Hash |  key  |  value  
---|---|---|---  
001 |    
|    
|    
  
010 |    
|    
|    
  
011 |    
|    
|    
  
100 |    
|    
|    
  
101 |    
|    
|    
  
110 |    
|    
|    
  
111 |    
|    
|    
  
  
  


  


  


  


**What happens when you delete an element?**

Intuitively, you might think that deleting a key, or multiple keys, will mean that python will garbage collect that and memory consumption will reduce. In practice, this is  _not_ happening. The reason being that python assumes that if you made the slot empty, you will fill something soon enough. Moreover, we are using open addressing. So, when you delete a key, that position is not necessarily the first position it was hashed to. Now, if you make that slot empty, and you search for another key (that is on a depth higher than this one), python can find an empty slot and incorrectly return key not found! You do not want that and hence, if you keep deleting keys a lot, python uses up the space with <dummy> values. If you have deleted quite a lot of keys and have no intention of adding more keys, then, dict.copy() is a good way. When the copying is done, the dormant slots are deleted.
