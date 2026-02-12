# C : Static and Dynamic libs

```c

// https://stackoverflow.com/questions/2734719/how-to-compile-a-static-library-in-linux
// https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/developer_guide/creating-libraries-gcc
// https://lsi.vc.ehu.eus/pablogn/docencia/ISO/Act5%20Libs/crealibdin.html
// https://www.cprogramming.com/tutorial/shared-libraries-linux-gcc.html
// https://serverfault.com/questions/279068/cant-find-so-in-the-same-directory-as-the-executable
// https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/developer_guide/gcc-using-libraries
// https://aicybernetworx.co.uk/2015/06/16/intro-to-linux-shared-libraries-how-to-create-shared-libraries/
// https://stackoverflow.com/questions/13089166/how-to-make-gcc-link-strong-symbol-in-static-library-to-overwrite-weak-symbol
// https://volito.digital/creating-a-c-library-with-a-makefile-a-comprehensive-guide/
// https://maelvls.dev/static-libraries-and-autoconf-hell/
// https://medium.com/@Anatolii_Zhadan/makefile-to-create-a-library-in-c-3c2ad3d281
// https://earthly.dev/blog/static-and-dynamic-linking/
// https://docencia.ac.upc.edu/FIB/USO/Bibliografia/unix-c-libraries.html
// https://www.linkedin.com/pulse/c-static-libraries-beginners-mariem-matri
// https://stackoverflow.com/questions/16027854/gcc-static-library-linking-vs-dynamic-linking
// https://opensource.com/article/22/6/static-linking-linux
// https://opensource.com/article/22/5/dynamic-linking-modular-libraries-linux
// https://www.tabbykatz.com/writings/dynamic-static-libraries

/*

gcc -c static_funcs.c
ar -rc libstatic_funcs.a static_funcs.o
gcc -c -fPIC shared_funcs.c
gcc -shared -o libshared_funcs.so shared_funcs.o

export LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH
gcc main.c -L. -lstatic_funcs -lshared_funcs

// gcc main.c -L. -lstatic_funcs -L/usr/lib64/ -lshared_funcs
// sudo cp libshared_funcs.so /usr/lib64/
// sudo ldconfig

./a.out 

----
Main: main_func()
Static: main_func_weak_static()
Main: main_func_weak_shared()
Main: main_func_static()
Main: main_func_shared()
Static: static_func()
Shared: shared_func()
----

*/

```

```c
// main.c

#include <stdio.h>

void main_func()
{
	printf("Main: main_func()\n");
}

__attribute__((weak))
void main_func_weak_static()
{
	printf("Main: main_func_weak_static()\n");
}

__attribute__((weak))
void main_func_weak_shared()
{
	printf("Main: main_func_weak_shared()\n");
}

void main_func_static()
{
	printf("Main: main_func_static()\n");
}

void main_func_shared()
{
	printf("Main: main_func_shared()\n");
}

extern void static_func();
extern void shared_func();

int main(void)
{
	main_func();
	main_func_weak_static();
	main_func_weak_shared();
	main_func_static();
	main_func_shared();
	static_func();
	shared_func();
	return 0;
}
```

```c
// shared_funcs.c

#include <stdio.h>

void main_func_weak_shared()
{
	printf("Shared: main_func_weak_shared()\n");
}

void main_func_shared()
{
	printf("Shared: main_func_shared()\n");
}

void shared_func()
{
	printf("Shared: shared_func()\n");
}
```

```c
// static_funcs.c

#include <stdio.h>

void main_func_weak_static()
{
	printf("Static: main_func_weak_static()\n");
}

/*
// Error
void main_func_static()
{
	printf("Static: main_func_static()\n");
}
*/

void static_func()
{
	printf("Static: static_func()\n");
}
```
