# count\_if

**作用**：计算给定容器中**满足特定条件**的元素的个数

```cpp
array<int, 10> arr{1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
auto num = count_if(cbegin(arr), cend(arr) - 3, [](int a) { return a > 5; });
cout << num << endl;   //2
```

