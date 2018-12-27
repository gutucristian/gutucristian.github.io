Palindrome Permutation: Given a string write a function to check is a string is a permutation of a palindrome.

Observer that in order for a word to be a palindrome at most one character can have an odd count all the other characters must have even counts. Knowing this there are various solutions to this problem.

1. The first and most obvious solution is to count the frequencies of each character and make sure that the above property is satisfied.

2. Assume input string contains only ASCII characters. Based on this assumption, maintatin a fixed sized buffer of length 128 (regular, unextended, ASCII has 128 character encodings). Let each index `i` in this buffer indicate the frequency of the character in our input whose ASCII code is `i`. That said, instead of maintaing the actual character frequency at `i` we will simply flip the value from 0 to 1 if the freq. is odd and 1 to 0 if it even. At the end we can apply a `reduce()` function to calculate the sum of the buffer and return false if sum > 1, and otherwise return true.

{% highlight python linenos %}
from functools import reduce

def is_palindrome(s1):  
  buf = [0] * 128
  for char in s1:
    if (buf[ord(char)] == 0):
      buf[ord(char)] = 1      
    else:
      buf[ord(char)] = 0 

  return reduce(lambda x, y: x + y, buf) <= 1

print(is_palindrome("tacocat"))
{% endhighlight %}

3. A small "improvement" to the solution above is to simple maintain the number of odd character frequencies as we go and return True or False depending on the number of odd character frequencies without having to use `reduce()` at the end of the function.

{% highlight python linenos %}
def is_palindrome(s1):
  count_odds = 0
  buf = [0] * 128
  for char in s1:
    if (buf[ord(char)] == 0):
      buf[ord(char)] = 1
      count_odds += 1
    else:
      buf[ord(char)] = 0
      count_odds -= 1
    print(count_odds)
  return count_odds <= 1
{% endhighlight %}

This, however, does not change the time complexity which is still `O(n)` where n = the length of `s1`.

4. Finally, a solution using a bit vector.

{% highlight python linenos %}
def is_palindrome_permutation(s1):
  bit_vector = create_bit_vector(s1) # assume ASCII string
  return bit_vector == 0 or check_exactly_one_bit_is_set(bit_vector)

def create_bit_vector(s1):
  bit_vector = 0
  for char in s1:
    x = get_char_number(char)
    bit_vector = toggle(bit_vector, x)
  return bit_vector

def check_exactly_one_bit_is_set(bit_vector):
  return (bit_vector & (bit_vector - 1)) == 0

def toggle(bit_vector, index):
  if index < 0:
    return bit_vector

  mask = 1 << index

  if ((bit_vector & mask) == 0):
    bit_vector = bit_vector | mask # OR: set bit at "index" to 1
  else:
    bit_vector = bit_vector & (~mask) # XOR: toggle bit at "index" to 0 if 1 or 1 if 0

  return bit_vector

def get_char_number(char):
  a = ord('a')
  z = ord('z')
  val = ord(char)

  if ((val >= a) and (val <= z)):
    return val - a

  return -1

s1 = "tacocat"
print(is_palindrome_permutation(s1))
{% endhighlight %}
