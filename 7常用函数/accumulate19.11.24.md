**头文件**：<numeric>

#### accumulate()

#### 1. 求和：

```c++
array<int, 4> arr = {1, 2, 3, 4};

auto res = accumulate(cbegin(arr), cend(arr), 0);  //0是求和的起始值
cout << res << endl;   //求和
```



#### 2. 接收谓词回调函数：

```c++
auto res = accumulate(cbegin(arr), cend(arr), 1, [](int a, int b) { return a * b; });   //连乘，1是连乘的起始值，是lambda表达式的参数之一，意思是1跟每一个值相乘
cout << res << endl;
```

