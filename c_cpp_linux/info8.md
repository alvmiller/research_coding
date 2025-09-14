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

# C : Return Array from Function

- https://stackoverflow.com/questions/11656532/returning-an-array-using-c
- https://www.geeksforgeeks.org/c/return-an-array-in-c/#return-an-array-in-c-using-structures
- https://www.tutorialspoint.com/cprogramming/c_return_arrays_from_function.htm
- https://www.scaler.com/topics/c-return-array/

```c
#include <stdio.h>
#include <stdlib.h>

struct sarr {
    char a[3];
};
struct sarr get_arr0()
{
    struct sarr arr = {};
    arr.a[0] = 42;
    return arr;
}

char *get_arr1()
{
    static char arr[3] = {42,1,1};
    return arr;
}

char *get_arr2()
{
    char *arr = malloc(3);
    arr[0] = 42;
    return arr;
}

char *get_arr3(unsigned *val, char *arr)
{
    if (val == NULL) return NULL;
    if (arr == NULL && *val == 0) {
        *val = 3;
        return NULL;
    }
    if (arr != NULL && val == NULL) return NULL;
    
    arr[0] = 42;
    return arr;
}

int main()
{
    struct sarr arr0 = get_arr0();
    printf("get_arr0 = %d\n", (int)arr0.a[0]);
    
    char *arr1 = get_arr1();
    printf("get_arr1 = %d\n", (int)arr1[0]);
    
    char *arr2 = get_arr2();
    printf("get_arr2 = %d\n", (int)arr2[0]);
    free(arr2);
    
    char *arr3 = NULL;
    unsigned val = 0;
    (void)get_arr3(&val, NULL);
    arr3 = malloc(val);
    arr3 = get_arr3(&val, arr3);
    printf("get_arr3 = %d\n", (int)arr3[0]);
    free(arr3);

    return 0;
}
```


































