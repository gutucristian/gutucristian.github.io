In computer science, a hash table is a **data structure** that implements an **associative array abstract data type (ADT)**. Fundamentally, a hash table maps **keys** to **values** using a **hash function** that computes an index into an array of buckets from which the desired value can be found. Ideally, the hash function will assign each key to a unique bucket. However, most hash functions are imperfect and cause **hash collisions**. A hash collision is when the hash function generates the same index for more than one key. Such collisions are usually accomodated for in some way (e.g., linear probing). In a well-dimensioned hash table, the average cost for lookup and insertion is independent of the number of elements stored in the table. Some hash table designs also allow for arbitrary insertions and deletions of key-value pairs.

## Hashing

The idea of hashing is to distribute the entries (key-value pairs) across an array of buckets. Given a key, the hash function computes an **index** that suggests in what bucket the entry can be found:

{% highlight java linenos %}
 index = f(key, arraySize)
{% endhighlight %}

Often this is done in two steps:

{% highlight java linenos %}
 hash = hashfunc(key)
 index = hash % arraySize
{% endhighlight %}

In this method, the hash is independent of the array size and is then _reduced_ (a number between `0` and `arraySize - 1`) to an index using the modulo operator (`%`).

## Choosing a hash function

A basic requirement of most hash functions is that it should provide a **uniform distribution** of hash values. Non-uniform distributions increase the number of collisions and the cost of resolving them. As expected, uniformity is sometimes hard to achieve by design. Of course, it may be evaluated empirically using various statistical tests (e.g., [Pearson's chi-squared test](https://en.wikipedia.org/wiki/Pearson%27s_chi-squared_test#Discrete_uniform_distribution) for discrete random distributions).

## Key statistics

A critical statistic for a hash table is the **load factor** defined as:

{% highlight java linenos %}
 load factor = n / k
{% endhighlight %}

where

`n` = number of entries (key-value pairs) in the hash table, and

`k` = number of buckets

The larger the load factor the slower the hash table becomes. Namely, the expected constant time property of the hash table assumes that the load factor is kept below some bound. If we have a fixed number of buckets, the time for a lookup grows with the number of entries added. In Java 10, the default load factor is set to `.75`. However, the load factor alone is not indicative enough of the spread of data in the hash table. For example, assume we have two table that have `1,000` entries and `1,000` buckets. One table has exactly one entry in each bucket, the other has all entries in the same bucket. Clearly, the hash function for the second table is not working properly. Thus, **variance** is also an important parameter to consider when analyzing the efficiency of a hash table and its hash function. Lastly, a load factor is not always beneficial. In fact, as the load factor approaches `0`, the proportion of unused aread in the hash table increases, but there is not any reduction in search cost. Evidently, this results in wasted memory. Ultimately, we conclude that (as always) balance is key!

## Collision resolution

There are various ways to deal with collisions:
1. Chaining with linked lists:
With this approach (which is the most common), the hash table's array maps to a linked list of items. We just add items to this linked list. As long as number of collisions is small, this is quite efficient. In the worst case, lookup is `O(n)`, where `n` is the number of elements in the hash table. This would only happen with either very strange data or a poor hash function or both. It is also worth that this approach inherits the disadvantages of linked lists. Namely, when storing small keys and values, the space overhead of the `next` pointer can be a significant overhead. Additionally, traversing linked lists had poor cache performance, making the processor cache(s) (e.g., L1, L2, L3 cache) ineffective.
2. Chaining with binary search trees
3. Open addressing with linear probing
4. Quadratic probing and double hashing
