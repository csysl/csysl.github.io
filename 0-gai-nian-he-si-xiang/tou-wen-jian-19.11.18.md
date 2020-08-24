# 头文件

c++高级编程 p246

* 避免循环引用或者多次包含同一个头文件
* 尽可能在头文件中使用**前置声明**，而不是包含其他头文件。**这可以减少编译和重编译的时间，因为破坏了头文件对其他头文件的依赖。**  **实现文件必须包含前置类的头文件**，否则不能编译。

```cpp
#ifndef XXXXX
#define XXXXX

//do something

#endif  //XXXXX
/***********或者**********/
#promga once
```

c++17头文件检查

```cpp
#if __has_include(<optional>)
    #include <optional>
#elif __has_include(<experimental/optional>)
    #include <experimental/optional>
#endif
```

