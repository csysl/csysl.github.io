# 05继承05构造函数继承

\[TOC\]

c++中，可以派生类可以显式的继承其父类的构造函数。这样就可以使用父类的构造函数创建对象。

**使用`using`继承父类构造函数**。

```cpp
class Base {
public:
    Base()= default;//{cout<<"Base default construction\n";};
    explicit Base(string_view sv){cout<<sv<<endl;};
    virtual ~Base() = default;
};

class Derived : public Base {
public:
    using Base::Base;   //使用using显式的继承父类的构造函数
    explicit Derived(int i){cout<<i<<endl;};
    ~Derived()override = default;

    const char* mString= R"(I love you!)";
};

int main() {
    Derived d1(1);    //Derived(int i)
    Derived d2("Hello world!");  //Base(string_view sv)
    Derived d3{};                //Base()
    cout<<d3.mString<<endl;
    return 0;
}
```

派生类有与**父类**构造函数**相同参数列表的构造函数**时，优先**调用派生类自己的构造函数**

```cpp
class Base {
public:
    Base()= default;//{cout<<"Base default construction\n";};
    explicit Base(string_view sv){cout<<"Base:"<<sv<<endl;};
    virtual ~Base() = default;
};

class Derived : public Base {
public:
    using Base::Base;
    Derived()= default;
    explicit Derived(int i):Base("Hello world"){cout<<i<<endl;};
    explicit Derived(string_view sv){cout<<"Derived:"<<sv<<endl;};
    ~Derived()override = default;

    const char* mString= R"(I love you!)";
};

int main() {
    Derived d1(1);   //Derived  output:Base:Hello world! 1
    Derived d2("Hello world!");  //执行了Derived的构造函数  output:Derived:Hello world! 
    Derived d3{};  //执行了Derived的无参构造函数
    cout<<d3.mString<<endl;
    return 0;
}
```

在**多继承**中，如果**多个父类有相同参数列表的构造参数**，则会发生冲突。

4.

```cpp
class Base1 {
public:
    Base1()= default;//{cout<<"Base1 default construction\n";};
    explicit Base1(string_view sv){cout<<"Base2:"<<sv<<endl;};
    virtual ~Base1() = default;
};

class Base2 {
public:
    Base2()= default;//{cout<<"Base2 default construction\n";};
    explicit Base2(string_view sv){cout<<"Base2:"<<sv<<endl;};
    virtual ~Base2() = default;
};

class Derived : public Base1, public Base2 {  //多继承
public:
    using Base1::Base1;   
    using Base2::Base2;
    Derived()= default;
    explicit Derived(int i):Base1("Hello world"){cout<<i<<endl;};
    //explicit Derived(string_view sv){cout<<"Derived:"<<sv<<endl;};
    ~Derived()override = default;

    const char* mString= R"(I love you!)";
};

int main() {
    //Derived d2("Hello world!");//编译器报错，无法识别是使用了Base1还是Base2的构造函数

    return 0;
}
```

**解决办法**：在派生类中显式的声明一个同参的构造函数，将父类的构造函数覆盖\(觉得这办法很蠢\)

```cpp
class Base1 {
public:
    Base1()= default;//{cout<<"Base default construction\n";};
    explicit Base1(string_view sv){cout<<"Base2:"<<sv<<endl;};
    virtual ~Base1() = default;
};

class Base2 {
public:
    Base2()= default;//{cout<<"Base default construction\n";};
    explicit Base2(string_view sv){cout<<"Base2:"<<sv<<endl;};
    virtual ~Base2() = default;
};

class Derived : public Base1, public Base2 {
public:
    using Base1::Base1;
    using Base2::Base2;
    Derived()= default;
    explicit Derived(int i):Base1("Hello world"){cout<<i<<endl;};
    explicit Derived(string_view sv){cout<<"Derived:"<<sv<<endl;};  //显式的声明一个接受string_view类型参数的构造函数，该构造函数覆盖了父类的同参构造函数
    ~Derived()override = default;

    const char* mString= R"(I love you!)";
};

int main() {
    Derived d2("Hello world!");  //不再报错，调用了Derived的构造函数

    return 0;
}
```

