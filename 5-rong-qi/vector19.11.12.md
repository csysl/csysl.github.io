# vector

\[TOC\]

[参考](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/5容器/c++高级编程17.2%20顺序容器)

### 0、注意

* 声明时编译器开辟一段连续的内存，所以内存不够时，会把它**整体复制**到一块更大的内存，**之前的迭代器失效**，为了效率最好事先声明大小
* vector的operate\[\]没有边界检查，越过边界会引发未定义行为
* 在空容器使用front\(\)和back\(\)会引发未定义行为
* **vector**特化用单个bits存储，不建议使用.建议使用bitset或者vector

### 1、定义

```cpp
template<
    class T,
    class Allocator = std::allocator<T>   //指定内存分配器，基本不用用户自己指定
> class vector;
```

分配器什么时候需要用户指定呢？

### 2、初始化方法

```cpp
vector<int> v1;
cout<<v1.capacity()<<endl;   //0

vector<int> v2(100);    //100个元素全部初始化为0，float为0.0，指针为nullptr
cout<<v2.capacity()<<endl;  //100

vector<int> v3(100, -1);  //100个元素初始化为-1
vector<string> v4(100, "hello");

vector<int> vec{45, 4, 5, 12};   //统一初始化
```

### 2、迭代器

```cpp
std::vector<int> vec{45, 4, 5, 12};

for (auto iter = std::begin(vec); iter != end(vec); ++iter) {}  //非const迭代器
for (auto citer = std::cbegin(vec); citer != cend(vec); ++citer) {}  //const迭代器
for (auto &it:vec) {}  //基于区间的循环
```

### 2、移动语义

```cpp
vector<string> v;
string el;
v.push_back(move(el));
```

### 3、函数

#### 3.1 assign\(\)  删除并重新分配元素

```cpp
vector<int> intVector(10);
// Other code …
intVector.assign(5, 100);   //删除并重分配

intVector.assign({ 1, 2, 3, 4 });
```

#### 3.2 swap\(\) 交换两个vector的内容，常量时间复杂度

```cpp
vector<int> vectorOne(10);
vector<int> vectorTwo(5, 100);
vectorOne.swap(vectorTwo);
// vectorOne now has 5 elements with the value 100.
// vectorTwo now has 10 elements with the value 0.
swap(vectorOne,vectorTwo);   //这么写也可以
```

#### 3.3 front\(\)&back\(\)返回vector的首尾元素，vector为空则会引发未定义行为

```cpp
std::vector<int> vec{45, 4, 5, 12};
cout << vec.front() << " " << vec.back() << endl;  //45  12
```

#### 3.4 [insert\(\)](https://zh.cppreference.com/w/cpp/container/vector/insert)&[erase](https://zh.cppreference.com/w/cpp/container/vector/erase) 插入和删除元素

**insert\(\)**

```cpp
void print_vec(const std::vector<int>& vec){
    for (auto x: vec) std::cout << ' ' << x;std::cout << '\n';
}

int main ()
{
    std::vector<int> vec(3,100);
    print_vec(vec);       //100 100 100

    auto it = vec.begin();
    it = vec.insert(it, 200);
    print_vec(vec);      //200 100 100 100

    vec.insert(it,2,300);
    print_vec(vec);      //300 300 200 100 100 100

    // "it" 不再合法，获取新值：
    it = vec.begin();

    std::vector<int> vec2(2,400);
    vec.insert(it+2, vec2.begin(), vec2.end());
    print_vec(vec);      //300 300 400 400 200 100 100 100

    int arr[] = { 501,502,503 };
    vec.insert(vec.begin(), arr, arr+3);
    print_vec(vec);      //501 502 503 300 300 400 400 200 100 100 100
}
```

**erase\(\)**

```cpp
int main( )
{
    std::vector<int> c{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    for (auto &i : c) std::cout << i << " ";std::cout << '\n';

    c.erase(c.begin());

    for (auto &i : c) std::cout << i << " ";std::cout << '\n';//1 2 3 4 5 6 7 8 9

    c.erase(c.begin()+2, c.begin()+5);  //前闭后开

    for (auto &i : c) std::cout << i << " ";std::cout << '\n';//1 2 6 7 8 9
}
```

#### 3.5 emplace\(\)&emplace\_back\(\)  就地构造对象

c++17开始emplace\_back\(\)返回对象的引用

```cpp
vector<int> vec{12, 44, 8, 6, 1, 11};
auto iter = cbegin(vec) + 3;
//auto ree = vec.emplace(iter, 100);   //wrong
vec.emplace(iter, 100);
for (auto &v:vec)cout << v << " ";cout << endl;   //12 44 8 100 6 1 11

auto re = vec.emplace_back(15);
cout << re << endl;    //15
```

#### 3.6 data\(\) 返回vector数组的指针

```cpp
vector<int> vec{12, 44, 8, 6, 1, 11};

auto ptr = vec.data();
cout << *(ptr + 3) << endl;    //6

auto ptr2 = data(vec);
cout << *(ptr2 + 4) << endl;   //1
```

#### 3.7 reserve\(\) 预留存储空间

