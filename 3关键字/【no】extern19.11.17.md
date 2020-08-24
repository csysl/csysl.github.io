[参考](https://docs.microsoft.com/en-us/cpp/cpp/extern-cpp?view=vs-2019)

[TOC]

### extern：将修饰的对象指定为外部链接

  [外部链接和内部链接19.11.17.md](..\概念和思想\外部链接和内部链接19.11.17.md) 



#### **extern修饰非const全局变量**

```c++
//fileA.cpp
int i = 42; // declaration and definition
void print_i(){
    cout<<i<<endl;
}

//fileB.cpp
extern int i;  // declaration only. same as i in FileA

//fileC.cpp
extern int i;  // declaration only. same as i in FileA

//fileD.cpp
int i = 43; // LNK2005! 'i' already has a definition.
extern int i = 43; // same error (extern is ignored on definitions)

//fileE.cpp
extern int i;
void print_i();  //函数本身具有外部链接

i=14;
print_i();   //14
```



#### extern修饰const对象

```c++
//fileA.cpp
extern const int i = 42; // extern const definition  const默认为内部链接，用extern声明为外部链接

//fileB.cpp
extern const int i;  // declaration only. same as i in FileA
```



#### 修饰constexpr对象

```c++

```



#### extern "C"     

<font color=red>待补充</font> 

```c++

```

