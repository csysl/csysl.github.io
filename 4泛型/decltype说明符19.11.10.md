[参考](https://zh.cppreference.com/w/cpp/language/decltype)    

**环境**：Ubuntu18.04    g++ 9.2.1

decltype ( 实体 )

decltype ( 表达式)

可以自动推导**实体或者表达式**的类型，推到规则见参考

```c++
struct T {
    int a{};
    string str;

    T()= default;
    T(int ta, string &&tstr) : a(ta), str(tstr) {};
};

int main() {
    int i;
    decltype(i) j = 10;    //推导类型为int

    T tt;
    T t(1, "hello");
    decltype(t) t2(2, "world");    //推导类型为T

    return 0;
}
```



#### 跟auto的模板函数一起使用，可以自动推导类型

c++14开始可以推导返回类型

```c++
auto ssum(A a, B b) -> decltype(a + b) {   //自动推导返回值的类型
    cout << "the type of a is " << typeid(a).name() << endl;
    cout << "the type of b is " << typeid(b).name() << endl;
    return a + b;
};

int main() {
    auto res = ssum(10.0, 20);     //10.0是double类型，20是int
    cout << typeid(res).name() << endl;  //返回值是double类型

    return 0;
}
```



#### 跟lambda表达式一起使用

<font color=red>**lambda表达式本身看做一种类型而不是推导为返回值**</font>

```c++
int main() {
    auto func = [](int a, int &&b) -> int { return a * b; };

    decltype(func) g = func;
    //auto g = func;          //可以直接换为auto
    cout << g(3, 5) << endl;  
    
    return 0;
}
```

