# 05继承06总结using在继承中的用法

using主要在派生类中用来指定使用父类的哪个函数

**`using Base::FuncName`的意思是在派生类中使用父类中全部名为`FuncName`的函数**，如果派生类重写了将会覆盖。

## 1. 用来在派生类中继承父类的构造函数

详见： [05继承05构造函数继承19.12.13.md](05-ji-cheng-05-gou-zao-han-shu-ji-cheng-19.12.13.md)

## 2. 用来指定使用哪个父类的函数

**可以用来解决多继承中父类方法的命名冲突**。不是多继承也可以指定

```cpp
class Base1 {
public:
    Base1() { cout << "Base1" << endl; };
    virtual void test() { cout << "Base1 test\n"; }
    int data = 10;
};

class Base2 {
public:
    Base2() { cout << "Base2" << endl; };
    virtual void test() { cout << "Base2 test\n"; }
    int data = 20;
};

class Son : public Base2, public Base1 {
public:
    using Base1::test;     //using 指定使用Base1的test函数
    Son() { cout << "Son" << endl; }
};

int main(){
    Son son{};
    son.test();  //output: Base1 test
    return 0;
}
```

