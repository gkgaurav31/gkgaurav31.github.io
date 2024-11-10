---
layout: post
title: Secure Coding in C and C++ (Part 1)
date: 2024-11-09 15:24 +0530
author: "Gaurav Kumar"
tags: "c c++ theory important"
categories: "clean code"
---

## SECURE CODING IN C AND C++

### SIZEOF(ARRAY)

In C/C++, when you pass an array to a function, it becomes a pointer to the first element. This means that `sizeof(array)` inside the function gives the size of the pointer, not the actual array.

In the example:

```c
void clear(int array[]) {
    for (size_t i = 0; i < sizeof(array) / sizeof(array[0]); ++i) {
        array[i] = 0;
    }
}
```

sizeof(array) / sizeof(array[0]) returns 1 because sizeof(array) gives the size of the pointer, not the whole array, so only the first element is set to zero.

To fix this, pass the array size as a separate parameter:

```c
void clear(int array[], size_t size) {
    for (size_t i = 0; i < size; ++i) {
        array[i] = 0;
    }
}
```

### EXECUTION CHARACTER SET

In C, characters in a string are interpreted based on the _execution character set_, which includes both a basic character set and any extended characters defined by the environment. Here's a breakdown -

#### Basic Character Set

- The basic set includes 26 uppercase letters, 26 lowercase letters, 10 digits, 29 symbols (like `+`, `-`, `*`), space, and control characters (e.g., newline, tab).
- Each basic character is represented in one byte. A byte of all zeros represents the _null character_, used to end strings.

#### Extended Character Set

- C can also support additional characters beyond the basic set, depending on the system's _locale_ settings (configurable with `setlocale()`).
- Locales define language-specific conventions and the set of additional characters available in the program.

#### Multibyte Character Set

- Some characters in the extended set may require multiple bytes to represent, known as _multibyte characters_.
- While basic characters are always single-byte, multibyte characters are locale-dependent and can vary in byte length.
- A multibyte character set may use a _shift state_ encoding, where certain characters change the state and affect the interpretation of following bytes.

### WIDE STRINGS

Wide strings represent characters in a larger character set using _wide characters_, which typically use 16 or 32 bits. This allows for a broader range of symbols than ordinary 8-bit characters.

## UNBOUNDED STRING COPIES

Copying data from an unbounded source to a fixed-length array can cause buffer overflows if the input exceeds the array's size.

Example

```c
#include <stdio.h>
#include <stdlib.h>

void get_y_or_n(void) {
    char response[8];
    puts("Continue? [y] n: ");
    gets(response); // Unsafe: no input size control
    if (response[0] == 'n')
        exit(0);
}
```

If more than 8 characters are entered, this causes undefined behavior since `gets()` doesn't limit input size.

Here's the implementation of `gets()` method in C:

```c
char *gets(char *dest) {
    int c = getchar();
    char *p = dest;
    while (c != EOF && c != '\n') {
        *p++ = c;
        c = getchar();
    }
    *p = '\0';
    return dest;
}
```

Safer Alternative:
Use `fgets()` to specify a max input length:

```c
fgets(char *str, int n, FILE *stream)
```

- char \*str: a pointer to an array of characters where the read values will be stored

- int n: maximum number of characters to be read

- FILE \*stream: pointer to the file stream from where the input is to be taken (it can be replaced with stdin when taking input from the standard input ie through the console)

