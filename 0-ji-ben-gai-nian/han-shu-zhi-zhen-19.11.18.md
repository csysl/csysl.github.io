# 函数指针

c++17 g++9.2.1 ubuntu 19.11.18

\[TOC\]

**什么是解引用**

```cpp
int a = 10;
int *pa = &a;
cout << *pa << endl;   //*pa就叫解引用

bool cmp(int &&a, int &&b) { return a > b; };
auto pcmp = &cmp;
//cout << (*pcmp)(5, 6) << endl;   //(*pcmp)(5,6) 就叫解引用  前面的括号是因为，后面传参用的()的优先级要比*高，所以用(*pcmp)
cout << pcmp(6, 5) << endl;   //现在的编译器会自动解引用，不需要手动实现，但是非静态的类成员函数不可以
```

## 0. 声明函数指针的方法

```cpp
bool cmp(int &&a, int &&b) { return a > b; };
/*使用auto*/
auto pcmp = &cmp;   //是一个对象，等价于  bool(*pcmp)(int &&,int &&)=cmp;,不加&也可以，函数名本身就是个指针
/*使用using*/
using pcmp = bool(*)(int ,int;   //是一种类型
/*使用typedef*/
typedef bool(*pcmp)(int ,int);  //是一种类型

/***********************************/
class Test {
public:
    void test() const {
        cout << "hello world!" << endl;
    };
};
/*使用auto*/
auto pt = &Test::test;
/*使用using*/
using pt1 = bool (Test::*)() const;    //pt1是一种类型
/*使用typedef*/
typedef bool(Test::*pt2)() const;    //pt2是一种类型

Test t;
(t.*pt)();

pt1 p1 = &Test::test;   //声明pt1 类型的对象p1
(t.*p1)();

pt2 p2 = &Test::test;   //声明pt2 类型的对象p2
(t.*p2)();
```

## 1. 指向普通函数的指针

```cpp
bool cmp(int &&a, int &&b) { return a > b; };
auto pcmp = &cmp;
//cout << (*pcmp)(5, 6) << endl;   //(*pcmp)(5,6) 就叫解引用  前面的括号是因为，后面传参用的()的优先级要比*高，所以用(*pcmp)
cout << pcmp(6, 5) << endl;   //现在的编译器会自动解引用，不需要手动实现，但是非静态的类成员函数不可以
```

## 2. 指向类成员函数的指针：

**这里附带指向类数据成员的指针，因为两者原理是相似的**

**指向类方法和数据成员的指针通常不会使用**

与指向普通函数的函数指针的不同点：

* 可以声明指向其数据成员和方法的指针，但是**对于非静态成员，必须依托实例化对象来解除引用**\(即只有对象存在了，才能调用函数\)。但是对于**非静态成员，即使没有实例化对象，也可以解除引用**

```cpp
#include <bits/stdc++.h>
using namespace std;

class Test {
public:
    void test() const {
        cout << "hello world!" << endl;
    };

    static void test2() {
        cout << "i love you baby!" << endl;
    }
};

int main() {
    auto ptr = &Test::test2;      //static
    //(*ptr)();                     //i love you  baby!
    ptr();    //编译器会自己实现解引用，但是非静态的成员函数指针不可以这样使用

    auto ptr1 = &Test::test;      //non-static
    //(*ptr1)();                  //编译器报错，在没有实例化对象的情况下解引用

    Test t1, t2;

    (t1.*ptr1)();                 //hello world!   不能使用编译器自动解引用
    (t2.*ptr1)();                 //hello world!

    return 0;
}
```

