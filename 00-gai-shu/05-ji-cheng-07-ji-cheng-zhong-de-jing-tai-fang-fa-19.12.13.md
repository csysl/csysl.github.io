# 05继承07继承中的静态方法

静态方法不能被重写（不能被virtual修饰）

如果基类和派生类有同名的静态方法，则两者是两个独立的方法。

**静态方法属于特定的类，而不属于该类的对象**。调用静态方法时，c++不关心实际对象的类型，只关心编译时的类型。例如：

```cpp
class Base {
public:
    Base() = default;//{cout<<"Base default construction\n";};
    virtual ~Base() = default;

    static void test() { cout << "Base test\n"; };
    virtual void test2() { test();//实际上是Base::test();};
};

class Derived : public Base {
public:
    Derived() = default;
    ~Derived() override = default;

    static void test() { cout << "Derived test\n"; };
    void test2() override { test();//实际上是Derived::test(); };
};

int main() {
    Derived derived{};
    Base &ref_d = derived;
    ref_d.test();    //output: Base test    虽然ref_d指向derived对象，但是ref_d类型是Base&,所以调用的是Base的test函数
    ref_d.test2();   //output:Derived test  test2()不是静态的，所以调用的是Derived的test2()函数。

    return 0;
}
```

