# list

双链表，不支持随机访问，即迭代器不能进行加减操作和指针运算

适应于在数据结构上执行很多插入删除操作

vector通常更快

### 串联或插入整个list: 被插入的list会被破坏掉\(因为是链表\)

```cpp
list<int> l1{1, 5, 6, 7, 8};
list<int> l2{9, 8, 4, 2, 3};

l1.splice(++cbegin(l1), l2, cbegin(l2), --cend(l2)); //插入
for (const auto &item:l1)cout << item << " ";cout << endl; //1 9 8 4 2 5 6 7 8

l1.sort();   //排序
//l1.sort(greater<>());   //从大到小排序
for (const auto &item:l1)cout << item << " ";cout << endl; //1 2 4 5 6 7 8 8 9 

l1.unique();  //删除连续相同的元素
for (const auto &item:l1)cout << item << " ";cout << endl;//1 2 4 5 6 7 8 9

l1.reverse();  //反转
for (const auto &item:l1)cout << item << " ";cout << endl;//9 8 7 6 5 4 2 1

list<int> ll{9, 8, 4, 2, 3};
l1.merge(ll);  //合并list
```

## 成员函数

| [\(构造函数\)](https://zh.cppreference.com/w/cpp/container/list/list) | 构造 `list` \(公开成员函数\) |
| :--- | :--- |
| [\(析构函数\)](https://zh.cppreference.com/w/cpp/container/list/~list) | 析构 `list` \(公开成员函数\) |
| [operator=](https://zh.cppreference.com/w/cpp/container/list/operator%3D) | 赋值给容器 \(公开成员函数\) |
| [assign](https://zh.cppreference.com/w/cpp/container/list/assign) | 将值赋给容器 \(公开成员函数\) |
| [get\_allocator](https://zh.cppreference.com/w/cpp/container/list/get_allocator) | 返回相关的分配器 \(公开成员函数\) |
| 元素访问 |  |
| [front](https://zh.cppreference.com/w/cpp/container/list/front) | 访问第一个元素 \(公开成员函数\) |
| [back](https://zh.cppreference.com/w/cpp/container/list/back) | 访问最后一个元素 \(公开成员函数\) |
| 迭代器 |  |
| [begincbegin](https://zh.cppreference.com/w/cpp/container/list/begin) | 返回指向容器第一个元素的迭代器 \(公开成员函数\) |
| [endcend](https://zh.cppreference.com/w/cpp/container/list/end) | 返回指向容器尾端的迭代器 \(公开成员函数\) |
| [rbegincrbegin](https://zh.cppreference.com/w/cpp/container/list/rbegin) | 返回指向容器最后元素的逆向迭代器 \(公开成员函数\) |
| [rendcrend](https://zh.cppreference.com/w/cpp/container/list/rend) | 返回指向前端的逆向迭代器 \(公开成员函数\) |
| 容量 |  |
| [empty](https://zh.cppreference.com/w/cpp/container/list/empty) | 检查容器是否为空 \(公开成员函数\) |
| [size](https://zh.cppreference.com/w/cpp/container/list/size) | 返回容纳的元素数 \(公开成员函数\) |
| [max\_size](https://zh.cppreference.com/w/cpp/container/list/max_size) | 返回可容纳的最大元素数 \(公开成员函数\) |
| 修改器 |  |
| [clear](https://zh.cppreference.com/w/cpp/container/list/clear) | 清除内容 \(公开成员函数\) |
| [insert](https://zh.cppreference.com/w/cpp/container/list/insert) | 插入元素 \(公开成员函数\) |
| [emplace](https://zh.cppreference.com/w/cpp/container/list/emplace)\(C++11\) | 原位构造元素 \(公开成员函数\) |
| [erase](https://zh.cppreference.com/w/cpp/container/list/erase) | 擦除元素 \(公开成员函数\) |
| [push\_back](https://zh.cppreference.com/w/cpp/container/list/push_back) | 将元素添加到容器末尾 \(公开成员函数\) |
| [emplace\_back](https://zh.cppreference.com/w/cpp/container/list/emplace_back)\(C++11\) | 在容器末尾就地构造元素 \(公开成员函数\) |
| [pop\_back](https://zh.cppreference.com/w/cpp/container/list/pop_back) | 移除末元素 \(公开成员函数\) |
| [push\_front](https://zh.cppreference.com/w/cpp/container/list/push_front) | 插入元素到容器起始 \(公开成员函数\) |
| [emplace\_front](https://zh.cppreference.com/w/cpp/container/list/emplace_front)\(C++11\) | 在容器头部就地构造元素 \(公开成员函数\) |
| [pop\_front](https://zh.cppreference.com/w/cpp/container/list/pop_front) | 移除首元素 \(公开成员函数\) |
| [resize](https://zh.cppreference.com/w/cpp/container/list/resize) | 改变容器中可存储元素的个数 \(公开成员函数\) |
| [swap](https://zh.cppreference.com/w/cpp/container/list/swap) | 交换内容 \(公开成员函数\) |
| 操作 |  |
| [merge](https://zh.cppreference.com/w/cpp/container/list/merge) | 合并二个已排序列表 \(公开成员函数\) |
| [splice](https://zh.cppreference.com/w/cpp/container/list/splice) | 从另一个`list`中移动元素 \(公开成员函数\) |
| [removeremove\_if](https://zh.cppreference.com/w/cpp/container/list/remove) | 移除满足特定标准的元素 \(公开成员函数\) |
| [reverse](https://zh.cppreference.com/w/cpp/container/list/reverse) | 将该链表的所有元素的顺序反转 \(公开成员函数\) |
| [unique](https://zh.cppreference.com/w/cpp/container/list/unique) | 删除连续的重复元素 \(公开成员函数\) |
| [sort](https://zh.cppreference.com/w/cpp/container/list/sort) | 对元素进行排序 |

