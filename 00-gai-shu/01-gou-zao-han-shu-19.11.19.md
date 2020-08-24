# 01构造函数

c++高级编程140-148

\[TOC\]

```cpp
class Cell {
public:
    Cell() = default;  //无参构造函数，=default指定使用编译器自动生成
    explicit Cell(const double &&inValue, int inlen);  //显式自定义的构造函数
    Cell(std::initializer_list<double> &&args);  //初始化列表构造函数
    explicit Cell(int len) : Cell(10, len){};  //委托构造函数
    Cell(const Cell &cell);   //复制构造函数

    ~Cell();  //析构函数
    double getValue();  //成员函数

private:
    double mdVlaue{};
    int *mpValue = nullptr;
    int mlen{};
    std::vector<int> mvec;
};
```

## -2. 一点说明 p148

* c++在没有显式定义复制构造函数的时候，编译器会自动创建默认的复制构造函数。
* c++没有显式定义构造函数的时候，编译器会自动创建一个无参构造函数作为默认构造函数\(即只要定义了任意构造函数，编译器就不会生成默认构造函数\)

## -1. explicit

使用explicit修饰的构造函数，必须显式的调用

* **自己定义的构造函数，使用explicit修饰**
* **无参构造函数，复制构造函数，初始化列表构造函数不使用explicit修饰**，这样可以使用=隐式的调用构造函数

## 0. =default & =delete

> **=default**：表示使用编译器提供的默认的实现方法，而不需要自己实现
>
> **=delete**：表示不允许使用其修饰的方法，不止可以修饰构造函数

```cpp
class Cell {
public:
    Cell() = default;   //无参构造函数，= default表示使用系统默认的无参构造函数而不需要自己实现
    //Cell() = delete;  //=delete表示不允许使用无参构造函数
    Cell(const Cell &cell) = default;  //使用编译器默认的复制构造函数
    //Cell(const Cell &cell) = delete;  //不允许使用复制构造函数创建对象

};
```

## 0. 初始化器的规则  ：p144

c++创建对象之前，会先创建对象的所有数据成员，在**构造函数内**对数据成员初始化，过程**实际上是创建了数据成员，再对数据成员赋值**。

**初始化器使数据成员在创建时就赋值**。以下数据类型必须在初始化器中初始化。

| 数据成员 | 说明 |
| :---: | :---: |
| const数据成员 | const必须在创建时赋值 |
| 引用数据成员 | 引用在创建时必须指向引用的位置 |
| 没有默认构造函数的对象数据成员 | c++尝试用默认构造函数初始化对象数据成员，如果对象数据成员没有默认构造函数，就不能初始化。 |
| 没有默认构造函数的积累的基类 |  |

初始化器初始化的顺序：**与数据成员在类中声明的顺序有关，跟初始化列表中的从顺序无关。**

```cpp
class Cell {
public:
    explicit Cell(const double &&inValue, int inlen):mlen(inlen),mdValue(inValue){};//这里顺序是mlen.mdvalue，实际的初始化顺序是mdValue,len跟生命的顺序相符

//private:
    double mdVlaue{};
    int mlen{};
};
```

## 1. 无参构造函数: 默认构造函数

**最好要定义默认构造函数**，比如要创建某个类对象数组的时候\(因为有其他构造函数存在的时候，不自己声明默认构造函数，则编译器不会生成默认构造函数\)

**调用无参构造函数创建对象时，不使用括号\(\)**，因为可能被编译器理解为创建了函数

```cpp
class Cell {
public:
    Cell() = default;   //无参构造函数，=default表示使用系统默认的默认构造函数而不需要自己实现
    //Cell() = delete;  //=delete表示不允许使用无参构造函数
    explicit Cell(const double &inValue);
private:
    double mValue{0.0};
};

int main{
    Cell cells[10];//调用了无参构造参数，如果声明了别的构造函数而没有显式的声明无参构造参数，编译出错
    Cell cell;   //使用无参构造函数创建类对象，不能写成 Cell cell();的形式
    //Cell cell{}; //也可以写成这种形式

    /*在堆上创建对象*/
    auto pcell = make_unique<Cell>(12.34);

    return 0;
}
```

