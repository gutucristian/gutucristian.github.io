My personal (unfinished) compilation of Python tips and tricks.

# Sorting

`sorted()` returns a **new** sorted list. It works on any `iterable`. It sorts using the Timsort algorithm and has an average time complexity of `O(nlogn)`, the worst case time complexity is also `O(nlogn)`.

`list.sort()` sorts a list in place (also using the Timsort sorting algorithm).

# Miscellaneous

`isinstance(c1, c2)` returns `True` if `c1` is an instance of `c2` as well as if `c1` is a subclass of `c2`

`type(c1, c2)` only returns `True` if `c1` is an instance of `c2` 
