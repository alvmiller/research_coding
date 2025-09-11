# Linux : Signals

- https://prateeksrivastav598.medium.com/understanding-linux-signals-a-comprehensive-guide-339ecc2d16d4
- https://man7.org/linux/man-pages/man7/signal.7.html
- https://faculty.cs.niu.edu/~hutchins/csci480/signals.htm
- https://www.chromium.org/chromium-os/developer-library/reference/linux-constants/signals/
- https://en.wikipedia.org/wiki/Signal_(IPC)
- https://www.ic.unicamp.br/~celio/mc514/linux/linux_pgsignals.html
- https://priyanka-vedeshwar.medium.com/understanding-signals-in-linux-059c5608eca2
- https://devopedia.org/linux-signals

> SIGINT (Interrupt)
Usage: Sent when the user presses Ctrl+C in the terminal.
> SIGTERM (Termination)
Usage: Requests a process to terminate.
Purpose: Allows a process to perform cleanup before exiting, considered more graceful than SIGKILL.
This guide covers the first fifteen signals commonly used in Linux.
In the next part, we’ll explore the remaining signals and their applications.

> Every signal has a default action. The default may be:
> - ignore
> - terminate
> - terminate and dump core
> - stop or pause the program
> - resume a program paused earlier

```c++
#include <iostream>
#include <signal.h>
#include <unistd.h>

using namespace std;

//signal handler for SIGINT
void signal_handler(int sig){
    if(sig != SIGINT)
        return;
    cout<<"SIGINT caught, Press Ctrl \\ to exit\n";
}

//register signal handler for SIGINT
void install_signal_handler(){
    struct sigaction sa;
    bzero(&sa,sizeof(sa));
    sa.sa_handler = &signal_handler;
    sigaction(SIGINT,&sa,NULL);
}

//block a signal
void block_signal(int sig){
    cout<<"Blocking signal\n";
    sigset_t sigset;
    sigemptyset(&sigset);
    sigaddset(&sigset,sig);
    sigprocmask(SIG_BLOCK,&sigset,NULL);
}

//unblock a signal
void unblock_signal(int sig){
    cout<<"Unblocking signal\n";
    sigset_t sigset;
    sigemptyset(&sigset);
    sigaddset(&sigset,sig);
    sigprocmask(SIG_UNBLOCK,&sigset,NULL);
}

int main(){
    //register a signal hadnler for SIGINT
    install_signal_handler();

    //block SIGINT for a while
    block_signal(SIGINT);
    sleep(5);
    //unblock SIGINT now
    unblock_signal(SIGINT);
    
    while(1);
    
    return 0;
}
```

