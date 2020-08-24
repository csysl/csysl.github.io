**作用**：将迭代器范围内的数值替换

```c++
array<int, 10> arr{1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
generate(begin(arr), end(arr), [x = 0]()mutable {
    ++x;
    return x * x;
});
for (auto x:arr)cout << x << " ";cout << endl;  //1 4 9 16 25 36 49 64 81 100 
```

