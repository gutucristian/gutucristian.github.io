This semester I am a teaching assistant for Systems Programming. This post is a collection of useful notes.

# Building

There are [four steps](https://www.calleerlandsson.com/the-four-stages-of-compiling-a-c-program/) in compiling a C program:
1. Pre-processing
2. Compiling
3. Assembling
4. Linking

When compiling, there are various command line arguments you can pass depending on what you wish to do. A few useful arguments are:
- `-Wall`: prints all warnings to `stdout`. More [here](https://www.rapidtables.com/code/linux/gcc/gcc-wall.html).
- `-std=c99`: specifies which standard to use to compile, in this case `c99`
- `-g`: includes debug information in executable, this helps when using gdb
- `-o`: gives executable a custom name instead of generic `a.out`
- `-S`: saves assembler output (does not link and executable is not produced)
- `-c`: only compiles. The output is an object file with a `.o` extension that contains machine code. **Note** the file is not yet executable as it was not _linked_ yet

# Makefile

Sample Makefile:
{% highlight Makefile linenos %}
  #define more variables so it is easier to make changes
  CC=gcc
  CFLAGS=-g -Wall -std=c99
  TARGETS=fibonacci whileloop seq

  all: $(TARGETS)

  whileloop: whileloop.c
    $(CC) $(CFLAGS) -o $@ $@.c

  fibonacci: fibonacci.c
    $(CC) $(CFLAGS) -o $@ $@.c

  seq: seq.c
    $(CC) $(CFLAGS) -o $@ $@.c

  clean:
    rm -rf *.o *~ $(TARGETS)
{% endhighlight %}

`$@` is a special macro whose value is the file name of the target. More [here](https://stackoverflow.com/questions/3220277/what-do-the-makefile-symbols-and-mean).

# Bit Manipulation and Boolean Logic

XOR: One or the other, but not both

OR: One, or the other, or both

Information on how to set, clear, and toggle bits can be found [here](https://stackoverflow.com/questions/47981/how-do-you-set-clear-and-toggle-a-single-bit).

# Data Types

- size of `int`: 4 bytes (`int` is signed by defaut, so its range is limited because of the sign bit)
- size of `long`: 8 bytes
- size of `unsigned int`: 4 bytes (`unsigned int` has increased range because it does not need a sign bit)
- size of `float`: 4 bytes (`float` has smaller range than int, but it provides increased accuracy -- see IEEE 754)
- size of `double`: 8 bytes (`double` provides increased range and accuracy compared to float)
- size of `char`: 1 byte

The size of these data types is dependent on the underlying computer architecture ([more here](https://stackoverflow.com/questions/35844586/can-i-assume-the-size-of-long-int-is-always-4-bytes)). Since C99, fixed width integers are available. They are defined in the `<stdint.h>` header. More [here](https://en.cppreference.com/w/c/types/integer) and [here](https://stackoverflow.com/questions/1331821/fixed-width-floating-point-numbers-in-c-c). Fixed width `long` may also be available depending on if the compiler you use meets that part of the standard ([source](https://stackoverflow.com/questions/1331821/fixed-width-floating-point-numbers-in-c-c)).

A common question I hear is: "why is `sizeof(int)` 4 bytes if I have a 64-bit machine?" The answer to this is simple: 64-bit machine can mean many things. Usually, though, this means that the CPU has registers this big. The __size of the data types__, however, __is determined by the compiler__. That said, the size of a pointer on a 64-bit machine has to be 8 bytes (1 byte = 8 bits). The reason for this is that it needs to be able to access the entire main memory address space which is 64 bits ([source](https://stackoverflow.com/questions/10197242/what-should-be-the-sizeofint-on-a-64-bit-machine/10197311)). Remember main memory is actually your random access memory (a.k.a., RAM). If you are wondering how we can use an 8 byte pointer to access volatile memory (e.g., HDD) which expands beyond `2^64` number of bytes read [here](https://superuser.com/questions/487076/why-is-it-so-that-32-bit-is-limited-to-4-gb-ram-but-it-can-easily-support-1-tb-h/487079).

# Misc

__Two's Complement__: one of many ways to represent negative integers with bit patterns. To change a bit sequence to two's complement perform the following steps:
1. Perform 1s complement (flip all 1s and 0s)
2. Add 1
3. Result is the value of the original bit sequence `* -1`

More [here](https://chortle.ccsu.edu/AssemblyTutorial/Chapter-08/ass08_17.html).

The maximum value of an unsigned integer is `2^n - 1` and not `2^n` because integers start at 0, but our counting starts at 1. So, `2^32-1` is the maximum value for a 32-bit unsigned integer (32 binary digits). `2^32` is the number of possible values.

__Escape character:__ in computing an escape character is a character which invokes an alternate interpretation of subsequent character(s). For example the backslash ("\") is used as a marker to tell the compiler/interpreter that the following character has some special meaning. In C, "\n" means new line and "\t" means tab. The word "escape" refers to temporarily escaping out of parsing the text and into another mode where the subsequent character is treated differently.

# Exit Code

Upon finishing execution programs must return an "exit code" which is a value between 0 and 255 that indicated to the OS how the execution went. Some status codes are:

- `0` - everything went just peachy
- `1` - Catchall for general errors
- `2` - Misuse of shell builtins (according to Bash documentation)
- `126` - Command invoked cannot execute
- `127` - “command not found”
- `128` - Invalid argument to exit
- `128+n` - Fatal error signal “n”
- `130` - Script terminated by Control-C
- `255\*` - Exit status out of range

To check the status code first run your program and then execute: `echo $?`. More [here](https://shapeshed.com/unix-exit-codes/).

# Real vs. User. vs System time

- **Real** time is wall clock time (time from start to finish). This is all elapsed time including time slices used by other processes (due to CPU time sharing) and time the process spends blocked (for example if it is waiting for I/O to complete).
- **User** time is the amount of CPU time spent in _user-mode code_ (outside the kernel) on behalf of this process. Other processes and time spent blocked are not included towards this figure.
- **System** time is the amount of time spent in the _kernel_ on behalf of this process. This refers to CPU time spent on system calls within the kernel, as opposed to library code, which is still running in user-space.

`usr + sys` time will tell you how much _CPU time_ your process used. Note that this is across all CPUs, so if the process has multiple threads and this process is running on a machine with more than one processor it could exceed the the wall clock time reported by `real`. For instance, the process may take only `16` seconds to run but it could have been using four threads each of which ran `8` seconds in parallel. In this case the user + system time will count the time for each thread (so `8 * 4 = 24` seconds) plus the additional `8` seconds for the main thread. So in total `user + sys` time will be `32` seconds while real time is `16` seconds. Another potential issue could be I/O: if your application spends a good deal of time waiting to receive a file or stream, then obviously the real time would greatly exceed the user/sys time because no CPU time is used while waiting to get access to a file or something similar. Read more [here](https://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1).

# Struct
An array is a continuous structure in memory that allows you to store data of the same type. In C, a **structure** is a user defined data type that allows the combination of data items of different types. 

{% highlight C linenos %}
  struct Book {
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
  };

  int main(void) {
    struct Book book1;
    strcpy(book.title, "Shrek");
    strcpy(book1.author, "Andrew Adamson, Vicky Jenson");
    strcpy(book.subject, "An ogre named Shrek.");
    book1.book_id = 27;
    return 0;
  }
{% endhighlight %}

Structs are commonly used with **typedef** which are used to give a type a new name.

{% highlight C linenos %}
  typedef struct Books {
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
  } book;
{% endhighlight %}

Another example: `typedef unsigned char BYTE;`

A `char` takes up one byte of memory, so we can rename an `unsigned char` to a `BYTE`.
