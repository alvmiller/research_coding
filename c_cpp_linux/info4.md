# C++ : std::find_if() and std::count_if()

- https://en.cppreference.com/w/cpp/algorithm/count.html
- https://en.cppreference.com/w/cpp/algorithm/find.html
- https://www.fluentcpp.com/2017/01/16/how-to-stdfind-something-efficiently-with-the-stl/
- https://cplusplus.com/reference/algorithm/find_if/
- https://www.cppstories.com/2022/ranges-alg-part-one/
- https://www.learncpp.com/cpp-tutorial/introduction-to-standard-library-algorithms/

```c++
template< class ExecutionPolicy, class ForwardIt, class UnaryPred >
ForwardIt find_if( ExecutionPolicy&& policy,
                   ForwardIt first, ForwardIt last, UnaryPred p );
```

> Parameters
> - first, last	-	the pair of iterators defining the range of elements to examine
> - value	-	value to compare the elements to
> -policy	-	the execution policy to use
> - p	-	unary predicate which returns ​true for the required element.
> The expression p(v) must be convertible to bool for every argument v of type (possibly const) VT,
> where VT is the value type of InputIt, regardless of value category, and must not modify v.
> Thus, a parameter type of VT&is not allowed, nor is VT unless for VT a move is equivalent to a copy(since C++11).​
>
> q	-	unary predicate which returns ​false for the required element.
> The expression q(v) must be convertible to bool for every argument v of type (possibly const) VT,
> where VT is the value type of InputIt, regardless of value category, and must not modify v.
> Thus, a parameter type of VT&is not allowed, nor is VT unless for VT a move is equivalent to a copy(since C++11).​

```c++
#include <algorithm>
#include <array>
#include <cassert>
#include <complex>
#include <iostream>
#include <iterator>
 
int main()
{
    constexpr std::array v{1, 2, 3, 4, 4, 3, 7, 8, 9, 10};
    std::cout << "v: ";
    std::copy(v.cbegin(), v.cend(), std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
 
    // Determine how many integers match a target value.
    for (const int target : {3, 4, 5}) {
        const int num_items = std::count(v.cbegin(), v.cend(), target);
        std::cout << "number: " << target << ", count: " << num_items << '\n';
    }
 
    // Use a lambda expression to count elements divisible by 4.
    int count_div4 = std::count_if(v.begin(), v.end(), [](int i) { return i % 4 == 0; });
    std::cout << "numbers divisible by four: " << count_div4 << '\n';
 
    // A simplified version of `distance` with O(N) complexity:
    auto distance = [](auto first, auto last) {
        return std::count_if(first, last, [](auto) { return true; });
    };
    static_assert(distance(v.begin(), v.end()) == 10);
 
    std::array<std::complex<double>, 3> nums{{{4, 2}, {1, 3}, {4, 2}}};
    #ifdef __cpp_lib_algorithm_default_value_type
        // T gets deduced making list-initialization possible
        auto c = std::count(nums.cbegin(), nums.cend(), {4, 2});
    #else
        auto c = std::count(nums.cbegin(), nums.cend(), std::complex<double>{4, 2});
    #endif
    assert(c == 2);
}
```

# C++ : std::sort() vs std::stable_sort()

- https://www.geeksforgeeks.org/cpp/stable_sort-c-stl/
- https://en.cppreference.com/w/cpp/algorithm/stable_sort.html
- https://cplusplus.com/reference/algorithm/stable_sort/

- https://www.geeksforgeeks.org/dsa/stable-and-unstable-sorting-algorithms/
- https://medium.com/@rishi.barve0409/importance-of-stable-sorting-5a7a04955690

> stable_sort() is used to sort the elements in the range [first, last) in ascending order.
> It is like std::sort, but stable_sort() keeps the relative order of elements with equivalent values.
> It comes under the <algorithm> header file.

