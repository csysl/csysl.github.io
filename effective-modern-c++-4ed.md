# Effective Modern C++ 4ed

\[TOC\]

## 10. 优先使用enum class\(struct\) 而不是enum

## 11. 优先选择删除函数\(=delete\)，而不是private为定义函数

```cpp
class Test {
public:
    Test(const Test &test) = delete;  //删除函数，类Test不允许通过复制构造函数构造对象

private:
    //Test(const Test &test);   //通过将复制构造函数声明为private(不需要提供定义)，表示不允许通过复制构造函数构造对象
};
```

## 13. 优先使用const\_iterator，而不是iterator

## 24. 区分万能引用和右值引用

[万能引用&右值引用19.11.20.md](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/基本语法/万能引用&右值引用19.11.20.md)

## 41.

