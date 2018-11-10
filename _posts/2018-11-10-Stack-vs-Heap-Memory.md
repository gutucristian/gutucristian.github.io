---
layout: post
title: Stack vs. Heap Memory (and more)
---

In this fabulous post, I will explain stack vs. heap memory and more!! So, I suggest you grab some hot chocolat and learn a thing or two. Anyways, let's get to the point homeboy.

But, before we get into the meat and potatos of this post (or tofu and potaoes if you're vegan ;)) we shall mention the various types of memory and how they are organized.

![an image alt text]({{ site.baseurl }}/images/memoryLayout.png "memory layout")

The typical memory layout for a C program is as follows:
1. The "text" segment
2. Initialized Data Segment (i.e., static data)
3. Unitialized Data Segment (i.e., static data)
4. Stack
5. Heap

So, the stack and heap are just _two_ parts of the entire memory layout for a program. That said, they are two very important parts.

Think of the stack memory buffer as the "scratch space" for a thread of execution. When a function is called, a block is reserved on top of the stack for local variables and some bookeeping data (such as info. on how much we increased the stack pointer so that we know by how much to decrease it when we exit the function, and things like values of sp ("special" register in MIPS) which *must* be preserved across function calls). When the function returns, that block of memory is "released" and becomes free to use. Releasing memory from the stack is easy bec. is just pointer arithmetic (i.e., add 12 bytes to the current stack pointer, or remove 12 bytes). 
