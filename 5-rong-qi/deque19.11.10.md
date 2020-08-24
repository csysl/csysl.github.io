# deque

[参考](https://zh.cppreference.com/w/cpp/container/deque)

```cpp
#include<deque>
```

双端队列：

* 顺序结构，**内存存储位置不是连续的**\(vector是连续的\)
* 能够在队头和队尾删除和添加元素
* **可以使用下标访问**

复杂度：

* 随机访问——常数 _O\(1\)_
* 在结尾或起始插入或移除元素——常数 _O\(1\)_
* 插入或移除元素——线性 _O\(n\)_

```cpp
std::deque<int> dq = {1, 6, 8, 2, 3};
std::cout << dq[0] << std::endl;   //1
dq.push_front(4);
std::cout << dq[0] << std::endl;   //4
```

相关函数

| 元素访问 |  |
| :--- | :--- |
| [at](https://zh.cppreference.com/w/cpp/container/deque/at) | 访问指定的元素，同时进行越界检查  \(公开成员函数\) |
| [operator\[\]](https://zh.cppreference.com/w/cpp/container/deque/operator_at) | 访问指定的元素  \(公开成员函数\) |
| [front](https://zh.cppreference.com/w/cpp/container/deque/front) | 访问第一个元素  \(公开成员函数\) |
| [back](https://zh.cppreference.com/w/cpp/container/deque/back) | 访问最后一个元素  \(公开成员函数\) |
| 迭代器 |  |
| [begin cbegin](https://zh.cppreference.com/w/cpp/container/deque/begin) | 返回指向容器第一个元素的迭代器  \(公开成员函数\) |
| [end cend](https://zh.cppreference.com/w/cpp/container/deque/end) | 返回指向容器尾端的迭代器  \(公开成员函数\) |
| [rbegin crbegin](https://zh.cppreference.com/w/cpp/container/deque/rbegin) | 返回指向容器最后元素的逆向迭代器  \(公开成员函数\) |
| [rend crend](https://zh.cppreference.com/w/cpp/container/deque/rend) | 返回指向前端的逆向迭代器  \(公开成员函数\) |
| 容量 |  |
| [empty](https://zh.cppreference.com/w/cpp/container/deque/empty) | 检查容器是否为空  \(公开成员函数\) |
| [size](https://zh.cppreference.com/w/cpp/container/deque/size) | 返回容纳的元素数  \(公开成员函数\) |
| [max\_size](https://zh.cppreference.com/w/cpp/container/deque/max_size) | 返回可容纳的最大元素数  \(公开成员函数\) |
| [shrink\_to\_fit](https://zh.cppreference.com/w/cpp/container/deque/shrink_to_fit)\(C++11\) | 通过释放未使用的内存减少内存的使用  \(公开成员函数\) |
| 修改器 |  |
| [clear](https://zh.cppreference.com/w/cpp/container/deque/clear) | 清除内容  \(公开成员函数\) |
| [insert](https://zh.cppreference.com/w/cpp/container/deque/insert) | 插入元素  \(公开成员函数\) |
| [emplace](https://zh.cppreference.com/w/cpp/container/deque/emplace)\(C++11\) | 原位构造元素  \(公开成员函数\) |
| [erase](https://zh.cppreference.com/w/cpp/container/deque/erase) | 擦除元素  \(公开成员函数\) |
| [push\_back](https://zh.cppreference.com/w/cpp/container/deque/push_back) | 将元素添加到容器末尾  \(公开成员函数\) |
| [emplace\_back](https://zh.cppreference.com/w/cpp/container/deque/emplace_back)\(C++11\) | 在容器末尾就地构造元素  \(公开成员函数\) |
| [pop\_back](https://zh.cppreference.com/w/cpp/container/deque/pop_back) | 移除末元素  \(公开成员函数\) |
| [push\_front](https://zh.cppreference.com/w/cpp/container/deque/push_front) | 插入元素到容器起始  \(公开成员函数\) |
| [emplace\_front](https://zh.cppreference.com/w/cpp/container/deque/emplace_front)\(C++11\) | 在容器头部就地构造元素  \(公开成员函数\) |
| [pop\_front](https://zh.cppreference.com/w/cpp/container/deque/pop_front) | 移除首元素  \(公开成员函数\) |
| [resize](https://zh.cppreference.com/w/cpp/container/deque/resize) | 改变容器中可存储元素的个数  \(公开成员函数\) |
| [swap](https://zh.cppreference.com/w/cpp/container/deque/swap) | 交换内容  \(公开成员函数\) |

