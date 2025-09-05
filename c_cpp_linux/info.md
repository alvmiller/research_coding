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
    
    if (n == 0 || dst == NULL || src == NULL) {
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

# C : Matrix Multiplication

- https://www.geeksforgeeks.org/c/c-matrix-multiplication/
- https://www.programiz.com/c-programming/examples/matrix-multiplication
- https://www.programiz.com/c-programming/examples/matrix-multiplication-function
- https://en.wikipedia.org/wiki/Matrix_multiplication
- https://www.wscubetech.com/resources/c-programming/programs/matrix-multiplication
- https://www.geeksforgeeks.org/cpp/cpp-matrix-multiplication/
- https://www.codewithc.com/matrix-multiplication-in-c/?amp=1
- https://www.w3resource.com/c-programming-exercises/array/c-array-exercise-21.php#google_vignette
- https://www.scaler.com/topics/matrix-multiplication-in-c/
- https://www.startertutorials.com/blog/c-program-add-multiply-two-compatible-matrices.html
- https://docs.vultr.com/clang/examples/multiply-two-matrices-using-multi-dimensional-arrays
- https://labex.io/tutorials/c-multiply-two-matrices-in-c-435191
- https://www.sanfoundry.com/c-program-perform-matrix-multiplication/#google_vignette
- https://www.c-sharpcorner.com/article/2d-array-with-matrix-multiplication-in-c-programming/

```c
// C program to multiply two matrices
#include <stdio.h>
#include <stdlib.h>

// matrix dimensions so that we dont have to pass them as
// parametersmat1[R1][C1] and mat2[R2][C2]
#define R1 2 // number of rows in Matrix-1
#define C1 2 // number of columns in Matrix-1
#define R2 2 // number of rows in Matrix-2
#define C2 3 // number of columns in Matrix-2

void multiplyMatrix(int m1[][C1], int m2[][C2])
{
    int result[R1][C2];

    printf("Resultant Matrix is:\n");

    for (int i = 0; i < R1; i++) {
        for (int j = 0; j < C2; j++) {
            result[i][j] = 0;

            for (int k = 0; k < R2; k++) {
                result[i][j] += m1[i][k] * m2[k][j];
            }

            printf("%d\t", result[i][j]);
        }

        printf("\n");
    }
}

// Driver code
int main()
{
    // R1 = 4, C1 = 4 and R2 = 4, C2 = 4 (Update these
    // values in MACROs)
    int m1[R1][C1] = { { 1, 1 }, { 2, 2 } };

    int m2[R2][C2] = { { 1, 1, 1 }, { 2, 2, 2 } };

    // if coloumn of m1 not equal to rows of m2
    if (C1 != R2) {
        printf("The number of columns in Matrix-1 must be "
               "equal to the number of rows in "
               "Matrix-2\n");
        printf("Please update MACROs value according to "
               "your array dimension in "
               "#define section\n");

        exit(EXIT_FAILURE);
    }

    // Function call
    multiplyMatrix(m1, m2);

    return 0;
}
```

# C / C++ : Synchronization

- https://www.geeksforgeeks.org/linux-unix/mutex-lock-for-linux-thread-synchronization/
- https://en.cppreference.com/w/cpp/thread/mutex.html
- https://www.codequoi.com/en/threads-mutexes-and-concurrent-programming-in-c/
- https://en.wikipedia.org/wiki/Lock_(computer_science)
- https://www.geeksforgeeks.org/c/use-posix-semaphores-c/
- https://docs.oracle.com/cd/E19683-01/806-6867/6jfpgdcnj/index.html
- https://en.wikipedia.org/wiki/Spinlock
- https://wiki.osdev.org/Spinlock
- https://dev.to/ivinieon/features-and-differences-of-spinlock-mutex-and-semaphore-3l1m
- https://medium.com/@lakshminath_alamuru/mutex-vs-semaphore-and-its-internal-implementation-1500a9d58242
- https://www.geeksforgeeks.org/operating-systems/mutex-vs-semaphore/
- https://medium.com/@everythingismindgame/synchronization-using-condition-variable-in-c-dc4d427d1af6
- https://en.cppreference.com/w/cpp/thread/condition_variable.html
- https://www.geeksforgeeks.org/linux-unix/condition-wait-signal-multi-threading/
- https://medium.com/@jtchen2k/using-c-conditional-variables-for-thread-synchronization-87fb1ac3601c
- https://en.wikipedia.org/wiki/Monitor_(synchronization)

- https://courses.cs.washington.edu/courses/cse451/24wi/lectures/08-24wi%20Semaphores,%20Condition%20Variables%20&%20Monitors.pdf

![Image!](https://miro.medium.com/v2/resize:fit:1400/1*X0V54LQtlH0SpUvdjcsk_Q.png "Image")

```c++
#include <cstdlib>
#include <iostream>

#include <thread>
#include <mutex>
#include <condition_variable>
#include <chrono>

#define STORAGE_MIN 10
#define STORAGE_MAX 20

int storage = STORAGE_MIN;

std::mutex globalMutex;
std::condition_variable condition;

void consumer()
{
	std::cout << "[CONSUMER] thread started" << std::endl;
	int toConsume = 0;
	
	while(true) {
		std::unique_lock<std::mutex> lock(globalMutex);
		if (storage < STORAGE_MAX) {
			condition.wait(lock , []{return storage >= STORAGE_MAX;} ); // Атомарно _отпускает мьютекс_ и сразу же блокирует поток
			toConsume = storage-STORAGE_MIN;
			std::cout << "[CONSUMER] storage is maximum, consuming "
				      << toConsume << std::endl;
		}
		storage -= toConsume;
		std::cout << "[CONSUMER] storage = " << storage << std::endl;
	}
}

/* Функция потока производителя */
void producer()
{
	std::cout << "[PRODUCER] thread started" << std::endl;

	while (true) {
		std::this_thread::sleep_for(std::chrono::milliseconds(200));
		std::unique_lock<std::mutex> lock(globalMutex);
		++storage;
		std::cout << "[PRODUCER] storage = " <<  storage << std::endl;
		if (storage >= STORAGE_MAX) {
			std::cout << "[PRODUCER] storage maximum" << std::endl;
			condition.notify_one();
		}
	}
}

int main(int argc, char *argv[])
{
	std::thread thProducer(producer);
	std::thread thConsumer(consumer);
	
	thProducer.join();
	thConsumer.join();
	
	return 0;
}
```

# C / C++ : Atomic

- https://en.cppreference.com/w/cpp/atomic/atomic.html
- https://cplusplus.com/reference/atomic/
- https://www.geeksforgeeks.org/cpp/cpp-11-atomic-header/
- https://medium.com/developer-rants/c-threads-and-atomic-variables-oversimplified-b37bbbe3f2e6
- https://en.cppreference.com/w/cpp/header/atomic.html
- https://www.modernescpp.com/index.php/atomic-ref/
- https://cplusplus.com/reference/atomic/atomic/atomic/

```c++
#include <atomic>
#include <iostream>
#include <thread>
using namespace std;

atomic<int> counter(0);

void increment_counter(int id)
{
    for (int i = 0; i < 100000; ++i) {
        // Increment counter atomically
        counter.fetch_add(1);
    }
}

int main()
{
    thread t1(increment_counter, 1);
    thread t2(increment_counter, 2);

    t1.join();
    t2.join();

    cout << "Counter: " << counter.load() << std::endl;
    return 0;
}
```

# C++ : thread pool

- https://www.geeksforgeeks.org/cpp/thread-pool-in-cpp/
- https://medium.com/@bhushanrane1992/getting-started-with-c-thread-pool-b6d1102da99a
- https://nixiz.github.io/yazilim-notlari/2023/10/07/thread_pool-en
- https://matgomes.com/thread-pools-cpp-with-queues/
- https://dev.to/ish4n10/making-a-thread-pool-in-c-from-scratch-bnm
- https://arxiv.org/html/2407.15805v2
- https://blog.eiler.eu/posts/20210512/

> The Thread Pool in C++ is used to manage and efficiently resort to a group (or pool) of threads. Instead of creating threads again and again for each task and then later destroying them, what a thread pool does is it maintains a set of pre-created threads now these threads can be reused again to do many tasks concurrently. By using this approach we can minimize the overhead that costs us due to the creation and destruction of threads. This makes our application more efficient.

![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*bW8heZuEzTDPpw1HmpNZmA.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*ZbYw8sUijxHYi3G8TmtXOw.png "Image")
![Image!](https://nixiz.github.io/yazilim-notlari/assets/img/use_future_high_order_sequence_diagram.png "Image")

```c++
#include <condition_variable>
#include <functional>
#include <iostream>
#include <mutex>
#include <queue>
#include <thread>
using namespace std;

// Class that represents a simple thread pool
class ThreadPool {
public:
    // // Constructor to creates a thread pool with given
    // number of threads
    ThreadPool(size_t num_threads
               = thread::hardware_concurrency())
    {

        // Creating worker threads
        for (size_t i = 0; i < num_threads; ++i) {
            threads_.emplace_back([this] {
                while (true) {
                    function<void()> task;
                    // The reason for putting the below code
                    // here is to unlock the queue before
                    // executing the task so that other
                    // threads can perform enqueue tasks
                    {
                        // Locking the queue so that data
                        // can be shared safely
                        unique_lock<mutex> lock(
                            queue_mutex_);

                        // Waiting until there is a task to
                        // execute or the pool is stopped
                        cv_.wait(lock, [this] {
                            return !tasks_.empty() || stop_;
                        });

                        // exit the thread in case the pool
                        // is stopped and there are no tasks
                        if (stop_ && tasks_.empty()) {
                            return;
                        }

                        // Get the next task from the queue
                        task = move(tasks_.front());
                        tasks_.pop();
                    }

                    task();
                }
            });
        }
    }

    // Destructor to stop the thread pool
    ~ThreadPool()
    {
        {
            // Lock the queue to update the stop flag safely
            unique_lock<mutex> lock(queue_mutex_);
            stop_ = true;
        }

        // Notify all threads
        cv_.notify_all();

        // Joining all worker threads to ensure they have
        // completed their tasks
        for (auto& thread : threads_) {
            thread.join();
        }
    }

    // Enqueue task for execution by the thread pool
    void enqueue(function<void()> task)
    {
        {
            unique_lock<std::mutex> lock(queue_mutex_);
            tasks_.emplace(move(task));
        }
        cv_.notify_one();
    }

private:
    // Vector to store worker threads
    vector<thread> threads_;

    // Queue of tasks
    queue<function<void()> > tasks_;

    // Mutex to synchronize access to shared data
    mutex queue_mutex_;

    // Condition variable to signal changes in the state of
    // the tasks queue
    condition_variable cv_;

    // Flag to indicate whether the thread pool should stop
    // or not
    bool stop_ = false;
};

int main()
{
    // Create a thread pool with 4 threads
    ThreadPool pool(4);

    // Enqueue tasks for execution
    for (int i = 0; i < 5; ++i) {
        pool.enqueue([i] {
            cout << "Task " << i << " is running on thread "
                 << this_thread::get_id() << endl;
            // Simulate some work
            this_thread::sleep_for(
                chrono::milliseconds(100));
        });
    }

    return 0;
}
```

# C++ : Iterator Invalidation

- https://www.geeksforgeeks.org/cpp/iterator-invalidation-cpp/
- https://lightcone.medium.com/iterator-invalidation-in-modern-c-ca0f3c161c5f
- https://learnmoderncpp.com/2024/09/04/understanding-iterator-invalidation/
- https://www.tutorialspoint.com/iterator-invalidation-in-cplusplus
- https://dotnettutorials.net/lesson/iterator-invalidation-in-cpp/
- https://intellipaat.com/blog/iterator-invalidation-cpp/
- https://labex.io/tutorials/cpp-how-to-resolve-iterator-lifetime-issues-419975

> When the container to which an Iterator points changes shape internally, i.e. when elements are moved from one position to another, and the initial iterator still points to the old invalid location, then it is called Iterator invalidation.

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    vector<int> v = { 1, 5, 10, 15, 20 };

    // Changing vector while iterating over it
    // (This causes iterator invalidation)
    for (auto it = v.begin(); it != v.end(); it++)
        if ((*it) == 5)
            v.push_back(-1);

    for (auto it = v.begin(); it != v.end(); it++)
        cout << (*it) << " ";
    return 0;
}
```

# C++ : STL Algorithms

- https://en.cppreference.com/w/cpp/algorithm.html
- https://www.geeksforgeeks.org/cpp/c-magicians-stl-algorithms/
- https://cplusplus.com/reference/algorithm/
- https://www.w3schools.com/cpp/cpp_ref_algorithm.asp
- https://www.quantstart.com/articles/C-Standard-Template-Library-Part-III-Algorithms/
- https://www.sandordargo.com/blog/2019/01/30/stl-algos-intro
- https://medium.com/@shrutipokale2016/c-stl-algorithms-one-shot-81ecede7657d
- https://www.geeksforgeeks.org/cpp/algorithms-library-c-stl/

> 1. Searching Algorithms
> 2. Sorting and Rearranging Algorithms
> 3. Manipulation Algorithms
> 4. Counting and Comparing Algorithms

# C++ : STL Iterators

- https://www.geeksforgeeks.org/cpp/iterators-c-stl/
- https://www.w3schools.com/cpp/cpp_iterators.asp
- https://en.cppreference.com/w/cpp/iterator.html
- https://www.programiz.com/cpp-programming/iterators
- https://cplusplus.com/reference/iterator/


> - Input
> - Output
> - Forward
> - Bidirectional
> - Random Access

![Image!](https://www.programiz.com/sites/tutorial2program/files/vector-iterator.png "Image")

```c++
#include <iostream>
#include<vector>
using namespace std;

int main() {
    vector <string> languages = {"Python", "C++", "Java"};
    vector<string>::iterator itr;

    // iterate over all elements
    for (itr = languages.begin(); itr != languages.end(); itr++) {
        cout << *itr << ", ";
    }
    return 0;
}
```
