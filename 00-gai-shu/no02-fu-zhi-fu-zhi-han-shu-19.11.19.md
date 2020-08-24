# 『no』02复制赋值函数

c++高级编程 p150-151

\[TOC\]

### 声明&实现

```cpp
class Cell {
public:
    Cell() = default;  //无参构造函数，=default指定使用编译器自动生成
    explicit Cell(double &&inValue, int inlen);  //显式自定义的构造函数
    Cell &operator=(const Cell &cell)=default;  //默认复制赋值函数
    //Cell &operator=(const Cell &cell);  


    ~Cell();  //析构函数

//private:
    double mdVlaue{};
    char *mpValue = nullptr;
    int mlen{};
    std::vector<int> mvec;
};
/*********************************************/
//自定义构造函数
Cell::Cell(double &&inValue, int inlen) : mdVlaue(inValue), mlen(inlen) {
    mpValue = new char[mlen]();
    for (size_t i = 0; i < mlen; ++i) {
        mpValue[i] = i;
    }
}
//自定义复制赋值函数
Cell &Cell::operator=(const Cell &cell) {
    if (this == &cell) {//自赋值检查
        /*do something*/
        return *this;
    }

    mdVlaue = cell.mdVlaue;
    mlen = cell.mlen;
    mpValue = new char[mlen]();
    for (size_t i = 0; i < mlen; ++i) {
        mpValue[i] = cell.mpValue[i];
    }
    return *this;
}

int main() {
    Cell cell1, cell2(12.34, 10);
    cell1 = cell2;

    cout << cell1.mpValue << " " << cell2.mpValue << endl;
    //使用默认的复制赋值函数cell1和cell2的地址一致，所以在涉及到指针的时候避免使用默认复制赋值函数
    //使用自定义复制赋值函数，输出结果不一致
    return 0;
}
```

## 复制赋值函数

注意点：

* `return *this`允许连续赋值
* 传入的参数设置为`const &`
* `if(this==&)`判断**自赋值**：**当对象涉及到动态内存的时候，必须对自赋值设置检查**。如下例所示，如果没有检查，那么mpvlaue会新开辟一段内存，而原内存会被抛弃，造成内存泄漏
* 对象指针释放原有内存，赋新值

```cpp
//自定义复制赋值函数
Cell &Cell::operator=(const Cell &cell) {
    if (this == &cell) {//自赋值检查,是本身的话两者地址一样
        /*do something*/
        return *this;
    }

    mdVlaue = cell.mdVlaue;
    mlen = cell.mlen;

    //释放原内存，申请新内存
    delete[]mpValue;
    mpValue = new char[mlen]();
    for (size_t i = 0; i < mlen; ++i) {
        mpValue[i] = cell.mpValue[i];
    }
    return *this;   //允许连续赋值
}
```

指针释放旧内存申请新内存时**如果申请失败，会抛出异常\(异常安全\)，指针会变成空指针**，在后续可能会造成不良后果。**解决思路**是：如果分配新内存失败，就不复制。**解决方法**可以采取：先创建一个临时实例，然后将临时实例和原来的实例交换（**可以专门编写一个标记为noexcept的友元swap函数，用来交换**）。保证异常安全性。 c++高级编程160-161

```cpp
Cell &Cell::operator=(const Cell &cell) {
    if (this == &cell) {//自赋值检查,是本身的话两者地址一样
        /*do something*/
        return *this;
    }

    mdVlaue = cell.mdVlaue;
    mlen = cell.mlen;

    //释放原内存，申请新内存
    Cell tmp(cell);
    char *ptmp=tmp.mpValue;
    tmp.mpValue= mpValue;
    mpValue = ptmp;
    ptmp=nullptr;

    for (size_t i = 0; i < mlen; ++i) {
        mpValue[i] = cell.mpValue[i];
    }
    return *this;   //允许连续赋值
}
/*******************************************/
class Spreadsheet {
public:
    Spreadsheet &operator=(const Spreadsheet &rhs);

    friend void swap(Spreadsheet &first, Spreadsheet &second) noexcept;
    // Code omitted for brevity
};

void swap(Spreadsheet &first, Spreadsheet &second) noexcept {
    using std::swap;
    swap(first.mWidth, second.mWidth);
    swap(first.mHeight, second.mHeight);   //标准库中的swap不抛出异常
    swap(first.mCells, second.mCells);
}

Spreadsheet &Spreadsheet::operator=(const Spreadsheet &rhs) {
    // Check for self-assignment
    if (this == &rhs) {
        return *this;
    }
    Spreadsheet temp(rhs); // Do all the work in a temporary instance
    swap(*this, temp); // Commit the work with only non-throwing operations
    return *this;
}
```

