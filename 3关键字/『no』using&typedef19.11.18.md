c++17 g++9.2.1 ubuntu 19.11.18

[TOC]

### 1、using：c++高级编程 p235-238

#### 1.1、创建类型别名：是一种类型

```c++
using ptr_int=int *;

ptr_int pa{nullptr}, pb{nullptr};  //如果是int *pa,pb;则前者是指针，后者是int数据
int a = 1, b = 2;
pa = &a, pb = &b;
cout << *pa << " " << *pb << endl;
/****************************************/
using vec2_int=vector<vector<int>>;

vec2_int vec2Int(10);
cout << size(vec2Int) << endl;
```

#### 1.2、函数指针的类型别名: 是一种类型

函数存放在某个地址，可以使用函数指针指向某个函数，函数指针可以使用类型别名。

```c++
template<typename T>
bool cmp_max(T &a, T &b) { return a > b; }

template<typename T>
bool cmp_min(T &a, T &b) { return a < b; }

template<typename T>
using csort=bool (*)(T &, T &);    //为什么不能用&&  可能是因为标准库的sort不接受右值的比较函数,可能是因为函数指针的&&不能推倒为&

template<typename T>
void usort(vector<T> &vec, csort<T> s) {    //函数指针前加&会报错，可能是因为指针的引用语法是*&而不是&*
    sort(begin(vec), end(vec), s);
}

vector<int> vec{5, 9, 1, 8, 6, 10, 0, 4};

usort(vec, &cmp_max);
for (const auto &item:vec)cout << item << " ";cout << endl;

usort(vec, cmp_min);    //&可以不写，而只用一个函数名，编译器知道函数名所在的地址
for (const auto &item:vec)cout << item << " ";cout << endl;
```

#### 1.3、方法和数据成员指针的类型别名

**指向类方法和数据成员的指针通常不会使用** 

```c++
class Test {
public:
    void test() const {
        cout << "hello world!" << endl;
    };
};

Test test;
using PtrToGet = void (Test::*)() const;  
PtrToGet methodPtr = &Test::test;
(test.*methodPtr)();    //hello world！

/***************************************/
//可以直接使用auto
Test test;
//using PtrToGet = void (Test::*)() const;  
//PtrToGet methodPtr = &Test::test;
auto methodPtr = &Test::test;
(test.*methodPtr)();
```



#### 1..4、泛型中使用using

<font color=red>待补充</font> 



### 2、typedef

using几乎能实现typedef的功能，**优先使用using而不是typedef，using支持模板化而typedef不支持**