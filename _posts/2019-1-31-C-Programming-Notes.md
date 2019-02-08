This semester I am a teaching assistant for Systems Programming. This post is a collection of useful notes.

# Building

There are four steps in compiling a C program:
1. Pre-processing
2. Compiling
3. Assembling
4. Linking

When compiling, there are various command line arguments you can pass depending on what you wish to do. A few useful arguments are:
- `-Wall`

For example, if you compile with `cc -Wall file.c` then compiler messages will be generated and printed to `stdout`. More [here](https://www.rapidtables.com/code/linux/gcc/gcc-wall.html).

# Makefile

Sample Makefile:
```
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
```

`$@` is a special macro whose value is the file name of the target.

# Bit Manipulation and Boolean Logic

XOR: One or the other, but not both

OR: One, or the other, or both

# Misc

__Escape character:__ in computing an escape character is a character which invokes an alternate interpretation of subsequent character(s). For example the backslash ("\") is used as a marker to tell the compiler/interpreter that the following character has some special meaning. In C, "\n" means new line and "\t" means tab. The word "escape" refers to temporarily escaping out of parsing the text and into another mode where the subsequent character is treated differently.

| Type | Size | Notes |
| --- | --- | --- |
| `int` | 4 bytes | `int` is signed by defaut, so its range is limited because of the sign bit |
| `long` | 8 bytes |
| `unsigned int` | 4 bytes | `unsigned int` has increased range because it does not need a sign bit |
| `float` | 4 bytes | `float` has smaller range than int, but it provides increased accuracy (see IEEE 754) |
| `double` | 8 bytes | `double` provides increased range and accuracy compared to float |
| `char` | 1 byte |

__Note:__ the size of these data types is dependent on the underlying computer architecture. Since C99, fixed width integers are available. They are defined in the `<stdint.h>` header. More [here](https://en.cppreference.com/w/c/types/integer).
