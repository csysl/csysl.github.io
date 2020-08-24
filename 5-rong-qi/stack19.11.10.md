# stack

[参考](https://zh.cppreference.com/w/cpp/container/stack)

**标准容器** [**std::vector**](https://zh.cppreference.com/w/cpp/container/vector) **、** [**std::deque**](https://zh.cppreference.com/w/cpp/container/deque) **和** [**std::list**](https://zh.cppreference.com/w/cpp/container/list) **满足这些要求。若不为特定的 `stack` 类特化指定容器类，则使用标准容器** [**std::deque**](https://zh.cppreference.com/w/cpp/container/deque) **。**

**先进后出**

实现

```cpp
template<
    class T,
    class Container = std::deque<T>
> class stack;
```

相关函数

| 元素访问 |  |
| :--- | :--- |
| [top](https://zh.cppreference.com/w/cpp/container/stack/top) | 访问栈顶元素  \(公开成员函数\) |
| 容量 |  |
| [empty](https://zh.cppreference.com/w/cpp/container/stack/empty) | 检查底层的容器是否为空  \(公开成员函数\) |
| [size](https://zh.cppreference.com/w/cpp/container/stack/size) | 返回容纳的元素数  \(公开成员函数\) |
| 修改器 |  |
| [push](https://zh.cppreference.com/w/cpp/container/stack/push) | 向栈顶插入元素  \(公开成员函数\) |
| [emplace](https://zh.cppreference.com/w/cpp/container/stack/emplace)\(C++11\) | 于顶原位构造元素  \(公开成员函数\) |
| [pop](https://zh.cppreference.com/w/cpp/container/stack/pop) | 删除栈顶元素  \(公开成员函数\) |
| [swap](https://zh.cppreference.com/w/cpp/container/stack/swap) | 交换内容  \(公开成员函数\) |

