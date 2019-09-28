My personal (unfinished) list of Python tips and tricks.

# Sorting

`sorted()` returns a **new** sorted list. It works on any `iterable`. It sorts using the Timsort algorithm and has an average time complexity of `O(nlogn)`, the worst case time complexity is also `O(nlogn)`.

`list.sort()` sorts a list in place (also using the Timsort sorting algorithm).

# Miscellaneous

`isinstance(c1, c2)` returns `True` if `c1` is an instance of `c2` as well as if `c1` is a subclass of `c2`

`type(c1, c2)` only returns `True` if `c1` is an instance of `c2` 

# Generators

A python generator function returns values on a per need basis. This is useful for situations where we work with infinite sequences that may not be calculated and stores at once (e.g., the 2,000,000 th fibonacci number). When a function includes the `yield` keyword it automatically becomes a generator. Everytime the function is called we receive the next number in the sequence (as such generators store their state).

{% highlight python linenos %}
 def fibonacci(n):
   curr = 1
   prev = 0
   counter = 0
   while counter < n:
     yield curr
     prev, curr = curr, prev + curr
     counter += 1

 for value in fibonacci(20):
   print(value)
{% endhighlight %}

# OOP

Four pillars to OOP:
1. Abstraction
2. Encapsulation
3. Inheritance
4. Polymorphism

__Polymorphism__ is the ability to leverage the same interface for different underlying forms such as data types or classes [source](https://www.digitalocean.com/community/tutorials/how-to-apply-polymorphism-to-classes-in-python-3). This permits functions to use entities of different types at different times.
