Creating and passing data to threads.

Use `pthread_create` to create threads and pass data using a `struct`:

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
