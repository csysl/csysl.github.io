# 0概述2

## 容器可以存放引用

```cpp
std::string s = "hello";
std::string s2 = "world!";

std::vector<std::reference_wrapper<std::string>> str{std::ref(s)};
str.emplace_back(std::ref(s2));

for (auto &ss:str)std::cout << ss.get() << " ";
std::cout << std::endl;
```

## 移动语义要正确的用在标准容器库，必须把移动构造函数和移动复制运算符标记为noexcept

## 容器适配器和bitset类不支持迭代器

## `const_iterators` and `const_reverse_iterators` provide read-only access to elements of the container.

* 链表都不支持随机访问

