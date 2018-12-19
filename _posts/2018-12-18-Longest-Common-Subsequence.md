The longest increasing subsequence problem is

{% highlight python linenos %}
s1 = "asdf"
s2 = "df"
def LCS(s1, s2, i, j):  
  if (i == 0 or j == 0):
    return 0
  elif s1[i-1] == s2[j-1]:
    return 1 + LCS(s1, s2, i-1, j-1)
  else:
    return max(LCS(s1, s2, i-1, j), 
print(LCS(s1, s2, len(s1), len(s2)))
{% endhighlight %}
