#### find() :

查找范围内的指定元素并返回迭代器，搜索不到则返回**搜索区间的尾迭代器** 

```c++
array<int, 10> arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};

if (auto iter = find(cbegin(arr), cend(arr) - 2, 0);iter != cend(arr) - 2)
    cout << "a:" << *iter << endl;
else
    cout << "b:" << *iter << endl;   //b:9
```



#### find_if()&find_if_not(): 与find()功能类似，接收谓词函数回调

查找**指定范围**内**指定条件**的**第一个**元素的迭代器，搜索不到则返回**搜索区间的尾迭代器** 

```c++
array<int, 10> arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};

auto iter = find_if(cbegin(arr), cend(arr), 
                    [](int num) -> bool { return num > 5; });
cout << *iter << endl;   //6
```