![Image!](https://devopedia.org/images/article/197/5091.1562685662.png "Image")
![Image!](https://devopedia.org/images/article/197/8283.1562685627.png "Image")
![Image!](https://devopedia.org/images/article/197/9373.1562685696.png "Image")

# Linux :  GDB, strace, ltrace

- https://www.geeksforgeeks.org/linux-unix/gdb-command-in-linux-with-examples/
- https://man7.org/linux/man-pages/man1/gdb.1.html
- https://sourceware.org/gdb/
- https://www.baeldung.com/linux/gdb-debug

- https://man7.org/linux/man-pages/man1/strace.1.html
- https://www.geeksforgeeks.org/linux-unix/strace-command-in-linux-with-examples/
- https://strace.io/
- https://linux.die.net/man/1/strace

- https://man7.org/linux/man-pages/man1/ltrace.1.html
- https://ltrace.org/
- https://linux.die.net/man/1/ltrace
- https://docs.redhat.com/en/documentation/red_hat_developer_toolset/11/html/user_guide/chap-ltrace
- https://labex.io/tutorials/linux-linux-ltrace-command-with-practical-examples-422784
- https://www.tutorialspoint.com/unix_commands/ltrace.htm

# Linux : Dynamic Libraries

- https://medium.com/@bdov_/https-medium-com-bdov-c-dynamic-libraries-what-why-and-how-66cf777019a7
- https://amirrachum.com/shared-libraries/

![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*W2ignMgup4FDUdDeIyjD1A.png "Image")
![Image!](https://miro.medium.com/v2/format:webp/1*7aohEqbwIO5h30eHB0MQQg.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*rFbZvSgbv6joOUYUbuzDiw.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*BtDyLptLojXhkxIX4XkxLQ.png "Image")
![Image!](https://amirrachum.com/images/posts/elf.gif "Image")

# Hash Functions

- https://en.wikipedia.org/wiki/Cryptographic_hash_function
- https://www.geeksforgeeks.org/competitive-programming/cryptography-hash-functions/
- https://www.investopedia.com/news/cryptographic-hash-functions/
- https://csrc.nist.gov/glossary/term/cryptographic_hash_function
- https://www.tutorialspoint.com/cryptography/cryptography_hash_functions.htm

> A cryptographic hash function (CHF) is a hash algorithm (a map of an arbitrary binary string to a binary string with a fixed size of 
n bits) that has special properties desirable for a cryptographic application
>
> A cryptographic hash function must be able to withstand all known types of cryptanalytic attack. In theoretical cryptography, the security level of a cryptographic hash function has been defined using the following properties:
> - Pre-image resistance
> Given a hash value h, it should be difficult to find any message m such that h = hash(m).
> This concept is related to that of a one-way function. Functions that lack this property are vulnerable to preimage attacks.
> - Second pre-image resistance
> Given an input m1, it should be difficult to find a different input m2 such that hash(m1) = hash(m2).
> This property is sometimes referred to as weak collision resistance. Functions that lack this property are vulnerable to second-preimage attacks.
> - Collision resistance
> It should be difficult to find two different messages m1 and m2 such that hash(m1) = hash(m2).
> Such a pair is called a cryptographic hash collision. This property is sometimes referred to as strong collision resistance.
> It requires a hash value at least twice as long as that required for pre-image resistance; otherwise, collisions may be found by a birthday attack.[5]

![Image!](https://en.wikipedia.org/wiki/File:Cryptographic_Hash_Function.svg "Image")

# Digital signature

- https://www.sectigo.com/resource-library/how-digital-signatures-work
- https://en.wikipedia.org/wiki/Digital_signature
- https://www.docusign.com/products/electronic-signature/learn/digital-signature-faq
- https://www.okta.com/identity-101/digital-signature/
- https://penneo.com/blog/digital-signatures/
- https://oneflow.com/digital-signatures/

![Image!](https://www.sectigo.com/uploads/images/Digital-Signature-Illustration.png "Image")
![Image!](https://en.wikipedia.org/wiki/Digital_signature#/media/File:Private_key_signing.svg "Image")
![Image!](https://oneflow.com/app/uploads/2023/11/231106_Electronic-signature-1-1440x810.png "Image")

# Optimize I/O operations

- https://medium.com/@addys_page/understanding-storage-efficiency-the-foundation-of-big-data-optimization-with-parquet-and-orc-5b8b57318ad7
- https://uneos.au/understanding-iops-and-optimising-storage-performance/
- https://moldstud.com/articles/p-designing-efficient-data-flow-in-io-systems-strategies-and-tools-for-optimal-performance
- https://blog.paessler.com/iops-vs-throughput-what-you-need-to-know-for-optimal-storage-performance

```c
/*

To optimize I/O operations for a high-throughput system, implement strategies like upgrading to NVMe SSDs, utilizing RAID 0 for data striping, implementing caching with RAM or faster SSDs, adopting tiered storage, optimizing the network connection to the storage, using columnar storage formats like Parquet for analytics, and employing data compression. Additionally, leverage asynchronous I/O and batching to reduce latency and maximize throughput, and use database indexes to speed up data retrieval. 
Here are detailed strategies for optimizing I/O operations:
Hardware & Storage Solutions
Upgrade to High-Performance Storage:
Replace traditional Hard Disk Drives (HDDs) with Solid State Drives (SSDs), especially NVMe SSDs, which offer superior speeds and lower latency for both read and write operations. 
Implement RAID 0:
Use RAID 0 (striping) to distribute data across multiple disks, increasing the overall IOPS by allowing parallel access. 
Tiered Storage:
Store frequently accessed data on high-performance tiers (SSDs) and less critical data on slower, larger-capacity tiers (HDDs) to optimize performance where it matters most. 
Utilize Caching:
Implement caching solutions to store frequently accessed data in faster storage media, such as RAM or SSDs, to significantly reduce the number of I/O operations to slower disks. 
Use Columnar Storage:
For analytical workloads, adopt columnar formats like Parquet or ORC, which store data by column rather than by row, improving data locality, compression, and query speed. 
Employ Object Storage:
For petabytes of data, consider object storage for its high scalability and cost-effectiveness compared to traditional file or block storage. 
Software & Data Strategies
Implement Data Compression:
Use lossless compression to reduce data size, requiring less bandwidth and fewer I/O operations for transfer and storage. 
Use Database Indexes:
Create indexes on your database tables to allow the system to quickly locate and retrieve specific records, bypassing the need to scan through every row. 
Choose Asynchronous I/O:
For I/O-bound tasks, use asynchronous operations, which allow the application to continue processing other tasks while I/O operations are in progress, improving overall responsiveness. 
Perform Batching:
Group multiple small I/O requests into larger, single batches to reduce the overhead of individual I/O operations. 
Network & Monitoring
Optimize the Storage Network:
Ensure the network infrastructure, especially for SAN and NAS systems, uses high-speed connections (e.g., Fibre Channel or 10GbE) to minimize latency and maximize throughput. 
Monitor and Profile:
Continuously monitor your storage system's performance using key metrics like IOPS, throughput, and latency to identify bottlenecks and dynamically adjust resources.
*/
```

# C : Inline

- https://www.geeksforgeeks.org/c/inline-function-in-c/
- https://en.wikipedia.org/wiki/Inline_(C_and_C%2B%2B)
- https://en.cppreference.com/w/c/language/inline.html
- https://gcc.gnu.org/onlinedocs/gcc/Inline.html
- https://isocpp.org/wiki/faq/inline-functions
- https://wnjoon.github.io/2025/03/17/inline-function-c/
- https://www.greenend.org.uk/rjk/tech/inline.html
- https://learn.microsoft.com/en-us/cpp/cpp/inline-functions-cpp?view=msvc-170

> In C, an inline function is a type of function where the compiler replaces the function call with its actual code of the function, invoked through the usual function call mechanism. This can improve performance by reducing the overhead of function calls. The inline keyword is used to declare such functions, which are normally small and frequently called.

```c
inline return_type function_name(parameters) {
   // Function body
}
```

# C : Generic Macro

- https://en.cppreference.com/w/c/language/generic.html
- https://learn.microsoft.com/en-us/cpp/c-language/generic-selection?view=msvc-170
- https://medium.com/@andrew_johnson_4/understanding-c-generic-selections-53ec66ff71e4
- https://www.chiark.greenend.org.uk/~sgtatham/quasiblog/c11-generic/
- https://www.ibm.com/docs/en/zos/2.4.0?topic=expressions-generic-selection-c11
- https://www.geeksforgeeks.org/c/_generic-keyword-c/
- https://man7.org/linux/man-pages/man3/_Generic.3.html
- https://andrewjohnson4.substack.com/p/understanding-c-generic-selections
- https://www.ibm.com/docs/de/xl-c-aix/13.1.2?topic=expressions-generic-selection-c11
- https://dev.to/pauljlucas/generic-in-c-i48
- http://www.robertgamble.net/2012/01/c11-generic-selections.html

```c
// -std=gnu11

#include <stdio.h>

#define TYPE_OF(x) _Generic((x), \
    int: "int", \
    float: "float", \
    double: "double", \
    char*: "string", \
    default: "unknown type" \
)
int main() {
    int i = 42;
    float f = 3.14f;
    double d = 2.71828;
    char *s = "Hello, World!";
    
    printf("Type of i: %s\n", TYPE_OF(i));
    printf("Type of f: %s\n", TYPE_OF(f));
    printf("Type of d: %s\n", TYPE_OF(d));
    printf("Type of s: %s\n", TYPE_OF(s));
    return 0;
}
```

```c
#include <stdio.h>
int main(void)
{
    // _Generic keyword acts as a switch that chooses
    // operation based on data type of argument.
    printf("%d\n", _Generic(1.0L, float : 1, double : 2,
                            long double : 3, default : 0));
  
    printf("%d\n", _Generic(1L, float : 1, double : 2,
                            long double : 3, default : 0));
  
    printf("%d\n", _Generic(1.0L, float : 1, double : 2,
                            long double : 3));
    return 0;
}
```

# C : Macros and its types

- https://www.geeksforgeeks.org/c/macros-and-its-types-in-c-cpp/
- https://stackoverflow.com/questions/4364971/and-in-macros
- https://gcc.gnu.org/onlinedocs/cpp/Concatenation.html
- https://www.w3schools.com/c/c_macros.php
- https://www.cs.yale.edu/homes/aspnes/pinewiki/C(2f)Macros.html
- https://www.geeksforgeeks.org/c/stringizing-and-token-pasting-operators-in-c/
- https://www.ibm.com/docs/en/i/7.6.0?topic=directive-function-like-macros

```c
#define DEBUG

int main() {
  #ifdef DEBUG
    printf("Debug mode is ON\n");
  #endif
  return 0;
}
```

```c
#define Square(x) ((x)*(x))
#define FOO (12)
```

```c
   1 #include <stdio.h>
   2 
   3 #define Warning(...) fprintf(stderr, __VA_ARGS__)
   4 
   5 int
   6 main(int argc, char **argv)
   7 {
   8     Warning("%s: this program contains no useful code\n", argv[0]);
   9     
  10     return 1;
  11 }
```

```c
#define NoisyInc2(x) (puts("Incrementing " #x), x++)
#define FakeArray(n) fakeArrayVariableNumber ## n
```







