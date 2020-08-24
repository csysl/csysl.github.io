[文档](https://gmplib.org/manual/index.html#Top) 

### 安装

直接使用命令

```shell
sudo apt install libgmp-dev
```



- 前往[官网](https://gmplib.org/)下载最新的GMP包 最新的是v6.12，更新时间是2016年
- 解压，进入解压后的目录，顺序执行下面命令

```shell
./configure --enable-cxx
make
make check
sudo make install
```

可能会提示缺少m4，解决：

```shell
sudo apt install m4
```



### cmake配置

只需要添加下面的语句就可以了

```cmake
target_link_libraries(paggy_c gmp gmpxx)
```



### 头文件

```c++
#include <gmpxx.h>   //c++
#include <gmp.h>     //c
```

### API

```c++
mpz_class a;    //c++的整数
a.get_XXX();    //get方法，查阅文档
a.set_XXX();    //set 方法，查阅文档
a.fits_XXX();   //查阅文档
```

支持直接运算

```c++
mpz_class a;
a="123123444";
b=564;
c=456;
cout<<a*b%c<<endl;

```

