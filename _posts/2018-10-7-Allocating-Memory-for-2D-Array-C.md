 Allocate memory (in heap) for 2D array in C using pointers.
 
 ```c
 29 int main(void) {
 30   int **rows; // a pointer whose value is the address of another pointer
 31   int m = 2;
 32   int n = 2;
 33   // set the value of 'rows' to point to a block of memory which holds
 34   // enough space for 'm' int pointers
 35   rows = malloc(sizeof(int *) * m);
 36   for (int i = 0; i < m; i++) {
 37     // set the first (second, third, ..) pointer to point to a block of
 38     // memory which holds enough space for n ints
 39     rows[i] = malloc(sizeof(int)*n);
 40   }
 41   
 42   return 0;
 43 }
 
```
