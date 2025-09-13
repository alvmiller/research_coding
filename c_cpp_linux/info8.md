# C / C++ : Online

- https://www.programiz.com/cpp-programming/online-compiler/
- https://www.onlinegdb.com/online_c++_compiler
- https://cpp.sh/
- https://godbolt.org/
- https://www.codechef.com/c-online-compiler

# C : Examples

# C : Arrays

```c
/******************************************************************************

                            Online C Compiler.
                Code, Compile, Run and Debug C program online.
Write your code in this editor and press "Run" button to compile and execute it.

*******************************************************************************/

#include <stdio.h>

#define N (2)

typedef struct {
    int x;
    int y;
} s0;

static const struct big_arr {
    int a;
    s0 arr0[N];
} b_array[] = {
    {
        .a = 10,
        .arr0 = { { .x = 1, .y = 2 }, { .x = 3, .y = 4 } }
    },
    {
        .a = 20,
        .arr0 = { { .x = 10, .y = 20 }, { .x = 30, .y = 40 } }
    }
};

int main()
{
    printf("Hello World");

    return 0;
}
```




