[fgets() by Scaler](https://www.scaler.com/topics/fgets-function-c/)

## COPYING AND CONCATENATING STRINGS

Errors can occur when copying strings with functions like `strcpy()` or `sprintf()`, as they don't limit the data copied.

Command-line arguments are stored as null-terminated strings in `argv`:

```c
int main(int argc, char *argv[]) {
    /* ... */
}
```

argv[0] contains the program name by convention, and argv[1] to argv[argc-1] are the actual arguments.

Copying an argument into a fixed-size buffer without checking length can cause vulnerabilities. For example:

```c
int main(int argc, char *argv[]) {
    char prog_name[128];
    strcpy(prog_name, argv[0]); // Risky if argv[0] is too large
}
```

To safely handle string copying, use dynamic allocation:

```c
int main(int argc, char *argv[]) {

    // Check if argv[0] is not null; if it is, use an empty string instead
    const char *name = argv[0] ? argv[0] : "";

    // Dynamically allocate memory for prog_name with enough space for name and null terminator
    char *prog_name = (char *)malloc(strlen(name) + 1);

    // Verify that memory allocation was successful
    if (prog_name) {
        // Copy the content of name into prog_name safely, now that it's properly sized
        strcpy(prog_name, name);
    } else {
        // Handle the case where memory allocation failed
        /* Handle allocation failure */
    }
}
```

The `strdup()` function can be used to copy a string. It takes a pointer to a string and returns a pointer to a newly allocated duplicate string. The memory allocated for the duplicate string can be freed by passing the returned pointer to `free()`.

### STRLEN vs SIZEOF

`strlen` measures the actual length of the string (excluding null).
`sizeof` measures the memory allocated for the variable, including null terminator.

```C
#include <stdio.h>

int main() {
    char str[] = "Hello";  // String with 5 characters + 1 for null terminator

    printf("strlen: %lu\n", strlen(str));  // Output: 5 (Length of the string)
    printf("sizeof: %lu\n", sizeof(str));  // Output: 6 (Size in bytes, including null terminator)

    return 0;
}
```

```sql
str[] = "Hello"
Memory Layout (each block is 1 byte):
+---+---+---+---+---+---+
| H | e | l | l | o | \0 |
+---+---+---+---+---+---+

- `strlen("Hello")` returns 5.
- `sizeof(str)` returns 6 (because of the null terminator).
```

### MALLOC()

- Takes an unsigned 32-bit integer as its only argument
- Returns a pointer to void in anticipation of a cast

`int _ptr = (int _) malloc(n \* sizeof(int));`:  
Allocates memory dynamically for n integers (40 bytes on most systems where int is 4 bytes).

The malloc function returns a pointer to the allocated memory. If memory allocation is successful, ptr points to the allocated memory block; otherwise, ptr is NULL.

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main()
{

    int n = 10;
    int *ptr = (int *) malloc(n * sizeof(int));

    if(ptr != NULL){

        for(int i=0; i<n; i++){
            ptr[i] = i + 1;
        }

        for(int i=0; i<n; i++){
            printf("%d ", ptr[i]);
        }

        free(ptr);

    }else{
        puts("Memory allocation failed.");
    }

    printf("\n\n--------------------END--------------------\n\n");

    return 0;
}
```

### CALLOC()

- Takes two unsigned 32 bit integers as arguments
- Allocates and then initializes memory block to 0
- Like malloc, returns a pointer to void in anticipation of a cast
- Slower than malloc

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main()
{

    int n = 10;
    int *ptr = (int *) calloc(n, sizeof(int));

    if(ptr != NULL){

        for(int i=0; i<n; i++){
            ptr[i] = i+1;
        }

        for(int i=0; i<n; i++){
            printf("%d ", ptr[i]);
        }

        free(ptr);

    }else{
        puts("Memory allocation failed.");
    }

    printf("\n\n--------------------END--------------------\n\n");

    return 0;
}
```

### REALLOC()

- Takes a generic pointer and an unsigned, 32 bit integer as an argument
- Returns a pointer to void in anticipation of a cast

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main()
{
    // Allocated Memory of Size 10 using calloc()
    int n = 10;
    int *ptr = (int *)calloc(n, sizeof(int));

    if(ptr != NULL){

        // Assign values to the allocated memory
        for(int i=0; i<n; i++){
            ptr[i] = i+1;
        }

        // Print allocated memory values
        for(int i=0; i<n; i++){
            printf("%d ", ptr[i]);
        }

        // free(ptr);

    }else{
        puts("Memory allocation failed.");
    }

    printf("\n\n--------------------REALLOCATION TEST--------------------\n\n");

    // Double the allocated size using realloc()
    ptr = (int *)realloc(ptr, sizeof(int) * (n * 2));

    if(ptr != NULL){

        // Assign new values to the expanded memory
        for(int i=n; i<n*2; i++){
            ptr[i] = i+1;
        }

        // Print new expanded array
        for(int i=0; i<n*2; i++){
            printf("%d ", ptr[i]);
        }

        free(ptr);

    }else{
        puts("Reallocation of memory failed.");
    }

    printf("\n\n--------------------END--------------------\n\n");

    return 0;
}
```

- We can reduce the size as well. For example:
  `ptr = (int *) realloc(ptr, sizeof(int) * (n / 2));`
  After resizing to a smaller size, any data beyond the new allocated size (in this case, beyond n/2 elements) will be lost.
- Always assign the result of `realloc()` to the original pointer (`ptr` in this case). This is important because realloc() can return a new memory address, and you would lose the reference to the old memory if you didn't reassign the pointer.
- Always check if `realloc()` returns `NULL`. If it does, it means the memory reallocation failed, and the original memory remains valid.

### FREE()

Deallocate memory that was previously allocated dynamically using functions like `malloc()`, `calloc()`, or `realloc()`.

```C
void free(void *ptr);
```

After freeing, the pointer becomes a dangling pointer if not set to `NULL`, meaning it still holds the memory address of the deallocated memory. Accessing or dereferencing this pointer after freeing memory is undefined behavior.
To prevent this, it's a good practice to set the pointer to `NULL` after freeing memory.

### COMMON PITFALLS

```C
#include <stdio.h>
#include <stdlib.h>

// Structure to represent a person
typedef struct Person {
    int age;
    char* name;
} Person;

int main(int argc, char* argv[]) {

    // #1. Dangling Pointers
    Person *person1 = (Person*)malloc(sizeof(Person));

    free(person1);  // Free the memory

    // person1 is now a dangling pointer (no longer valid).
    person1 = NULL;  // Set to NULL to avoid accidental use

    // #2. Double Free Error
    Person *person2 = (Person*)malloc(sizeof(Person));
    free(person2);

    // Double free: free the same memory block again, which causes undefined behavior
    free(person2);

    // #3. Forgetting to use "sizeof"
    int *int_ptr = malloc(sizeof(int));  // Correct allocation using sizeof
    free(int_ptr);

    // #4. Dereferencing a NULL Pointer
    char *name = malloc(sizeof(char));
    if (name != NULL) {
        *name = 'A';  // Safe to dereference
        free(name);
    }

    // Example of incorrect code:
    // char *name = NULL;
    // *name = 'A';  // ERROR: Dereferencing a NULL pointer

    // Another example:
    // char *name = (char *) malloc(100000000);
    // NULL pointer can be returned from malloc, calloc or realloc
    // *name = 'A';
    // We could be deferencing an invalid pointer here

    printf("\n\n--------------------END--------------------\n\n");

    return 0;
}
```
