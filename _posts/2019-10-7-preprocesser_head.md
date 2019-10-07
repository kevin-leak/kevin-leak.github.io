---
layout:     post                    # 使用的布局（不需要改）
title:      预处理器与头文件                  # 标题 
subtitle:   实例与原则           #副标题
date:       2019-10-07              # 时间
author:     kevin-leak                      # 作者
header-img: img/post/c_c++/bg-2019-10-07.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - c
    - c++
---
## 宏

> 所谓宏，就是一些命令组织在一起，作为一个单独命令完成一个特定任务
>
> 计算机科学里的[宏](https://baike.baidu.com/item/%E5%AE%8F/2648286)（Macro)，是一种批量处理的称谓

用来**标识**执行文本替换。



## 预处理

预处理，是一个软件，用来执行宏替换，分别是：#define，#include，这种带有#的代码。

主要功能是进行**宏定义与条件编译**

为什么要有预处理？

因为宏定义的存在，是为了能应用资源文件，同时做到一个平台的兼容性。

### 预处理指令

- 定义宏的指令

  ```c
  #define              定义一个预处理宏
  #undef               取消宏的定义
  ```

- 包含文件指令

  ```c
  #include             包含文件命令
  #include_next    与#include相似, 但它有着特殊的用途
  ```

- 条件编译指令

  ```
  #if                  编译预处理中的条件命令, 相当于C语法中的if语句
  #ifdef               判断某个宏是否被定义, 若已定义, 执行随后的语句
  #ifndef              与#ifdef相反, 判断某个宏是否未被定义
  
  #elif     
  #else    
  #endif              
  ```

- 宏运算

  ```c
  #                      将宏参数替代为以参数值为内容的字符窜常量
  ##                    将两个相邻的标记(token)连接为一个单独的标记
  // 例子
  #defined connect(x)  n##x
  int connect(1);
  // 预处理后：int n1;
  ```

- defined 运算符

  ```c
  defined              与#if, #elif配合使用, 判断某个宏是否被定义
  ```

- 预定义的宏

  ```c
  #line  标志该语句所在的行号，如本来是第4行，你有第4行写了#line 100，它就认为是第100行（__LINE__)
  ...
  ```

-  其他指令

  ```c
  #pragma           说明编译器信息
  #warning          显示编译警告信息
  #error              显示编译错误信息
  ```

  

## 头文件与源文件

[贴一篇文章](<https://blog.csdn.net/xhbxhbsq/article/details/78955216>) 

头文件的意义就是进行代码的划分，分块，与函数同一的思想，也可以说是java中包。

### #include指令

两种：

- `#include  <stido.h>`

  用于c语言本身的库，会在系统的库文件存放的地方寻找

- `#include "kk.h"`

  自己写的库，会在当前文件下进行一个寻找。

include的路径可以衍生，对于头文件中的函数，可以重复，利用宏命令进行一个解决就行了。



### 代码

- 建立一个头文件`kk.h`
- 在`kk.c` 实现
- 在`main.c` 中使用
- 再cmake配置文件中配置。

`kk.h`

```c
#ifndef C_FUNC_KK_H // 如果没有定义C_FUNC_KK_H
#define C_FUNC_KK_H // 进行定义。
void pp(int wufu);
#endif //C_FUNC_KK_H
```

kk.c

```c
#include "kk.h"
#include <stdio.h>

void pp(int m){
    printf("%d", m);
}
```

main.c

```c
int main() {
    pp(1);
    return 0;
}
```

