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


