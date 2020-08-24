# 迭代器

* 迭代器建议使用非成员函数而不是成员函数

```cpp
vector<int> vec{45, 4, 5, 12};

auto s=cbegin(vec);   //而不是auto s = vec.cbegin();
auto t=cend(vec);
```

* 带有c的是const迭代器，**只读** 

```cpp
std::vector<int> vec{45, 4, 5, 12};
*std::begin(vec) /= 2;   //right
*std::cbegin(vec) /= 2;  //error 编译器报错
```

* 通常情况下，迭代器和指针一样，**非常不安全**
* 使用迭代器的好处 p369 待补充