```cpp

```

#### 4、

| [\(构造函数\)](https://zh.cppreference.com/w/cpp/container/vector/vector) | 构造 `vector` \(公开成员函数\) |
| :--- | :--- |
| [\(析构函数\)](https://zh.cppreference.com/w/cpp/container/vector/~vector) | 析构 `vector` \(公开成员函数\) |
| [operator=](https://zh.cppreference.com/w/cpp/container/vector/operator%3D) | 赋值给容器 \(公开成员函数\) |
| [assign](https://zh.cppreference.com/w/cpp/container/vector/assign) | 将值赋给容器 \(公开成员函数\) |
| [get\_allocator](https://zh.cppreference.com/w/cpp/container/vector/get_allocator) | 返回相关的分配器 \(公开成员函数\) |
| 元素访问 |  |
| [at](https://zh.cppreference.com/w/cpp/container/vector/at) | 访问指定的元素，同时进行越界检查 \(公开成员函数\) |
| operator\[\] | 访问指定的元素 \(公开成员函数\) |
| [front](https://zh.cppreference.com/w/cpp/container/vector/front) | 访问第一个元素 \(公开成员函数\) |
| [back](https://zh.cppreference.com/w/cpp/container/vector/back) | 访问最后一个元素 \(公开成员函数\) |
| [data](https://zh.cppreference.com/w/cpp/container/vector/data) | 返回指向内存中数组第一个元素的指针 \(公开成员函数\) |
| 迭代器 |  |
| [begin  cbegin](https://zh.cppreference.com/w/cpp/container/vector/begin) | 返回指向容器第一个元素的迭代器 \(公开成员函数\) |
| [end  cend](https://zh.cppreference.com/w/cpp/container/vector/end) | 返回指向容器尾端的迭代器 \(公开成员函数\) |
| [rbegin crbegin](https://zh.cppreference.com/w/cpp/container/vector/rbegin) | 返回指向容器最后元素的逆向迭代器 \(公开成员函数\) |
| [rend  crend](https://zh.cppreference.com/w/cpp/container/vector/rend) | 返回指向前端的逆向迭代器 \(公开成员函数\) |
| 容量 |  |
| [empty](https://zh.cppreference.com/w/cpp/container/vector/empty) | 检查容器是否为空 \(公开成员函数\) |
| [size](https://zh.cppreference.com/w/cpp/container/vector/size) | 返回容纳的元素数 \(公开成员函数\) |
| [max\_size](https://zh.cppreference.com/w/cpp/container/vector/max_size) | 返回可容纳的最大元素数 \(公开成员函数\) |
| [reserve](https://zh.cppreference.com/w/cpp/container/vector/reserve) | 预留存储空间 \(公开成员函数\) |
| [capacity](https://zh.cppreference.com/w/cpp/container/vector/capacity) | 返回当前存储空间能够容纳的元素数 \(公开成员函数\) |
| [shrink\_to\_fit](https://zh.cppreference.com/w/cpp/container/vector/shrink_to_fit)\(C++11\) | 通过释放未使用的内存减少内存的使用 \(公开成员函数\) |
| 修改器 |  |
| [clear](https://zh.cppreference.com/w/cpp/container/vector/clear) | 清除内容 \(公开成员函数\) |
| [insert](https://zh.cppreference.com/w/cpp/container/vector/insert) | 插入元素 \(公开成员函数\) |
| [emplace](https://zh.cppreference.com/w/cpp/container/vector/emplace)\(C++11\) | 原位构造元素 \(公开成员函数\) |
| [erase](https://zh.cppreference.com/w/cpp/container/vector/erase) | 擦除元素 \(公开成员函数\) |
| [push\_back](https://zh.cppreference.com/w/cpp/container/vector/push_back) | 将元素添加到容器末尾 \(公开成员函数\) |
| [emplace\_back](https://zh.cppreference.com/w/cpp/container/vector/emplace_back)\(C++11\) | 在容器末尾就地构造元素 \(公开成员函数\) |
| [pop\_back](https://zh.cppreference.com/w/cpp/container/vector/pop_back) | 移除末元素 \(公开成员函数\) |
| [resize](https://zh.cppreference.com/w/cpp/container/vector/resize) | 改变容器中可存储元素的个数 \(公开成员函数\) |
| [swap](https://zh.cppreference.com/w/cpp/container/vector/swap) | 交换内容 \(公开成员函数\) |

## 非成员函数

| [operator==operator!=operatoroperator&gt;=](https://zh.cppreference.com/w/cpp/container/vector/operator_cmp) | 按照字典顺序比较 vector 中的值 \(函数模板\) |
| :--- | :--- |
| [std::swap\(std::vector\)](https://zh.cppreference.com/w/cpp/container/vector/swap2) | 特化 [std::swap](https://zh.cppreference.com/w/cpp/algorithm/swap) 算法 \(函数模板\) |
| [erase\(std::vector\)erase\_if\(std::vector\)](https://zh.cppreference.com/w/cpp/container/vector/erase2)\(C++20\) | 擦除所有满足特定判别标准的元素 \(函数模板\) |