>  Stable sort doesn’t matter in case of an array of numbers. The actual use of stable sorting comes when there is some more information stored in the array along with the number in the array. Like in the figure above, you have numbers on the balls along with the colour. So if you have an array of balls and you apply a non stable sorting algorithm on that array, the result will be same in terms of numbers but the colour sequence might change.
>
> This concept becomes very important in case an array needs to be sorted first on some comparator then on another comparator. For example you have a class Car with parameters top_speed and price. And lets say you want to arrange the cars on the basis of price. And for all the cars having same price on the basis of top_speed. Here you can sort the cars first on top_speed and then on price. However, in second sort(on price) you will have to use stable sorting, else the output of first sort will have no effect on final answer. Similar algorithm is used when we use 2 or more columns in “order by” clause in SQL queries.

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int arr[] = { 1, 5, 8, 9, 6, 7, 3, 4, 2, 0 };
    int n = sizeof(arr) / sizeof(arr[0]);

    stable_sort(arr, arr + n);

    cout << "Array after sorting using "
            "stable_sort is : \n";
    for (int i = 0; i < n; ++i)
        cout << arr[i] << " ";

    return 0;
}
```

# TPM , eSE , HSM , RPMB , TEE

- https://goteleport.com/blog/tpm-vs-hsm-difference/
- https://www.ssl2buy.com/wiki/tpm-vs-hsm
- https://sidechainsecurity.com/what-is-the-difference-between-tpm-and-hsm-security/
- https://cheapsslweb.com/blog/what-is-the-difference-between-tpm-vs-hsm/
- https://sslinsights.com/tpm-vs-hsm/
- https://www.wolfssl.com/difference-hsm-tpm-secure-enclave-secure-element-hardware-root-trust/
- https://www.tropicsquare.com/blogs/hardware-security-understanding-the-differences-between-a-secure-element-tpm-hsm-and-a-tee

- https://www.sdcard.org/developers/boot-and-new-security-features/replay-protected-memory-block/
- https://sergioprado.blog/rpmb-a-secret-place-inside-the-emmc/
- https://www.sciencedirect.com/science/article/pii/S2666281723002019
- https://docs.nvidia.com/jetson/archives/r36.4.3/DeveloperGuide/SD/Security/FSKP.html

> Hardware Security Module (HSM)
> HSM is perhaps the most overloaded term in the hardware security space. The term HSM is sometimes used to refer to any type of discrete hardware security device including a Secure Element or TPM. This is particularly true in the automotive market where Secure Elements are often referred to as a HSM.
> In addition to being used as a generic term, an HSM also refers to large (and often expensive) hardware security appliances primarily used in data center operations. Context is therefore key to understanding how the term HSM is used.
> 
> Secure Element (SE)
> The term Secure Element generally refers to discrete hardware security chips that are built into larger devices. The functionality of a Secure Element varies based on the use-case, with prices ranging from sub-one-dollar to several dollars.
>
> Trusted Platform Module (TPM)
> A TPM is a specific type of secure element that is implemented in accordance with the TPM standard developed by the Trusted Computing Group (TCG). The functionality of a TPM is standardized, with prices ranging from slightly under one dollar to a few dollars, depending upon the manufacturer and volume.
>
> Replay Protected Memory Block (RPMB)
RPMB is introduced to store data in an authenticated memory area for the purpose of protecting data from a replay attack or avoiding unexpected data updates (makes it possible to store and retrieve data with integrity and authenticity support).

![Image!](https://cdn.prod.website-files.com/67a8fa554bf28c588cb0f98f/687f4a6a9070078c9f0e4636_Table.png "Image")
![Image!](https://www.sdcard.org/cms/wp-content/uploads/2022/08/image12.png "Image")
![Image!](https://www.sdcard.org/cms/wp-content/uploads/2022/04/image8.png "Image")
![Image!](https://sergioprado.blog/images/202305240-rpmb-partition.png "Image")
![Image!](https://sergioprado.blog/images/202305240-rpmb-write.png "Image")

# C++ : RAII

- https://en.cppreference.com/w/cpp/language/raii.html
- https://medium.com/@gealleh/the-importance-of-practicing-raii-in-c-87011f3e3931
- https://www.tomdalling.com/blog/software-design/resource-acquisition-is-initialisation-raii-explained/
- https://www.geeksforgeeks.org/cpp/resource-acquisition-is-initialization/
- https://medium.com/@abanoubharby/raii-295ff1a56bf1
- https://medium.com/swlh/what-is-raii-e016d00269f9

> Resource Acquisition Is Initialization or RAII, is a C++ programming technique which binds the life cycle of a resource that must be acquired before use (allocated heap memory, thread of execution, open socket, open file, locked mutex, disk space, database connection—anything that exists in limited supply) to the lifetime of an object.
> 
> RAII guarantees that the resource is available to any function that may access the object (resource availability is a class invariant, eliminating redundant runtime tests). It also guarantees that all resources are released when the lifetime of their controlling object ends, in reverse order of acquisition. Likewise, if resource acquisition fails (the constructor exits with an exception), all resources acquired by every fully-constructed member and base subobject are released in reverse order of initialization. This leverages the core language features (object lifetime, scope exit, order of initialization and stack unwinding) to eliminate resource leaks and guarantee exception safety. Another name for this technique is Scope-Bound Resource Management (SBRM), after the basic use case where the lifetime of an RAII object ends due to scope exit.

```c++
std::mutex m;
 
void bad() 
{
    m.lock();             // acquire the mutex
    f();                  // if f() throws an exception, the mutex is never released
    if (!everything_ok())
        return;           // early return, the mutex is never released
    m.unlock();           // if bad() reaches this statement, the mutex is released
}
 
void good()
{
    std::lock_guard<std::mutex> lk(m); // RAII class: mutex acquisition is initialization
    f();                               // if f() throws an exception, the mutex is released
    if (!everything_ok())
        return;                        // early return, the mutex is released
}
```

![Image!](https://miro.medium.com/v2/resize:fit:1400/1*sMhY9dDDYP0FRisYTkqPcw.png "Image")

# C : XOR swap

- https://en.wikipedia.org/wiki/XOR_swap_algorithm
- https://medium.com/@jalal92/the-magic-of-xor-swapping-in-c-4ed944f2ff0a
- https://www.geeksforgeeks.org/cpp/swap-two-variables-using-xor/
- https://betterexplained.com/articles/swap-two-variables-using-xor/

![Image!](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8f/XOR_Swap.svg/960px-XOR_Swap.svg.png")

```c
void xor_swap(int *x, int *y) 
{
  if (x == y) return;
  *x ^= *y;
  *y ^= *x;
  *x ^= *y;
}

#define XORSWAP(a, b)                                                         \
  ((&(a) == &(b)) ? (a) /* Check for distinct addresses */                    \
                  : XORSWAP_UNSAFE(a, b))
```



