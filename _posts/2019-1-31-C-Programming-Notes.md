This semester I am a teaching assistant for Systems Programming. This post is just be a collection of useful things I plan to note.

Escape character: in computing an escape character is a character which invokes an alternate interpretation of subsequent character(s). For example the backslash ("\") is used as a marker to tell the compiler/interpreter that the following character has some special meaning. In C, "\n" means new line and "\t" means tab. The word "escape" refers to temporarily escaping out of parsing the text and into another mode where the subsequent character is treated differently.

size of `int` = 4 bytes (int is signed by defaut, so its range is limited because of the sign bit)

size of `long` = 8 bytes

size of `unsigned int` = 4 bytes (unsigned int has increased range because it does not need a sign bit)

size of `float` = 4 bytes (float has smaller range than int, but it provides increased accuracy -- see IEEE 754)

size of `double` = 8 bytes (double provides increased range and accuracy compared to float)

size of `character` = 1 byte

However, it is imporant to note that the size of these data types is dependent on the underlying computer architecture. 

There are four steps in compiling a C program:
1. Pre-processing
2. Compiling
3. Assembling
4. Linking

When compiling, there are various command line arguments you can pass depending on what you wish to do. A few useful arguments are:
- `-Wall`

For example, if you compile with `cc -Wall file.c` then compiler messages will be generated and printed to `stdout`. More [here](https://www.rapidtables.com/code/linux/gcc/gcc-wall.html).
