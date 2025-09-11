# C++ : Virtual Destructor

- https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88046682

- https://www.geeksforgeeks.org/cpp/virtual-destructor/

```c++
#include <iostream>

using namespace std;

class base {
  public:
    base()     
    { cout << "Constructing base\n"; }
    virtual ~base()
    { cout << "Destructing base\n"; }     
};

class derived : public base {
  public:
    derived()     
    { cout << "Constructing derived\n"; }
    ~derived()
    { cout << "Destructing derived\n"; }
};

int main()
{
  derived *d = new derived();  
  base *b = d;
  delete b;
  getchar();
  return 0;
}

/*
Constructing base
Constructing derived
Destructing derived
Destructing base
*/
```

# C++ : delete , delete[] , new , new[]

- https://www.geeksforgeeks.org/cpp/delete-in-c/
- https://www.cpp-junkie.com/2020/04/c-delete-vs-delete.html
- https://learn.microsoft.com/en-us/cpp/cpp/delete-operator-cpp?view=msvc-170
- https://en.cppreference.com/w/cpp/memory/new/operator_delete.html
- https://www.ibm.com/docs/en/i/7.4.0?topic=operators-delete-expressions-c-only
- https://www.geeksforgeeks.org/cpp/delete-and-free-in-cpp/
- https://learn.microsoft.com/en-us/cpp/cpp/delete-operator-cpp?view=msvc-170
- https://www.geeksforgeeks.org/cpp/deletion-of-array-of-objects-in-c/

- https://learn.microsoft.com/en-us/cpp/cpp/new-operator-cpp?view=msvc-170
- https://cplusplus.com/reference/new/operator%20new[]/
- https://www.geeksforgeeks.org/cpp/new-and-delete-operators-in-cpp-for-dynamic-memory/
- https://www.geeksforgeeks.org/cpp/placement-new-operator-cpp/
- https://medium.com/@ajay.patel305/placement-new-c-b7f1e2e2077a
- https://en.cppreference.com/w/cpp/language/new.html
- https://en.cppreference.com/w/cpp/memory/new/operator_new.html
- https://cplusplus.com/reference/new/operator%20new/

```c++
// throwing (1)	
void* operator new[] (std::size_t size);
// nothrow (2)	
void* operator new[] (std::size_t size, const std::nothrow_t& nothrow_value) noexcept;
// placement (3)	
void* operator new[] (std::size_t size, void* ptr) noexcept;
```

```c++
char (*pchar)[10] = new char[dim][10];
delete [] pchar;
```

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    // Allocate Heap memory
    int* array = new int[10];

    // Deallocate Heap memory
    delete[] array;

    return 0;
}
```

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    // Creating int pointer
    int* ptr1 = new int;

    // Initializing pointer with value 20
    int* ptr2 = new int(20);

    cout << "Value of ptr1 = " << *ptr1 << "\n";
    cout << "Value of ptr2 = " << *ptr2 << "\n";

    // Destroying ptr1
    delete ptr1;
    // Destroying ptr2
    delete ptr2;

    return 0;
}
```

```c++
class A {
public:
  virtual ~A() = default;
  virtual int *foo() = 0;
};
```

# C : Zero Array Size in Sructure

- https://gcc.gnu.org/onlinedocs/gcc/Zero-Length.html

> The preferred mechanism to declare variable-length types like struct line above is the ISO C99 flexible array member,
> with slightly different syntax and semantics:
> - Flexible array members are written as contents[] without the 0.
> - Flexible array members have incomplete type, and so the sizeof operator may not be applied.
As a quirk of the original implementation of zero-length arrays, sizeof evaluates to zero.
> - Flexible array members may only appear as the last member of a struct that is otherwise non-empty.
> - A structure containing a flexible array member, or a union containing such a structure (possibly recursively),
may not be a member of a structure or an element of an array. (However, these uses are permitted by GCC as extensions, see details below.)

```c
struct f1 {
  int x; int y[];
} f1 = { 1, { 2, 3, 4 } };

////////

struct line {
  int length;
  char contents[0];
};

struct line *thisline = (struct line *)
  malloc (sizeof (struct line) + this_length);
thisline->length = this_length;
```

# C : Auto Integer Promotions

- https://www.geeksforgeeks.org/c/integer-promotions-in-c/

```c
#include <stdio.h>

int main()
{
    unsigned char a = 255;
    unsigned char b = 2;
    unsigned val = a + b;
    printf("val = %u\n", val);

    return 0;
}
// 257
```

```c
#include <stdio.h> 
int main()
{
    char a = 30, b = 40, c = 10;
    char d = (a * b) / c;
    printf ("%d ", d); 
    return 0;
}
// 120
```

```c
#include <stdio.h>

int main()
{
    char a = 0xfb;
    unsigned char b = 0xfb;

    printf("a = %c", a);
    printf("\nb = %c", b);

    if (a == b)
      printf("\nSame");
    else
      printf("\nNot Same");
    return 0;
}
/*
a = ?
b = ?
Not Same
*/
```

# C : Overflow

- https://en.wikipedia.org/wiki/Integer_overflow

> In C, unsigned integer overflow is defined to wrap around, while signed integer overflow causes undefined behavior.

# C | C++ : sequence point , order of evaluation

