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

Write to file char by char:
{% highlight c linenos %}
 char *str = "cibureh";
 
 FILE *fp;
	fp = fopen("data.txt", "w"); // write mode file is created or truncated to zero length on write
	
 int len = strlen(str);
	int i;
	char c;

	for (i = 0; i < len; i++) {
		c = fputc(str[i], fp);
		printf("wrote \"%c\" to file\n", c);
	}

	fclose(fp);
{% endhighlight %}
