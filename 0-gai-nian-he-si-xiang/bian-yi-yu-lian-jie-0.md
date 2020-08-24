# 编译与链接0

[参考](https://cloud.tencent.com/developer/article/1037718)

C++中有的东西需要放在可以在.h文件中定义，有的东西则必须放在.cpp文件中定义，有的东西在不同的cpp文件中的名字可以一样，而有的则不能一样

那么究竟哪些东西可在头文件中定义，声明，哪些东西又必须在.cpp中定义，声明呢?

\*以下所有的讨论都是在全局命名空间中（即不定义自己的namespace）下进行的

**函数**

1、在.h中只能声明函数，在.cpp中可以声明与定义函数

如果在.h中声明并定义一个函数，则该函数只能被\#include一次，否则则会出现重定义错误

比如

1.h

```javascript
#pragma once

void show()
{

}
```

a.cpp

```javascript
#include "1.h"
```

b.cpp

```javascript
#include "1.h"
```

error LNK2005: "void \_\_cdecl show\(void\)" \(?show@@YAXXZ\) 已经在 a.obj 中定义

所以要避免在头文件中定义函数

2、在不同.cpp中定义的函数原型（函数返回值，函数名称，函数参数）不能完全一样，

比如如果有在两个.cpp文件中均存在

void show\(\){};

会出现重定义错误

**内联函数**

为了确保所有调用该inline函数的文件中的定义一样，所以需要是在.h文件中定义

注意这里的inline对于编译器来说只是建议性的，关于该内联函数被拒绝会在下一篇文章中介绍

**typedef**

在不同的cpp中可以一样

**变量**

1、在.h中只能声明，在.cpp中可以声明与定义一个变量

如果在.h中的定义一个变量，则该变量被include两次以上时则会出现重定义错误

2、在不同.cpp中定义的变量的名字与类型不同一样

**常量**

1、如果const常量是用常量表达式进行初始化的，则可以在.h中声明与定义

2、如果const变量是用非常量表达式进行初始化的，那么该变量应该在cpp文件中定义，而在.h文件中进行声明。

3、不同cpp中以定义名字与类型一样的变量

**static变量**

1、在不同的cpp中可以定义名字与类型一样的变量

2、如果在.h中定义一个static成员，则所有include该文件的文件均拥有一份独立的该static成员，一个文件对其的修改不会影响到另一个文件

所以static变量一般是放在.cpp出现并定义.

例如

1.h

```javascript
#pragma once

static int a = 5;
```

a.cpp

```javascript
#include "1.h"
#include <iostream>
using namespace std;

void showstatic()
{
    cout << "In a.cpp:" << a << endl;
    a = 1;
    cout << "In a.cpp:" << a << endl;
}
```

b.cpp

```javascript
#include "1.h"
#include <iostream>
using namespace std;


void showstatic();
int main()
{
    showstatic();
    cout << "In b.cpp:" << a << endl;
    system("pause");
}
```

![img](https://github.com/csysl/csysl.github.io/tree/0bd19b8dc72814ad1fdc23adbfb9a9ba2446d2d2/0概念和思想/D:/0文件/netstore/notebook/c++/概念和思想/orvhcjtpcw.png)

**static函数**

在不同的cpp中可以定义函数原型一样的函数

**类**

不同的cpp中类的名字可以一样

**类成员与函数**

在.h中定义，所有成员必须在类中声明，在cpp中实现

非静态的常量整形数据成员不能就地初始化（**\*C++11**中，标准允许使用等号=或者花括号{}进行就地的非静态成员变量初始化）

在类内部定义的成员函数将自动作为inline处理

在.h外部定义的函数需要加上inline说明

否则在被include多次时会出现重定义错误

1.h

```javascript
#pragma once
#include <iostream>


class A
{
public:
    void show();
};
void A::show()//无inline
{
    std::cout << "hello" << std::endl;
}
```

a.cpp

```javascript
#include "1.h"
#include <iostream>
using namespace std;
```

b.cpp

```javascript
#include "1.h"
#include <iostream>
using namespace std;
```

error LNK2005: "public: void \_\_thiscall A::show\(void\)" \(?show@A@@QAEXXZ\) 已经在 a.obj 中定义

**类的const成员**

在类中声明变量为const类型的成员不可以就地初始化

const常量的初始化必须在构造函数初始化列表中初始化，而不可以在构造函数函数体内初始化（**\*C++11**中，标准允许使用等号=或者花括号{}进行就地的非静态成员变量初始化）

```javascript
#pragma once
class A
{
public:
    const int i=50;
};
```

error C2864: “A::i”: 只有静态常量整型数据成员才可以在类中初始化 d:\我的资料库\documents\visual studio 2010\projects\fasd\fasd\1.h 5 1 fasd

**类的静态的数据成员**

不可以就地初始化，需要到.cpp中进行定义

\(对于非常量的静态成员变量，C++11与C++98保持了一致。需要到头文件以外去定义它\)

**类的静态的常量整形数据成员**

可以就地初始化

如

```javascript
class A
{
private:
    const static int i = 5;
};
```

**模板（不考虑export）**

模板函数与模板类的声明与实现必须放在一个文件中

**总结**

|  | 是否可以在.h中定义 | 在不同.cpp中是否可以重名 | 特殊说明 |
| :--- | :--- | :--- | :--- |
| 函数 | 不可以，会出现重定义错误 | 不可以 |  |
| 内联函数 | 可以 | 可以 | 为了确保所有调用该inline函数的文件中的定义一样，所以需要是在.h文件中定义 |
| typedef | ---------------------- | 可以 |  |
| 常量 | 可以 | 可以 | 1、常量表达式进行初始化的，则可以在.h中声明与定义 2、非常量表达式进行初始化的，那么该变量应该在cpp文件中定义，而在.h文件中进行声明。 |
| 变量 | 不可以，会出现重定义错误 | 不可以（类型与名字） |  |
| static变量 | 可以 | 可以 | 在.h中定义一个static成员，则所有include该文件的文件均拥有一份独立的该static成员，一个文件对其的修改不会影响到另一个文件 所以static变量一般是放在.cpp出现并定义. |
| static函数 | 可以 | 可以 |  |

|  | 是否可以在.h中定义 | 是否可以就地初始化 | 特殊说明 |
| :--- | :--- | :--- | :--- |
| 类 | 可以 |  |  |
| 类数据成员 | ------------------ | 不可以 | （\*C++11中，标准允许使用等号=或者花括号{}进行就地的非静态成员变量初始化） |
| 类成员函数 | ------------------ | ---------------- | 在.h外部定义的函数需要加上inline说明 否则在被include多次时会出现重定义错误 |
| 类const数据 | ------------------ | 不可以 | 1、在类中声明变量为const类型的成员不可以就地初始化 const常量的初始化必须在构造函数初始化列表中初始化，而不可以在构造函数函数体内初始化 2、同类数据成员中的特殊说明 |
| 类的静态的数据成员 | ------------------- | 不可以 | 不可以就地初始化，需要到.cpp中进行定义 \(对于非常量的静态成员变量，C++11与C++98保持了一致。需要到头文件以外去定义它\) |
| 类的静态的常量整形数据成员 | ------------------ | 可以 |  |

|  | 特殊说明 |
| :--- | :--- |
| 模板 | 模板函数与模板类的声明与实现必须放在一个文件中 |

至于为什么会这样，与C++的编译和链接，和编译产生的目标文件\(.obj\)，内部链接，外部链接有关，

