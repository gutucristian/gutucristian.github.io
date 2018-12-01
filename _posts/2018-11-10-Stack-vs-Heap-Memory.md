---
layout: post
title: Stack vs. Heap Memory
---

In this fabulous post, I will explain stack vs. heap memory and more! So, I suggest you grab some hot chocolat and learn a thing or two.

But, before we get into the meat and potatoes of this post (or tofu and potatoes if you're vegetarian) we shall mention the various types of memory and how they are organized.

![an image alt text]({{ site.baseurl }}/images/memoryLayout.png "memory layout")

The typical memory layout for a C program is as follows:
1. Stack
2. Heap (a.k.a., dynamic data)
3. Initialized Data Segment (i.e., static data)
3. Unitialized Data Segment (i.e., static data)
4. The "text" segment
5. Reserved (OS reserved memory buffer)

So, the stack and heap are just _two_ of several parts of the entire memory layout for a program. That said, they are two very important parts.

Think of the stack memory buffer as the "scratch space" for a thread of execution. When a function is called, a block is reserved on top of the stack. This block includes the function local variables and some bookeeping data. When the function returns, that block of memory is "released" and becomes free to use. The stack is managed by the LIFO (last in first out) principle which makes it really easy to keep track of the stack. Namely, freeing and allocating space on the stack is nothing more than adding or subtracting from the stack pointer (stack pointer is stored in the `$sp` register in MIPS).

The heap is a memory buffer set aside specifically for dynamic allocation. Unlike the stack, there is no enforced pattern regarding allocation and deallocation. As such, it becomes rather difficult to keep track of which parts of the heap are allocated or free at any given time.

Also, each thread gets its own stack where as the heap is shared across threads.

To keep it simple, short here are some fundmanetal differences between stack and heap in bullet form.

__Stack:__
* stored in ram (just like heap)
* variables allocated on stack will go out of scope and be deallocated when you leave current execution context
* implemented using LIFO principle (like the stack data structure)
