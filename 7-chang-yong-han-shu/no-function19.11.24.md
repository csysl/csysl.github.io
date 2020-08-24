# 『no』function

**头文件**：

可以用来**创建**指向**函数、函数对象、lambda表达式**的**类型**\(创建的是一种类型\)，被称作多态函数包装器，可以当作**函数指针**，也可当作**回调函数的参数**。

## 1. 实现函数指针

与普通函数指针相比，**做参数能够传入lambda表达式**，能够做回调函数

```cpp
void test(int a, string &&s) {
    cout << a << ":" << s << endl;
}

int main() {

    array<int, 4> arr = {1, 2, 3, 4};
    function<void(int, string &&)> f = test;   //函数指针
    //auto f= test;  //推导类型为void(*f)(int, string &&)而不是function
    f(10, "hello world!"s);

    return 0;
}
```

## 2. 将回调做类的成员变量

```cpp

```

## 3. function做函数变量

**function对象**做参数：可以接受**函数指针、function对象、lambda对象**

**函数指针**做参数：可以接受**函数指针、lambda对象**。

```cpp
void cal(const array<int, 4> &arr, const function<void(int)> &f) {
    for (auto &v:arr) {
        f(v);
    }
    cout << endl;
}

void print(int a) {
    cout << a << " ";
}

int main() {
    array<int, 4> arr = {1, 2, 3, 4};
    function<void(int)> p = print;
    cal(arr, print);   //1 2 3 4    函数指针
    cal(arr, p);       //1 2 3 4    function对象
    cal(arr, [](int a) { cout << a << " "; });   //1 2 3 4   lambda表达式
    return 0;
}
/***********************************************************/
void print(int a) {
    cout << a << " ";
}

using func_p=void (*)(int);   

template<typename T>
void cal(const array<T, 4> &arr, func_p f) {    //使用函数指针作为参数
    for (auto &v:arr) {
        f(v);
    }
    cout << endl;
}

int main() {
    array<int, 4> arr = {1, 2, 3, 4};
    function<void(int)> p = print;
    cal(arr, print);   //接收函数指针
    //cal(arr, p);     //报错，不能接收function对象
    cal(arr, [](int a) { cout << a << " "; });  //接收lambda对象

    return 0;
}
```

## 4. function支持模板

模板可接收任何对象

```cpp
template<typename T, typename F>
void cal(const array<T, 4> &arr, F f) {    //模板化
    for (auto &v:arr) {
        f(v);
    }
    cout << endl;
}

void print(int a) {
    cout << a << " ";
}

int main() {
    array<int, 4> arr = {1, 2, 3, 4};
    function<void(int)> p = print;
    cal(arr, print);   //函数接收函数指针
    cal(arr, p);   //函数接收function对象
    cal(arr, [](int a) { cout << a << " "; });
    return 0;
}
```

