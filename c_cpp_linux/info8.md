# C / C++ : Online

- https://www.programiz.com/cpp-programming/online-compiler/
- https://www.onlinegdb.com/online_c++_compiler
- https://cpp.sh/
- https://godbolt.org/
- https://www.codechef.com/c-online-compiler

# C : Examples

# C : Arrays

```c
/******************************************************************************

                            Online C Compiler.
                Code, Compile, Run and Debug C program online.
Write your code in this editor and press "Run" button to compile and execute it.

*******************************************************************************/

#include <stdio.h>

#define N (2)

typedef struct {
    int x;
    int y;
} s0;

static const struct big_arr {
    int a;
    s0 arr0[N];
} b_array[] = {
    {
        .a = 10,
        .arr0 = { { .x = 1, .y = 2 }, { .x = 3, .y = 4 } }
    },
    {
        .a = 20,
        .arr0 = { { .x = 10, .y = 20 }, { .x = 30, .y = 40 } }
    }
};

int main()
{
    printf("Hello World");

    return 0;
}
```

# C : Return Array from Function

- https://stackoverflow.com/questions/11656532/returning-an-array-using-c
- https://www.geeksforgeeks.org/c/return-an-array-in-c/#return-an-array-in-c-using-structures
- https://www.tutorialspoint.com/cprogramming/c_return_arrays_from_function.htm
- https://www.scaler.com/topics/c-return-array/

```c
#include <stdio.h>
#include <stdlib.h>

struct sarr {
    char a[3];
};
struct sarr get_arr0()
{
    struct sarr arr = {};
    arr.a[0] = 42;
    return arr;
}

char *get_arr1()
{
    static char arr[3] = {42,1,1};
    return arr;
}

char *get_arr2()
{
    char *arr = malloc(3);
    arr[0] = 42;
    return arr;
}

char *get_arr3(unsigned *val, char *arr)
{
    if (val == NULL) return NULL;
    if (arr == NULL && *val == 0) {
        *val = 3;
        return NULL;
    }
    if (arr != NULL && val == NULL) return NULL;
    
    arr[0] = 42;
    return arr;
}

int main()
{
    struct sarr arr0 = get_arr0();
    printf("get_arr0 = %d\n", (int)arr0.a[0]);
    
    char *arr1 = get_arr1();
    printf("get_arr1 = %d\n", (int)arr1[0]);
    
    char *arr2 = get_arr2();
    printf("get_arr2 = %d\n", (int)arr2[0]);
    free(arr2);
    
    char *arr3 = NULL;
    unsigned val = 0;
    (void)get_arr3(&val, NULL);
    arr3 = malloc(val);
    arr3 = get_arr3(&val, arr3);
    printf("get_arr3 = %d\n", (int)arr3[0]);
    free(arr3);

    return 0;
}
```

# C | Makefile : simple example

- https://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/
- https://makefiletutorial.com/
- https://www.codeproject.com/articles/Creating-A-Basic-Make-File-for-Compiling-C-Code
- https://www.cs.swarthmore.edu/~newhall/unixhelp/howto_makefiles.html
- https://www.cs.usask.ca/staff/oster/makefiles.html
- https://www.gnu.org/software/make/manual/html_node/Simple-Makefile.html

```
# A simple Makefile

myprogram: main.o part1.o part2.o
      gcc -o myprogram main.o part1.o part2.o

part1.o: part1.c part1.h header.h
      gcc -O -c part1.c

part2.o: part2.c header.h
      gcc -O -c part2.c

main.o: main.c header.h
      gcc -O -c main.c

clean:
      rm -f myprogram main.o part1.o part2.o
```

```
# A more complex Makefile

CC = gcc
CFLAGS = -O

OBJS = part1.o part2.o main.o

myprogram: ${OBJS}
      ${CC} -o myprogram ${CFLAGS} ${OBJS}

part1.o: part1.c part1.h header.h
      ${CC} ${CFLAGS} -c part1.c

part2.o: part2.c header.h
      ${CC} ${CFLAGS} -c part2.c

main.o: main.c header.h
      ${CC} ${CFLAGS} -c main.c

clean:
      rm -f myprogram ${OBJS}
      @echo "all cleaned up!"
```

