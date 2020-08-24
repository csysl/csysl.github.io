l代码环境：g++ 9.1.3  std17

[TOC]

### cstring

```c++
#include<cstring>
```

c风格的字符串最后一个字符以'\0'结尾，在使用**`strlen`函数获取其长度是，不会记录'\0'的长度**，但在初始化字符串时，要给'\0'分配内存。

```c++
char* result = new char[strlen(str1)+1];   //+1是分配给\0的内存
```

char *和char[]实际是代表着不同的意思，后者包含字符串的长度信息并且会自己释放内存

```c++
char text1[] = "abcdef";
size_t s1 = sizeof(text1); // is 7    这里获取的是字符串的实际长度
size_t s2 = strlen(text1); // is 6

const char* text2 = "abcdef";
size_t s3 = sizeof(text2); // is platform-dependent   这里获取的是指针的长度，在32位环境是4,64位是8
size_t s4 = strlen(text2); // is 6
```

### string

实际是一个类而不是内置类型，会自动管理内存，需要完成大量分配内存和调整大小的工作。

- 使用`c_str()`获取string的c风格字符串，返回`const char*`
- 使用`data()`获取c风格字符串，返回`const char*`，不包括**\0**,**从c++17开始，非const对象返回`char*`** 。

```c++
auto str = "hello world!"s;

auto str1=str.c_str();
cout<< typeid(str1).name()<<endl;   //PKc

auto str2=str.data();
cout<< typeid(str2).name()<<endl;   //Pc
```

#### 高级数值转换

- 使用`to_string()`可以将数值转化为string字符串

```c++
auto str=to_string(123.456488);
```

#### 低级数值转换

p45



### string_view类  p46  c++17

有时候，函数形参的形式为`const char*`，这时候string对象可以通过`c_str()`或者`data()`传值，最好的方式是函数直接使用`const string&`作为参数，**可以直接使用string_view作为形参，就可以接受这两种实参**。

1. string_view通常使用**按值传递**，因为其只包含了**指向字符串的指针还有字符串的长度**，复制成本极低。

2. 当函数将只读字符串作为参数时，**可以使用string_view代替cosnt string&或者const char*。**

```c++
string_view extractExtension(string_view fileName){   //按值传递
	return fileName.substr(fileName.rfind('.'));	
}

string fileName = R"(c:\temp\my file.ext)";
cout << "C++ string: " << extractExtension(fileName) << endl;

const char* cString = R"(c:\temp\my file.ext)";
cout << "C string: " << extractExtension(cString) << endl;

cout << "Literal: " << extractExtension(R"(c:\temp\myfile.ext)") << endl;
```

3. string和string_view拼接：

```c++
string str = "Hello";
string_view sv = " world";
auto result = str + sv.data();   //string_view只含有.data()方法没有.c_str()方法
```



### 字面量

- c风格字符串，字面量以`R"(`开始，以`)"`结束
- **string**字面量，后面加**s**，auto编译器会自动推导为string
- **string_view**字面量，后面加**sv**，auto编译器会自动推导为string_view

```c++
auto str1 = R"(hello world!)";
auto str2 = "hello world!"s;
auto str3 = "hello world!"sv;
cout << str1 << " " << typeid(str1).name() << endl;  //PKc
cout << str2 << " " << typeid(str2).name() << endl;  //NSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEE
cout << str3 << " " << typeid(str3).name() << endl;  //St17basic_string_viewIcSt11char_traitsIcEE
```

