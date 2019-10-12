---
layout:     post                    # 使用的布局（不需要改）
title:      C++总结梳理                  # 标题 
subtitle:   c++相对于c的对比与分析           #副标题
date:       2019-10-12             # 时间
author:     kevin-leak                      # 作者
header-img: img/post/c_c++/bg-2019-10-07.jpg    #这篇文章标题背景图片 catalog: true                       # 是否归档
tags:                               #标签
    - c
    - c++
---
**思考**：

- c 与 c++ 相同却不同意义的部分
- c++兼容问题
- c++ 新增的部分

**新增的意义？**

对象的新增：封装、继承、多态



指针与引用的区别
----------------

实验代码

```c++
int a = 12345;
cout << "a value is :" << a << "\ta address is :" << &a << endl;
int &p = a;
cout << "a value is :" << p << "\tp address is : " << &p << endl;
int *q = &a;
cout << "q value is :" << q << "\tq address is : " << &q << endl;
int* &pr = q;
cout << "pr value is :" << pr << "\tpr address is : " << &pr << endl;
```

输出信息

```
a value is : 12345      a address is :0x61ff04
a value is : 12345      p address is : 0x61ff04
q value is : 0x61ff04   p address is : 0x61ff00
pr value is : 0x61ff04  p address is : 0x61ff00
```

发现：

- 值变量的引用，引用的值和地址都与值变量一样
- 指针变量的引用，引用的值和地址都与值变量一样

### 总结

引用就是一个别名，有点类似于宏定义，一个变量名字的取代而已

<img src="https://www.crabglory.club/img/post/c_c++/mind/reference_point.png" height="300px"/>

命名空间
--------