```
#
# 'make depend' uses makedepend to automatically generate dependencies 
#               (dependencies are added to end of Makefile)
# 'make'        build executable file 'mycc'
# 'make clean'  removes all .o and executable files
#

# define the C compiler to use
CC = gcc

# define any compile-time flags
CFLAGS = -Wall -g

# define any directories containing header files other than /usr/include
#
INCLUDES = -I/home/newhall/include  -I../include

# define library paths in addition to /usr/lib
#   if I wanted to include libraries not in /usr/lib I'd specify
#   their path using -Lpath, something like:
LFLAGS = -L/home/newhall/lib  -L../lib

# define any libraries to link into executable:
#   if I want to link in libraries (libx.so or libx.a) I use the -llibname 
#   option, something like (this will link in libmylib.so and libm.so:
LIBS = -lmylib -lm

# define the C source files
SRCS = emitter.c error.c init.c lexer.c main.c symbol.c parser.c

# define the C object files 
#
# This uses Suffix Replacement within a macro:
#   $(name:string1=string2)
#         For each word in 'name' replace 'string1' with 'string2'
# Below we are replacing the suffix .c of all words in the macro SRCS
# with the .o suffix
#
OBJS = $(SRCS:.c=.o)

# define the executable file 
MAIN = mycc

#
# The following part of the makefile is generic; it can be used to 
# build any executable just by changing the definitions above and by
# deleting dependencies appended to the file from 'make depend'
#

.PHONY: depend clean

all:    $(MAIN)
        @echo  Simple compiler named mycc has been compiled

$(MAIN): $(OBJS) 
        $(CC) $(CFLAGS) $(INCLUDES) -o $(MAIN) $(OBJS) $(LFLAGS) $(LIBS)

# this is a suffix replacement rule for building .o's from .c's
# it uses automatic variables $<: the name of the prerequisite of
# the rule(a .c file) and $@: the name of the target of the rule (a .o file) 
# (see the gnu make manual section about automatic variables)
.c.o:
        $(CC) $(CFLAGS) $(INCLUDES) -c $<  -o $@

clean:
        $(RM) *.o *~ $(MAIN)

depend: $(SRCS)
        makedepend $(INCLUDES) $^

# DO NOT DELETE THIS LINE -- make depend needs it
```

```
objects = foo.o bar.o all.o
all: $(objects)
	$(CC) $^ -o all

# Syntax - targets ...: target-pattern: prereq-patterns ...
# In the case of the first target, foo.o, the target-pattern matches foo.o and sets the "stem" to be "foo".
# It then replaces the '%' in prereq-patterns with that stem
$(objects): %.o: %.c
	$(CC) -c $^ -o $@

all.c:
	echo "int main() { return 0; }" > all.c

# Note: all.c does not use this rule because Make prioritizes more specific matches when there is more than one match.
%.c:
	touch $@

clean:
	rm -f *.c *.o all
```

```
obj_files = foo.result bar.o lose.o
src_files = foo.raw bar.c lose.c

all: $(obj_files)
# Note: PHONY is important here. Without it, implicit rules will try to build the executable "all", since the prereqs are ".o" files.
.PHONY: all 

# Ex 1: .o files depend on .c files. Though we don't actually make the .o file.
$(filter %.o,$(obj_files)): %.o: %.c
	echo "target: $@ prereq: $<"

# Ex 2: .result files depend on .raw files. Though we don't actually make the .result file.
$(filter %.result,$(obj_files)): %.result: %.raw
	echo "target: $@ prereq: $<" 

%.c %.raw:
	touch $@

clean:
	rm -f $(src_files)
```

