# 编译与链接1

[参考](https://cloud.tencent.com/developer/article/1037709)

大家知道计算机使用的一系列的1和0

那个一个C++语言程序又是如何从一个个.h和.cpp文件变成包含1和0的可执行文件呢？

可以认为有以下的几个环节

源程序－&gt;预处理－&gt;编译和优化-&gt;生成目标文件－&gt;链接－&gt;可执行文件

**1.预处理**

C++的预处理是指在C++程序源代码被编译之前，由预处理器对C++程序源代码进行的处理。这个过程并不对程序的源代码进行解析。

这里的预处理器（preprocessor）是指真正的编译开始之前由编译器调用的一个独立程序。

预处理器主要负责以下的几处

**1.宏的替换**

**2.删除注释**

**3.处理预处理指令，如\#include，\#ifdef**

如我们有以下代码

temp.h

```javascript
#ifndef   _HEADERNAME_H
#define  _HEADERNAME_H  1

#include <iostream>
inline void show(char *a)
{
    std::cout << a<< std::endl;//annotation
}

#endif
```

main.cpp

```javascript
#include "temp.h"
#define MACRO "This is a macro"

extern int i;
int main()
{
        std::cout<<i<<std::endl;
        show(MACRO);
}
```

a.cpp

```javascript
#include <iostream>
int i=100;
```

\*在vs2013中可以使用“VS2013 开发人员命令提示”

![img](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/0概念和思想/D:/0文件/netstore/notebook/c++/概念和思想/0pcya2n27i.png)

使用cl /P main.cpp只进行预编译生成main.i文件

\*g++中可以使用（在以下只使用g++进行演示）

g++ –E main.cpp&gt;main.i命令

g++ –E a.cpp&gt;main.i

打开生成的A.i文件

我们发现

1、show函数中的注释已经被删掉了

![img](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/0概念和思想/D:/0文件/netstore/notebook/c++/概念和思想/it4pxjgu2g.png)

2、main函数中的MACRO宏被替成了"this is a macro”

windows vs下

![img](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/0概念和思想/D:/0文件/netstore/notebook/c++/概念和思想/jylvdftvfw.png)

3、temp.h和main.cpp中的\#include 和\#include “temp.h”也在相应位置被展开了

![img](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/0概念和思想/D:/0文件/netstore/notebook/c++/概念和思想/mi2tcxb9dl.png)

**2.编译和优化**

![img](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/0概念和思想/D:/0文件/netstore/notebook/c++/概念和思想/n27fdv7z9t.png)

词法分析 -- 识别单词,确认词类;比如int i;知道int是一个类型，i是一个关键字以及判断i的名字是否合法

语法分析 -- 识别短语和句型的语法属性；

语义分析 -- 确认单词、短语和句型的语义特征；

代码优化 -- 修辞、文本编辑；

代码生成 -- 生成译文。

内联函数的替换就发生在这一阶段

在g++中可以使用

g++ -S将预处理阶段生成的.i文件生成相应的汇编文件

g++ –S main.i main.s

g++ –S a.i a.s

生成的部分代码如下：

![img](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/0概念和思想/D:/0文件/netstore/notebook/c++/概念和思想/zf0uw3ihnv.png)

**3.生成目标文件**

汇编过程实际上指把汇编语言代码翻译成目标机器指令的过程。

在最终的目标文件中

除了拥有自己的数据和二进制代码之外，还要至少提供2个表：未解决符号表和导出符号表，分别告诉链接器自己需要什么和能够提供什么。

编译器把一个cpp编译为目标文件的时候，除了要在目标文件里写入cpp里包含的数据和代码，还要至少提供3个表：未解决符号表，导出符号表和地址重定向表。 未解决符号表提供了所有在该编译单元里引用但是定义并不在本编译单元里的符号及其出现的地址。 导出符号表提供了本编译单元具有定义，并且愿意提供给其他编译单元使用的符号及其地址。 地址重定向表提供了本编译单元所有对自身地址的引用的记录。

g++中可以使用g++ -c命令

g++ –c main.s –o main.o

g++ –c a.s –o a.o

**4.链接**

由汇编程序生成的目标文件并不能立即就被执行，其中可能还有许多没有解决的问题。例如，某个源文件中的函数可能引用了另一个源文件中定义的某个符号（如变量或者函数调用等）；在程序中可能调用了某个库文件中的函数，等等。所有的这些问题，都需要经链接程序的处理方能得以解决。

g++ a.o main.o –o main.out

最终运行结果如下

100

This is a macro

_**参考文献**_

C/C++程序从编译到最终生成可执行文件的过程分析 [http://blog.csdn.net/wyb19890515/article/details/7211006](http://blog.csdn.net/wyb19890515/article/details/7211006)

c/c++程序编译连接过程 [http://blog.csdn.net/hitprince/article/details/7880241](http://blog.csdn.net/hitprince/article/details/7880241)

