#### 0. 说明

c++17引入

**作用**：可以通过一组参数调用**任何可调用对象**(函数指针、函数对象、lambda表达式、function对象、成员函数等)

```c++
void printMessage(string_view message) { cout << message << endl; }
int main() {
	invoke(printMessage, "Hello invoke.");  //函数指针
	invoke([](const auto& msg) { cout<<msg<<endl; }, "Hello invoke.");//lambda表达式
	string msg = "Hello invoke.";  
	cout << invoke(&string::size, msg) << endl;   //对msg实例调用其size生源函数
}
```

本身意义不大，因为完全可以不使用。**但由于其可以调用任何可调用对象，在模板中会有用！！！！**

```c++

```

