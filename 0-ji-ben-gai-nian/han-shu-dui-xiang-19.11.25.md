# 函数对象

c++高级编程 18.4 p417-423 函数对象

\[TOC\]

## 0. 说明

可以在class中重载**函数调用运算符`operator()`，**使class对象可以取代函数指针。将其称为**函数对象**

## 0. 一些原则

* 尽可能使用lambda表达式而不是**函数对象**。
* 建议**总是使用透明运算符仿函数**。

## 1. 内置函数对象

### 1.1 算术函数对象

> plus\minus\multiplies\divides\modulus: 加、减、乘、除、取模

```cpp
array<int, 9> arr{1, 2, 3, 4, 5, 6, 7, 8, 9};

auto res = accumulate(begin(arr), end(arr), 1, multiplies<>());
cout << res << endl;  //362880
```

### 1.2 比较函数对象

> equal\_to, not\_equal\_to, less, greater, less\_equal, and greater\_equal.

```cpp
array<int, 9> arr{1, 2, 3, 4, 5, 6, 7, 8, 9};

sort(begin(arr), end(arr), greater<>());
for (auto it:arr)cout << it << " "; cout << endl;  //9 8 7 6 5 4 3 2 1
```

### 1.3 逻辑函数对象

> logical\_not \(operator!\), logical\_and \(operator&&\), and logical\_or\(operator\|\|\).

```cpp
bool allTrue(const vector<bool>& flags){  //检查是否全是true
    return accumulate(begin(flags), end(flags), true,logical_and<>());
}
```

### 1.4 按位函数对象

> bit\_and \(operator&\), bit\_or \(operator\|\), bit\_xor \(operator^\), and bit\_not \(operator~\).

## 2. 函数对象适配器

### 2.1 绑定器  bind\(\)

**作用**：将函数和参数绑定

```cpp
auto it=find_if(begin(vec),end(vec),bind(greater_equal<>(),placeholders::_1, 100));
//可替代为
auto it=find_if(begin(vec),end(vec),[](int a){return a>=100;});
```

* 没有绑定固定值的参数使用`_1,_2,..._n`表示，存放在命名空间`placeholders`中。

```cpp
void func(int a, const string &b) {
    cout << "func(" << a << ", " << b << ")" << endl;
}

auto f1 = bind(func, placeholders::_1, "Hello");   //第一个参数没绑定，用_1表示，第二个参数绑定了"hello"
f1(10);   //func(10, Hello)
```

* `_1,_2,..._n`可以不按顺序出现

```cpp
auto f2 = bind(func, placeholders::_2, placeholders::_1);   //_2在前，_1在后
f2("Hello", 10);   //func(10, Hello)
```

* bind\(\)绑定的参数为参数副本，需使用ref\(\)或者cref\(\)传递引用

```cpp
void func(int &x) { ++x; }

int x = 0;
//auto f = bind(func, x);  //这样调用的话最后输出为0
auto f = bind(func, ref(x));
f();
cout << x << endl;  //1
```

* **bind\(\)绑定重载函数要分别绑定**\(相当不方便！！！\)

```cpp
void func(int &x) { ++x; }
void func(double &x) { x += 1; }

int x = 0;
//auto f = bind(func, ref(x));  //报错，bind无法识别func是哪个函数
auto f = bind((void (*)(int &)) func, ref(x));  //int
//auto f = bind((void (*)(double &)) func, ref(x));//double,但是调用时报错，因为x是整形
f();
cout << x << endl;
```

### 2.2 取反器 ：not\_fn\(c++17\)\not1\not2

`not1`和`not2`已不建议使用

**作用**：类似于bind\(\)，但**对调用结果取反**

```cpp
array<int, 9> arr{1, 2, 3, 4, 5, 6, 7, 8, 9};
//返回array中第一个>=5的数字的迭代器
auto iter = find_if(cbegin(arr), cend(arr), not_fn([](int a) { return a < 5; }));
cout << iter - cbegin(arr) << endl;  //4
```

## 3. 可以自己编写函数对象

在要完成更复杂的任务，lambda不够简洁时可以考虑编写自己的函数对象，尽量不使用！！！

**实现方法**：**重载**函数运算符**operator\(\)**，**注意必须使用const修饰**！！！！

```cpp
class myIsDigit {
public:
    bool operator()(char c) const { return ::isdigit(c) !=0; }
};
bool isNumber(string_view str) {
    auto endIter = end(str);
    auto it = find_if(begin(str), endIter, not_fn(myIsDigit()));
    return (it == endIter);
}
```

