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

# Sort a string according to the order defined by another string

- https://www.geeksforgeeks.org/dsa/sort-string-according-order-defined-another-string/
- https://leetcode.com/problems/custom-sort-string/description/
- https://leetcode.com/problems/custom-sort-string/solutions/
- https://www.ascii-code.com/

# LaTeX

- https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes
- https://www.overleaf.com/learn/latex/Mathematical_expressions
- https://latex-tutorial.com/tutorials/amsmath/
- https://www.docx2latex.com/tutorials/mathematical-equations-latex/
- https://en.wikibooks.org/wiki/LaTeX/Mathematics

# Erase-Remove Idiom in C++

- https://www.geeksforgeeks.org/cpp/erase-remove-idiom-in-cpp/
- https://en.wikipedia.org/wiki/Erase%E2%80%93remove_idiom
- https://cengizhanvarli.medium.com/erase-remove-idiom-e30f75282a83
- https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/Erase-Remove
- https://en.cppreference.com/w/cpp/algorithm/remove.html

> Erase-Remove-Idiom is a C++ STL (Standard Template Library) technique to remove elements from a container.
> It is used to remove all the elements satisfying certain conditions from the container.
> The erase-remove idiom is especially useful in array-based containers like vectors,
> where each elimination requires all the remaining elements to adjust.
> In the worst case, this may lead to O ( n2 ) time complexity.
> This technique avoids this by providing the elimination of the elements in a single parse.
> That is why, the erase-remove idiom provides O( n ) time complexity.
> In this technique, we use the combination of two member functions of the container to remove the elements efficiently
> (std::erase , std::remove or std::remove_if).
> It is due to the use of this function that this technique is called the erase-remove idiom.

```c++
#include <algorithm>
#include <iostream>
#include <vector>

void printV(std::vector<int>& v)
{
    for (auto i : v) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
}

int main()
{
    std::vector<int> v{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };

    std::cout << "Original Vector\t\t\t: ";
    printV(v);

    // using remove_if method to move all the odd elements
    // to the end and get the new logical end
    auto new_logical_end = std::remove_if(
        v.begin(), v.end(), [](int a) { return a % 2; });

    std::cout << "After using remove_if()\t: ";
    printV(v);

    // erasing the elements from new logical end
    v.erase(new_logical_end, v.end());
    std::cout << "After using erase()\t\t: ";
    printV(v);
}
```

# Linux Device Model

- https://linux-kernel-labs.github.io/refs/heads/master/labs/device_model.html
- https://linux-kernel-labs.github.io/refs/pull/190/merge/labs/device_model.html

- https://medium.com/@emanuele.santini.88/sysfs-in-linux-kernel-a-complete-guide-part-1-c3629470fc84
- https://docs.kernel.org/driver-api/driver-model/device.html
- https://sysprog21.github.io/lkmpg/
- https://www.kernel.org/doc/html/v5.7/core-api/kobject.html
- https://medium.com/dvt-engineering/how-to-write-your-first-linux-kernel-module-cf284408beeb
- https://thenewstack.io/demystifying-linux-kernel-initialization/
- https://docs.kernel.org/driver-api/driver-model/driver.html
- https://www.kernel.org/doc/html/next/PCI/pci.html
- https://cbtw.tech/insights/from-novice-to-expert-building-robust-linux-character-device-drivers
- https://www.xml.com/ldd/chapter/book/ch02.html

