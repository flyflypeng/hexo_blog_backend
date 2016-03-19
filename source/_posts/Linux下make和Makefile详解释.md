title: "Linux中make和Makefile详解"
date: 2015-09-03 22:03
categories: 技术
tags:
- Linux
- make
- Makefile
---

### **目录**
>1. [前言](#1)
>2. [make和Makefile](#2)
>	2.1 [make简介](#2.1)
>	2.2 [Makefile简介](#2.2)
>	2.3 [Makefile规则介绍](#2.3)
>	2.4 [一个简单的Makefile的例子](#2.4)
>	2.5 [make是如何处理一个Makefile文件](#2.5)
>	2.6 [清空目标文件规则](#2.6)


## 1. 前言 <span id="1"></span>

如果你有手动从源码编译过linux应用程序的经历，那么你肯定不会对”./configure“,"make","make install"这些命令感到陌生。下面我们就来探索一下这个过程中的每一都发生了什么。

在Linux系统环境中使用GNU的make工具可以很方便地构建和编译一个大型的工程,整个工程的编译只需要一个命令"make"就可以完成编译、链接和运行。不过在使用"make"命令之前，我们需要完成一个叫做"Makefile"文件的编写，这个文件详细描述了我们整个工程该如何取编译、链接以及最后生成一个可执行的文件，以及这个过程中所需要依赖的文件和其他工具软件。

<!--more-->

在介绍make和Makefile之前，我们来补充说明一下linux下一个简单C语言源程序的编译、链接的过程。

编译: 编译就是把文本形式的源代码翻译称为及其语言形式的目标文件的过程。
链接: 链接就是把目标文件和使用到的库文件进行组织，最终形成可执行代码的过程。
整个编译和链接的过程的图解如下:
![编译链接图解][compile_and_link_url]

从上图可知，整个代码编译的过程分为编译和链接两个阶段，其中编译对应的是大括号对应的部分，其余则是链接过程。

在这里我们还要特别说明一下linux中的库文件！
库: 库是一组**预先编译好的函数的集合**，这些函数都是按照可重用的原则编写的。

在linux中库文件可以分为静态库和共享库(有的地方也叫做动态库)。
静态库: 它也称为归档文件(archive file)，按照惯例它们的文件都是以".a"结尾，比如标准C语言函数库"/usr/lib/libc.a"。静态库在程序链接时会使用，链接器会将程序中使用到函数的代码从静态库文件中拷贝到应用程序中。一旦链接完成，在执行程序的时候就不需要静态库了。由于每个使用静态库的应用程序都需要拷贝所用到函数的代码，所以静态链接生成的可执行文件都会比较大。

共享库: 共享库以".so"结尾(so = shared objecct)。共享库在程序链接的时候并不像静态库那样在拷贝所使用到函数的代码到应用程序中，而只是作些标记。然后在程序加载到内存中开始运行的时候，动态地加载所需的共享库。所以，应用程序在运行的时候仍然需要共享库的支持，共享库链接出来的文件要比静态库要小的多。


## 2. make和Makefile <span id="2"></span>

### 2.1 make简介 <span id="2.1"></span>

make是一个GNU/Linux中自带一个使用软件工具，它能够自动地判断出一个大的工程文件中那些源文件需要被重新编译，并且可以直接使用特定的命令来对这些文件进行重新编译。

### 2.2 Makefile简介 <span id="2.2"></span>
当你准备使用"make"命令来编译你的工程文件前，你需要准备好一个叫做Makefile的文件。通常，这个文件会告诉make如何编译和链接程序，在这个文件中主要描述了你程序中各个文件之间的关系并且提供了一些命令来编译文件，最终生成可执行的目标文件。

当使用make工具进行编译时，工程中以下几种文件在执行make时将会被编译(重新编译):
1. 所有的源文件都没有被编译过，则对各个源文件进行编译和链接，生成最后可执行的程序。
2. 每一个在上次执行make之后修改过的源文件在本次执行make时，都会被重新编译。
3. 头文件在上次执行make之后被修改过，那么所有包含该修改过头文件的源程序文件都会在执行本次make命令时重新进行编译。

### 2.3 Makefile规则介绍 <span id="2.3"></span>
一个简单的Makefile由以下这种类型的“规则”构成:
> target...: prerequistes...
> [tab] command

**target: 规则目标**。target通常是最后需要生成的文件名或者为了实现这个目的而必须的中间过程文件名，可以是目标文件名“*.o”或者最后可执行的程序的文件名。另外，目标也可以是一个make执行动作的的名称，比如“clean”(目标"clean"不是一个文件，它仅仅代表执行一个动作的标识)，我们将这样的目标称为“伪目标”。

**prerequisites:规则依赖**。生成规则目标所需要的文件列表。通常一个目标依赖于一个或者多个文件。

**command: 规则的命令行**，是规则所要执行的动作（任意的shell命令或者是可在shell中执行的程序）。它限定了make执行这条规则时所需要的动作。一个规则可以有多个命令行，每一条命令占一行。
**注意: 每一个命令行必须以[Tab]字符开始，[Tab]字符告诉make此行是一个命令行。make按照命令完成相应的动作。这也是书写Makefile中容易产生并且十分隐蔽的错误。**

命令就是在任何一个目标的依赖文件发生变化后重建目标的动作描述。一个目标可以没有依赖而只有动作（指定的命令）。比如Makefile中的目标“clean”，此目标没有依赖，只有命令。它所定义的命令用来删除make过程中生成的中间文件，进行清理工作。

在Makefile中“规则”就是描述在什么情况下、如何重建规则的目标文件，通常规则中包括了目标的依赖关系（目标的依赖文件）和重建目标的命令。make执行重建目标的命令，来创建或者重建规则的目标（此目标文件也可以是触发这个规则的上一个规则中依赖文件）。规则包含了文件之间的依赖关系和更新此规则目标所需要的命令。

一个Makefile文件中通常还包含了除规则以外的很多东西。一个最简单的Makefile可能只包含规则。规则在有些Makefile中可能看起来非常复杂，但是无论规则的书写是多么复杂，它都符合规则的基本格式。

**规则告诉make就是两件事情，第一就是文件的依赖关系，第二就是如何生成目标文件**

## 2.4 一个简单的Makefile的例子 <span id="2.4"></span>

为了更好地理解Makefile，我们就通过一个简单的例子来进行阐述说明。在这个例子中，我们有总共有三个文件file1.c,file2.c和file2.h，通过这三个文件我们要构造一个可执行的目标程序“helloworld”。

```cpp
//file1.c
#include <stdio.h>
#include "file2.h"

int main()
{
	printf("print file1$$$$$$$$$$$$$$$$$$$$\n");
	File2Print();
	return 0;
}
```

```cpp
//file2.h
#ifndef FILE2_H_
#define FILE2_H_
	#ifdef __cplusplus
		extern "C" {
	#endif
	void File2Print();
	#ifdef __cplusplus
		}
	#endif
#endif
```

```cpp
//file2.c
#include <stdio.h>
#include "file2.h"

void File2Print()
{
	printf("Print file2********************\n");
}
```

```makefile
helloworld:file1.o file2.o
	gcc file1.o file2.o -o helloworld
file1.o: file1.c file2.h
	gcc -c file1.c -o file1.o
file2.o: file2.c file2.h
	gcc -c file2.c -o file2.o
clean:
	rm -rf *.o helloworld
```

上面的makefile文件目的就是要编译一个helloworld的可执行文件。让我们一句句来解释:
```makefile
helloworld:file1.o file2.o  #目标helloworld依赖于file1.o和file2.o两个目标文件。

	gcc file1.o file2.o -o helloworld  #使用gcc编译出helloworld可执行文件。-o 表示执行输出的目标文件名。
    
file1.o: file1.c file2.h   #file1.o依赖于file1.c和file2.h文件。

	gcc -c file1.c -o file1.o  #编译出file1.o文件。-c表示gcc只把源文件编译成目标文件。
    
file2.o: file2.c file2.h
	gcc -c file2.c -o file2.o
#这两句的含义上面两句的含义相同

clean:
	rm -rf *.o helloworld
#当用户在shenll终端中输入make clean的命令时，就会删除*.o和helloworld文件。
```

写好Makefile文件之后，只需要简单地在终端命令行中输入make命令，就会开始执行Makefile中的内容了。

### 2.4.1 使用变量

如果我们需要编译cpp文件时，只需要把gcc改成g++就可以了。但是如果Makefile中有很多gcc，那不就很麻烦嘛。
下面我们就使用变量来改写上面的Makefile文件。
```makefile
OBJS = file1.o file2.o
CC = gcc
CFLAGS = -Wall -O -g
helloworld:$(OBJS)
	$(CC) $(OBJS) -o helloworld
file1.o:file1.c file2.h
	$(CC) $(CFLAGS) -c file1.c -o file1.o
file2.o:file2.c file2.h
	$(CC) $(CFLAGS) -c file2.c -o file2.o
clean:
	rm -rf *.o helloworld
```

在上面我们使用到了变量。如果要设定一个变量，你只要在一行的开始写下这个变量的名字，后面跟一个“=”号，后面跟你要设定的这个变量的值。已有你要引用这个变量，写一个“$“符号在变量名前，就表示引用变量的值。

CFLAGS = -Wall -O -g，解释一下，这个是gcc编译器的参数设置，并把它赋值给CFLAGS变量。

| 参数 | 意义 |
|--------|--------|
|    -Wall   |   输出所有的警告信息    |
|    -O   |  在编译时进行优化   |
|    -g   |   表示编译debug版本    |

这样写的Makefile文件比较简单，但很容易就会发现缺点，那就是要列出所有的c文件。如果你添加一个c文件，那就需要对Makefile文件进行修改，这在项目开发中是比较麻烦的。

### 2.4.2 使用函数

学到这里，你可能发现在Makefile中既有变量，又有函数，这不就是在编程吗？其实写Makefile就是在编程，只不过使用不同的语言而已。

下面我们就使用函数来继续改写上面的Makefile。
```makefile
CC = gcc
XX = g++
CFLAGS = -Wall -O -g
TARGET =  ./helloworld
%.o:%.c
	$(CC) $(CFLAGS) -c $< -o $@
%.o:%.cpp
	$(XX) $(CFLAGS) -c $< -o $@
SOURCES = $(wildcard *.c *.cpp)
OBJS = $(patsubst %.c,%.o,$(patsubst %.cpp,%.o,$(SOURCES)))

$(TARGET):$(OBJS)
	$(XX) $(OBJS) -o $(TARGET)
    chmod a+x $(TARGET)
clean:
	rm -rf *.o helloworld
```

看到上面这个Makefile文件我们的感觉就是要炸了，什么语法啊，看不懂啊，所以我们要一步一步来分析。

函数1: wildcard
功能: 产生一个所有以“.c”结尾的文件的列表
SOURCES = $(wildcard *.c *.cpp)  #这句代码表示产生一个所有以.c，.cpp结尾的文件的列表，然后存入变量SOURCES中。

函数: patsubst
功能: 匹配替换，有三个参数。第一个参数是一个需要匹配的式样，第二个表示用什么来替换它，第三个是一个需要**被处理的由空格分隔的文件列表**
OBJS = $(patsubst %.c,%.o,$(patsubst %.cpp,%.o,$(SOURCES)))  #这句代码表示把文件列表中的所有的.c,.cpp字符变成.o，形成一个新的文件列表，然后存入OBJS变量中。

```makefile
%.o:%.c
	$(CC) $(CFLAGS) -c $< -o $@
%.o:%.cpp
	$(XX) $(CFLAGS) -c $< -o $@
```
这几句命令表示把所有的 .c , .cpp 文件编译成.o文件。
这里有三个比较有用的内部变量:

| 内部变量 | 含义 |
|--------|--------|
|   $@     |   表示目标文件  |
|   $<     |   表示依赖文件列表中第一个依赖文件 |
|   $^     |   表示整个依赖文件列表  |
|   $?     |   表示比目标文件还要新的依赖文件列表  |

chmod a+x $(TARGET)表示把helloworld强制变成可执行的文件。

到此，我们就可以编写一个比较简单通用的Makefile文件了。

**上面的所有例子，都假定所有的文件都在同一个目录下，不包括字目录。**

### 2.5 Make是如何处理一个Makefile文件 <span id="2.5"></span>

在默认情况下，也就是我们只在shell环境中输入一个make命令，那么make接下来会执行以下的操作:

1. make会在当前目录下寻找一个名字叫做"Makefile"的文件。
2. 然后make会在Makefile文件中找到第一个目标(target)，并将该目标当做最终要生成的目标，例如上面例子中的“helloworld”。
3. 在make执行第一条规则之前（例如我们上面例子中生成helloworld可执行程序的规则），首先需要把目标可执行程序(target)所依赖的其他目标文件(object)的规则先执行处理完（在上面的例子中就是要先生成目标文件file1.o和file2.o）。
4. 当规则中依赖文件中源程序文件或者头文件经过修改导致更新的时间比目标文件更新时或者目标文件根本不存在时，则这个规则就会进行编译(complication)或者重建(recomplication)。
5. 在编译生成或者重新编译生成第一个目标所需的依赖文件后，make最后就会执行第一条规则中的命令，将一个或者多个依赖文件链接生成最终的可执行程序。如果所依赖的目标文件更新的时间比原来已经存在的目标文件生成的时间还新的话，那么也会进行目标重建。

### 2.6 清空目标文件的规则 <span id="2.6"></span>

每个Makefile都应该编写一个清空目标文件(.o和执行文件)的规则，这不仅便于重编译，也很利于保持文件的整洁。
一般的风格是:
```makefile
clean:
	rm *.o helloworld
```
但是更为稳健的方法是:
```makefile
.PHONY: clean
clean:
	-rm *.o helloworld
```

在这里".PHONY"的意思表示clean是一个“伪目标”，而在rm命令前机上一个小减号"-"表示的意思是:也许莫邪文件出现了问题，但是不要管，继续做后面的事。当然，clean的规则不要放在文件的开头，不然会被make以为是默认的目标。所有有个不成文的规定是——"clean从来都是放在最后面的"。









[compile_and_link_url]:http://7xj51c.com1.z0.glb.clouddn.com/compile_and_link_procedure.jpg