- https://en.wikipedia.org/wiki/Sequence_point
- https://learn.microsoft.com/en-us/cpp/c-language/c-sequence-points?view=msvc-170
- https://www.geeksforgeeks.org/c/sequence-points-in-c-set-1/
- https://www.gnu.org/software/c-intro-and-ref/manual/html_node/Sequence-Points.html
- https://www.linkedin.com/pulse/sequence-points-deep-dive-inner-workings-c-ali-el-bana-h5x3f
- https://en.cppreference.com/w/cpp/language/eval_order.html
- https://publications.gbdirect.co.uk/c_book/chapter8/sequence_points.html
- https://www.geeksforgeeks.org/cpp/how-do-sequence-points-relate-to-undefined-behaviour-in-cpp/
- https://www.naukri.com/code360/library/sequence-points-in-c

> To illustrate how sequence points affect the order of evaluation and the behavior of expressions, let us look at some examples:
> - f() && g(): The logical AND operator (&&) introduces a sequence point between its operands.
This means that f() is evaluated first, and its side effects are completed before g() is evaluated.
Moreover, if f() returns false, then g() is not evaluated at all (short-circuiting).
> - (f(), g()): The comma operator (,) introduces a sequence point between its operands.
This means that f() is evaluated first, and its side effects are completed before g() is evaluated.
However, unlike the logical AND operator, the comma operator always evaluates both operands, regardless of their values.
> - i = ++i + 1: The prefix increment operator (++) introduces a sequence point between its operand and the result.
This means that i is incremented first, and its side effects are completed before the addition is performed.
The result is well-defined and equal to i + 2.
> - i = i++ + 1: The postfix increment operator (++) does not introduce a sequence point between its operand and the result.
This means that i is modified twice in the same expression, without a sequence point between them.
The order of evaluation is unsequenced, and the result is undefined.

```c++
i = ++i + 2;       // well-defined
i = i++ + 2;       // undefined behavior until C++17
f(i = -2, i = -2); // undefined behavior until C++17
f(++i, ++i);       // undefined behavior until C++17, unspecified after C++17
i = ++i + i++;     // undefined behavior

cout << i << i++; // undefined behavior until C++17
a[i] = i++;       // undefined behavior until C++17
n = ++i + i;      // undefined behavior

union U { int x, y; } u;
(u.x = 1, 0) + (u.y = 2, 0); // undefined behavior

//////

i = ++i + i++;     // undefined behavior
i = i++ + 1;       // undefined behavior
i = ++i + 1;       // undefined behavior
++ ++i;            // undefined behavior
f(++i, ++i);       // undefined behavior
f(i = -1, i = -1); // undefined behavior

cout << i << i++; // undefined behavior
a[i] = i++;       // undefined behavior
```

# C++ : Int to Float

- https://www.wscubetech.com/resources/c-programming/programs/int-to-float
- https://stackoverflow.com/questions/28010565/why-does-c-promote-an-int-to-a-float-when-a-float-cannot-represent-all-int-val
- https://www.geeksforgeeks.org/dsa/convert-int-to-float/
- https://forums.unrealengine.com/t/c-convert-int-to-float/431242
- https://www.geeksforgeeks.org/cpp/how-to-convert-float-to-int-in-cpp/
- https://en.cppreference.com/w/cpp/numeric/bit_cast.html
- https://cppreference-45864d.gitlab-pages.liu.se/en/cpp/numeric/bit_cast.html
- https://medium.com/@simontoth/daily-bit-e-of-c-std-bit-cast-4268f5109db7
- https://www.w3computing.com/articles/cpp-20-std-bit_cast/

```c++
#include <stdio.h>

int main() {
    int num = 8;
    float result;

    result = (float)num;

    printf("Integer value: %d\n", num);
    printf("After conversion to float: %.2f\n", result);

    return 0;
}
```

```c++
#include <iostream>
using namespace std;

int main() {
    int intValue = 10;
    float floatValue = static_cast<float>(intValue); // Using static_cast<>
    cout << "Integer value: " << intValue << endl;
    cout << "Float value: " << floatValue << endl;
    return 0;
}
```

```c++
// !!!

#include <iostream>
#include <cstdint>
#include <cstring>
#include <bitset>

float int_to_float(int i) {
    float f = 0.0;
    static_assert(sizeof(i) == sizeof(f));
    std::memcpy(&f, &i, sizeof(f));
    return f;
}

int main()
{
    int i = 10;
    float f = int_to_float(i);
    std::bitset<20> af(f);
    std::bitset<20> ai(i);
    std::cout << af << std::endl;
    std::cout << ai << std::endl;

    return 0;
}
/*
00000000000000000000
00000000000000001010
*/
```

```c++
// !!!

#include <iostream>
#include <cstdint>
#include <cstring>
#include <bitset>

float int_to_float(int i) {
    float f = 0.0;
    static_assert(sizeof(i) == sizeof(f));
    f = std::bit_cast<float>(i);
    return f;
}

int main()
{
    int i = 10;
    float f = int_to_float(i);
    std::bitset<20> af(f);
    std::bitset<20> ai(i);
    std::cout << af << std::endl;
    std::cout << ai << std::endl;

    return 0;
}
/*
00000000000000000000
00000000000000001010
*/
```









