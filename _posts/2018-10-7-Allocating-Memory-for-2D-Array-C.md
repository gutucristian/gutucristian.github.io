 Allocate memory (in heap) for 2D array in C using pointers.
 
 ```c
int main(void) {
 int **rows; // a pointer whose value is the address of another pointer
 int m = 2;
 int n = 2;
 // set the value of 'rows' to point to a block of memory which holds
 // enough space for 'm' int pointers
 rows = malloc(sizeof(int *) * m);
 for (int i = 0; i < m; i++) {
 // set the first (second, third, ..) pointer to point to a block of
 // memory which holds enough space for n ints
  rows[i] = malloc(sizeof(int)*n);
 }

 return 0;
}

```
