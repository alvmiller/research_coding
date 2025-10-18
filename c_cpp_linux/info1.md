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

- https://www.f5.com/glossary/quic-http3
- https://kinsta.com/blog/http3/
- https://medium.com/@gtamilarasan/a-closer-look-at-http3-quic-1b2b97a5cd50
- https://www.haproxy.com/blog/choosing-the-right-transport-protocol-tcp-vs-udp-vs-quic
- https://www.geeksforgeeks.org/computer-networks/what-is-quic-and-http-3/
- https://http3-explained.haxx.se/en/specs
- https://en.wikipedia.org/wiki/QUIC
- https://www.stackscale.com/blog/http3/
- https://www.debugbear.com/blog/http3-quic-protocol-guide
- https://www.cdnetworks.com/blog/media-delivery/what-is-quic/

> QUIC is a transport protocol that runs on top of User Datagram Protocol (UDP)
> and serves as the foundation for HTTP/3, which is the latest version of the Hypertext Transfer Protocol.
> QUIC uses UDP instead of the older Transmission Control Protocol (TCP) to improve web performance
> by reducing latency and eliminating head-of-line blocking, a problem where a single lost packet can delay an entire connection.  
> How they work together
> - QUIC on UDP: QUIC was designed to replace TCP and is built on top of UDP, which is less reliable but faster and more flexible for this purpose. 
> - HTTP/3 on QUIC: HTTP/3 is the application layer protocol that uses QUIC as its transport layer, moving away from the TCP+HTTP/2 model. 
> - Built-in encryption: Unlike previous versions that added TLS for security, QUIC integrates Transport Layer Security (TLS) encryption from the start, combining the handshake process and speeding up connection establishment. 
> - Eliminating head-of-line blocking: In TCP, a lost packet can halt all streams on a connection. QUIC handles multiple streams independently, so a delay in one stream does not impact others. 
> - Faster connections: QUIC enables faster connection establishment with 0-RTT and 1-RTT handshakes, meaning the client and server can often exchange data sooner. 
> Why this matters
> - Faster performance: The combination of UDP, QUIC, and HTTP/3 leads to faster page load times and a smoother user experience. 
> - Improved mobile support: QUIC's ability to handle network changes smoothly is ideal for mobile users who frequently switch between Wi-Fi and cellular networks. 
> - Increased efficiency: By streamlining the connection and security setup, QUIC and HTTP/3 make web communication more efficient. 

![Image!](https://www.debugbear.com/assets/images/http11-2-3-comparison-88d3a12d6cc3f5422c3700a7f2f8c76b.jpg "Image")
![Image!](https://www.debugbear.com/assets/images/tlsv13-vs-quic-handshake-d9672525e7ba84248647581b05234089.jpg "Image")
![Image!](https://www.debugbear.com/assets/images/http2-vs-http3-roundtrips-1b14a7c6b6f7badbd345f67895dbf142.jpg "Image")
![Image!](https://www.debugbear.com/assets/images/http2-vs-quic-multiplexing-36d594c1c356cae410fe60cfc2945d91.jpg "Image")
![Image!](https://www.f5.com/content/dam/f5-com/nginx-import/HTTP-v1-v2-v3-stacks.png "Image")
![Image!](https://www.f5.com/content/dam/f5-com/nginx-import/primer-QUIC-networking-encryption_stream-anatomy.png "Image")

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
