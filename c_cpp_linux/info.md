- https://www.markdownguide.org/basic-syntax/
- https://markdownlivepreview.com/
- https://www.markdownguide.org/extended-syntax/

# memset_s

- https://en.cppreference.com/w/c/string/byte/memset.html
- https://man.freebsd.org/cgi/man.cgi?query=memset_s&sektion=3

> memset_s() is	not removed through Dead Store	Elimination  (DSE),  making  it useful for clearing sensitive	data.

* https://www.onlinegdb.com/online_c_compiler
* https://godbolt.org/

```c
// -std=gnu11

#include <stdio.h>
#include <stdlib.h>
#define __STDC_WANT_LIB_EXT1__ 1
#include <string.h>

int main()
{
    char str[] = "bbbbbbbb";
    puts(str);
    
#ifdef __STDC_LIB_EXT1__
    int r = memset_s(str, sizeof str, 'a', 5);
    (void)r;
    printf("%s\n", str);
#endif

    return 0;
}
```

# Restrict Pointer

- https://en.wikipedia.org/wiki/Restrict

> By adding this type qualifier, a programmer hints to the compiler that for the lifetime of the pointer,
> no other pointer will be used to access the object to which it points.
> This allows the compiler to make optimizations (for example, vectorization) that would not otherwise have been possible.

```c
void updatePtrs(size_t* restrict ptrA, size_t* restrict ptrB, size_t* restrict val);
```
