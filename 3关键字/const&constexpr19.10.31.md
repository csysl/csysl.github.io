

[TOC]

### 1、const变量和参数

#### 1.1、const标记类成员变量和全局变量

必须在声明的时候初始化，可以代替`#define`来定义常量

```c++
#include <bits/stdc++.h>
using namespace std;

const double pi = 3.1415926;   //定义内部变量，定义时初始化   
//#define pi 3.1415926;

constexpr double getpi(double &&a, double &&b) {
    return a * b;
}

class T {
public:
    const int ca;   //定义类const成员，创建类对象时初始化
    int &ref_b;  //引用对象，创建类对象时初始化

    explicit T(int a, int &&b) : ca(a), ref_b(b) {   //对const和引用的数据成员，要使用列表初始化
        cout << "construction succeed!" << endl;
    };
};

int main() {
    T t(getpi(3.0, 4.0), 10);
    return 0;
}
```



#### 1.2、const指针

|                                         |                      |                                                          |
| --------------------------------------- | -------------------- | -------------------------------------------------------- |
| int * const x;                          | 指向整型的常量指针   | x不能指向其他地址                                        |
| int const *x;(const int *x; )           | 指向常整型的指针     | x所指向的数据**不能通过x被修改**,x可以指向不同的对象     |
| int const *const x;(const int *const x) | 指向常整型的常量指针 | x所指向的数据不能被修改，同时x自身也不能指向其他地址空间 |

```c++
int a = 10;
int const *ptr_ca = &a;     //等同于const int *ptr_ca = &a;
int *const cptr_a = &a;
int const *const cptr_ca = &a;    //第一个const在int右边，所以int不变；第二个const在*右边，来修饰指针    也可以写作const int *const cptr_ca = &a;
```



#### 1.3、const引用

- 引用默认为const，无法改变引用的对象
- 在作为函数参数时，尽可能使用`const &`传递参数，

```c++
#include <bits/stdc++.h>
using namespace std;

constexpr double getpi(double &&a, double &&b) {
    return a * b;
}

class T {
public:
    const int ca;   //定义类const成员，创建类对象时初始化
    int &ref_b;  //引用对象，创建类对象时初始化

    explicit T(int a, int &&b) : ca(a), ref_b(b) {   //对const和引用的数据成员，要使用列表初始化
        cout << "construction succeed!" << endl;
    };
};

int main() {

    int a = 10;
    int &cref_a = a;    //实际上为int &const cref_a = a;
    int const &cref_ca = a;  //实际上为int const & const cref_ca = a;  也可写作const int & const cref_ca=a;
    

    T t(getpi(3.0, 4.0), 10);
    return 0;
}
```



### 2、class const成员函数

标记为const的成员函数，**不能修改**类的成员变量

**const对象不能调用非const成员函数**，**非const对象可以调用所有方法。**

静态成员函数const对象和非const对象都可以调用。

```c++
class Test {
public:
    static void setvalue(const int invalue) {svalue = invalue;}
	void setvalue2(const int invalue) {mvalue = invalue;}
    int getvalue() const {return svalue;}
    int getvalue2() {return mvalue;}

private:
    static inline int svalue = -1;
    int mvalue;
};

int main() {

    Test t{};
    t.setvalue(10);
    t.setvalue2(10);
    cout << t.getvalue() << "" << t.getvalue2() << endl;

    const Test ct{};
    ct.setvalue(15);   //const对象可以调用static函数
    //ct.setvalue2(15);   //编译器报错，const对象不能调用非const函数
    cout << ct.getvalue() << "" << endl;// cout<<ct.getvalue2(); //const对象不能调用非const函数

    return 0;
}
```



### 3、constexpr关键字

**使用constexpr将函数声明为常量表达式和构造函数**，常量表达式在**编译时计算**。

```c++
constexpr int getArraySize() { return 32; }
const int getArraySize2() { return 32; }
int main(){
	int myArray[getArraySize()];    //ok
    int myArray[getArraySize2()];    //error
    return 0;
}
/*********构造函数*********/
class Rect{
public:
	constexpr Rect(size_t width, size_t height): mWidth(width), mHeight(height){}
	constexpr size_t getArea() const { return mWidth * mHeight; }
private:
	size_t mWidth, mHeight;
};
```

**使用constexpr的函数的限制**：c++高级编程第11章 p232



**constexpr可以声明构造函数**限制：c++高级编程第11章 p232

