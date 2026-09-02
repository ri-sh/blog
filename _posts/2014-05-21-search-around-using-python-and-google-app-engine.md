---
layout: post
title: "Search Around Using Python and Google App Engine"
date: 2014-05-21 11:35:00
tags: ["GoogleAppengine", "Python", "Google Apis"]
thumbnail: /assets/img/migrated/search-around-using-python-and-google-app-engine/img0.gif
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2014-05-21._

###  This summer i tried to complete my web application a place search application using google App engine and google places api 

the application is written in python and hosted at <http://searchrishabh.appspot.com/>

this application can search restaurants , hospitals , schools near a location

I know the sites html is not formatted properly you might not be satisfied by the visual appearance o the application sorry for that but i wanted to demonstrate the power taht google has give us that just using a python script and Google app engine we can make web apps that millions of people can use 

like type 

atm in master canteen bhubaneswar

  


it will list all the ATMs in that location with the address of the atms .  
  
  


.

you can view the source code on [github](https://github.com/ri-sh/mobiapps/blob/master/textweb/src/searcharound.py)

  
  
you also might be wondering why there is Txtweb written on the home page .  
actually i wanted to make the application for Txtweb a platform for integrating web apps and mobile SMS service so that these locations u can search using mobile phone without an internet service   
just by messaging the things to a number and then you will be getting the results related to your query .  
currently iam 

  


i would like to answer some questions about Google app engine which some of you might be having like .

### 

###  What is Google App Engine ?

[![](/assets/img/migrated/search-around-using-python-and-google-app-engine/img0.gif)](http://2.bp.blogspot.com/_Nwfd_2Q90bw/TC0Xo_Hf_4I/AAAAAAAAA2k/_hCAgKc3xk0/s1600/appengine_lowres.gif)

Google App Engine is an online platform to host your web application. It is what is popularly called a Cloud Computing platform to host and enable your web applications on the cloud. Essentially having a cloud platform for hosting your applications abstracts you from worrying about the various platform setup and environment related issues like server configuration, network configuration, bandwidth requirements among others.  


  


  


  
What languages are supported ?  
  
If there is one limitation from the platform it is the language support. Recently by adding Java support has been a huge plus and should significantly improve adoption of the platform.  
  
Currently the App Engine supports Python and Java (JVM).What this means is that App Engine applications can also be written in Java or any JVM-compatible language (e.g. JRuby, Groovy, Scala, etc.). It supports Java runtime 6  
  
App Engine's Python runtime supports Python 2.5  
  
  
What Frameworks are supported ?  
  
Using frameworks to be able speed up the development cycle has become such a key necessity that it is critical to check support for frameworks in any cloud environment that you want to work with. Google App Engine supports Struts 2 and Spring MVC for Java and most Python web frameworks including Django versions 1.0.2.  
  
How much does it cost ?  
  
This is the greatest thing about the App Engine. Its free if you use less than 500 MB space and have less than 5 million page views a month. This is the reason we rank this as the #1 platform for building cloud applications especially for student projects or small POC applications.  
  
You can build around 10 applications for each google account that you have, and nobody stops you from creating any number of google logins, so you can always get as many free apps as you want.  
  
For Apps where you expect more usage and need more space you have to pay per unit of the resource that you are consuming. So really it is a pay based on usage model which is still highly cost effective. I do not see a need for this unless you are writing commercial apps and for most cases the free limits are good enough.  
  
  
How to get started ?  
  
All you need is a gmail or a google account. That's it. If you have one you are ready to get started with building your first cloud app. If not you can always get an account, it just takes a couple of minutes.
