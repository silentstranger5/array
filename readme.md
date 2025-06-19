# Array implementation in C

Dynamic Array implementation in C.

## How to build

```sh
git clone https://github.com/silentstranger5/array
cd array
cmake -B build -S .
cmake --build build
```

## How to use

```c
#include <array.h>
#include <stdio.h>

int main(int argc, char **argv)
{
    array arr;
    int ok = array_init(&arr, sizeof(int));
    if (!ok)
    {
        printf("failed to initialize array: memory allocation failure\n");
        return 1;
    }
    int v = 5;
    ok = array_push(&arr, &v);
    if (!ok)
    {
        printf("failed to push value: memory allocation failure\n");
        return 1;
    }
    int *vptr = array_get(&arr, 0);
    if (!vptr)
    {
        printf("failed to get value from array: index out of bounds\n");
        return 1;
    }
    printf("vptr value: %d\n", *vptr);
    array_free(&arr);
    return 0;
}
```

## About

It is often desirable to have a dynamically allocated contiguous storage that can grow over time.
Implementation for such data structure may be trivial for a specific type, but it quickly becomes 
hard to deal with when multiple types need to be implemented. 
There are many ways to implement such structure, and each of them has their own advantages and disatvantages. 
While macros are powerful and useful tool, using them in this situation can be problematic. 
In some cases, macros require too much configuration, and in other cases, they can be very hard to debug and maintain. 
In any case, macros can be hard to read compared to code. 
This might suggest that macros were not intended to be used as a tool for extensive code generation. 

I use void pointers in my implementation of dynamic arrays. 
This approach has its own problems, of course. 
The primary issue here is absense of the type system support. 
You have to carefully keep track of the data type used in the dynamic array on your own. 
This however, matches closely with semantics of the data structure: we intend to be able to store any type in our container. 
This leads to very simple and succinct code. 
The downside, however, is absense of type system guardrails. 
Burden of the type management lies on the user.
