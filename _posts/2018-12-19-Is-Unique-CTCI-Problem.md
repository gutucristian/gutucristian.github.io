Is Unique: Implement an algorithm to determine if a string has all unique characters.

There are a few ways to solve this problem.

**1.** If we assume an ASCII string then we know the alphabet has 128 characters (or 256 for extended ACII). In this case we can use a buffer of length 128 to indicate whether or not the character at index `i` is in the string. The second time we see this character we can immediatly return false.

{% highlight python linenos %}
  def is_unique(s1):  
    # string cannot be unique if its length is greater than the total alphabet size (128 for ASCII string)
    if len(s1) > 128:
      return False

    lst = [None] * 128
    for char in s1:
      if lst[ord(char)]:
        return False
      lst[ord(char)] = 1

    return True
{% endhighlight %}

The time complexity for this is `O(n)` where n is the length of the string. The space complexity is `O(1)` because we use a fixed size buffer.

**2.** We can take the original string and remove all duplicates (if any). If the length of the set after removing duplicates is equal to the length of the original string, then the string must be unique.

{% highlight python linenos %}
  def is_unique(s1):  
    return len(set(s1)) == len(s1)
{% endhighlight %}

The time complexity for this is `O(n + n + n)` which is just `O(n)`. The `len()` function is `O(n)` because it scans through the list and `set(s1)` also takes `O(n)` because it scans through every character in the string and adds it to a hash table. Adding to a hash table is usually `O(1)` (unless we have collisions, in which case it is `O(n)`). 
