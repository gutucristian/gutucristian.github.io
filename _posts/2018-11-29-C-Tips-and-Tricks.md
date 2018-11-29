A compilation of useful IO commands.

Opening and closing file
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

**Method signatures for convenience**
`int fgetc( FILE *stream);`
- Read one (ASCII) character (8-bits) at a time (slow for large files):
- Returns EOF when at the end of file or on error.
- **Note**: to read from a file pass a file pointer, to read from `stdin` pass `stdin`

`int fputc(int c, FILE *stream);`
- Write one (ASCII) character (8-bits) at a time (slow for large files):
- Returns EOF on error.
- **Note**: to write to a file pass a file pointer, to write to `stdout` pass `stdout`
