# 05继承02利用父类

\[TOC\]

c++高级编程10.3

## 1.构造函数执行顺序

创建某个子类对象时，构造函数执行顺序：**该规则递归使用**（父类的父类）

1. 执行基类的**默认构造函数**。
2. 类的**非静态数据成员按照声明顺序创建**。
3. 执行**该类的构造函数**。

```cpp
class Something{
public:
    Something() { cout << "2"; }
};

class Base{
public:
    Base() { cout << "1"; }
};

class Derived : public Base{
public:
    Derived() { cout << "3"; }
private:
    Something mDataMember;
};

int main() {
    Derived myDerived;   //输出123，先创建基类，在创建非静态成员，最后创建当前类
    return 0;
}
```

如果**没有默认构造函数**或者**有默认构造函数但是想调用其他构造函数**，那么需要在子类构造函数的**初始化列表**中构造 基类。构造顺序依然如前面。

```cpp
class Something {
public:
    Something() { cout << "2"; }
};

class Base {
public:
    explicit Base(int i) { cout << "1"; }
};

class Derived : public Base {
public:
    Derived() : Base(0) { cout << "3"; };   //初始化列表中构造基类
    explicit Derived(int i) : Base(i) { cout << "3"; }  //初始化列表中构造基类
private:
    Something mDataMember;
};


int main() {
    Derived myDerived;
    return 0;
}
```

所以如果将数据成员作为参数传递给父类构造函数，则传递时该数据成员还没有被初始化。因为数据成员在基类之后初始化

```cpp
class Something {
public:
    Something() { cout << "2"; }
};

class Base {
public:
    explicit Base(int i) { cout << "1"; }
};

class Derived : public Base {
public:
    Derived():Base(q),q(9){ cout<<"3";};//初始化列表中构造基类,q在被基类构造函数使用之前没有被初始化
private:
    Something mDataMember;
    int q;
};

int main() {
    Derived myDerived;
    return 0;
}
```

## 2.析构函数执行顺序

析构函数执行顺序**与构造函数执行顺序相反**：该规则也可以递归使用\(父类的父类\)

1. 调用子类的析构函数
2. 销毁子类的数据成员，与创建顺序相反
3. 如果有父类，调用父类的析构函数   （即所有类都是先调用析构函数，再销毁数据成员）

```cpp
class Something {
public:
    Something() { cout << "2"; }
    virtual ~Something() { cout << "2"; }
};

class Base {
public:
    Base() { cout << "1"; }
    virtual ~Base() { cout << "1"; }
};

class Derived : public Base {
public:
    Derived() { cout << "3"; }
    ~Derived() override { cout << "3"; }   //父类析构函数使用了virtual，所以这里也是virtual，不需要显式修饰
private:
    Something mDataMember;
};


int main() {
    Derived myDerived;   //123321
    return 0;
}
```

## 3.子类中使用父类的方法

需要用到作用域解析符`::`

```cpp
/*父类*/
class WeatherPrediction {
public:
    virtual std::string getTemperature() const;
    // Omitted for brevity
};
/*子类*/
class MyWeatherPrediction : public WeatherPrediction {
public:
    virtual std::string getTemperature() const override;
    // Omitted for brevity
};

string MyWeatherPrediction::getTemperature() const{
    // Note: \u00B0 is the ISO/IEC 10646 representation of the degree symbol.
    //return getTemperature() + "\u00B0F"; // BUG，调用的是子类自己的函数，这里会无尽递归
    return WeatherPrediction::getTemperature() + "\u00B0F";  //正确
    //return __super::getTemperature() + "\u00B0F";  //正确,mvc++支持__super关键字
}
/***********************************************************************************/
class Book {
public:
    virtual ~Book() = default;
    [[nodiscard]] virtual string getDescription() const { return "Book"; }
    [[nodiscard]] virtual int getHeight() const { return 120; }
};

class Paperback : public Book {
public:
    [[nodiscard]] string getDescription() const override {
        return "Paperback " + Book::getDescription();
    }
};

class Romance : public Paperback {
public:
    [[nodiscard]] string getDescription() const override {
        return "Romance " + Paperback::getDescription();
    }
    [[nodiscard]] int getHeight() const override {
        return Paperback::getHeight() / 2;
    }
};

class Technical : public Book {
public:
    [[nodiscard]] string getDescription() const override {
        return "Technical " + Book::getDescription();
    }
};

int main() {
    Romance novel;
    Book book;
    cout << novel.getDescription() << endl; // Outputs "Romance Paperback Book"
    cout << book.getDescription() << endl; // Outputs "Book"
    cout << novel.getHeight() << endl; // Outputs "60"
    cout << book.getHeight() << endl; // Outputs "120"
    return 0;
}
```

## 4.向上/向下转型

**上转型**：通过基类使用派生类，有**指针**和**引用**两种方式（不使用直接类型转换，会发生截断）

```cpp
class Father {};
class Son : public Father {};

Son son{};

Father *ptr_f=&son;
Father &ref_f=son;
```

**下转型**：通过派生类使用基类。下转型是不好的设计（**因为无法保证该对象确实属于该派生类**，这句没太懂），尽量避免。如果一定要使用下转型，**一定要**使用`dynamic_cast()`：

* 使用`dynamic_cast()`时类**至少有一个虚方法**，不然会编译报错（详见c++高级编程11章类型转换部分）
* 如果类型转换没有意义：
  * 转换**引用**时，会抛出`std::bad_cast`异常
  * 转换**指针**时，会抛出空指针`nullptr`

```cpp
class Base{
public:
    virtual ~Base() = default;
};
class Derived : public Base{
public:
    ~Derived() override = default;
};
/*指针正确用法*/
Base* b;
Derived* d = new Derived();
b = d;
d = dynamic_cast<Derived*>(b);   //下转型
/*引用正确用法*/
Base base;
Base& br = base;
try {
    Derived& dr = dynamic_cast<Derived&>(br);  //下转型
} catch (const bad_cast&) {
    cout << "Bad cast!" << endl;
}
```

