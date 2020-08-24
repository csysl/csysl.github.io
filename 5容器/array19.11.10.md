[参考](https://zh.cppreference.com/w/cpp/container/array)

顺序数组，用来代替c风格数组，与c风格数组不同，**array本身不会退化为指针**

```c++
template< 
    class T, 
    std::size_t N 
> struct array;
```



```c++
std::array<int, 6> arr = {6, 1, 5, 8, 4, 3};
std::sort(arr.begin(), arr.end(), std::greater<>());
```