```
# Thanks to Job Vranish (https://spin.atomicobject.com/2016/08/26/makefile-c-projects/)
TARGET_EXEC := final_program

BUILD_DIR := ./build
SRC_DIRS := ./src

# Find all the C and C++ files we want to compile
# Note the single quotes around the * expressions. The shell will incorrectly expand these otherwise, but we want to send the * directly to the find command.
SRCS := $(shell find $(SRC_DIRS) -name '*.cpp' -or -name '*.c' -or -name '*.s')

# Prepends BUILD_DIR and appends .o to every src file
# As an example, ./your_dir/hello.cpp turns into ./build/./your_dir/hello.cpp.o
OBJS := $(SRCS:%=$(BUILD_DIR)/%.o)

# String substitution (suffix version without %).
# As an example, ./build/hello.cpp.o turns into ./build/hello.cpp.d
DEPS := $(OBJS:.o=.d)

# Every folder in ./src will need to be passed to GCC so that it can find header files
INC_DIRS := $(shell find $(SRC_DIRS) -type d)
# Add a prefix to INC_DIRS. So moduleA would become -ImoduleA. GCC understands this -I flag
INC_FLAGS := $(addprefix -I,$(INC_DIRS))

# The -MMD and -MP flags together generate Makefiles for us!
# These files will have .d instead of .o as the output.
CPPFLAGS := $(INC_FLAGS) -MMD -MP

# The final build step.
$(BUILD_DIR)/$(TARGET_EXEC): $(OBJS)
	$(CXX) $(OBJS) -o $@ $(LDFLAGS)

# Build step for C source
$(BUILD_DIR)/%.c.o: %.c
	mkdir -p $(dir $@)
	$(CC) $(CPPFLAGS) $(CFLAGS) -c $< -o $@

# Build step for C++ source
$(BUILD_DIR)/%.cpp.o: %.cpp
	mkdir -p $(dir $@)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c $< -o $@


.PHONY: clean
clean:
	rm -r $(BUILD_DIR)

# Include the .d makefiles. The - at the front suppresses the errors of missing
# Makefiles. Initially, all the .d files will be missing, and we don't want those
# errors to show up.
-include $(DEPS)
```

```
IDIR =../include
CC=gcc
CFLAGS=-I$(IDIR)

ODIR=obj
LDIR =../lib

LIBS=-lm

_DEPS = hellomake.h
DEPS = $(patsubst %,$(IDIR)/%,$(_DEPS))

_OBJ = hellomake.o hellofunc.o 
OBJ = $(patsubst %,$(ODIR)/%,$(_OBJ))


$(ODIR)/%.o: %.c $(DEPS)
	$(CC) -c -o $@ $< $(CFLAGS)

hellomake: $(OBJ)
	$(CC) -o $@ $^ $(CFLAGS) $(LIBS)

.PHONY: clean

clean:
	rm -f $(ODIR)/*.o *~ core $(INCDIR)/*~
```

# C : Endianness

- https://keasigmadelta.com/blog/how-to-convert-endianness-in-c-c-in-4-different-ways/
- https://aticleworld.com/little-and-big-endian-importance/
- https://electro4u.net/blog/little-endian-and-big-endian-in-c-456
- https://medium.com/mycsdegree/sockets-in-c-little-and-big-endian-machines-23c9ed484c20
- https://cs-fundamentals.com/tech-interview/c/c-program-to-check-little-and-big-endian-architecture#google_vignette
- https://embetronicx.com/tutorials/p_language/c/little-endian-and-big-endian/

> The endian will decide how to store multiple bytes in computer memory.
> It doesn’t mean, the order of bits inside a byte,  nor the way the computer reads an array of bytes or a file.
> It’s all about the order of the bytes of a word (multi-byte variable) in memory.
> 
> In Little-endian, LSB (Least significant byte) is stored first or to a lower memory address. Intel x86, Pentium are using this Little Endian.
> In Big Endian, MSB (Most significant byte) is stored first or to a lower memory address. Big-endian is implemented in PowerPC and most networking devices.

> There is a lot of function which has used when we send the data to the network.
> - htons() – “Host to Network Short”
> - htonl() – “Host to Network Long”
> - ntohs() – “Network to Host Short”
> - ntohl() – “Network to Host Long”

```
Big Endian storing 1:
+-----------+-----------+-----------+-----------+
| 0000 0000 | 0000 0000 | 0000 0000 | 0000 0001 |
+-----------+-----------+-----------+-----------+
     1001        1002        1003        1004
  low                                        high
```

