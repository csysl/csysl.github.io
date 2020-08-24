# 引用

c++高级编程224-229 g++9.2.1 c++17

\[toc\]

## 一些原则

* 引用是一个变量的**别名**。
  * 引用创建后就不能再修改。
* 引用和指针优先使用引用。\(引用的功能指针几乎都能完成\)
  * 声明引用**必须初始化**：**类内引用成员变量除外**
* **右值引用**和**const引用**才可以接受未命名值。\(即普通引用**不接受临时变量**，const引用和右值引用可以\)

  * 普通初始化

  ```cpp
  int a=10;
  int& ref_a=a;  
  //int &ref_t=10;   //报错，不能直接指向左值  
  //int&& ref_t=10;  //正确，右值引用
  cout<<ref_a<<endl;//10
  ```

  * const初始化和右值引用    

```cpp
//int &ref_t=10;   //报错，不能直接指向左值  
int&& ref_t=10;  //正确，右值引用
const int& ref_t=10;  //正确

/***************************/

std::string getstr(){
    return "hello world!";
}
//string& ref_s=getstr();   //wrong
string&& ref_s=getstr();
const string& ref_s=getstr();
```

## 指针和引用

* **指向引用的指针**

```cpp
int a = 10;
int &ref_a = a;
int *ptr_a = &ref_a;
*ptr_a = 100;
cout << ref_a << " " << a << endl;//100 100
```

* **指针的引用**：很少见，但有时候很有用 **p225**  这个有时候以后再补充

```cpp
int *ptr_a = nullptr;
int *&ref_pa = ptr_a;
ref_pa = new int(100);
cout << *ptr_a << endl;    //100
```

## 类引用成员

在类中可以不初始化

在声明类对象时，必须初始化：在初始化器（**只能列表初始化，不能再构造函数体内**）中，不能在构造函数中初始化，**实质上还是在声明的时候就已经初始化了**（涉及到初始化器和在构造函数初始化的区别）

```cpp
class T {
public:
    explicit T(int &a) : aRef(a) {};   //只能列表初始化

    //explicit T(int &a) { aRef = a; };  //编译器报错
    //explicit T(int a) : aRef(a) {};   //不能值传递，不然aRef=0

    int &geta() {     //这里能返回引用的原因是，aRef本身是数据成员(已存在)，作用域在整个类对象，所以可以
        return aRef;
    }

private:
    int &aRef;
};

int a = 10;
T t{a};
std::cout << t.geta() << std::endl;
```

## 引用在函数中使用

* **作为参数**

> 普通传递：不复制，函数可以修改引用对象
>
> ```cpp
> void changeitem(std::vector<int> &vec) {
>     *std::begin(vec)=10;
> }
> vector<int> vec{1, 2, 3, 9, 8, 7, 5, 4, 6};
> changeitem(vec);    //10 2 3 9 8 7 5 4 6
> ```
>
> const传递：不复制，函数不能对引用对象修改
>
> ```cpp
> void getitem(const std::vector<int> &vec) {
>     for (const auto &item:vec) {
>         std::cout << item << " ";
>     }
>     std::cout << std::endl;
> }
> vector<int> vec{1, 2, 3, 9, 8, 7, 5, 4, 6};
> getitem(vec);    //1 2 3 9 8 7 5 4 6
> ```
>
> **指针可以解除函数的引用**：最好别用，ide会给提示
>
> ```cpp
> void uswap(int &a, int &b) {
>     auto tmp = a;
>     a = b;
>     b = tmp;
> }
>
> int a = 5, b = 10;
> int *ptr_a = &a, *ptr_b = &b;
> uswap(*ptr_a, *ptr_b);
> cout << a << " " << b << endl;   //10 5
> ```

* **作为返回值  感觉有点鸡肋**

**如果作用于局限于函数和方法内部，则不能返回！！！**

```cpp
int &uget() {
    int a = 10;
    int &ref_a = a;
    return ref_a;
}

auto tmp = uget();
cout << tmp << endl;    //编译器报错
```

类

```cpp
class T {
public:
    explicit T(int &a) : aRef(a) {};   //只能列表初始化

    int &geta() {     //这里能返回引用的原因是，aRef本身是数据成员(已存在)，作用域在整个类对象，所以可以
        return aRef;
    }

private:
    int &aRef;
};

int a = 10;
T t{a};
std::cout << t.geta() << std::endl;    //ok
```

