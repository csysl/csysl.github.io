# greater&less

内置的比较器

### 原型如下

* 不同编译器实现方法可能不同，实际上是重载了`()`运算符
* greater对应**&gt;**函数，less对应**&lt;**函数

```cpp
template<typename _Tp>
struct greater : public binary_function<_Tp, _Tp, bool>
{
    _GLIBCXX14_CONSTEXPR
    bool operator()(const _Tp& __x, const _Tp& __y) const
    { return __x > __y; }
};

  /// One of the @link comparison_functors comparison functors@endlink.
template<typename _Tp>
struct less : public binary_function<_Tp, _Tp, bool>
{
    _GLIBCXX14_CONSTEXPR
    bool operator()(const _Tp& __x, const _Tp& __y) const
    { return __x < __y; }
};
```

## 使用：

### sort函数

```cpp
std::array<int, 6> arr = {6, 1, 5, 8, 4, 3};
std::sort(arr.begin(), arr.end(), std::greater<>());  //从大到小排序
for(auto &item:arr)std::cout<<item<<" ";std::cout<<std::endl;
```

### 最大最小堆

```text

```

### 优先队列

```cpp
#include<queue>

/*最大优先队列*/
std::priority_queue<int> q;   //最大优先队列，默认
std::priority_queue<int, std::vector<int>, std::less<>> q;  //跟前面等效

/*最小优先队列*/
std::priority_queue<int, std::vector<int>, std::greater<>> q; //最小优先队列   greater是排序器，
```