```
int x = 0x76543210;
char *c = (char*) &x;

Big endian format:
------------------
Byte address  | 0x01 | 0x02 | 0x03 | 0x04 | 
              +++++++++++++++++++++++++++++
Byte content  | 0x76 | 0x54 | 0x32 | 0x10 |
			 
Little endian format:
---------------------
Byte address  | 0x01 | 0x02 | 0x03 | 0x04 | 
              +++++++++++++++++++++++++++++
Byte content  | 0x10 | 0x32 | 0x54 | 0x76 |
```

```c
#include <stdio.h>
#include <inttypes.h>
//Function to change one endian to another
uint32_t ChangeEndianness(uint32_t u32Value)
{
    uint32_t u32Result = 0;
    u32Result |= (u32Value & 0x000000FF) << 24;
    u32Result |= (u32Value & 0x0000FF00) << 8;
    u32Result |= (u32Value & 0x00FF0000) >> 8;
    u32Result |= (u32Value & 0xFF000000) >> 24;
    return u32Result;
}
int main()
{
    uint32_t u32CheckData  = 0x11223344;
    uint32_t u32ResultData =0;
    //swap the data
    u32ResultData = ChangeEndianness(u32CheckData);
    //converted data
    printf("0x%x\n",u32ResultData);
    return 0;
}
```

```c
#include <stdio.h>
#include <inttypes.h>
//Macro to swap the byte
#define CHANGE_ENDIANNESS(A)   ((((uint32_t)(A) & 0xff000000) >> 24) \
                               | (((uint32_t)(A) & 0x00ff0000) >> 8) \
                               | (((uint32_t)(A) & 0x0000ff00) << 8)  \
                               | (((uint32_t)(A) & 0x000000ff) << 24))
int main()
{
    uint32_t u32CheckData  = 0x11223344;
    uint32_t u32ResultData =0;
    u32ResultData = CHANGE_ENDIANNESS(u32CheckData);
    printf("0x%x\n",u32ResultData);
    return 0;
}
```

```c
#include <stdio.h>
#include <inttypes.h>
typedef union
{
    uint32_t u32RawData;
    uint8_t  au8DataBuff[4];
} RawData;
uint32_t ChangeEndianness(uint32_t u32Value)
{
    RawData uChangeData,uOrginalData;
    uOrginalData.u32RawData = u32Value;
    //change the value
    uChangeData.au8DataBuff[0]  = uOrginalData.au8DataBuff[3];
    uChangeData.au8DataBuff[1]  = uOrginalData.au8DataBuff[2];
    uChangeData.au8DataBuff[2]  = uOrginalData.au8DataBuff[1];
    uChangeData.au8DataBuff[3]  = uOrginalData.au8DataBuff[0];
    return (uChangeData.u32RawData);
}
int main()
{
    uint32_t u32CheckData  = 0x11223344;
    uint32_t u32ResultData =0;
    u32ResultData = ChangeEndianness(u32CheckData);
    printf("0x%x\n",u32ResultData);
    return 0;
}
```

![pic1](https://miro.medium.com/v2/resize:fit:1100/format:webp/0*07nm__pJLtkD74X_.png "pic1")
![pic1](https://aticleworld.com/wp-content/uploads/2016/08/little-endian-big-endian.png "pic1")
![pic1](https://aticleworld.com/wp-content/uploads/2016/08/4b.png "pic1")

```c
#include <stdio.h>
int main ()
{
  unsigned int x = 0x76543210;
  char *c = (char*) &x;
 
  printf ("*c is: 0x%x\n", *c);
  if (*c == 0x10)
  {
    printf ("Underlying architecture is little endian. \n");
  }
  else
  {
     printf ("Underlying architecture is big endian. \n");
  }
 
  return 0;
}
```

```c
int check_for_endianness()
{
  unsigned int x = 1;
  char *c = (char*) &x;
  return (int)*c;
}
```

```c
#include <stdio.h>
#include <inttypes.h>
int main()
{
    uint8_t au8RawBuf[4] = {0x01, 0x00,0x00,0x00};
    uint32_t  u32RawData = *(uint32_t *)au8RawBuf;
    printf("0x%x\n", u32RawData);
    return 0;
}
```





























