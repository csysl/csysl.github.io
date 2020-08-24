# 强制类型转换

\[TOC\]

## 1. 内置类型转换

直接转换：

```cpp
double d = 3.14;
int i = int(d);
cout << i<< endl;   //3
```

使用**static\_cast**转换：static\_cast\(\)其他用法见c++高级编程11章

```cpp
double d = 3.14;
int i = static_cast<int>(d);
cout << i << endl;
```

## 2.继承层次结构中的类型转换

使用**dynamic\_cast\(\)**进行转换：**下转型**

* 它为继承层次结构内的类型转换提供运行时检测。（**安全的**）
* 它可以**转换指针或引用**。
* 他在运行时会检测底层对象的类型信息，**如果转换没有意义**：对于**指针**，会返回一个**空指针**；对于**引用**，会抛出一个**bad\_cast异常**。
* 使用dynamic\_cast转换的类，**至少要含有一个虚方法**

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

