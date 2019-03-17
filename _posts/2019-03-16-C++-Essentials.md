C++ and object oriented programing (OOP)

## Template

Templates are the foundation of generic programming, which involves writing code in a way that is independent of any particular type.

Templates allow us to define a blueprint (or formula) for creating a generic class or function. Many STL containers and algorithms are implemented using templates, which allows a great flexibility in the types of supported elements.

For example, in the STL, there is a single definition of the `vector` container, but we can define many different kinds of vectors: `vector<int>`, `vector<float>`, `vector<string>`, and `vector<T>` where `T` is any type.

In C++ we can write functions this can be achieved using Generic formal parameter are the way C++ enables

{% highlight c++ linenos %}
  template <class myType>
  myType GetMax (myType a, myType b) {
    return (a>b?a:b);
  }
{% endhighlight %}

## Container

A container is a holder object that stores a collection of other objects. Containers are implemented as class templates, which allows for greater flexibility in the types of supported elements.

## Lambdas

Lambdas exist in lots of other programming languages that are coming from the functional programming family (e.g., Scheme). They also exist in C based languages like Java, C++, Objective C. We will start by looking at a very primitive form of lambda because in reality even the C language had the embryo of an idea of what lambdas could provide to programmers however it is called something different. So in C we refer to these as pointers to functions.

## Object Oriented Programmin (OOP)

The four pillars of OOP:
1. Polymorphism
2. Abstraction
3. Encapsulation
4. Inheritance

Encapsulation: the act of hiding unnecessary details from the user.
- As long as the usage contract remains the same, implementation details can change without affecting all users
- Wrap data and methods into a unit, data is not accessible to the outside world
- Insulating data from direct access is known as _data hiding_
- Example: `ssize_t send(int sockfd, const void *buf, size_t len, int flags);`. User need not know entire network stack and how `send()` transmits data reliably from one end host to another.

Inheritance: process by which objects of one class aquire properties of another class.

Polymorphism: when you can treat an object as a generic version of something, but when you access it, the code determines which exact type it is and calls the associated code. Two types:
1. Static polymorphism: determined at compile time (e.g., method overloading)
2. Dynamic polymorphism: determined at run time (e.g, method overriding)

Class: a blueprint of data and methods that work with that data

Object: a specific instance of that class

Instance variable(s): belong to a specific instance of a class

Class variable(s): also known as static member variables and there is only one copy of these variables that is shared with all instances of that class

## Algorithms
- sorting (mergesort, quicksort, heapsort, radix sort)

## Data Structures
- heap (min heap, max heap)
- queue (using array, using linked list)
- stack (using array, using linked list)
- hashtable (a.k.a., map or dictionary)
- array
- trees
- linked list
