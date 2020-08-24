# 05继承08基类方法重载

\[TOC\]

## 0. 派生类方法可以修改返回的指针类型

**1. 要求返回父类指针的函数，可以返回子类的指针**

**对智能指针无效，因为智能指针是类模板**

```cpp
class Father {
public:
    virtual Father *getPtr() { return new Father{}; }
    virtual void test() { cout << "Father" << endl; }

    string str = "Father"s;
};

class Son : public Father {
public:
    Father *getPtr() override { return new Son{}; }   //返回的子类Son的指针
    void test() override { cout << "Son" << endl; }

    string str = "Son"s;
};


int main() {
    Son son{};
    auto res = son.getPtr();
    cout << res->str << endl;   //output: Father
    res->test();                //output: Son

    return 0;
}
```

**2. 如果派生类方法和父类虚方法同名但是参数列表不同，则该方法是一个独立的方法**

## 1. 基类方法重载

**基类中的同名方法要么不重载，要么全部重载。**如果只有一部分被重载，那么派生类对象不能调用没有重载的函数。（因为我们考虑设计时，我们是需要全部重载的）

```cpp
class Base {
public:
    Base() = default;//{cout<<"Base default construction\n";};
    virtual ~Base() = default;
    virtual void test(){cout<<"Base test\n";}
    virtual void test(int i){cout<<"Base test int\n";}
};


class Derived : public Base {
public:
    Derived() = default;
    explicit Derived(string_view sv) { cout << "Derived:" << sv << endl; };
    ~Derived() override = default;

    //using Base::test;
    void test()override {cout<<"Derived test\n";}  //test(int i)没有被重载
};

int main() {
    Derived derived{};
    Base &ref_d = derived;
    ref_d.test(1);    //可以运行 output : Base test
    //derived.test(1); //编译器报错，因为派生类中test(int i)没有被重载，存在他的同名函数被重载

    return 0;
}
```

**使用using可以避免全部重载**（实际上是使用using指定使用父类的函数而避免重载）

```cpp
class Base {
public:
    Base() = default;//{cout<<"Base default construction\n";};
    virtual ~Base() = default;
    virtual void test(){cout<<"Base test\n";}
    virtual void test(int i){cout<<"Base test int\n";}
};


class Derived : public Base {
public:
    Derived() = default;
    explicit Derived(string_view sv) { cout << "Derived:" << sv << endl; };
    ~Derived() override = default;

    using Base::test;   //使用using来声明使用Base类的test函数
    void test()override {cout<<"Derived test\n";}  

};

int main() {
    Derived derived{};
    Base &ref_d = derived;
    ref_d.test(1);   //output : Base test
    derived.test(1);   //不再报错，输出 Base test

    return 0;
}
```

## 2. **派生类**不能调用父类private的函数，但是**可以重写父类private的函数**。

Java和C\#不可以

## 3. 基类方法的默认参数

尽可能的**不要在虚函数及其重写函数中使用默认参数**。

如果要使用，**建议使用相同的值**（可以用同一个符号表示）

在下面的例子中，ref\_d绑定的是Derived对象，运行的是Derived的test\(int i\)函数，参数却使用的是Base::test\(int i\)绑定的参数，原因是：**c++根据表达式编译时（而不是运行时的类型）的类型绑定的默认参数。**虽然ref\_d指向的是Derived对象，但是绑定的参数却是Base类型的参数。

```cpp
class Base {
public:
    Base() = default;//{cout<<"Base default construction\n";};
    virtual ~Base() = default;

    virtual void test(int i=10){cout<<"Base test int "<<i<<endl;}
};


class Derived : public Base {
public:
    Derived() = default;
    explicit Derived(string_view sv) { cout << "Derived:" << sv << endl; };
    ~Derived() override = default;

    void test(int i=5)override {cout<<"Derived test int "<<i<<endl;}
};

int main() {
    Derived derived{};
    Base &ref_d = derived;
    ref_d.test();  //output：Derived test int 10   ref_d绑定的是Derived对象，运行的是Derived的test(int i)函数，参数却使用的是Base::test(int i)绑定的参数
    derived.test();  //output：Derived test int 5

    return 0;
}
```

## 4. 修改方法的访问级别

修改访问级别没什么意义，唯一的意义是对protected方法提供比较宽松的访问级别。

**基类protected派生类public**

* 可以通过这种方式用派生类使用基类的protected函数

```cpp
class Base {
public:
    Base() = default;//{cout<<"Base default construction\n";};
    virtual ~Base() = default;

protected:
    virtual void test(int i) { cout << "Base test int " << i << endl; }
};


class Derived : public Base {
public:
    Derived() = default;
    explicit Derived(string_view sv) { cout << "Derived:" << sv << endl; };
    ~Derived() override = default;

    using Base::test;
    //void test(int i) override { cout << "Derived test int " << i << endl; }
};

int main() {
    Derived derived{};
    derived.test(10);   //output : Base test int 10
    Base &ref_d = derived;
    //ref_d.test(10);  //报错，Base的test函数是protected，不能在外部被调用

    return 0;
}
```

**基类public派生类protected**

* 基类的引用和指针能够访问派生类的protected函数

```cpp
class Base {
public:
    Base() = default;//{cout<<"Base default construction\n";};
    virtual ~Base() = default;

    virtual void test(int i){cout<<"Base test int "<<i<<endl;}
};

class Derived : public Base {
public:
    Derived() = default;
    explicit Derived(string_view sv) { cout << "Derived:" << sv << endl; };
    ~Derived() override = default;

protected:
    void test(int i)override {cout<<"Derived test int "<<i<<endl;}
};

int main() {
    Derived derived{};
    Base &ref_d = derived;
    ref_d.test(10);  //调用的还是Derived的test函数
    Base *ptr_d = &derived;
    ptr_d->test(10);  //调用的还是Derived的test函数
    //derived.test(10);  //error protected函数不能直接调用

    return 0;
}
```

## 5. 复制构造函数和复制赋值函数

如果派生类显式的指定了复制构造函数（复制赋值函数），那么必须显式的链接到父类的复制构造函数，否则会调用父类的默认构造函数来初始化对象的父类部分

```cpp
class Base {
public:
    Base() {cout<<"Base default construction\n";};
    virtual ~Base() = default;
    Base(const Base &base) {cout << "Base copy construction" << endl;};
    Base&operator=(const Base&base){
        cout << "Base copy operator" << endl;
        return *this;
    }
};


class Derived : public Base {
public:
    Derived() = default;
    ~Derived() override = default;
    Derived(const Derived &derived) : Base(derived) {   //显式的调用Base的复制构造函数
        cout << "Derived copy construction" << endl;
    }   

    Derived &operator=(const Derived &derived) {
        Base::operator=(derived);  //显式的调用Base的复制赋值函数
        cout << "Derived copy operator" << endl;
        return *this;
    }
};

int main() {
    Derived derived1{};   //输出：Base default construction
    Derived derived2{derived1};
    //输出：Base copy construction 和 Derived copy construction  
    derived2 = derived1;
    //输出：Base copy operator 和 Derived copy operator
    return 0;
}
```

