# 04友元

类可以可以将以下对象声明为友元，**被声明为友元的对象，可以访问该类的`private/protected`数据成员和方法**。

* 其他类
* 其他类的成员函数
* 独立函数

友元**违反了封装原则**，所以除了特定情况不要随便使用

```cpp
class Foo {
    friend class Bar;
    // …
};
class Foo {
    friend void Bar::processFoo(const Foo& foo);
    // …
};
class Foo {
    friend void dumpFoo(const Foo& foo);
    // …
}
```

