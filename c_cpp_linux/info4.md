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
