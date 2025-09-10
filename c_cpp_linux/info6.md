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




