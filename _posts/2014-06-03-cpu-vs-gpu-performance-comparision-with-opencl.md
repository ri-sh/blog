---
layout: post
title: "CPU vs GPU Performance Comparison with OpenCL"
date: 2014-06-03 14:27:00
tags: ["Python", "Opencl"]
---

_Originally posted on my old blog on 2014-06-03._

I recently had opportunity to explore an awesome library called OpenCL (Open Computing Language) which enables me to create programs which helps me utilize the computation power of my Graphic Card. I wanted to try out how much faster a normal program (addition of elements to two arrays) would work if I parallize the program using OpenCL. 

**Source**  
_Using OpenCL_  

    
    
    import pyopencl as cl
    import numpy
    import sys
    
    class CL(object):
        def __init__(self, size=10):
            self.size = size
            self.ctx = cl.create_some_context()
            self.queue = cl.CommandQueue(self.ctx)
    
        def load_program(self):
            fstr="""
      __kernel void part1(__global float* a, __global float* b, __global float* c)
      {
             unsigned int i = get_global_id(0);
    
            c[i] = a[i] + b[i];
      }
          """
            self.program = cl.Program(self.ctx, fstr).build()
    
        def popCorn(self):
            mf = cl.mem_flags
    
            self.a = numpy.array(range(self.size), dtype=numpy.float128)
            self.b = numpy.array(range(self.size), dtype=numpy.float128)
    
            self.a_buf = cl.Buffer(self.ctx, mf.READ_ONLY | mf.COPY_HOST_PTR,
                                   hostbuf=self.a)
            self.b_buf = cl.Buffer(self.ctx, mf.READ_ONLY | mf.COPY_HOST_PTR,
                                   hostbuf=self.b)
            self.dest_buf = cl.Buffer(self.ctx, mf.WRITE_ONLY, self.b.nbytes)
    
        def execute(self):
            self.program.part1(self.queue, self.a.shape, None, self.a_buf, self.b_buf, self.dest_buf)
            c = numpy.empty_like(self.a)
            cl.enqueue_read_buffer(self.queue, self.dest_buf, c).wait()
            print "a", self.a
            print "b", self.b
            print "c", c
    
    if __name__ == '__main__':
        matrixmul = CL(10000000)
        matrixmul.load_program()
        matrixmul.popCorn()
        matrixmul.execute()
    

_Normal program without Optimization_  

    
    
    def add(size=10):
        a = tuple([float(i) for i in range(size)])
        b = tuple([float(j) for j in range(size)])
        c = [None for i in range(size)]
        for i in range(size):
            c[i] = a[i]+b[i]
    
        #print "a", a
        #print "b", b
        print "c", c[:1000]
    
    add(1000000)
    

I compared the performance of both the programs using the tool “time” available in Linux and I noted down the “sys time”  
Heres the comparision:  
  
Size | Using GPU | Without GPU ( i.e. CPU)  
---|---|---  
100 | 0.130s | 0.030s  
1000 | 0.100s | 0.010s  
10000 | 0.130s | 0.010s  
100000 | 0.150s | 0.050s  
1000000 | 0.170s | 0.150s  
10000000 | 0.600s | 1.150s  
  
Cleary you see that the GPU outperforms CPU at higher values of size as the program is able to use multiple threads provided by the GPU. At lower values of size there is an appreciable access time associated with GPU, so CPU performs faster.
