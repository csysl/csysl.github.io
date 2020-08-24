# 万能引用&右值引用

**参考**：effective modern c++ 条款24

\[TOC\]

## 0. 说明

右值引用只接受右值；万能引用接受左值时推导为左值引用，接受右值时推导为右值引用。

## 1. 右值引用

**对`type&&`**，判断是否是右值引用：

* 如果**type**是一个**明确的类型**，那么这里`type&&`代表右值引用

```cpp
void getNum(int &&inValue) {    //明确的int类型，右值引用
    cout << "invalue is " << inValue << endl;
}
```

* 如果**type**是一个**模板参数**，但是该模板参数**类型明确\(不会发生推导\)**，那么`type&&`是一个右值引用

```cpp
template<typename T>
void getVec(vector<T> &&vec) {   //T是模板参数，但是T不会被推导，所以这里是右值引用
    for (const auto &v:vec) cout << v << " "; cout << endl;
}
int main() {
    vector<int> vec{1, 2, 3, 4, 5, 6, 7, 8, 9};
    getVec(move(vec));

    return 0;
}
```

* **存在const，就是右值引用**

```cpp
template<typename T>
void getNum(const T &&inValue) {    //因为存在const，所以是右值引用
    cout << "invalue is " << inValue << endl;
}
```

### 注意：右值做参数会变成左值

```cpp
void getNum(int &&inValue) {
    cout << "invalue is " << inValue << endl;
}

void getvalue(int &&invalue) {
    //getNum(invalue);    //编译报错，invalue虽然接受的是右值，但是invalue因为有名称，本身是个左值，而getNum()接受右值，所以这里报错
    getNum(move(invalue));  //正确
}
```

## 2. 万能引用

* **对`type&&`**，如果type是模板参数且类型不明确\(会发生推导\)，那么这里是万能引用

```cpp
template<typename T>
void getNum(T &&inValue) {    //T类型不明确，万能引用
    cout << "invalue is " << inValue << endl;
}
```

* `auto &&` 声明类型，都是万能引用

```cpp
int a = 10;
auto &&a1 = 10;    //接受右值，右值引用
auto &&a2 = a;     //接受左值，左值引用

a1 = 5; 
a2 = 6;
cout << a << endl;   //6
```

