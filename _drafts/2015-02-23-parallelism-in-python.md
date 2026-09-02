---
layout: post
title: "Parallelism in Python"
date: 2015-02-23 12:46:00
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2015-02-23._

One aspect of coding in Python that we have yet to discuss in any great detail is how to optimise the execution performance of our simulations. While NumPy, SciPy and pandas are extremely useful in this regard when considering  _vectorised_ code, we aren't able to use these tools effectively when building  _event-driven_ systems. Are there any other means available to us to speed up our code? The answer is yes - but with caveats!

In this article we are going to look at the different models of  _parallelism_ that can be introduced into our Python programs. These models work particularly well for simulations that do not need to share  _state_. Monte Carlo simulations used for options pricing and backtesting simulations of various parameters for algorithmic trading fall into this category.

In particular we are going to consider the [Threading](https://docs.python.org/2/library/threading.html) library and the [Multiprocessing](https://docs.python.org/2/library/multiprocessing.html) library

  


Many programs, particularly those relating to network programming or data input/output (I/O) are often  _network-bound_ or  _I/O bound_. This means that the Python interpreter is awaiting the result of a function call that is manipulating data from a "remote" source such as a network address or hard disk. Such access is far slower than reading from local memory or a CPU-cache.

Hence, one means of speeding up such code if many data sources are being accessed is to generate a  _thread_ for each data item needing to be accessed.

For example, consider a Python code that is scraping many web URLs. Given that each URL will have an associated download time well in excess of the CPU processing capability of the computer, a single-threaded implementation will be significantly I/O bound.

  


  


.........to be continued
