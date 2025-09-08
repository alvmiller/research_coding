# C , Bash : Swap 2 files

- https://unix.stackexchange.com/questions/32894/best-way-to-swap-filenames
- https://stackoverflow.com/questions/1987303/how-to-swap-filenames-in-unix
- https://askubuntu.com/questions/983324/script-to-swap-the-names-of-two-files
- https://lwn.net/Articles/569134/
- https://www.ibm.com/docs/sr/aix/7.1.0?topic=ks-file-name-substitution-in-korn-shell-posix-shell

```sh
mv file .phfile
mv file_1 file
mv .phfile file
```

```sh
touch f1
touch f2
echo 1 > f1 
echo 2 > f2
cat f1 
cat f2 
dd if=/dev/zero of=filename bs=$((1024*1024)) count=$((10*1024))
pwd
gcc main.c 
./a.out 
cat f1 
cat f2
```

```c
#define _GNU_SOURCE
#include <stdio.h>
#include <fcntl.h>
#include <errno.h>

int main()
{
    printf("Hello World\n");
    int olddirfd = 0;
    int newdirfd = 0;
    int ret = renameat2(
	    olddirfd,
	    "/home/user1/linux_src/f1", 
	    newdirfd, 
	    "/home/user1/linux_src/f2",
	    RENAME_EXCHANGE);
    if (ret != 0) perror("renameat2 error");

    return 0;
}
```

# C++ : SOLID , KISS

- https://medium.com/@oleksandra_shershen/solid-principles-implementation-and-examples-in-c-99f0d7e3e868
- https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/
- https://github.com/fx-biocoder/solid-in-cpp
- https://medium.com/@sureshjangale137/solid-design-principles-with-examples-e6e8baf5f85f
- https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design
- https://www.linkedin.com/pulse/applying-solid-principles-c-yamil-garcia
- https://nexwebsites.com/blog/solid-design-principles/
- https://www.geeksforgeeks.org/software-engineering/kiss-principle-in-software-development/
- https://codesignal.com/learn/courses/applying-clean-code-principles-in-cpp/lessons/applying-the-kiss-principle-for-simplicity-in-code-design
- https://codefinity.com/blog/The-KISS-Principle
- https://dev.to/kwereutosu/the-k-i-s-s-principle-in-programming-1jfg
- https://www.simplifycpp.org/?id=a0210
- https://medium.com/@reetesh043/embracing-simplicity-in-coding-the-kiss-principle-bbaef4c9a99c
- https://en.wikipedia.org/wiki/KISS_principle
- https://en.wikipedia.org/wiki/SOLID

> S — Single Responsibility Principle (SRP)
> O — Open-Closed Principle (OCP)
> L — Liskov Substitution Principle (LSP)
> I — Interface Segregation Principle (ISP)
> D — Dependency Inversion Principle (DIP)
>
> Single Responsibility Principle (SRP)
The Single Responsibility Principle (SRP) is one of the five SOLID principles of object-oriented design. It states that a class should have only one responsibility, which means it should have only one reason to change. This principle helps in reducing the coupling between classes and makes the code more maintainable and scalable. In C++, SRP can be implemented by dividing the responsibilities of a class into smaller, more focused classes. Each class should be responsible for only one task and should have no other reason to change. This makes the code easier to understand, modify and test.
>
> Open-Closed Principle (OCP)
The Open-Closed Principle (OCP) is one of the five SOLID principles of object-oriented design. It states that a class should be open for extension but closed for modification. This principle helps in making the code more maintainable and flexible. In C++, OCP can be implemented by creating classes that can be extended by adding new functionality without modifying the existing code. This is achieved by using inheritance, polymorphism, and interfaces. By following this principle, we can make the code more modular and less prone to bugs.
>
> Liskov Substitution Principle (LSP)
The Liskov Substitution Principle (LSP) is one of the SOLID principles of object-oriented design. It states that objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program. In other words, a subclass should be able to be substituted for its parent class without causing any errors or unexpected behavior. In C++, LSP can be implemented by ensuring that subclasses adhere to the same interface and behavior as their parent class. This allows us to use a subclass object wherever we would normally use a parent class object.
>
> Interface Segregation Principle (ISP)
The Interface Segregation Principle (ISP) is one of the SOLID principles of object-oriented design. It states that a client should not be forced to depend on interfaces that it does not use. In other words, a class should not have to implement methods that it does not need. In C++, ISP can be implemented by breaking down larger interfaces into smaller, more specific interfaces. This allows clients to only depend on the specific methods they need, rather than being forced to implement unnecessary methods.
>
> Dependency Inversion Principle (DIP)
The Dependency Inversion Principle (DIP) is one of the SOLID principles of object-oriented design. It states that high-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions. In C++, DIP can be implemented by using interfaces or abstract classes to decouple high-level modules from low-level modules. This allows for more flexibility in the design, as different implementations of the same interface can be easily swapped out without affecting the overall structure of the program.

> The KISS principle in Software Development has several implications:
> - Simplicity: Prioritize simplicity in design and development. Avoid unnecessary complexity, abstraction, or over-engineering.
> - Clarity: Simple designs are usually easier to understand and maintain. They reduce ambiguity and make it easier for others to comprehend the system or product.
> - Efficiency: Simpler solutions often require fewer resources (such as time, effort, and code) to implement and maintain. They can lead to faster development cycles and lower costs.
> - Usability: Simple designs tend to result in better user experiences. Products or interfaces that are intuitive and easy to use are more likely to be adopted and appreciated by users.
> - Flexibility: Simple designs are often more adaptable to changes and future requirements. They can be easier to modify or extend without introducing unnecessary complications.
> - Risk Reduction: Complexity can introduce risks such as bugs, performance issues, and maintenance challenges. Simplifying designs can help mitigate these risks by reducing the likelihood of errors and making it easier to address issues when they arise.

# TOCTOU

- https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use
- https://wiki.sei.cmu.edu/confluence/display/c/FIO45-C.+Avoid+TOCTOU+race+conditions+while+accessing+files
- https://www.sonarsource.com/blog/winning-the-race-against-toctou-vulnerabilities/
- https://github.com/davidenetti/TOCTOU_Vulnerability
- https://fdzdev.medium.com/guide-to-identifying-and-exploiting-toctou-race-conditions-in-web-applications-c5f233e32b7f
- https://www.taringamberini.com/en/blog/programming/method-parameters-and-toctou/
- https://tty4.dev/development/prevent-toctou-attacks/

> A TOCTOU (time-of-check, time-of-use) race condition is possible when two or more concurrent processes are operating on a shared file system [Seacord 2013b]. Typically, the first access is a check to verify some attribute of the file, followed by a call to use the file. An attacker can alter the file between the two accesses, or replace the file with a symbolic or hard link to a different file. These TOCTOU conditions can be exploited when a program performs two or more file operations on the same file name or path name.

```java
public boolean checkDomain(HttpCookie httpCookie) {
    if (httpCookie == null) {
        throw new NullPointerException();
    }

    httpCookie = (HttpCookie) httpCookie.clone();

    if (httpCookie.hasExpired()) {
        // throw exception
    }

    return isDomainValid(httpCookie.getDomain());
}
```



