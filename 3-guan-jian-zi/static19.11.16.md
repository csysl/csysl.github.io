# static

c++高级编程 11章 p232-234 **讲的很清楚**

\[TOC\]

## 1、修饰类成员和方法

static修饰的数据成员和方法，有所有类对象公用（**只有一个副本**，属于类层次而不是对象层次），不属于任何对象，**没有this指针**。

### 1.1 静态数据成员

static修饰的数据成员，不能在类中初始化，必须在**类外分配内存**并初始化。**默认情况下会被初始化为0，指针初始化为nullptr**。

```cpp
class Test {
public:
    static void setValue(const int invalue) {
        svalue = invalue;
    }

    static int getValue()  {
        return svalue;
    }

private:
    static int svalue;   //类内声明
};

int Test::svalue = 10;   //类外分配内存并初始化，没有分配内存会报错

int main() {
    int a = 5;
    Test::setValue(a);
    cout << Test::getValue() << endl;
    return 0;
}
```

c++17可以将静态数据成员声明为**inline**，这样可以**在类中**对静态数据成员**初始化**。

```cpp
class Test {
public:
    static void setValue(const int invalue) {
        svalue = invalue;
    }

    static int getValue()  {
        return svalue;
    }

private:
    static inline int svalue = 10;//inline修饰，可以直接在类中初始化
};

int main() {
    int a = 5;
    Test::setValue(a);
    cout << Test::getValue() << endl;
    return 0;
}
```

静态数据成员可以声明为常量，**整形和枚举类型**的静态**常量**数据成员可以**在类中直接初始化**。

```cpp
enum class Color {
    Yellow, Green, Red
};

class Test {
public:
    static void setValue(const int invalue) {
        svalue = invalue;
    }

    static int getValue() {
        return svalue;
    }

    static const int height = 100;   //静态常量数据成员

private:
    static inline int svalue = 10;  //inline修饰的静态数据成员
    static const Color mcolor = Color::Green;   //静态常量数据成员：枚举类型
};
```

### 1.2 静态方法

静态方法**只可以访问类的静态数据成员和其他静态方法**

**静态方法和普通方法**都**可以**调用静态数据成员和方法。

静态方法**不能声明为const**，因为static成员并不能看作是对象实例所拥有的成员？？？？？？

对静态成员的引用**不需要用对象名**。

```cpp
enum class Color {
    Yellow, Green, Red
};

class Test {
public:
    static void setValue(const int invalue) {   //静态方法
        //mvalue = invalue;  //错误，非静态成员
        svalue = invalue;
    }

    static int getValue() {  //静态方法
        return svalue;
    }

    static const int height = 100;   //静态常量数据成员

private:
    static inline int svalue = 10;  //inline修饰的静态数据成员
    static const Color mcolor = Color::Green;   //静态常量数据成员：枚举类型
    int mvalue;
};
```

## 2、用于静态链接

> **外部链接**：名称在其他源文件中也有效。**函数**和**全局变量**都拥有外部链接。
>
> **内部链接\(静态链接\)**：只在当前cpp有效在其他源文件无效。
>
> 要使用静态链接，**建议**使用匿名namespace，**不建议**使用static。

**当不使用static时**

```cpp
/**********************func.cpp*****************************/
#include <iostream>

void f();

void f() {
    std::cout << "hello world!" << std::endl;
}

/**********************main.cpp*****************************/
#include <bits/stdc++.h>
using namespace std;

void f();

int main() {
    f();       //输出hello world!
    return 0;
}
```

**使用static时**

```cpp
/**********************func.cpp*****************************/
#include <iostream>

static void f();    //使用了static关键字，函数f()作用域仅限于func.cpp

void f() {
    std::cout << "hello world!" << std::endl;
}

/**********************main.cpp*****************************/
#include <bits/stdc++.h>
using namespace std;

void f();

int main() {
    f();       //编译错误
    return 0;
}
```

## 匿名namespace : 实现内部链接的另一种方式

在同一源文件\(cpp\)中，可以在声明命名空间**之后\(之前不可以\)**的任意位置访问该命名空间中的项，而**不能被其他文件访问**。

```cpp
/**********************func.cpp*****************************/
#include <iostream>

namespace {
    void f() {
        std::cout << "hello world!" << std::endl;   //匿名命名空间，作用域为当前文件
    }
}

void ref(){
    f();
}

/**********************main.cpp*****************************/
#include <bits/stdc++.h>
using namespace std;

//void f();
void ref();

int main() {
    //f();     //编译错误
    ref();     //hello world!

    return 0;
}
```

## 3、对函数中的变量使用static    ：  尽量避免使用

**在函数中的static变量，每次调用函数时共享**。常见用法是标记某个函数是否执行了特定的初始化操作

**避免使用单独的静态变量，为了维持状态可以使用对象。**  why p234

```cpp
enum class state {
    LABEL1, LABEL2
};
void test(bool i) {      //并不能保持线程安全
    static auto s = state::LABEL1;
    cout << static_cast<int>(s) << endl;
    if (i)s = state::LABEL2;
}
int main() {
    test(true);  //0
    test(true);  //1
    return 0;
}
```

