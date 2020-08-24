# 02atomic库

## 概述

* 原子对象**不需要额外的同步机制**就可以执行并发的读写操作。
* 原子对象时线程安全的。
* atomic对象**不能复制和移动**

**头文件**

## 使用

某些类型的原子操作底层实现可能使用同步机制。

`atomic<T>`可以跟所有类型一起使用，但是`T`要求满足`is_trivall_copy`的特性。

```cpp
#include<atomic>
struct Test {
    int a;
    double b;
    //string str;   //string类型不可以
};

int main() {

    atomic<Test> t;
    cout << is_trivially_copyable<Test>() << endl;  //1
    cout << t.is_lock_free() << endl;   //1   该函数判断当前类型底层支不支持无锁操作


    return 0;
}
```

## [原子操作](https://zh.cppreference.com/w/cpp/atomic/atomic%20) c++高级编程p558

Atomic **integral types** support the following atomic operations: **fetch\_add\(\), fetch\_sub\(\), fetch\_and\(\), fetch\_or\(\), fetch\_xor\(\), ++, --, +=, -=, &=, ^=, and \|=**. Atomic **pointer types** support **fetch\_add\(\), fetch\_sub\(\), ++, --, +=, and -=**.

```cpp
/*一fetch_add()为例*/
atomic<int> value(10);
cout << "Value = " << value << endl;  //10
int fetched = value.fetch_add(4);    //加上4并返回当前值
cout << "Fetched = " << fetched << endl;  //10  
cout << "Value = " << value << endl;  //14
```

