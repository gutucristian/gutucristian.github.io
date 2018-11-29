A compilation of useful IO commands.

Opening and closing file:
{% highlight c linenos %}
FILE *fp;
fp = fopen("data.txt", "r"); // reading mode
fclose(fp);
{% endhighlight %}

Reading from file char by char until EOF:
{% highlight c linenos %}
FILE *fp;
fp = fopen("data.txt", "r"); // reading mode
char c;
while ((c = fgetc(fp)) != EOF) {
 printf("%c", c);
}
fclose(fp);
{% endhighlight %}
