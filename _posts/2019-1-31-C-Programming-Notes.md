This semester I am a teaching assistant for Systems Programming. This post is a collection of useful notes.

# Building

There are [four steps](https://www.calleerlandsson.com/the-four-stages-of-compiling-a-c-program/) in compiling a C program:
1. Pre-processing
2. Compiling
3. Assembling
4. Linking

When compiling, there are various command line arguments you can pass depending on what you wish to do. A few useful arguments are:
- `-Wall`

For example, if you compile with `cc -Wall file.c` then compiler messages will be generated and printed to `stdout`. More [here](https://www.rapidtables.com/code/linux/gcc/gcc-wall.html).

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

`$@` is a special macro whose value is the file name of the target.

# Bit Manipulation and Boolean Logic

XOR: One or the other, but not both

OR: One, or the other, or both

Information on how to set, clear, and toggle bits can be found [here](https://stackoverflow.com/questions/47981/how-do-you-set-clear-and-toggle-a-single-bit).

# Misc

__Two's Complement__: one of many ways to represent negative integers with bit patterns. To change a bit sequence to two's complement perform the following steps:
1. Perform 1s complement (flip all 1s and 0s)
2. Add 1
3. Result is the value of the original bit sequence `* -1`

More [here](https://chortle.ccsu.edu/AssemblyTutorial/Chapter-08/ass08_17.html).

The maximum value of an unsigned integer is `2^n - 1` and not `2^n` because integers start at 0, but our counting starts at 1. So, `2^32-1` is the maximum value for a 32-bit unsigned integer (32 binary digits). `2^32` is the number of possible values.

__Escape character:__ in computing an escape character is a character which invokes an alternate interpretation of subsequent character(s). For example the backslash ("\") is used as a marker to tell the compiler/interpreter that the following character has some special meaning. In C, "\n" means new line and "\t" means tab. The word "escape" refers to temporarily escaping out of parsing the text and into another mode where the subsequent character is treated differently.

| Type | Size | Notes |
| :---: | :---: | :---: |
| `int` | 4 bytes | `int` is signed by defaut, so its range is limited because of the sign bit |
| `long` | 8 bytes |
| `unsigned int` | 4 bytes | `unsigned int` has increased range because it does not need a sign bit |
| `float` | 4 bytes | `float` has smaller range than int, but it provides increased accuracy (see IEEE 754) |
| `double` | 8 bytes | `double` provides increased range and accuracy compared to float |
| `char` | 1 byte |

__More about data types:__ 

The size of these data types is dependent on the underlying computer architecture ([more here](https://stackoverflow.com/questions/35844586/can-i-assume-the-size-of-long-int-is-always-4-bytes)). Since C99, fixed width integers are available. They are defined in the `<stdint.h>` header. More [here](https://en.cppreference.com/w/c/types/integer) and [here](https://stackoverflow.com/questions/1331821/fixed-width-floating-point-numbers-in-c-c). Fixed width `long` may also be available depending on if the compiler you use meets that part of the standard ([source](https://stackoverflow.com/questions/1331821/fixed-width-floating-point-numbers-in-c-c)).

A common question I hear is: "why is `sizeof(int)` 4 bytes if I have a 64-bit machine?" The answer to this is simple: 64-bit machine can mean many things. Usually, though, this means that the CPU has registers this big. The __size of the data types__, however, __is determined by the compiler__. That said, the size of a pointer on a 64-bit machine has to be 8 bytes (1 byte = 8 bits). The reason for this is that it needs to be able to access the entire main memory address space which is 64 bits ([source](https://stackoverflow.com/questions/10197242/what-should-be-the-sizeofint-on-a-64-bit-machine/10197311)). Remember main memory is actually your random access memory (a.k.a., RAM). If you are wondering how we can use an 8 byte pointer to access volatile memory (e.g., HDD) which expands beyond `2^64` number of bytes read [here](https://superuser.com/questions/487076/why-is-it-so-that-32-bit-is-limited-to-4-gb-ram-but-it-can-easily-support-1-tb-h/487079).

<div class="datatable-begin"></div>

Food    | Description                           | Category | Sample type
------- | ------------------------------------- | -------- | -----------
Apples  | A small, somewhat round ...           | Fruit    | Fuji
Bananas | A long and curved, often-yellow ...   | Fruit    | Snow
Kiwis   | A small, hairy-skinned sweet ...      | Fruit    | Golden
Oranges | A spherical, orange-colored sweet ... | Fruit    | Navel

<div class="datatable-end"></div>