## 2. 复制构造函数

如果没有自己编写，编译器会自己生成一个**默认的复制构造函数**，大多数情况下不需要自己编写。

但对于指针数据成员，默认复制构造函数仅仅复制其指向的地址，而不复制堆中的数据，这时候析构可能会造成内存泄漏

**不建议使用explicit修饰**

```cpp
class Cell {
public:
    Cell() = default;  //无参构造函数
    explicit Cell(const double &&inValue, int inlen);  //显式定义的构造函数
    Cell(const Cell &cell) = default;  //使用编译器默认的复制构造函数

    ~Cell();
private:
    double mdVlaue{};
    int *mpValue = nullptr;
    int mlen{};
};
/************************/
Cell::Cell(const double &&inValue, int inlen) : mdVlaue(inValue), mlen(inlen) {
    mpValue = new int[mlen]();
    for (size_t i = 0; i < mlen; ++i) {
        mpValue[i] = i;
    }
}
/*************************/
int main() {
    Cell cell1(12.34, 10);
    auto cell2 = cell1;   //隐式的调用了复制构造函数，cell1和cell2的mpValue指针指向同一处内存，当其中一个析构，另一个再访问会出错，所以这时候需要自己编写复制构造函数而不是使用编译器提供的默认复制构造函数
    //auto cell2(cell1);  //显式的调用复制构造函数
    return 0;
}

/*自己编写复制构造函数，同时把=default属性去掉*/
Cell::Cell(const Cell &cell) : mdVlaue(cell.mdVlaue), mlen(cell.mlen) {
    mpValue = new int[mlen]();
    for (size_t i = 0; i < mlen; ++i) {
        mpValue[i] = cell.mpValue[i];
    }
}
int main() {
    Cell cell1(12.34, 10);
    auto cell2 = cell1;
    cout << cell1.mpValue << " " << cell2.mpValue << endl;  //两者地址不一样(为了方便输出暂时将mpValue的private属性去掉了)
    return 0;
}
```

## 4. 初始化列表构造函数

参数为初始化列表，包含头文件

**不使用explicit修饰。**

**初始化列表也可以用来编写普通函数**。

```cpp
class Cell {
public:
    Cell(std::initializer_list<double> &&args);   //初始化列表构造函数

//private:
    std::vector<int> mvec;
};
/************************************/
Cell::Cell(std::initializer_list<double> &&args) {
    //mvec.reserve(args.size());
    for (const auto &item :args) {
        mvec.push_back(item);
    }
}
/************************************/
int main() {
    Cell cell({1, 2, 3, 4, 5, 6, 7, 8}); //使用初始化列表构造函数创建对象
    //Cell cell = {1, 2, 3, 4, 5, 6, 7, 8};  //隐式调用

    for (const auto &item:cell.mvec)cout << item << " ";cout << endl;
    return 0;
}
```

## 5. 委托构造函数

委托构造函数允许构造函数调用**同一个类**的其他构造函数，但是**只能放在初始化器中初始化**，且**必须是初始化列表中唯一的成员初始化器**。

```cpp
class Cell {
public:
    explicit Cell(double &&inValue, int inlen);   
    explicit Cell(int len) : Cell(10, len) {};   //委托构造函数

//private:
    double mdVlaue{};
    int *mpValue = nullptr;
    int mlen{};
    std::vector<int> mvec;
};
```

避免委托初始化构造函数的递归，递归造成的后果不可控

```cpp
class MyClass{
    MyClass(char c) : MyClass(1.2) { }
    MyClass(double d) : MyClass('m') { }
};
```

## 6. 自定义的构造函数

**使用explicit修饰**

