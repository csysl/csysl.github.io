# 模板推导

## 总结

> 引用的实参会被当做非引用类型处理。 int &a=c; 传入a时&会被忽略
>
> **万能引用**形参进行推导时，**左值实参**会被特殊处理
>
> 对**按值传递的形参**进行推导时，const或者volatile修饰的实参会当做不带const或者volatile的量进行处理，因为按值传递时是直接复制给形参，无法保证其const的性质

## 1、按引用、指针传参

* 按引用、指针传参时，实参本身的引用会被忽略。
* 实参的引用性被忽略。  
* `T&` **模板传入const对象是安全的，实参的const属性会在形参得到保留。**

```cpp
template<typename T>
void f(T &a);

int a=10;
const int b=a;
const int &c=a;
const char* const str="hello world!";

f(a);  //T is int, int &
f(b);  //T is const int, param is const int &
f(c);  //T is const int, param is const int &
f(str); //

void f(const T &a);
/*T为int*/
f(a);  //const int &
f(b);  //const int &
f(c);  //const int &
f(str); //

void f(T *a);

int x=7;
const int *px=&x;

f(&x);  //T is int ,param is int *
f(px); //T is const int, param is const int *
```

## 2、按万能引用传参

* 如果实参是一个**左值**，则直接按**左值引用**处理
* 如果实参是一个**右值**，则按常规的**右值引用**处理

```cpp
template<typename T>
void f(T &&a);

int x=10;
const int b=x;
const int &px=x;

f(x);   //左值，T is int,param is int &
f(b);  //左值, T is const int,param is const int &
f(px);  //左值,T is const int,param is const int &

f(10);  //右值, T is int ,param is int &&
```

## 3、按值传递

这种情况下会将参数复制给形参，在这个过程中形参**不会保留参数的常量性**，

```cpp
template<typename T>
void f(T a);

int a=10;
const int b=a;
const int &c=a;
const char* const str="hello world!";

f(a);   //T & param is int
f(b);   //T & param is int
f(c);   //T & param is int
f(str); //T & param is const char *
```

