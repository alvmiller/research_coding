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
