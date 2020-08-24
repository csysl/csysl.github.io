# lambda表达式

\[TOC\]

## 0. 概述

* lambda表达式只捕获其封装的作用域内的**非静态局部变量**\(**不能捕获static变量、类的成员变量、全局或命名空间中的对象**\)
* 可以忽略返回类型，编译器会自动推导
* **通过复制捕获的对象不能在lambda中被修改，除非使用mutable修饰**：
  1. 在**捕获列表**中**按值捕获**的数据跟原数据具**有相同的const属性**。

```cpp
const int a = 5, b = 5;
int c = 5;

auto test = [b, c](auto a) {
    cout << a << ": ";
    a = 10;
    //b = 10;   //报错，捕获列表的值复制与原对象有相同的const属性
    //c = 10    //虽然是非const,但报错，原因见下条
    cout << a << endl;
};

test(a);  //5: 10  做为参数的对象跟普通函数一样，值复制没有const属性
```

1. **即使**前一条捕获的**是非const对象**，也**不能修改其复制的副本**，因为实现lambda的`operator()`有**const属性** 。可以使用**mutable修饰**\(修饰的是函数体，前面\)使得非const对象的副本可更改。

```cpp
const int a = 5, b = 5;
int c = 5;

auto test = [b, c](auto a) {
    cout << a << ": ";
    a = 10;
    //b = 10;   //报错，捕获列表的值复制与原对象有相同的const属性
    //c = 10    //虽然是非const
    cout << a << endl;
};
/*****************************/
auto test = [b, c](int a)mutable {
    cout << a << ": ";
    a = 10;
    //b = 10;   //报错，捕获列表的值复制与原对象有相同的const属性
    c = 10    //不再报错
    cout << a << endl;
};
/*****************************/
auto test = [b, &c](int a) {
    cout << a << ": ";
    a = 10;
    //b = 10;   //报错，捕获列表的值复制与原对象有相同的const属性
    c = 10    //不再报错，因为是引用捕获
    cout << a << endl;
};
```

1. 而作为参数的值传递跟普通函数一样，不具有const属性，**原因是**：编译器把lambda表达式转换为某种未命名的仿函数\(就是函数对象\)，仿函数总是调用`operator()`实现。

## 1. 结构

**原则**：

* **建议显式指定要捕获哪些变量**，不建议捕获全部变量

```cpp
[capture_block](parameters) mutable constexpr noexcept_specifier attributes
-> return_type {body}
```

| 符号 | 说明 |
| :---: | :---: |
| \[=\] | 按**复制**方法捕获作用域所有变量 |
| \[&\] | 按**引用**方法捕获作用域所有变量 |
| \[&x\] | 按引用捕获x，其他变量不捕获 |
| \[x\] | 按复制捕获x，其他变量不捕获 |
| \[=,&x,&y\] | 按引用捕获x和y，其他变量复制捕获 |
| \[&,x\] | 按复制捕获x，其他变量引用捕获 |
| \[&x,&x\] | **wrong** |
| \[this\] | **捕获当前的对象**，没有this-&gt;,lambda也可以访问该对象 |
| \[\*this\] | **捕获当前对象的副本** |

## 2. 应用

### 2.1 泛型lambda

```cpp
auto test = [](auto a, auto b) -> decltype(a + b) { return a + b; };
//auto test = [](auto a){cout<< a <<endl; };

int a = 10;
double b = 12.34;
auto c = test(a, b);
cout << c << " " << typeid(c).name() << endl;   //22.34  d（double）
```

### 2.2 lambda捕获表达式

可以**在捕获列表中**对数值进行**初始化**，初始化也支持`std::move()`\(比如unique\_ptr类型\)

```cpp
array<int, 10> arr{1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
generate(begin(arr), end(arr), [x = 0]()mutable {   
    ++x;
    return x * x;
});
for (auto x:arr)cout << x << " ";cout << endl;
```

### 2.3 做函数返回值

`function`可以返回lambda表达式

```cpp
function<int(void)> test(int x) {   //function<int(void)> 可以换成auto
    return [x] { return x * 2; };
}

int main() {
    auto t = test(10);
    cout << t() << endl;  //20

    return 0;
}
```

### 2.4 做函数参数

**`function`**和**函数指针型参数**可以接收lambda表达式

```cpp
void print(int a) {
    cout << a << " ";
}

using func_p=void (*)(int);   

/*实现1*************************/
template<typename T>
void cal(const array<T, 4> &arr, func_p f) {    //使用函数指针作为参数
    for (auto &v:arr) {
        f(v);
    }
    cout << endl;
}
/*实现2*****************************/
void cal(const array<int, 4> &arr, const function<void(int)> &f) {
    for (auto &v:arr) {
        f(v);
    }
    cout << endl;
}
/*实现3*/
template<typename T, typename F>
void cal(const array<T, 4> &arr, F f) {    //模板化
    for (auto &v:arr) {
        f(v);
    }
    cout << endl;
}

int main() {
    array<int, 4> arr = {1, 2, 3, 4};
    cal(arr, [](int a) { cout << a << " "; });

    return 0;
}
```

### Effective modern c++

* 避免默认捕获变量，而是显式指定。lambda表达式捕获指针变量时，可能会造成悬垂指针。
* 
