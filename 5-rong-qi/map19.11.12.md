# map

[参考](https://zh.cppreference.com/w/cpp/container/map)

通常实现为某种形式的平衡树，通常为**红黑树**，**顺序存储**唯一键值对

插入、删除和查找时间复杂度都是对数时间

### insert\(\)&insert\_or\_assign\(\)

> **insert\(\)**:插入元素，如果键已存在，则不执行。
>
> **insert\_or\_assign\(\)**: 插入元素，如果键已存在，则更新对应的值。
>
> 两者的返回值是`pair<map<int, Data>::iterator, bool>` 前者是迭代器，bool表示是否插入成功

```cpp
map<int, string> m = {
    {0, "hello"},
    {1, "world!"},
    {2, "love"}
};

if (auto[iter, value]=m.insert({1, "hyb"});value)
    cout << "yes\n";
else
    cout << "no\n";  /*no*/


if (auto[iter, value]=m.insert({3, "hyb"});value)
    cout << "yes\n";   //yes
else
    cout << "no\n";

/************************************************/
map<int, string> m = {
    {0, "hello"},
    {1, "world!"},
    {2, "love"}
};

if (auto[iter, value]=m.insert_or_assign(1, "hyb");value)
    cout << "yes\n";
else
    cout << "no\n";     //no
cout << m[1] << endl;  //hyb

if (auto[iter, value]=m.insert_or_assign(3, "hyb");value)
    cout << "yes\n";
else
    cout << "no\n";    //yes
```

### try\_emplace\(\)   c++17

如果键不存在，则原位插入元素；如果键已存在，则什么都不做

### find\(\)  查找元素，返回位置的迭代器，不存在返回end\(\)

也可以使用\[\]查找元素，推荐使用find\(\)，**因为如果元素不存在,\[\]会插入新元素**（不知道元素存不存在的情况下不使用\[\]）

```cpp
map<int, string> m = {
    {0, "hello"},
    {1, "world!"},
    {2, "love"}
};

auto[id, value]=*m.find(2);
cout << id << " " << value << endl;   //2 love
```

### count\(\) 判断元素存不存在

存在返回1，不存在返回0

### erase\(\) 删除元素，传入迭代器、范围迭代器、键

## c++17

引入了节点概念，一个键值对为一个节点，可以**移动**

```cpp
map<int, string> m1 = {
    {0, "hello"},
    {1, "world!"},
    {2, "love"}
};

map<int, string> m2 = {
    {5, "sun"},
    {6, "long"},
    {1, "guo"}
};

for (const auto&[id, value]:m1)cout << id << " " << value << endl;
for (const auto&[id, value]:m2)cout << id << " " << value << endl;

auto nod = m1.extract(0);   //m1中键为0的节点转移到m2
m2.insert(move(nod));  //m2.insert(m1.extract(0))   

for (const auto&[id, value]:m1)cout << id << " " << value << endl;
for (const auto&[id, value]:m2)cout << id << " " << value << endl;

m2.merge(m1);   //相同键元素不会插入

for (const auto&[id, value]:m1)cout << id << " " << value << endl;
for (const auto&[id, value]:m2)cout << id << " " << value << endl;
```

## 成员函数

| [\(构造函数\)](https://zh.cppreference.com/w/cpp/container/map/map) | 构造 `map` \(公开成员函数\) |
| :--- | :--- |
| [\(析构函数\)](https://zh.cppreference.com/w/cpp/container/map/~map) | 析构 `map` \(公开成员函数\) |
| [operator=](https://zh.cppreference.com/w/cpp/container/map/operator%3D) | 赋值给容器 \(公开成员函数\) |
| [get\_allocator](https://zh.cppreference.com/w/cpp/container/map/get_allocator) | 返回相关的分配器 \(公开成员函数\) |
| 元素访问 |  |
| [at](https://zh.cppreference.com/w/cpp/container/map/at)\(C++11\) | 访问指定的元素，同时进行越界检查 \(公开成员函数\) |
| [operator\[\]](https://zh.cppreference.com/w/cpp/container/map/operator_at) | 访问或插入指定的元素 \(公开成员函数\) |
| 迭代器 |  |
| [begincbegin](https://zh.cppreference.com/w/cpp/container/map/begin) | 返回指向容器第一个元素的迭代器 \(公开成员函数\) |
| [endcend](https://zh.cppreference.com/w/cpp/container/map/end) | 返回指向容器尾端的迭代器 \(公开成员函数\) |
| [rbegincrbegin](https://zh.cppreference.com/w/cpp/container/map/rbegin) | 返回指向容器最后元素的逆向迭代器 \(公开成员函数\) |
| [rendcrend](https://zh.cppreference.com/w/cpp/container/map/rend) | 返回指向前端的逆向迭代器 \(公开成员函数\) |
| 容量 |  |
| [empty](https://zh.cppreference.com/w/cpp/container/map/empty) | 检查容器是否为空 \(公开成员函数\) |
| [size](https://zh.cppreference.com/w/cpp/container/map/size) | 返回容纳的元素数 \(公开成员函数\) |
| [max\_size](https://zh.cppreference.com/w/cpp/container/map/max_size) | 返回可容纳的最大元素数 \(公开成员函数\) |
| 修改器 |  |
| [clear](https://zh.cppreference.com/w/cpp/container/map/clear) | 清除内容 \(公开成员函数\) |
| [insert](https://zh.cppreference.com/w/cpp/container/map/insert) | 插入元素或结点 \(C++17 起\) \(公开成员函数\) |
| [insert\_or\_assign](https://zh.cppreference.com/w/cpp/container/map/insert_or_assign)\(C++17\) | 插入元素，或若关键已存在则赋值给当前元素 \(公开成员函数\) |
| [emplace](https://zh.cppreference.com/w/cpp/container/map/emplace)\(C++11\) | 原位构造元素 \(公开成员函数\) |
| [emplace\_hint](https://zh.cppreference.com/w/cpp/container/map/emplace_hint)\(C++11\) | 使用提示原位构造元素 \(公开成员函数\) |
| [try\_emplace](https://zh.cppreference.com/w/cpp/container/map/try_emplace)\(C++17\) | 若键不存在则原位插入，若键存在则不做任何事 \(公开成员函数\) |
| [erase](https://zh.cppreference.com/w/cpp/container/map/erase) | 擦除元素 \(公开成员函数\) |
| [swap](https://zh.cppreference.com/w/cpp/container/map/swap) | 交换内容 \(公开成员函数\) |
| [extract](https://zh.cppreference.com/w/cpp/container/map/extract)\(C++17\) | 从另一容器释出结点 \(公开成员函数\) |
| [merge](https://zh.cppreference.com/w/cpp/container/map/merge)\(C++17\) | 从另一容器接合结点 \(公开成员函数\) |
| 查找 |  |
| [count](https://zh.cppreference.com/w/cpp/container/map/count) | 返回匹配特定键的元素数量 \(公开成员函数\) |
| [find](https://zh.cppreference.com/w/cpp/container/map/find) | 寻找带有特定键的元素 \(公开成员函数\) |
| [contains](https://zh.cppreference.com/w/cpp/container/map/contains)\(C++20\) | 检查容器是否含有带特定关键的元素 \(公开成员函数\) |
| [equal\_range](https://zh.cppreference.com/w/cpp/container/map/equal_range) | 返回匹配特定键的元素范围 \(公开成员函数\) |
| [lower\_bound](https://zh.cppreference.com/w/cpp/container/map/lower_bound) | 返回指向首个_不小于_给定键的元素的迭代器 \(公开成员函数\) |
| [upper\_bound](https://zh.cppreference.com/w/cpp/container/map/upper_bound) | 返回指向首个_大于_给定键的元素的迭代器 \(公开成员函数\) |
| 观察器 |  |
| [key\_comp](https://zh.cppreference.com/w/cpp/container/map/key_comp) | 返回用于比较键的函数 \(公开成员函数\) |
| [value\_comp](https://zh.cppreference.com/w/cpp/container/map/value_comp) | 返回用于在value\_type类型的对象中比较键的函数。 \(公开成员函数\) |

