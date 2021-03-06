# 编译与链接2

[参考](https://cloud.tencent.com/developer/article/1037714)

编译单元\*\*

首先让我们来认识一下编译单元，什么是编译单元呢？简单来说一个cpp文件就是一个编译单元。

在集成式的IDE中，我们往往点击一下运行便可以了，编译的所有工作都交给了IDE去处理，往往忽略了其中的内部流程

事实上编译每个编译单元\(.cpp\)时是相互独立的，即每个cpp文件之间是不知道对方的存在的（不考虑\#include “xxx.cpp" 这种奇葩的写法）

编译器会分别将每个编译单元\(.cpp\)进行编译，生成相应的obj文件

然后链接器会将所有的obj文件进行链接，生成最终可执行文件

**内部链接与外部链接**

那么什么内部链接和外部链接又是什么呢？

我们知道C++中声明和定义是可以分开的

例如在vs中，我们可以一个函数声明定义放在b.cpp中，在a.cpp只需再声明一下这个函数，就可以在a.cpp中使用这个函数了

a.cpp

```c
void show();

int main()
{
    show();
return 0;
}
```

b.cpp

```c
#include <iostream>
void show()
{
    std::cout << "Hello" << std::endl;
}
```

而通过之前的了解，我们知道每个编译单元间是相互独立不知道彼此的存在的

那么a.cpp又是如何知道show函数的定义的呢

其实在编译一个编译单元\(.cpp\)生成相应的obj文件过程中

编译器会将分析这个编译单元\(.cpp\)

将其所能提供给其他编译单元\(.cpp\)使用的函数，变量定义记录下来。

而将自己缺少的函数，变量的定义也记录下来。

所以可以认为a.obj和b.obj记录了以下的信息

![img](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/0概念和思想/D:/0文件/netstore/notebook/c++/概念和思想/o6zwverip9.png)

然后在链接器连接的时候就会知道a.obj需要show函数定义，而b.obj中恰好提供了show函数的定义，通过链接，在最终的可执行文件中我们能看到show函数的运行

哪这些又和内部链接，外部链接有什么关系呢？

那些编译单元\(.cpp\)中能向其他编译单元\(.cpp\)展示，提供其定义，让其他编译单元\(.cpp\)使用的的函数，变量就是外部链接，例如全局变量

而那些编译单元\(.cpp\)中不能向其他编译单元\(.cpp\)展示，提供其定义的函数，变量就是内部链接，例如static函数，inline函数等

好了让我们看下编译单元，内部链接和外部链接比较正式的定义吧

编译单元：当一个c或cpp文件在编译时，预处理器首先递归包含头文件，形成一个含有所有 必要信息的单个源文件，这个源文件就是一个编译单元。

内部连接：如果一个名称对编译单元\(.cpp\)来说是局部的，在链接的时候其他的编译单元无法链接到它且不会与其它编译单元\(.cpp\)中的同样的名称相冲突。

外部连接：如果一个名称对编译单元\(.cpp\)来说不是局部的，而在链接的时候其他的编译单元可以访问它，也就是说它可以和别的编译单元交互。

最后让我们回到文章开头处的那几个问题吧

**为什么有时会出现aaa已在bbb中重定义的错误？**

答：你可能在不同的cpp中重复定义了一个具有外部链接的函数或变量，链接器在链接时找到了多个一样的函数或变量定义

**为什么有时会出现无法解析的外部符号？**

答：你可能只提供了函数或变量的声明，没有提供其定义，或者声明和定义的函数原型不一致，链接器没有找到其定义在哪里，所以在链接环节出现了无法解析的外部符号的错误

**为什么有的内联函数的定义需要写在头文件中呢?**

答：因为内链函数是内部链接的，如果你在b.cpp中定义这个函数，那么在a.cpp中即使有这个函数声明，但由于内链函数是内部链接的，所以b.cpp不会提供其定义

所以在链接时a.obj无法找到这个函数的定义，便会出现无法解析的外部符号的错误

**为什么对于模板，声明和定义都要写在一起呢？**

答：我们假设我们有如下结构的代码

b.h

```cpp
#pragma once
template<typename T>
class A
{
public:
    A(const T &t);
};
```

b.cpp

```cpp
#include "b.h"
#include <iostream>

template<typename T>
A<T>::A(const T &t)
{
    std::cout << t << std::endl;
}
```

a.cpp

```cpp
#include "b.h"

int main()
{

    //A<int> a(5);
    return 0;
}
```

那么a.cpp中注释的那行代码能否正常运行呢？答案是不能我们首先来分析一下编译器在编译a.cpp时，发现其缺少A::a\(const int& t\)的定义而在编译器编译b.cpp时，由于每个编译单元是独立的，而模板只有被用到的时候才会被实例化，产生定义，b.cpp不知道a.cpp用了A::a\(const int& t\)，所以它不会提供A::a\(const int& t\)的定义，编译器不会有任何反应，这样在链接时a.obj无法找到A::a\(const int& t\)的定义，就会出现无法解析的外部符号的错误

**宏是内部链接还是外部链接**

答：都不是，宏在预处理环节时就被替换掉了，而内部链接与外部链接是针对编译环节与链接环节而言的