![Image!](https://linux-kernel-labs.github.io/refs/heads/master/_images/ditaa-a5f399cb84561893770eb45ceeb827ce6d4a2336.png "Image")
![Image!](https://linux-kernel-labs.github.io/refs/heads/master/_images/ditaa-f7ee56960e76c3e80fcbe59fafa38c3d93eac261.png "Image")
![Image!](https://linux-kernel-labs.github.io/refs/heads/master/_images/ditaa-4e1f9758808dba9e61bc0e48faf4365d377f9d32.png "Image")

# std::sort() vs std::stable_sort()

- https://www.studyplan.dev/pro-cpp/range-based-algorithms/q/std-sort-vs-stable-sort
- https://en.wikipedia.org/wiki/Sorting_algorithm#:~:text=Stable%20sort%20algorithms%20sort%20equal,versions%20of%20the%20original%20list

> Key Differences
> - Stability: std::sort() is not stable, meaning the relative order of equivalent elements is not guaranteed to be preserved. std::stable_sort(), on the other hand, guarantees that equivalent elements retain their relative order.
> - Performance: std::sort() is generally faster than std::stable_sort() due to fewer constraints. It uses an introspective sort algorithm, combining quicksort, heapsort, and insertion sort. std::stable_sort() typically uses a combination of merge sort, which ensures stability but with a slightly higher time complexity and potentially higher memory usage.
>
> When to Use Which
> - Use std::sort() when performance is critical and the order of equivalent elements does not matter.
> - Use std::stable_sort() when you need to maintain the relative order of equivalent elements.

```c++
#include <algorithm>
#include <iostream>
#include <vector>

struct Employee {
  std::string name;
  int department;

  bool operator<(const Employee& other) const {
    return department < other.department;
  }
};

int main() {
  std::vector<Employee> employees{
    {"Alice", 2},
    {"Bob", 1},
    {"Charlie", 2},
    {"David", 1}
  };

  // Stable sorting by department
  std::stable_sort(employees.begin(),
    employees.end());

  // Printing the sorted employees
  for (const Employee& emp : employees) {
    std::cout << emp.name << " (Dept "
      << emp.department << ")\n";
  }
}
```

# Memory Allocation in C

- https://www.geeksforgeeks.org/c/dynamic-memory-allocation-in-c-using-malloc-calloc-free-and-realloc/
- https://medium.com/@adebanjoemmanuel01/memory-allocation-in-c-2834a7a29200
- https://en.wikipedia.org/wiki/C_dynamic_memory_allocation
- https://stackoverflow.com/questions/64029219/why-does-malloc-call-mmap-and-brk-interchangeably
- https://www.linkedin.com/pulse/understanding-memory-allocation-linux-relation-brk-pratap-singh-y3icc
- https://notes.shichao.io/tlpi/ch7/
- https://man7.org/linux/man-pages/man3/alloca.3.html
- https://scottc130.medium.com/understanding-heap-memory-allocation-in-c-sbrk-and-brk-d9b95c344cbc
- https://www2.lawrence.edu/fast/GREGGJ/CMSC480/malloc/malloc.html
- https://www.w3schools.com/c/c_memory_allocate.php
- https://www.geeksforgeeks.org/c/dynamic-memory-allocation-in-c-using-malloc-calloc-free-and-realloc/
- https://medium.com/@Dev_Frank/c-memory-management-allocation-99ccb4f36386
- https://www.w3schools.com/c/c_memory_management.php
- https://www.w3schools.com/c/c_memory_allocate.php
- https://www.w3schools.com/c/c_memory_access.php
- https://www.w3schools.com/c/c_memory_reallocate.php
- https://www.w3schools.com/c/c_memory_deallocate.php
- https://www.w3schools.com/c/c_memory_reallife.php
- https://www.programiz.com/c-programming/c-dynamic-memory-allocation
- https://medium.com/@JohanneA/adventures-in-c-dynamically-allocated-stack-5ef44d3db140
- https://en.wikipedia.org/wiki/Stack-based_memory_allocation
- https://www.geeksforgeeks.org/dsa/stack-vs-heap-memory-allocation/
- https://www.cs.cornell.edu/courses/cs3410/2025sp/notes/mem.html
- 

> - alloca
> - malloc calloc free realloc
> - brk
> - sbrk
> - mmap

![Image!](https://www2.lawrence.edu/fast/GREGGJ/CMSC480/malloc/malloc1.png "Image")
![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20250305233229499334/Malloc-function-in-c.webp "Image")
![Image!](https://notes.shichao.io/tlpi/figure_6-1.png "Image")
![Image!](https://media.licdn.com/dms/image/v2/D5612AQE4ObNxiJM2Gg/article-cover_image-shrink_600_2000/B56ZbYWmyTGsAQ-/0/1747386528017?e=2147483647&v=beta&t=qveaeZ0Mx52wVLLfSfmJYOJss12-Df1Fxtsb0iMTixc "Image")

# Comparison of C++ and Posix Threads

- https://www.linkedin.com/pulse/comparision-c-posix-threads-amit-nadiger
- https://www.linkedin.com/pulse/why-c-threads-matter-despite-existence-posix-deepesh-menon-p-m-yyuyc

# Man-in-the-Middle

- https://medium.com/@mesibo-real-time-communication/enhancing-signals-end-to-end-encryption-algorithm-eliminating-man-in-the-middle-and-other-7ecb5b23a3df
- https://heimdalsecurity.com/blog/man-in-the-middle-mitm-attack/
- https://medium.com/@dillihangrae/understanding-diffie-hellman-key-exchange-and-man-in-the-middle-attacks-f9b08abe2c20
- https://www.geeksforgeeks.org/python/man-in-the-middle-attack-in-diffie-hellman-key-exchange/
- https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange
- https://www.linkedin.com/pulse/man-middle-attack-diffie-hellman-key-exchange-protocol-tiago-sousa-7fdhf
- https://dev.to/tiago_sousa_4baece16501be/man-in-the-middle-attack-to-the-diffie-hellman-key-exchange-g-parameter-injection-variant-4oe6
- https://clumsylulz.medium.com/cryptoclient-a-secure-p2p-chat-system-9e85d2de3762
- https://www.outseer.com/fraud-protection/man-in-the-middle-cyberattacks/
- https://www.veracode.com/security/man-middle-attack/
- https://www.vaadata.com/blog/what-is-a-man-in-the-middle-mitm-attack-types-and-security-best-practices/
- https://specopssoft.com/blog/man-in-the-middle-mitm-guide/
- https://sosafe-awareness.com/glossary/man-in-the-middle-attack/
- https://www.strongdm.com/blog/man-in-the-middle-attack-prevention
- https://www.memcyco.com/6-ways-to-prevent-man-in-the-middle-mitm-attacks/

![Image!](https://www.veracode.com/wp-content/uploads/2025/01/veracode-appsec_man-middle-attack.png "Image")
![Image!](https://upload.wikimedia.org/wikipedia/commons/c/c8/DiffieHellman.png "Image")
![Image!](https://www.memcyco.com/wp-content/uploads/2024/11/image2-5.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*RgYY52UrcONkQlYD524uTw.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*X3UM_5aqD5q6GEn0C8Rtng.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/0*7flHdkStmAVmxqae.jpg "Image")

# Post-Quantum Key Exchange for HTTPS Communication

- https://crypto.stackexchange.com/questions/111373/in-x25519kyber768-what-is-the-use-of-the-server-kyber-keys-if-only-the-client-pu
- https://medium.com/identity-beyond-borders/x25519kyber768-post-quantum-key-exchange-for-https-communication-70eba681931d
- https://asecuritysite.com/blog/2024-01-06_A-Signature-Fit-for-a-Post-Quantum-Era--Dilithium-Ed25519-8e3563be17d9.html
- https://peerj.com/articles/cs-2746/
- https://github.com/Quantum-Blockchains/dilithium
- https://security.googleblog.com/2023/08/toward-quantum-resilient-security-keys.html

> Kyber
> Dilithium

![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/0*Z1Wezde5MNiu4fjb "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*VSVMc_bo4Dk2T9bMTVNdJg.jpeg "Image")
![Image!](https://cdn-images-1.medium.com/v2/resize:fit:800/0*fBQqo71tUvD8BRxV.png "Image")
![Image!](https://lh3.googleusercontent.com/lS1UVVErSSJ4ZdsrcStziLa1Ntl7PoHTq-ZrtvXNHo5nkg4WtQhFeyvKlU_kEX7VE2pRw2ZDob8Q-f0Pda8H9-jW8_N6NF8-8Wm-BDQbrtCcm4eExDzQ4kp9VA2bKyhjvrqrkkkN-5_i_jJc_gEP8SMO8C8aLpypyUz96oFzhTzcsBhRuRZWvMXnmLk4o1BZ-Jwv2f4mxhlcVbeCxj4NGVWLG_brFH6my7HZHg "Image")

# Byte-Swapping

- https://gcc.gnu.org/onlinedocs/gcc/Byte-Swapping-Builtins.html
- https://stackoverflow.com/questions/21527957/htonl-vs-builtin-bswap32

```c
uint32_t __builtin_bswap32 (uint32_t x) 
```

# Linux Performance

- https://www.brendangregg.com/linuxperf.html
- https://medium.com/@chrishantha/linux-performance-observability-tools-19ae2328f87f
- https://www.brendangregg.com/Slides/Velocity2015_LinuxPerfTools.pdf
- https://serverauth.com/posts/linux-performance-monitoring-a-comprehensive-guide
- https://skillupwithsachin.medium.com/observability-in-linux-performance-a-visual-guide-4fae21796763

![Image!](https://www.brendangregg.com/Perf/linux_observability_tools.png "Image")
![Image!](https://www.brendangregg.com/Perf/linux_static_tools.png "Image")
![Image!](https://www.brendangregg.com/Perf/bpf_book_tools.png "Image")
![Image!](https://www.brendangregg.com/Perf/linux_bpftrace_tools.png "Image")
![Image!](https://www.brendangregg.com/Perf/linux_observability_sar.png "Image")
![Image!](https://www.brendangregg.com/Perf/linux_benchmarking_tools.png "Image")
![Image!](https://www.brendangregg.com/Perf/linux_tuning_tools.png "Image")
![Image!](https://www.brendangregg.com/Perf/perf-tools_2016.png "Image")
![Image!](https://www.brendangregg.com/Perf/bcc_tracing_tools.png "Image")

# FS

- https://www.sqlite.org/atomiccommit.html
- https://www3.nd.edu/~pbui/teaching/cse.30341.fa17/project06.html
- https://github.com/littlefs-project/littlefs
- https://randomnerdtutorials.com/esp32-write-data-littlefs-arduino/
- https://randomnerdtutorials.com/esp32-web-server-spiffs-spi-flash-file-system/
- https://hackaday.com/2023/10/04/littlefs-the-emphasis-is-on-little/
- https://github.com/littlefs-project/littlefs/blob/master/DESIGN.md
- https://github.com/littlefs-project/littlefs/blob/master/README.md
- https://www.thevtool.com/mounting-littlefs-on-linux-machine/
- https://docs.nordicsemi.com/bundle/ncs-2.9.0-nRF54H20-1/page/zephyr/samples/subsys/fs/littlefs/README.html
- https://www.techrm.com/file-management-on-esp32-spiffs-and-littlefs-compared/
- https://www.codembit.com/blogs/embedded/ekra6m4/littlefs/how_to_use_littlefs_with_cmsis_flash_driver.html#google_vignette
- https://docs.nordicsemi.com/bundle/ncs-2.5.1/page/zephyr/samples/subsys/fs/littlefs/README.html
- https://docs.zephyrproject.org/latest/samples/subsys/fs/littlefs/README.html
- https://os.mbed.com/docs/mbed-os/v6.16/apis/littlefilesystem.html
- https://www.instructables.com/Introduction-to-LittleFs-Write-LittleFs-Read-Littl/
- https://github.com/wreyford/demo_esp_littlefs
- https://medium.com/engineering-iot/using-littlefs-for-file-operations-on-esp32-with-platformio-01f203be751d
- https://randomnerdtutorials.com/esp32-write-data-littlefs-arduino/
- https://arduino-tex.ru/news/202/ustanovka-zagruzchika-littlefs-v-arduino-ide-2-dlya-zagruzki.html
- https://randomnerdtutorials.com/esp32-write-data-littlefs-arduino/
- https://randomnerdtutorials.com/esp32-littlefs-arduino-ide/
- https://infineon.github.io/mtb-littlefs/api_reference_manual/html/index.html
- https://docs.nordicsemi.com/bundle/ncs-2.6.3/page/zephyr/samples/subsys/fs/littlefs/README.html
- https://fids.git-pages.uni-regensburg.de/developer-skills/02_file_system.html
- https://pages.cs.wisc.edu/~jhe/eurosys17-he.pdf
- https://www.enterprisedb.com/blog/putting-postgresql-tablespace-ramdisk-risks-all-your-data
- https://github.com/google/leveldb
- https://scientific-zheng.medium.com/as-a-programmer-you-need-to-understand-these-about-files-b542c60d9285

- https://habr.com/ru/articles/347348/
- https://voltiq.ru/littlefs-esp32-tutorial/

![Image!](https://www3.nd.edu/~pbui/teaching/cse.30341.fa17/static/img/project06-components.png "Image")
![Image!](https://www3.nd.edu/~pbui/teaching/cse.30341.fa17/static/img/project06-layout.png "Image")
![Image!](https://www3.nd.edu/~pbui/teaching/cse.30341.fa17/static/img/project06-inode.png "Image")
![Image!](https://os.mbed.com/docs/mbed-os/v6.16/apis/images/classmbed_1_1_little_file_system.png "Image")

# Memory Barriers

- https://www.puppetmastertrading.com/images/hwViewForSwHackers.pdf
- https://documentation-service.arm.com/static/62a304f231ea212bb662321d
- https://people.freebsd.org/~lstewart/articles/cpumemory.pdf
- https://research.swtch.com/hwmm
- https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/
- https://preshing.com/20120913/acquire-and-release-semantics/
- https://devblogs.microsoft.com/oldnewthing/20210614-00/?p=105307
- https://s-o-c.org/what-are-atomic-operations-in-arm/
- https://cpufun.substack.com/p/atomics-in-aarch64

- https://habr.com/ru/articles/196548/

![Image!](https://research.swtch.com/mem-tso@2x.png "Image")

# Compiling a C Program

- https://www.geeksforgeeks.org/c/compiling-a-c-program-behind-the-scenes/
- https://unstop.com/blog/compilation-in-c
- https://www.tutorialspoint.com/cprogramming/c_compilation_process.htm
- https://users.cms.caltech.edu/~mvanier/CS11_C/misc/compiling_c.html
- https://andrew128.github.io/x86-binary/

![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20230404112946/Compilation-Process-in-C.png "Image")
![Image!](https://d8it4huxumps7.cloudfront.net/uploads/images/655df16819a37_compilation_in_c_01.jpg?d=2000x2000 "Image")
![Image!](https://www.tutorialspoint.com/cprogramming/images/compilation-process.jpg "Image")
![Image!](https://andrew128.github.io/images/x86BinaryBlog/compilationProcess.png "Image")

# C++ : The rule of five

- https://en.cppreference.com/w/cpp/language/rule_of_three.html
- https://www.geeksforgeeks.org/cpp/rule-of-five-in-cpp/
- https://medium.com/@nerudaj/understanding-the-rule-of-five-in-c-5944ca4b7bd3

> If any of the below functions is defined for a class, then it is better to define all of them.
> - Destructor
> - Copy Constructor
> - Copy Assignment Operator
> - Move Constructor
> - Move Assignment Operator

```c++
class rule_of_five
{
    char* cstring; // raw pointer used as a handle to a
                   // dynamically-allocated memory block
public:
    explicit rule_of_five(const char* s = "") : cstring(nullptr)
    { 
        if (s)
        {
            cstring = new char[std::strlen(s) + 1]; // allocate
            std::strcpy(cstring, s); // populate 
        } 
    }
 
    ~rule_of_five()
    {
        delete[] cstring; // deallocate
    }
 
    rule_of_five(const rule_of_five& other) // copy constructor
        : rule_of_five(other.cstring) {}
 
    rule_of_five(rule_of_five&& other) noexcept // move constructor
        : cstring(std::exchange(other.cstring, nullptr)) {}
 
    rule_of_five& operator=(const rule_of_five& other) // copy assignment
    {
        // implemented as move-assignment from a temporary copy for brevity
        // note that this prevents potential storage reuse
        return *this = rule_of_five(other);
    }
 
    rule_of_five& operator=(rule_of_five&& other) noexcept // move assignment
    {
        std::swap(cstring, other.cstring);
        return *this;
    }
 
// alternatively, replace both assignment operators with copy-and-swap
// implementation, which also fails to reuse storage in copy-assignment.
//  rule_of_five& operator=(rule_of_five other) noexcept
//  {
//      std::swap(cstring, other.cstring);
//      return *this;
//  }
};
```

# C pointer arithmetic

- https://www.geeksforgeeks.org/c/pointer-arithmetics-in-c-with-examples/
- https://www.tutorialspoint.com/cprogramming/c_pointer_arithmetic.htm
- https://www.gnu.org/software/c-intro-and-ref/manual/html_node/Pointer-Arithmetic.html
- https://medium.com/@codewithcoders96/pointer-arithmetic-in-c-c-ca328e76943e
- https://www.wscubetech.com/resources/c-programming/pointer-arithmetic
- https://www.geeksforgeeks.org/c/pointer-arithmetic-with-strings/

![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20230424100935/Pointer-Addition.webp "Image")

# C++ Constructors

- https://www.w3schools.com/cpp/cpp_constructors.asp
- https://www.geeksforgeeks.org/cpp/constructors-c/
- https://en.cppreference.com/w/cpp/language/initializer_list.html
- https://isocpp.org/wiki/faq/ctors
- https://www.programiz.com/cpp-programming/constructors
- https://www.mygreatlearning.com/blog/constructor-in-cpp/
- https://en.cppreference.com/w/cpp/language/list_initialization.html

- https://www.ibm.com/docs/en/xcfbg/121.141?topic=constructors-execution-order-class-objects

> - Default Constructor
> - Parameterized Constructor
> - Copy Constructor
> - Move Constructor

> When a class object is created using constructors, the execution order of constructors is:
> 1. Constructors of Virtual base classes are executed, in the order that they appear in the base list.
> 2. Constructors of nonvirtual base classes are executed, in the declaration order.
> 3. Constructors of class members are executed in the declaration order (regardless of their order in the initialization list).
> 4. The body of the constructor is executed.

```c++
// Syntax of Default Constructor
class ClassName {
public:
    ClassName(); // Default constructor declaration
};

// Syntax of Parameterized Constructor
class ClassName {
public:
    ClassName(Type1 parameter1, Type2 parameter2, ...); // Parameterized constructor declaration
};

// Syntax of Copy Constructor
class ClassName {
public:
    ClassName(const ClassName& obj); // Copy constructor declaration
};

// Syntax of Move Constructor in C++
class ClassName {
public:
    ClassName(ClassName&& obj); // Move constructor declaration
};

// Syntax of Destructors in C++
class ClassName {
public:
    ~ClassName(); // Destructor declaration
};
```

# TCP/IP, OSI

- https://en.wikipedia.org/wiki/OSI_model

- https://www.geeksforgeeks.org/computer-networks/differences-between-tcp-and-udp/
- https://www.spiceworks.com/tech/networking/articles/tcp-vs-udp/
- https://www.avast.com/c-tcp-vs-udp-difference
- https://networklessons.com/network-fundamentals/introduction-to-tcp-and-udp

- https://www.geeksforgeeks.org/computer-networks/tcp-3-way-handshake-process/
- https://developer.mozilla.org/en-US/docs/Glossary/TCP_handshake
- https://www.coursera.org/articles/three-way-handshake
- https://www.sciencedirect.com/topics/computer-science/three-way-handshake

![Image!](https://s7280.pcdn.co/wp-content/uploads/2018/06/osi-model-7-layers-1.png "Image")
![Image!](https://datageneral.co/wp-content/uploads/2023/12/Capas-modelo-OSI.png "Image")
![Image!](https://www.colocationamerica.com/wp-content/uploads/2018/12/udp-tcp.jpg "Image")
![Image!](https://www.netburner.com/wp-content/uploads/2020/06/TCP-vs-UDP-1024x687.png "Image")
![Image!](https://ipcisco.com/wp-content/uploads/2018/10/tcp-vs-udp-comparison-ipcisco.com_.png "Image")
![Image!](https://media.geeksforgeeks.org/wp-content/uploads/handshake-1.png "Image")
![Image!](https://ars.els-cdn.com/content/image/3-s2.0-B9780128182000000160-f05-09-9780128182000.jpg "Image")
![Image!](https://ars.els-cdn.com/content/image/3-s2.0-B0122272404001878-gr10.gif "Image")

# ARM Cortex-M

- https://en.wikipedia.org/wiki/ARM_Cortex-M
- https://developer.arm.com/Architectures/M-Profile%20Architecture
- https://community.arm.com/cfs-file/__key/telligent-evolution-components-attachments/01-2142-00-00-00-00-52-96/White-Paper-_2D00_-Cortex_2D00_M-for-Beginners-_2D00_-2016-_2800_final-v3_2900_.pdf
- https://kolegite.com/EE_library/books_and_lectures/%D0%9C%D0%B8%D0%BA%D1%80%D0%BE%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D0%BE%D1%80%D0%BD%D0%B0%20%D1%81%D1%85%D0%B5%D0%BC%D0%BE%D1%82%D0%B5%D1%85%D0%BD%D0%B8%D0%BA%D0%B0/%D0%9C%D0%B8%D0%BA%D1%80%D0%BE%D0%BA%D0%BE%D0%BD%D1%82%D1%80%D0%BE%D0%BB%D0%B5%D1%80%D0%B8/Cortex%20M%20Architecture.pdf
- https://mu.microchip.com/arm-cortex-m-architecture-overview
- https://www.ti.com/lit/ml/swrp141/swrp141.pdf?ts=1756821919118
- https://embeddedsecurity.io/sec-arm-arch-core
- https://docs.zephyrproject.org/latest/hardware/arch/arm_cortex_m.html

- https://svenssonjoel.github.io/pages-2021/cortex-m-assembler-0/index.html
- https://developer.arm.com/documentation/102438/0100/Learning-about-assembly-language
- https://nicerland.com/arm-asm-for-embedded/
- https://www.mikrocontroller.net/articles/ARM-ASM-Tutorial

![Image!](https://microcontrollerslab.com/wp-content/uploads/2020/09/ARM-Cortex-M4-Architecture-block-diagram.jpg "Image")
![Image!](https://www.watelectronics.com/wp-content/uploads/ARM-Architecture.jpg "Image")
![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20230426173520/ARM-Processor-and-its-Features.webp "Image")
![Image!](https://i.sstatic.net/MGQMH.jpg "Image")
![Image!](https://i.sstatic.net/1SPVA.png "Image")
![Image!](https://www.labs.cs.uregina.ca/301/ARM/ARM_Cortex-M3_CoreRegisters.jpg "Image")
![Image!](https://microcontrollerslab.com/wp-content/uploads/2020/07/Memory-Map-region-TM4C123GH6PM-arm-cortex-M4.jpg "Image")

# C++ : Static Methods and Fields

- https://www.luisllamas.es/en/cpp-static-methods/#:~:text=In%20C%2B%2B%20static%20methods,an%20object%20of%20the%20class.
- https://www.geeksforgeeks.org/cpp/static-keyword-cpp/
- https://en.cppreference.com/w/cpp/language/static.html
- https://www.embeddedrelated.com/showarticle/1598.php

> In C++ static methods and fields are elements that do not belong to a particular instance
> but belong directly to the class itself.
> This means you can access them without the need to create an object of the class.
>
> Static methods in C++ are defined using the static keyword.
> These methods do not have access to the non-static members of the class
> and can only access other static methods and fields.
> 
> A static field (or static member variable) is a variable that is shared by all instances of a class.
> Unlike non-static fields, which have a distinct value for each instance, static fields have a single shared value.

```c++
#include <iostream>

class Util
{
public:
    static int Add(int a, int b)
    {
        return a + b;
    }
};

int main()
{
    int result = Util::Add(5, 3);
    std::cout << "Result: " << result << std::endl; // Output: Result: 8

    return 0;
}
```

```c++
#include <iostream>

class Counter
{
public:
    static int Count;

    Counter()
    {
        Count++;
    }
};

// Definition of the static field
int Counter::Count = 0;

int main()
{
    Counter c1;
    Counter c2;
    Counter c3;

    std::cout << "Total instances created: " << Counter::Count << std::endl; // Output: Total instances created: 3

    return 0;
}
```

# C++ : Override and overload

- https://www.geeksforgeeks.org/cpp/function-overloading-vs-function-overriding-in-cpp/
- https://www.scaler.com/topics/difference-between-function-overloading-and-overriding-in-cpp/
- https://www.tutorialspoint.com/difference-between-function-overloading-and-overriding-in-cplusplus
- https://www.naukri.com/code360/library/function-overloading-and-overriding-in-cpp
- https://testbook.com/key-differences/difference-between-function-overloading-and-function-overriding-in-c-plus-plus

![Image!](https://media.licdn.com/dms/image/v2/D4D22AQGAo4grlCw8ZA/feedshare-shrink_800/feedshare-shrink_800/0/1693232702209?e=2147483647&v=beta&t=xI_0ZlJSBuYsFgZ62MWvULNHFR3NMKnsY2T6JEB9hUc "Image")

# C++ : Overloading operators

- https://www.ibm.com/docs/en/zos/2.4.0?topic=only-overloading-operators-c
- https://en.cppreference.com/w/cpp/language/operators.html
- https://www.geeksforgeeks.org/cpp/operator-overloading-cpp/
- https://www.geeksforgeeks.org/cpp/what-are-the-operators-that-can-be-and-cannot-be-overloaded-in-cpp/

![Image!](https://cdn-images-1.medium.com/v2/resize:fit:720/1*xuGMTu1tCAU3xl0EbQTtVg.png "Image")
![Image!](https://d14qv6cm1t62pm.cloudfront.net/ccbp-website/Blogs/home/binary-operator-overloading-in-cpp-image-1.png "Image")

# AES , RSA , etc.

- https://medium.com/@bs3440/the-aes-rsa-hybrid-encryption-a-balanced-approach-to-secure-communication-2999e5db596b
- https://www.linkedin.com/advice/0/how-do-you-use-aes-rsa-together-hybrid-encryption-schemes
- https://www.ijrar.org/papers/IJRAR23B1852.pdf
- https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation
- https://www.mdpi.com/2079-9292/9/7/1128
- https://medium.com/batc/combining-rsa-aes-encryption-to-secure-rest-endpoint-with-sensitive-data-eb3235b0c0cc
- https://www.geeksforgeeks.org/computer-networks/difference-between-aes-and-rsa-encryption/
- https://dev.to/hardy_mervana/encryption-standards-aes-rsa-ecc-sha-and-other-protocols-460c
- https://medium.com/javarevisited/appsec-a-developers-handbook-to-mastering-rsa-and-aes-encryption-9c0e6465452f
- https://aishwaryas.hashnode.dev/hybrid-encryption

> RSA and AES are combined in a "hybrid encryption" system to leverage the strengths of both:
> RSA's secure key exchange (asymmetric encryption) and AES's high-speed bulk data encryption (symmetric encryption).
> In this model, a temporary symmetric key (like an AES key) is generated,
> the bulk data is encrypted with this key, and then the key itself is encrypted using the recipient's public RSA key.
> The encrypted key and the encrypted data are sent to the recipient,
> who decrypts the AES key with their private RSA key, and then uses the decrypted AES key to decrypt the message.

![Image!](https://lh6.googleusercontent.com/XRmabv2XySfvG-CG3n7yjBBFBfWkmoVKYfdbnKGLDX46A8xdxVWWEIlO8ZMRVcQzR2pTvZZLQETPUToOozR0Q7O8-4yfK8UMoc4zP82HyDAKqKagHIW8dkQX5SCfntAFCdp6QzN6WF0JHh7ZuS-imnZllY4wSvSqy40yabhid5hylS_Ps4RNvj-JEvYwHQ "Image")

# C++ : Copy and Move

- https://www.modernescpp.com/index.php/copy-versus-move-semantic-a-few-numbers/
- https://www.geeksforgeeks.org/cpp/move-constructors-in-c-with-examples/
- https://how.dev/answers/what-are-move-semantics-and-copy-semantics-in-cpp11
- https://www.thecodedmessage.com/posts/cpp-move/
- https://en.cppreference.com/w/cpp/language/move_constructor.html

# C++ : Smart Pointer

- https://www.geeksforgeeks.org/cpp/smart-pointers-cpp/
- https://en.cppreference.com/book/intro/smart_pointers
- https://learn.microsoft.com/en-us/cpp/cpp/smart-pointers-modern-cpp?view=msvc-170
- https://medium.com/@lucky_rydar/guide-over-smart-pointers-in-c-46ed8b04448c
- https://medium.com/@AlexanderObregon/how-c-smart-pointers-manage-resource-ownership-52d0065c80a9
- https://www.modernescpp.com/index.php/c-core-guidelines-rules-to-smart-pointers/
- https://acodersjourney.com/top-10-dumb-mistakes-avoid-c-11-smart-pointers/
- https://www.codecademy.com/resources/docs/cpp/smart-pointers
- https://ortogonal.github.io/cpp/forward-declaration-and-smart-pointers/
- https://www.scaler.com/topics/cpp/smart-pointers-in-cpp/

> - auto_ptr
> - unique_ptr
> - shared_ptr
> - weak_ptr

```c++
#include <iostream>
#include <memory>
using namespace std;

int main() {
    
    // Pointer declaration
    auto_ptr<int> ptr1(new int(10));
    cout << *ptr1 << endl;
    
    // Transfer ownership to
    // pointer ptr2, 
    auto_ptr<int> ptr2 = ptr1;
    cout << *ptr2;
    return 0;
}

#include <iostream>
#include <memory>
using namespace std;

class Rectangle {
    int length;
    int breadth;

public:
    Rectangle(int l, int b) {
        length = l;
        breadth = b;
    }
    int area() { return length * breadth; }
};

int main() {

    unique_ptr<Rectangle> P1(new Rectangle(10, 5));
    cout << P1->area() << endl;

    unique_ptr<Rectangle> P2;

    // Copy the addres of P1 into p2
    P2 = move(P1);
    cout << P2->area();
    return 0;
}

#include <iostream>
#include <memory>
using namespace std;

class Rectangle {
    int length;
    int breadth;

public:
    Rectangle(int l, int b) {
        length = l;
        breadth = b;
    }
    int area() { return length * breadth; }
};

int main() {
    
    shared_ptr<Rectangle> P1(new Rectangle(10, 5));
    cout << P1->area() << endl;

    shared_ptr<Rectangle> P2;
    
    // P1 and P2 are pointing 
    // to same object
    P2 = P1;
    cout << P2->area() << endl;
    cout << P1->area() << endl;
    cout << P1.use_count();
    return 0;
}

#include <iostream>
#include <memory>
using namespace std;

class Rectangle {
    int length;
    int breadth;

public:
    Rectangle(int l, int b) {
        length = l;
        breadth = b;
    }

    int area() { return length * breadth; }
};

int main() {
    
    // Create shared_ptr Smart Pointer
    shared_ptr<Rectangle> P1(new Rectangle(10, 5));
    
    // Created a weak_ptr smart pointer
    weak_ptr<Rectangle> P2 (P1);
    cout << P1->area() << endl;
    
    // Returns the number of shared_ptr 
    // objects that manage the object
    cout << P2.use_count();
    return 0;
}
```

![Image!](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20190917111931/Auto-Pointer-in-C1-1024x630.jpg "Image")
![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20250402152108622799/unique-pointer-in-cpp.webp "Image")
![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20250402152148448561/shared-pointer-in-cpp.webp "Image")
![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20250402152221597690/weak-pointer-in-cpp.webp "Image")

# C++ : Const vs Constexpr

- https://medium.com/@sofiasondh/c-const-vs-constexpr-the-comparison-183f9dd92deb
- https://madhawapolkotuwa.medium.com/constant-types-in-c-const-constexpr-constinit-and-more-e713c3f31bbe
- https://www.cppstories.com/2022/const-options-cpp20/
- https://vishalchovatiya.com/posts/when-to-use-const-vs-constexpr-in-cpp/
- https://aticleworld.com/cpp-const-vs-constexpr-explained/

![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*hAyoDQ271QHcqVTus0TKRw.jpeg "Image")

# C : memcpy() naive

- https://medium.com/@caruychen_48871/the-curious-case-of-memcpy-bd93936e5136
- https://git.musl-libc.org/cgit/musl/tree/src/string/memcpy.c
- https://codebrowser.dev/glibc/glibc/string/memcpy.c.html
- https://www.embedded.com/optimizing-memcpy-improves-speed/

- https://stackoverflow.com/questions/35843365/how-to-detect-machine-word-size-in-c-c
- https://en.wikipedia.org/wiki/Word_(computer_architecture)

```c
#include <string.h>
#include <stdint.h>

#include <stdio.h>

void *tmp_memcpy(void * const restrict dst,
                 const void * const restrict src,
                 const size_t n)
{
    size_t len = n;
    const size_t lsize = sizeof(uintptr_t);
    
    if (n == 0) {
        return (dst);
    }
    
    uintptr_t *ld = (uintptr_t *)dst;
    uintptr_t const *ls = (uintptr_t const *)src;
    while (len >= lsize) {
        *ld++ = *ls++;
        len -= lsize;
    }

    char *cd = (char *)ld;
    char const *cs = (char const *)ls;
    while (len--) {
        *cd++ = *cs++;
    }

    return (dst);
}

int main()
{
    const char *src0 = "01234567890123456789";
    const char *src1 = "012345678";
    const char *src2 = "01234567";
    const char *src3 = "01237";
    char dst[100] = {};
    const char *src = NULL;
    
    printf("sizeof(uintptr_t) = %llu\n", (unsigned long long)sizeof(uintptr_t));
    printf("sizeof(dst)/sizeof(dst[0]) = %llu\n", (unsigned long long)sizeof(dst)/sizeof(dst[0]));
    printf("\n");
    
    src = src0;
    printf("len(src) = %lu | src = %s\n", strlen(src), src);
    (void)tmp_memcpy(dst, src, strlen(src));
    printf("len(dst) = %lu | dst = %s\n", strlen(dst), dst);
    memset(dst, '\0', sizeof(dst)/sizeof(dst[0]));
    printf("\n");
    
    src = src1;
    printf("len(src) = %lu | src = %s\n", strlen(src), src);
    (void)tmp_memcpy(dst, src, strlen(src));
    printf("len(dst) = %lu | dst = %s\n", strlen(dst), dst);
    memset(dst, '\0', sizeof(dst)/sizeof(dst[0]));
    printf("\n");
    
    src = src2;
    printf("len(src) = %lu | src = %s\n", strlen(src), src);
    (void)tmp_memcpy(dst, src, strlen(src));
    printf("len(dst) = %lu | dst = %s\n", strlen(dst), dst);
    memset(dst, '\0', sizeof(dst)/sizeof(dst[0]));
    printf("\n");
    
    src = src3;
    printf("len(src) = %lu | src = %s\n", strlen(src), src);
    (void)tmp_memcpy(dst, src, strlen(src));
    printf("len(dst) = %lu | dst = %s\n", strlen(dst), dst);
    memset(dst, '\0', sizeof(dst)/sizeof(dst[0]));
    printf("\n");
    

    return 0;
}
```

# C \ C++ : Alignment

- https://en.wikipedia.org/wiki/Data_structure_alignment
- https://learn.microsoft.com/en-us/cpp/c-language/alignment-c?view=msvc-170
- https://www.geeksforgeeks.org/dsa/data-structure-alignment-how-data-is-arranged-and-accessed-in-computer-memory/
- https://stackoverflow.com/questions/41719845/memory-alignment-in-c-c
- https://abstractexpr.com/2023/06/29/structures-in-c-from-basics-to-memory-alignment/
- https://medium.com/@mbed101/understanding-the-alignment-problem-in-c-structs-a-comprehensive-guide-c1c0d99983be
- https://hps.vi4io.org/_media/teaching/wintersemester_2013_2014/epc-14-haase-svenhendrik-alignmentinc-paper.pdf
- https://learn.microsoft.com/en-us/cpp/cpp/alignment-cpp-declarations?view=msvc-170
- https://www.gnu.org/software/c-intro-and-ref/manual/html_node/Type-Alignment.html
- https://softwareengineering.stackexchange.com/questions/328775/how-important-is-memory-alignment-does-it-still-matter
- https://en.cppreference.com/w/c/language/object.html
- https://levelup.gitconnected.com/how-struct-memory-alignment-works-in-c-3ee897697236
- https://ryonaldteofilo.medium.com/memory-and-data-alignment-in-c-b870b02c80fb
- https://en.cppreference.com/w/cpp/memory/align.html
- https://www.omi.me/blogs/firmware-guides/how-to-align-variables-in-memory-using-gcc-in-embedded-c?srsltid=AfmBOoo8CZJBetURWevoGT5yE2IMgzmqgCQG3AIf7PW3lB6ZRDCRaPen

# Network Mask (Subnet)

- https://en.wikipedia.org/wiki/Subnet
- https://wiki.teltonika-networks.com/view/What_is_a_Netmask%3F
- https://learn.microsoft.com/en-us/troubleshoot/windows-client/networking/tcpip-addressing-and-subnetting
- https://dnsmadeeasy.com/resources/subnet-mask-cheat-sheet
- https://www.dipolnet.com/what_is_the_ip_address_network_mask_gateway_bib538.htm
- https://www.geeksforgeeks.org/computer-networks/role-of-subnet-mask/
- https://www.aelius.com/njh/subnet_sheet.html
- https://jumpcloud.com/it-index/what-is-a-network-mask
- https://www.networkacademy.io/ccna/ip-subnetting/the-subnet-mask
- https://www.freecodecamp.org/news/subnet-cheat-sheet-24-subnet-mask-30-26-27-29-and-other-ip-address-cidr-network-references/
- https://www.auvik.com/franklyit/blog/what-is-subnet-mask/
- https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
- https://www.cloudaccess.net/cloud-control-panel-ccp/157-dns-management/322-subnet-masks-reference-table.html

![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20241216165032658788/table.webp "Image")
![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20250108114800758375/Role-of-Subnet-Mask.webp "Image")

# TLS

- https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/
- https://www.ssl.com/article/ssl-tls-handshake-ensuring-secure-online-interactions/
- https://www.ibm.com/docs/en/ibm-mq/9.3.x?topic=tls-overview-ssltls-handshake
- https://www.serverion.com/uncategorized/ssl-tls-handshake-process-step-by-step-guide/
- https://en.wikipedia.org/wiki/Transport_Layer_Security
- https://www.sectigo.com/resource-library/tls-ssl-handshake-errors-how-to-fix-them
- https://techcommunity.microsoft.com/blog/azurepaasblog/ssltls-connection-issue-troubleshooting-test-tools/2240059
- https://auth0.com/blog/the-tls-handshake-explained/
- https://sematext.com/glossary/ssl-tls-handshake/
- https://www.a10networks.com/glossary/key-differences-between-tls-1-2-and-tls-1-3/

![Image!](https://www.stg.ssl.com/wp-content/uploads/2023/09/SSLTLS-Handshake-600x600.png "Image")
![Image!](https://i.ytimg.com/vi/j9QmMEWmcfo/maxresdefault.jpg "Image")
![Image!](https://www.a10networks.com/wp-content/uploads/differences-between-tls-1.2-and-tls-1.3-full-handshake.png "Image")

# C declarations

- https://www.educative.io/blog/decoding-c-declarations
- https://www.geeksforgeeks.org/c/complicated-declarations-in-c/
- https://faculty.cs.niu.edu/~mcmahon/CS241/Notes/reading_declarations.html
- https://www.pearsonhighered.com/assets/samplechapter/0/1/3/1/0131774298.pdf
- https://users.ece.utexas.edu/~ryerraballi/CPrimer/CDeclPrimer.html
- http://unixwiz.net/techtips/reading-cdecl.html
- https://parrt.cs.usfca.edu/doc/how-to-read-C-declarations.html
- https://medium.com/@bartobri/untangling-complex-c-declarations-9b6a0cf88c96
- https://www.w3schools.com/c/c_functions_decl.php
- https://medium.com/@jalal92/c-gibberish-9d7912464242

![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*09XYwPFuQY79y_MuQRFHdQ.png "Image")

# x86 calling conventions

- https://en.wikipedia.org/wiki/X86_calling_conventions
- https://en.wikipedia.org/wiki/Calling_convention
- https://learn.microsoft.com/en-us/cpp/cpp/calling-conventions?view=msvc-170
- https://wiki.osdev.org/Calling_Conventions
- https://www.geeksforgeeks.org/cpp/calling-conventions-in-c-cpp/
- https://learn.microsoft.com/en-us/cpp/build/x64-calling-convention?view=msvc-170
- https://www.agner.org/optimize/calling_conventions.pdf
- https://riscv.org/wp-content/uploads/2024/12/riscv-calling.pdf
- https://andrew128.github.io/x86-binary/

![Image!](https://andrew128.github.io/images/x86BinaryBlog/callingConventions.png "Image")

# C++ Name Mangling

- https://en.wikipedia.org/wiki/Name_mangling
- https://www.ibm.com/docs/en/i/7.4.0?topic=linkage-name-mangling-c-only
- https://www.emmtrix.com/wiki/Demystifying_C%2B%2B_-_Name_Mangling
- https://www.geeksforgeeks.org/cpp/extern-c-in-c/
- https://web.mit.edu/tibbetts/Public/inside-c/www/mangling.html
- https://en.wikiversity.org/wiki/Visual_C%2B%2B_name_mangling
- https://medium.com/@abhishek.ec/c-name-mangling-ce3d0fedf88d

![Image!](https://media.licdn.com/dms/image/v2/D5622AQFjQFQTjCG6RA/feedshare-shrink_800/B56Zi46vZ0HMAg-/0/1755449066203?e=2147483647&v=beta&t=m7SZ_GNsFSMbnHmqJiRYKwyY4ZQxUb-plWNtgBi7uzM "Image")

# ELF

- https://andrew128.github.io/x86-binary/
- https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
- https://refspecs.linuxfoundation.org/elf/gabi4+/ch4.eheader.html
- https://wiki.osdev.org/ELF
- https://man7.org/linux/man-pages/man5/elf.5.html
- https://gist.github.com/x0nu11byt3/bcb35c3de461e5fb66173071a2379779
- https://docs.oracle.com/cd/E19620-01/805-4694/6j4enatct/index.html
- https://www.baeldung.com/linux/executable-and-linkable-format-file
- https://dev.to/bytehackr/understanding-the-basics-of-elf-files-on-linux-61c
- https://medium.com/@ajmewal/basics-of-elf-executable-and-linkable-format-file-88a516877356
- https://blog.k3170makan.com/2018/09/introduction-to-elf-format-elf-header.html
- https://blog.k3170makan.com/2018/09/introduction-to-elf-format-part-ii.html
- https://ics.uci.edu/~aburtsev/238P/hw/hw3-elf/hw3-elf.html
- https://cs4157.github.io/www/2024-1/lect/15-elf-intro.html
- https://linux-audit.com/elf-binaries-on-linux-understanding-and-analysis/
- https://medium.com/@4984_30211/elf-format-toolbox-ee110fe987ba

![Image!](https://andrew128.github.io/images/x86BinaryBlog/elfFile.png "Image")
![Image!](https://camo.githubusercontent.com/00cd4e64df02caf11e9c7c8f67a4d7e9470ea03c244e6d5bce8444a674b9143c/68747470733a2f2f692e696d6775722e636f6d2f4169394f714f422e706e67 "Image")
![Image!](https://camo.githubusercontent.com/94b1128b885c29e21c64fb3b247d0184c54f4248e4195462bd15671003afc319/68747470733a2f2f692e696d6775722e636f6d2f4c4e6464546d6b2e706e67 "Image")
![Image!](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiWiCzMg6xX9YzTX4JlU2EmSjt807TV3dFiwkfRrPB7a0cKnCnA9VAdFNm2fGuXvE5oMVG34zob4eDlFCweI_h-VB02ss-6RFal9IIAaT2W83EjMhavWdB_0ZZkiPrUPr0AcJmoVq44aII/s1600/ELF+Program+Headers+%25282%2529.png "Image")
![Image!](https://ics.uci.edu/~aburtsev/238P/hw/hw3-elf/img/typical_elf.jpg "Image")
![Image!](https://camo.githubusercontent.com/146b420583a08db00a96abca570e116388b080ee2de307c0d24614b714fe3d43/68747470733a2f2f692e696d6775722e636f6d2f6a364b397963482e706e67 "Image")
![Image!](https://cs4157.github.io/www/2024-1/lect/pix/15-csapp-fig7.13.png "Image")
![Image!](https://linux-audit.com/elf-binaries-on-linux-understanding-and-analysis/images/elf-program-headers-segments.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1000/1*4O3mgiWbsWv4wHL9kJwlxA.jpeg "Image")

# C++ : Multiple inheritance

- https://en.wikipedia.org/wiki/Multiple_inheritance
- https://www.geeksforgeeks.org/cpp/multiple-inheritance-in-c/
- https://www.w3schools.com/cpp/cpp_inheritance_multiple.asp
- https://isocpp.org/wiki/faq/multiple-inheritance
- https://www.programiz.com/cpp-programming/multilevel-multiple-inheritance
- https://www.tutorialspoint.com/cplusplus/cpp_multiple_inheritance.htm

![Image!](https://en.wikipedia.org/wiki/File:Diamond_inheritance.svg "Image")

# C++ : idioms

- https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms
- https://cppdepend.com/blog/top-7-most-used-c-idioms-part1/
- https://cppdepend.com/blog/top-7-most-used-c-idioms-part2/
- https://medium.com/@amalpp42/idioms-in-c-f6b1c19fa605
- https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/Print_Version
- https://learnmoderncpp.com/2021/04/23/a-handful-of-c-idioms/
- https://github.com/utilForever/CppIdioms
- http://seshbot.com/blog/2014/08/16/modern-c-plus-plus-idioms-i-use-every-day/

# C++ : Patterns

- https://refactoring.guru/design-patterns/cpp
- https://www.geeksforgeeks.org/system-design/modern-c-design-patterns-tutorial/
- https://github.com/Junzhuodu/design-patterns
- https://en.wikibooks.org/wiki/C%2B%2B_Programming/Code/Design_Patterns
- https://medium.com/@lokeshbihani99/design-patterns-in-c-5cdf245d594d
- https://dev.to/visheshpatel/what-is-design-pattern-just-another-article-573
- https://refactoring.guru/design-patterns/state/cpp/example
- https://www.alibabacloud.com/blog/discussing-the-four-aspects-of-the-c%2B%2B-design-pattern_600377
- https://medium.com/@oleksandra_shershen/solid-principles-implementation-and-examples-in-c-99f0d7e3e868
- https://simple.wikipedia.org/wiki/SOLID_(object-oriented_design)
- https://medium.com/@rishu__2701/mastering-object-oriented-programming-oop-in-c-a-comprehensive-guide-62b3554822eb
- https://www.w3schools.com/cpp/cpp_oop.asp
- https://www.programiz.com/cpp-programming/oop

# C++ : vtable

- https://pabloariasal.github.io/2017/06/10/understanding-virtual-tables/
- https://www.geeksforgeeks.org/cpp/vtable-and-vptr-in-cpp/
- https://en.wikipedia.org/wiki/Virtual_method_table
- https://levelup.gitconnected.com/digging-into-c-vtables-572f23aa9d43
- https://medium.com/@satyadirisala/demystifying-dynamic-dispatch-a-deep-dive-into-virtual-functions-vptr-and-vtable-9574c1ad9bed
- https://www.learncpp.com/cpp-tutorial/the-virtual-table/#google_vignette
- https://medium.com/@calebleak/fast-virtual-functions-hacking-the-vtable-for-fun-and-profit-25c36409c5e0
- https://www.timdbg.com/posts/vtables/
- https://fekir.info/post/write-custom-virtual-table-in-cpp/
- https://leimao.github.io/blog/CPP-Virtual-Table/
- https://shaharmike.com/cpp/vtable-part1/
- https://shaharmike.com/cpp/vtable-part2/
- https://shaharmike.com/cpp/vtable-part3/
- https://shaharmike.com/cpp/vtable-part4/
- https://people.montefiore.uliege.be/declercq/INFO0004/documents/vtable.html
- https://johnnysswlab.com/the-true-price-of-virtual-functions-in-c/
- https://www.cooldoger.com/2020/06/c-virtual-function.html
- https://alschwalm.com/blog/static/2016/12/17/reversing-c-virtual-functions/
- https://alschwalm.com/blog/static/2017/01/24/reversing-c-virtual-functions-part-2-2/

![Image!](https://i.sstatic.net/7VC6T.png "Image")
![Image!](https://alschwalm.com/blog/static/images/2016/12/2016-12-14-194724_796x759_scrot.png "Image")
![Image!](https://johnnysswlab.com/wp-content/uploads/vtable.gif "Image")
![Image!](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTSfJN7ifKdj8l3wySkjk37tdLZek7ahOJ61r_Dt7yVx3smPxpNvf88-5KVvVSOoY7KL2U&usqp=CAU "Image")
![Image!](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTSfJN7ifKdj8l3wySkjk37tdLZek7ahOJ61r_Dt7yVx3smPxpNvf88-5KVvVSOoY7KL2U&usqp=CAU "Image")
![Image!](https://media.geeksforgeeks.org/wp-content/uploads/20231122182639/vtable-for-derived2-class.webp "Image")
![Image!](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjH5JJpE7w7JBoomruPNfN2eswokLjK59K6z6K9HnSQWf-ONbBghinCRJOWGdbbp0YFBHdI9eB3boEnUTW2xxQUm9Q7e9gLepUfCTjfeyfX84V6xb23s3nhloUYVfnyCejRpjTt2w/s1600/CPP+Blog+-+SimpleInheritance_obj.png "Image")

# C++ : Small string optimization

- https://pvs-studio.com/en/blog/terms/6658/
- https://cppdepend.com/blog/understanding-small-string-optimization-sso-in-stdstring/
- https://devblogs.microsoft.com/oldnewthing/20230803-00/?p=108532
- https://tc-imba.github.io/posts/cpp-sso/
- https://github.com/elliotgoodrich/SSO-23
- https://cpp-optimizations.netlify.app/small_strings/
- https://devblogs.microsoft.com/oldnewthing/20240510-00/?p=109742
- https://www.cppstories.com/2022/sso-cpp20-checks/
- https://joellaity.com/2020/01/31/string.html
- https://www.embeddedrelated.com/showarticle/1519.php

![Image!](https://www.cppstories.com/2022/images/sso_checks.png "Image")








