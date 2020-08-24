[参考](https://zh.cppreference.com/w/cpp/container/queue)

包含：

- 队列：**queue**
- 优先队列：**priority_queue**

见 [二叉堆&优先队列19.11.10.md](../二叉堆&优先队列19.11.10.md) 

#### queue



#### priority_queue

实现

```c++
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```

