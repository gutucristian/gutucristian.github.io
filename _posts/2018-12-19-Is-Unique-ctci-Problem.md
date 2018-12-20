Is Unique: Implement an algorithm to determine if a string has all unique characters.

There are a few ways to solve this problem.

**1.** If we assume an ASCII string then we know the alphabet has 128 characters (or 256 for extended ACII). In this case we can use a buffer of length 128 to indicate whether or not the character at index `i` is in the string. The second time we see this character we can immediatly return false.

{% highlight python linenos %}
def is_unique(s1):  
  if len(s1) > 128: # string cannot be unique if its length is greater than the total alphabet size (128 for ASCII string)
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

**3.** If we sort the input string then all characters in the alphabet that are also in the string will be placed next to each other. Based on this fact, we can traverse the sorted string and check neighboring characters. If any two neighboring characters are different then we know the string is not unique.

{% highlight python linenos %}
def is_unique(s1):
  s1_sorted = sorted(s1) # returns new list with sorted string
  for i in range(len(s1) - 1):
    if s1_sorted[i] == s1_sorted[i+1]:
      return False
  return True
{% endhighlight %}

The time complexity in this case is `O(nlogn + n)` which is just `O(nlogn)` (where `n` is the length of `s1`). The `O(nlogn)` comes from the `sorted()` function call which uses Timsort. The `O(n)` comes from the fact that we iterate through `s1`. Space complexity is `O(n)` because the length of the returned (sorted) list `s1_sorted` depends on the size of `s1` which we call `n`.

**4.** And, finally, my favorite solution: we assume the string only contains characters 'a' through 'z'. Note:
- Python integer is 4 bytes = 32 bits
- English alphabet size is 26 characters
- Thus, we can fit each character into the integer representaion of 32 bits. Namely, each bit position 0-25 indicates a character. If the value at a bit position `i` is 0 then we say the character is **not** present, otherwise, if the value is 1, then we say it is present in the input string.

We can get the bit position of a character by using `ord()` which returns the Unicode code for a character. Since `ord('z') - ord('a')` equals 25 we know we have more than enough space using an integer which is 32 bits.

Our bit vector is an integer called `checker` which we will set to 0: `checker = 0`.

So, we get the bit position of some character (say 'j') by doing `ord('j') - ord('a')` which is `9`. Next, we need to switch bit 9 in our temporary bit vector from `0` to `1`. Do this with bit shifting 1 nine times to the left: `1 << 9`.

To check if bit 9 was already 1 in our bit vector `checker` we simply perform a bitwise AND with the temporary bit vector. If the result is > 0 then we know that bit position 9 was already switched to a 1 because:

10010 & 00010 = 00010

which is greater than 0.

If this is true, then return `False`. Else switch bit position 9 in `checker` to 1. 
Do this with a bitwise OR (denoted as '|' in python):

10000 OR 00010 = 10010

Repeat this process for every character in the input string.

{% highlight python linenos %}
def is_unique(s1):  
  checker = 0
  for i in range(0, len(s1)):
    val = ord(s1[i]) - ord('a')        
    if ((checker & (1 << val)) > 0):
      return False    
    checker |= (1 << val)    
  return True
{% endhighlight %}
