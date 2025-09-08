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
    
    if (n == 0 || dst == NULL || src == NULL || src == dst) {
        return dst;
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

    return dst;
}

int main()
{
    const char *src0 = "01234567890123456789";
    const char *src1 = "012345678";
    const char *src2 = "01234567";
    const char *src3 = "01237";
    const char *arr[] = {src0, src1, src2, src3};
    char dst[100] = {};
    const char *src = NULL;
    const size_t N = sizeof(arr)/sizeof(arr[0]);

    printf("sizeof(uintptr_t) = %llu\n", (unsigned long long)sizeof(uintptr_t));
    printf("sizeof(dst)/sizeof(dst[0]) = %llu\n", (unsigned long long)sizeof(dst)/sizeof(dst[0]));
    printf("sizeof(arr)/sizeof(arr[0]) = %llu\n", (unsigned long long)sizeof(arr)/sizeof(arr[0]));
    printf("\n");

    for (size_t i = 0; i < N; ++i) {
        memset(dst, '\0', sizeof(dst)/sizeof(dst[0]));
        src = arr[i];
        printf("len(src) = %lu | src = %s\n", strlen(src), src);
        (void)tmp_memcpy(dst, src, strlen(src));
        printf("len(dst) = %lu | dst = %s\n", strlen(dst), dst);
        if (strlen(src) != strlen(dst) || memcmp(src, dst, strlen(src)) != 0) {
            printf("\tError!\n");
        }
        printf("\n");
    }

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

![Image!](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8e/Diamond_inheritance.svg/270px-Diamond_inheritance.svg.png "Image")

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

# C : angle between vectors

- vector00s_angle.h
```c
#pragma once

#ifndef VECTORS_ANGLE_H
#define VECTORS_ANGLE_H

/*
   Used only 2D integer coordinates (not floating point)
   
   We always can move vector's start dot to (0,0)
   and recalculate vector's coordinates
   
   A(Ax,Ay) B(Bx,By) -> V(Vx,Vy) : Vx=Bx-Ax Vy=By-Ay
   
   (v1,v2)=|v1||v2|cosL=v1x*v2x+v1y*v2y
   [v1,v2]=|v1||v2|sinL=v1x*v2y-v1y*v2x
   tgL=sinL/cosL=(v1x*v2y-v1y*v2x)/(v1x*v2x+v1y*v2y)=RES1/RES2
   L=atan2(RES1,RES2)
   
   Currently, each vector should be started from (0,0)
   
   reset; gcc -Werror vector00s_angle.c main.c -lm -o main; ./main
 */

typedef enum angle_type_t {
  ANGLE_RADIANS_TYPE,
  ANGLE_DEGREES_TYPE
} angle_type;

/* Currently, each vector should be started from (0,0) */
typedef struct vector00_t {
    int x;
    int y;
} vector00;

/* Currently, each vector should be started from (0,0) */
int get_angle_between_vector00s(
    const vector00* restrict v1,
    const vector00* restrict v2,
    angle_type type,
    double *res);

#endif /* !VECTORS_ANGLE_H */
```

- vector00s_angle.c
```c
#include <assert.h>
#include <math.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

#include "vector00s_angle.h"

#define OVERFLOW_ERROR_CODE (-2)

#define IS_INT_TYPE(x) _Generic((x), int: true, default: false)
#define CHECK_VECTOR00_COORDINATES_TYPE() \
    ({ \
        typeof(((vector00 *)NULL)->x) _x; \
        typeof(((vector00 *)NULL)->y) _y; \
        _Static_assert(IS_INT_TYPE(_x), "Bad type of object (field x)"); \
        _Static_assert(IS_INT_TYPE(_y), "Bad type of object (field y)"); \
    })

static inline double convert_angle_radians_to_degrees(double radians)
{
    return radians * 180.0 / M_PI;
}

static inline double calculate_angle_in_radians(int cross_product, int dot_product)
{
    return atan2(cross_product, dot_product);
}

static inline int calculate_cross_product(const vector00* restrict v1, const vector00* restrict v2)
{
    int cross_product = 0;
    int tmp1 = 0;
    int tmp2 = 0;

    // (v1->x * v2->y - v1->y * v2->x) ~ sinA
    if (__builtin_mul_overflow(v1->x, v2->y, &tmp1)
     || __builtin_mul_overflow(v1->y, v2->x, &tmp2)
     || __builtin_sub_overflow(tmp1, tmp2, &cross_product)) {
        exit(OVERFLOW_ERROR_CODE);
    }

    return cross_product;
}

static inline int calculate_dot_product(const vector00* restrict v1, const vector00* restrict v2)
{
    int dot_product = 0;
    int tmp1 = 0;
    int tmp2 = 0;

    // (v1->x * v2->x + v1->y * v2->y) ~ cosA
    if (__builtin_mul_overflow(v1->x, v2->x, &tmp1)
     || __builtin_mul_overflow(v1->y, v2->y, &tmp2)
     || __builtin_add_overflow(tmp1, tmp2, &dot_product)) {
        exit(OVERFLOW_ERROR_CODE);
    }

    return dot_product;
}

static inline bool is_equal(const vector00* restrict v1, const vector00* restrict v2)
{
    if (v1 == v2) {
        return true;
    }
    if (v1->x == v2->x && v1->y == v2->y) {
        return true;
    }
    return false;
}

int get_angle_between_vector00s(
    const vector00* restrict v1,
    const vector00* restrict v2,
    angle_type type,
    double *res)
{
    double angle = 0.0;
    int cross_product = 0;
    int dot_product = 0;

    CHECK_VECTOR00_COORDINATES_TYPE();

    if (v1 == NULL
     || v2 == NULL
     || res == NULL
     || (type != ANGLE_RADIANS_TYPE && type != ANGLE_DEGREES_TYPE)) {
        return -1;
    }
    
    if (is_equal(v1, v2)) {
        *res = 0.0;
        return 0;
    }
    /** @TODO: Check that both vectors are in the same line */

    cross_product = calculate_cross_product(v1, v2);
    dot_product = calculate_dot_product(v1, v2);
    angle = calculate_angle_in_radians(cross_product, dot_product);

    if (type == ANGLE_DEGREES_TYPE) {
        angle = convert_angle_radians_to_degrees(angle);
    }

    *res = angle;
    return 0;
}
```

- main.c
```c
#include <assert.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>

#include "vector00s_angle.h"

int main()
{
    int res = -1;
    const double epsilon = 0.0001;
    double angle = 0.0;
    const vector00 va = { .x = 0, .y = 2 };
    const vector00 vb = { .x = 2, .y = 0 };
    angle_type type = ANGLE_DEGREES_TYPE;

    res = get_angle_between_vector00s(&va, &vb, ANGLE_DEGREES_TYPE, &angle);
    if (res != 0) {
        printf("Call failed (1)!\n");
        exit(1);
    }
    printf("Angle (1) = %lf\n", angle);
    assert(fabs(angle - (-90.0)) < epsilon);

    res = get_angle_between_vector00s(&vb, &va, ANGLE_DEGREES_TYPE, &angle);
    if (res != 0) {
        printf("Call failed (2)!\n");
        exit(2);
    }
    printf("Angle (2) = %lf\n", angle);
    assert(fabs(angle - 90.0) < epsilon);

    return 0;
}
```
