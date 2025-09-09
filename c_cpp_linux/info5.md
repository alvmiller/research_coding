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

# Linux : Process and Thread

- https://www.baeldung.com/linux/process-vs-thread
- https://www.geeksforgeeks.org/operating-systems/difference-between-process-and-thread/
- https://www.linkedin.com/pulse/linux-tasks-processes-threads-robert-luciani
- https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/7/html/reference_guide/chap-threads_and_processes
- https://medium.com/@gtamilarasan/context-switching-performance-threads-vs-processes-6a1b5d2c9954
- https://www.scaler.com/topics/linux-thread/
- https://www.site24x7.com/learn/threads-vs-processes-in-linux.html
- https://linuxhint.com/process-vs-thread-linux/

![Image!](https://i.ytimg.com/vi/4rLW7zg21gI/maxresdefault.jpg "Image")
![Image!](https://linuxhint.com/wp-content/uploads/2021/10/Process-vs-Threads-in-Linux-2.png "Image")

# Linux : Daemon

- https://lloydrochester.com/post/c/unix-daemon-example/
- https://github.com/pasce/daemon-skeleton-linux-c
- https://sandervanderburg.blogspot.com/2020/01/writing-well-behaving-daemon-in-c.html
- https://github.com/jirihnidek/daemon
- https://psychocod3r.wordpress.com/2019/03/19/how-to-write-a-daemon-process-in-c/
- https://pavaka.github.io/simple-linux-daemon-tutorial/
- https://man7.org/linux/man-pages/man3/daemon.3.html

> Daemons are long running processes that run in the background with no controlling terminal - tty.

```c
 1 #include <stdio.h>
 2 #include <stdlib.h>
 3 #include <unistd.h>
 4 #include <fcntl.h>
 5 #include <errno.h>
 6 #include <syslog.h>
 7 
 8 void daemonize( char *cmd ){
 9         pid_t pid;
10 
11         /* Clear file creation mask */
12         umask( 0 );
13 
14         /* Spawn a new process and exit */
15         if( (pid = fork()) < 0 ){ /* Fork error */
16                 fprintf( stderr, "%s: Fork error\n", cmd );
17                 exit( errno );
18         }
19         else if( pid > 0 ){ /* Parent process */
20                 exit( 0 );
21         }
22         /* Child process */
23         setsid();
24 
25         /* Change working directory to root directory */
26         if( chdir( "/" ) < 0 ){
27                 fprintf( stderr, "%s: Error changing directory\n", cmd );
28                 exit( errno );
29         }
30 
31         /* Close all open file descriptors */
32         for( int i = 0; i < 1024; i++ ){
33                 close( i );
34         }
35 
36         /* Reassociate file descriptors 0, 1, and 2 with /dev/null */
37         int fd0 = open( "/dev/null", O_RDWR );
38         int fd1 = dup( 0 );
39         int fd2 = dup( 0 );
40 
41         /* Open the log file */
42         openlog( cmd, LOG_CONS, LOG_DAEMON );
43         if( fd0 != 0 || fd1 != 1 || fd2 != 2 ){
44                 syslog( LOG_ERR, "Unexpected file descriptors %d, %d, %d\n",
45                         fd0, fd1, fd2 );
46                 exit( errno );
47         }
48 }
```

```c
#include <unistd.h>
#include <stdio.h>

int
main(int argc, char* argv[])
{
  // change to the "/" directory
  int nochdir = 0;

  // redirect standard input, output and error to /dev/null
  // this is equivalent to "closing the file descriptors"
  int noclose = 0;

  // glibc call to daemonize this process without a double fork
  if(daemon(nochdir, noclose))
    perror("daemon");

  // our process is now a daemon!
  sleep(60);

  return 0;
}
```

# Linux : Memory

- https://www.logicmonitor.com/blog/the-right-way-to-monitor-virtual-memory-on-linux
- https://medium.com/@vdczz.dev/memory-in-linux-ee513a2cbfbe
- https://tldp.org/LDP/tlk/mm/memory.html
- https://unix.stackexchange.com/questions/610444/the-structure-of-the-virtual-memory-of-a-linux-process
- https://zedas.fr/posts/linux-explained-7-memory-management/
- https://hotsushi.github.io/linux/virtual/memory/2018/01/19/Linux-Virtual-Memory-Explained.html
- https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/7/html/reference_guide/chap-memory_allocation
- https://tonylixu.medium.com/linux-memory-management-8a66932eb711
- https://en.wikipedia.org/wiki/Virtual_memory
- https://medium.com/@rayenhedri02/exploring-virtual-memory-mapping-kernel-handling-and-swapping-ec9130f4f2e0
- https://stackoverflow.com/questions/2445242/what-does-the-kernel-virtual-memory-of-each-process-contain

![Image!](https://i.sstatic.net/KJGw7.png "Image")
![Image!](https://zedas.fr/images/linux/virtual-memory.png "Image")
![Image!](https://zedas.fr/images/linux/linux-memory-distribution.png "Image")
![Image!](https://hotsushi.github.io/assets/linux-virtual-memory/MMU.png "Image")
![Image!](https://hotsushi.github.io/assets/linux-virtual-memory/abstract.png "Image")
![Image!](https://hotsushi.github.io/assets/linux-virtual-memory/LinuxMemory.png "Image")
![Image!](https://hotsushi.github.io/assets/linux-virtual-memory/process_switching.png "Image")
![Image!](https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux_for_Real_Time-7-Reference_Guide-en-US/images/e43a82d377ff4cf3a46398bc924a6893/5482.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1400/0*2Mea6PHrX_rMcRRk.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/0*JpIeKzp4hmgBCVoS.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:750/format:webp/0*NvCrS6tMv0fipPI9.gif "Image")
![Image!](https://i.sstatic.net/R4tVn.png "Image")
![Image!](https://i.sstatic.net/QU2ye.png "Image")








