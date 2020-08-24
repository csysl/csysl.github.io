# queue

[参考](https://zh.cppreference.com/w/cpp/container/queue)

包含：

* 队列：**queue**
* 优先队列：**priority\_queue**

见 [二叉堆&优先队列19.11.10.md](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/二叉堆&优先队列19.11.10.md)

## queue

## priority\_queue

实现

```cpp
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```

