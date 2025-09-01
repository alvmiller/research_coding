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

# Built-in Functions to Perform Arithmetic with Overflow Checking

- https://gcc.gnu.org/onlinedocs/gcc/Integer-Overflow-Builtins.html

> The following built-in functions allow performing simple arithmetic operations together
> with checking whether the operations overflowed.
>
> These built-in functions promote the first two operands into infinite precision signed type
> and perform addition on those promoted operands.
> The result is then cast to the type the third pointer argument points to and stored there.
> If the stored result is equal to the infinite precision result, the built-in functions return false,
> otherwise they return true. As the addition is performed in infinite signed precision,
> these built-in functions have fully defined behavior for all argument values.

```c
bool __builtin_add_overflow (type1 a, type2 b, type3 *res)
```

# Auto cleanup

- https://omeranson.github.io/blog/2022/06/12/cleanup-attribute-in-C
- https://gcc.gnu.org/onlinedocs/gcc/Common-Variable-Attributes.html
- https://echorand.me/site/notes/articles/c_cleanup/cleanup_attribute_c.html

> gcc and clang support the cleanup attribute. The attribute is applied to variables.
> It instructs the compiler to run a function when the variable goes out of scope.
> The function is called with a pointer to the variable.

```c
void clean_up(int *final_value)
{
  printf("Cleaning up\n");
  printf("Final value: %d\n", *final_value);
}

int main(int argc, char **argv)
{ 
  int avar __attribute__ ((__cleanup__(clean_up))) = 1;
  avar = 5;
  return 0;
}
```

# Array size

- https://zubplot.blogspot.com/2015/01/gcc-is-wonderful-better-arraysize-macro.html
- https://www.w3schools.com/c/c_arrays_size.php

```c
#define BUILD_BUG_ON_ZERO(e) \
    (sizeof(struct { int:-!!(e)*1234; }))
#define SAME_TYPE(a, b) \
     __builtin_types_compatible_p(typeof(a), typeof(b))
 #define MUST_BE_ARRAY(a) \
     BUILD_BUG_ON_ZERO(SAME_TYPE((a), &(*a)))

 #define ARRAY_SIZE(a) ( \
     (sizeof(a) / sizeof(*a)) \
     + MUST_BE_ARRAY(a))
#endif
```

# MAX() and MIN()

- https://lwn.net/Articles/983965/
- https://gcc.gnu.org/onlinedocs/gcc-3.2.3/gcc/Min-and-Max.html
- https://stackoverflow.com/questions/3437404/min-and-max-in-c

```c
#define MIN(x,y) ({ \
	const typeof(x) _x = (x);	\
	const typeof(y) _y = (y);	\
	(void) (&_x == &_y);		\
	_x < _y ? _x : _y; })
```
