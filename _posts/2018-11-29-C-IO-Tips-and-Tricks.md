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

Read from file line by line:
{% highlight c linenos %}
  FILE *fp; 
  fp = fopen("data.txt", "r");
  while (fgets(buffer, BUFFERSIZE, fp) != NULL) {
    printf("%s", buffer);
  }
  fclose(fp);
{% endhighlight %}

**Method signatures for convenience**

`int fgetc(FILE *stream);`
- Read one (ASCII) character (8-bits) at a time (slow for large files):
- Returns `EOF` when at the end of file or on error.
- **Note**: to read from a file pass a file pointer, to read from `stdin` pass `stdin`

`int fputc(int c, FILE *stream);`
- Write one (ASCII) character (8-bits) at a time (slow for large files):
- Returns EOF on error.
- **Note**: to write to a file pass a file pointer, to write to `stdout` pass `stdout`

`char *fgets(char *buf, int size, FILE *in);`
- Reads the next line from `in` into buffer `buf`
- Halts at `\n` or at `size-1` and adds `\0` (the C string termination character called NUL)
- Returns pointer to buf if successful or NULL

`int fputs(const char *str, FILE *out);`
- Writes `str` to `out` stopping at `\0`
- Returns number of characters written or `EOF`

Another option is `fread` and `fwrite`:
{% highlight c linenos %}
  #include <string.h>
  #include <stdio.h>

  #define BUFFERSIZE 100

  int main(int argc, char *argv[]) {
    FILE *fp;
    char buffer[BUFFERSIZE];
    char c[] = "this is cibureh";

    fp = fopen("data.txt", "w+");

    fwrite(c, strlen(c)+1, sizeof(char), fp);

    fseek(fp, 0L, SEEK_SET); // set file pointer to beggining of file

    fread(buffer, BUFFERSIZE, sizeof(char), fp);

    printf("%s\n", buffer);

    fclose(fp);

    return 0;
  }
{% endhighlight %}

Creating threads and passing data to threads:
{% highlight c linenos %}
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <assert.h>

#define NUM_THREADS 8

typedef struct {
	int thread_num;
	char *message;
	int len;
} thread_arg_t;

void *thread_main(void *p_arg) {
	thread_arg_t *p = p_arg;

	sleep(1 + 5*((*p).thread_num % 2));

	(*p).len = strlen((*p).message);

	printf("Thread #%d: %s length=%d\n", (*p).thread_num, (*p).message, (*p).len);

	return NULL;
}

int main(int argc, char *argv[]) {
	pthread_t tid[NUM_THREADS];
	thread_arg_t args[NUM_THREADS];

	char *messages[NUM_THREADS];

	int status = 0;

	messages[0] = "English: Hello World!";
	messages[1] = "French: Bonjour, le monde!";
	messages[2] = "Spanish: Hola al mundo";
	messages[3] = "Klingon: Nuq neH!";
	messages[4] = "German: Guten Tag, Welt!";
	messages[5] = "Russian: Zdravstvuyte, mir!";
	messages[6] = "Japan: Sekai e konnichiwa!";
	messages[7] = "Latin: Orbis, te saluto!";

	int i ;

	for (i = 0; i < NUM_THREADS; i++) {
		args[i].thread_num = i;
		args[i].message = messages[i];

		printf("Creating thread #%d\n", i);

		status = pthread_create(&tid[i], NULL, thread_main, &args[i]);
		assert(status == 0);
	}

	for (i = 0; i < NUM_THREADS; i++) {
		status = pthread_join(tid[i], NULL);
		assert(status == 0);
	}

	pthread_exit(NULL);
}
{% endhighlight %}
