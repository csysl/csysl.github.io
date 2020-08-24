# 函数\_值.&.&&传参

\[TOC\]

## 1. 值传递：接受左值和右值

```cpp
//void test(int a) { cout << a << endl; }
void test(const int a) { cout << a << endl; }

int main() {
    test(5);    //接受右值
    int n = 10;
    test(n);  //接受左值

    return 0;
}
```

## 2. &：左值引用带const，左右值都接受；不带const，只接受左值

**不带const**

```cpp
void test(int &a) { cout << a << endl; }
int main() {
    //test(5);    //报错，不接受右值
    int n = 10;
    test(n);  //接受左值

    return 0;
}
```

**带const**

```cpp
void test(const int &a) { cout << a << endl; }
int main() {
    test(5);    //接受右值
    int n = 10;
    test(n);  //接受左值

    return 0;
}
```

## 3. &&：右值引用，const修不修饰都只接受右值

```cpp
void test(int &&a) { cout << a << endl; }
void test(const int &&a) { cout << a << endl; }
int main() {
    test(5);    //接受右值
    int n = 10;
    //test(n);  //报错，不接受左值

    return 0;
}
```

