# 01创建线程

c++高级编程 第23.2 p550-555 \[TOC\]

## 0. 一些说明

> 参数按**引用传递**到线程函数，需要使用**ref/cref**函数

## 1. 使用函数指针创建线程

```cpp
void change(int &num) {
    random_device r;
    mt.lock();
    num = r();
    mt.unlock();
    cout << num << endl;
}

/********************************************/
int a{};
array<thread, 10> th;

for (size_t i = 0; i < 10; ++i)
    th[i] = thread{change, ref(a)};    //创建线程，引用通过cref/ref传递
for (size_t i = 0; i < 10; ++i)
    th[i].join();
```

## 2. 使用函数对象创建线程

函数对象？？？

```cpp

```

## 3. 使用lambda表达式创建线程

```cpp

```

## 4. 使用类成员函数创建线程

```cpp
class Test {
public:
    void setValue(const int inValue) {
        mValue = inValue;
    }

    void getValue() const {
        cout << mValue << endl;
    }

private:
    int mValue{};
};
/****************************************/
int a{};
array<thread, 10> th;
Test t{};

/*类成员函数创建线程*/
for (size_t i = 0; i < 10; ++i)
    th[i] = thread{&Test::setValue, &t, a};   //使用类成员函数创建线程时，类成员函数和类实例通过&传递，参数依然要通过ref/cref传递
for (size_t i = 0; i < 10; ++i)
    th[i].join();
```

