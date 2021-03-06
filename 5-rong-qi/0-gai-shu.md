# 0概述

## 0、[参考](https://zh.cppreference.com/w/cpp/container)  c++高级编程16章 p347-351

| 顺序容器 | vector、deque、list、forward\_list、array |
| :--- | :--- |
| **关联容器** | **map, multimap, set, multiset** |
| **无序关联容器/哈希表** | **unordered\_map,unordered\_multimap, unordered\_set, unordered\_multiset** |
| **容器适配器** | **queue, priority\_queue, stack** |

## 1、[forward\_list](https://zh.cppreference.com/w/cpp/container/forward_list) 单链表

**不支持**快速随机访问。它实现为单链表

```cpp
std::forward_list<int> fl={4,5,2,1,9};
fl.push_front(10);   //添加倒开头，无法向结尾添加元素
```

## 2、 [list](https://zh.cppreference.com/w/cpp/container/list) 双向链表

**不支持**快速随机访问。它通常实现为双向链表。

```cpp
// 创建含整数的 list
std::list<int> l = { 7, 5, 16, 8 };
// 添加整数到 list 开头
l.push_front(25);
// 添加整数到 list 结尾
l.push_back(13);
```

## 3、[array](https://zh.cppreference.com/w/cpp/container/array) 静态数组

顺序数组，用来代替c风格数组，与c风格数组不同，**array本身不会退化为指针**

```cpp
template< 
    class T, 
    std::size_t N 
> struct array;

std::array<int, 6> arr = {6, 1, 5, 8, 4, 3};
```

## 4、[vector](https://zh.cppreference.com/w/cpp/container/vector) 动态数组

**当原先分配的内存不够时，会重新分配内存，然后把全部内容复制到新内存当中，再销毁原内存**

```cpp
template<
    class T,
    class Allocator = std::allocator<T>
> class vector;
```

## 5、[deque](https://zh.cppreference.com/w/cpp/container/deque) 队列&优先队列

* 队列：**queue**
* 优先队列：**priority\_queue**

## 6、[queue](https://zh.cppreference.com/w/cpp/container/queue) 双端队列\(数组\)

顺序结构，**内存存储位置不是连续的**，**可以使用下标访问**。

```cpp
std::deque<int> dq = {1, 6, 8, 2, 3};
std::cout << dq[0] << std::endl;   //1
dq.push_front(4);
std::cout << dq[0] << std::endl;   //4
```

## 7、[stack](https://zh.cppreference.com/w/cpp/container/stack) 栈

**使用deque实现，可以直接使用deque代替**

```cpp
template<
    class T,
    class Container = std::deque<T>
> class stack;
```

## 8、

## 9、

