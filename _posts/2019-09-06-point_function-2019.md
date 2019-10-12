---
layout:     post                    # 使用的布局（不需要改）
title:      Point                  # 标题 
subtitle:   指针与面试题           #副标题
date:       2019-09-06              # 时间
author:     kevin-leak                      # 作者
header-img: img/post/c_c++/bg-2019-06.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - c
    - c++
---
这里就不详细的去介绍指针的用法，提一些要点。

## 多级指针

一般会用到二级指针。

这就会问道二级指针的意义。

先看一下代码：

```c
int test(int **p){
    int i = 9;
    *p = &i;
    return 0;
}

int main() {
    int *k;
    test(&k);
    printf("%d\n", *k);
    return 0;
}
```

二级指针的意义是：用来在函数中对指针变量的修改，那么递推下去。



## 可变参数

### 实例

```c
/**
 * @num: 参数的长度。可以自定义为其他。
 * */
void changeParam(int num, ...) {
    va_list vaList;
    va_start(vaList, num);
    double sum = 0.0f;
    for (int i = 0; i < num; i++) {
        // 获取数据，第二个参数为int类型的，你传入的啥，就是啥。
        sum += va_arg(vaList, int);
    }
    va_end(vaList);
    printf("%f", sum);
}


int main() {
    changeParam(5, 2,3,4,32,3);
    return 0;
}
```



### 使用简介

- 可变参数用三个点表示，必须要有长度的传入(看上面实例)

- 初始化

  ```c
  // 当前函数的第一个参数。
  va_start(vaList, num);
  ```

- 获取参数

  ```c
  // 获取数据，第二个参数为int类型的，你传入的啥，就是啥。
  va_arg(vaList, int);
  ```

- 清空内存

  ```c
  va_end(vaList);
  ```



### 原理

原理其实就是宏，可以在gcc中查看头文件

贴一篇文章：

- [C语言函数可变长度参数剖析](https://www.cnblogs.com/li-hao/p/3401942.html) 



## 函数名为参数

### 步骤

如何实现这种情况，有哪些步骤？

- 声明要传入函数的类型
- 编写第二个函数运行的位置
- 传入第三方，符合声明的行数类型的函数，可以自定义但不能重名。



### 例子与解释

```c
// 声明函数类型
typedef int (* test)(int);

// 函数要被包裹的位置
void startTest(test t, int  k){
    t(k);
}

// 编写实际要运行的函数。
int kk(int k){
    printf("%d", k);
}

int main() {
    startTest(&kk, 8);
    return 0;
}
```

这里先贴一片[参考文章](<https://blog.csdn.net/crfoxzl/article/details/2147744>) 

先解释`void (*func)(int)`： 

```c
void func()->void func(int)->void func(int)->void (func)(int) 
```

上面的图示能看明白吧？func是一个函数指针，它的返回类型为void，它所指向的函数接收一个int型的参数。

若是写成void *func(int)则变成了:

func是一个函数，它的返回类型是void 类型的指针，它接受一个int型参数。 

所以`void (*signal(int sinno,void(*func)(int)))(int)`意思是： 
signal是一个函数指针，它的返回类型是void,它接收一个int类型的参数；

不过这个指针是另一个函数的返回值，它接收2个参数，第一个是int,第二个已经解释过了。   



### 为什么可以这么做？

给一篇参考文章：[详解C/C++函数指针声明](<https://yq.aliyun.com/articles/573473?utm_content=m_45084>) 

函数名也是一个地址，通过编译器的作用，加了个括号，进行一个操作。





## 宏函数与内联函数

这里贴一篇文章：[C 中关键字 inline 用法解析](<https://blog.csdn.net/zqixiao_09/article/details/50877383>) 

### 为什么要引入？

提前可以知道的是：我们用的内置函数，都是引用，在编译的时候，会进行一个替代，但这样会造成频繁的出栈与入栈。而内联函数的作用就是利用宏进行一个代码的替代，

inline为C++的关键字，**后来被扩展到C语言**。所以早期的C语言ANSI C是不支持这个关键字的，如果使用inline关键字会编译出错。不过后续的C99规范扩展了这一关键字，于是在支持C99规范的编译器中，是可以使用inline的。

### 代码

```c
// 宏函数
#define mutil(x, y) x * y

int main() {
    printf("%d\n", mutil(2, 3));
    printf("%d\n", mutil(2 + 8, 3 + 7));
    return 0;
}
```

运行结果：

>6
>33

**注意这个**：2 + 8 * 3 + 7 = 33

### 需要注意

- 宏函数只是单纯的替换，不会进行检查，不会造成函数调用的开销，但会产生很多重复的代码。

  具体怎么理解单纯的替换，看上面的实验

- 内联函数

  ​	和宏函数工作模式相似，但是两个不同的概念，首先是函数，那么就会有类型检查同时也可以debug
  在编译时候将内联函数插入。

  不能包含复杂的控制语句，while、switch，并且内联函数本身不能直接调用自身。
  如果内联函数的函数体过大，编译器会自动的把这个内联函数变成普通函数。



## 常见面试题

### const 与 指针

和java对比，const = final

```c
const char *：修饰值
char const *：修饰值
char * const：修饰指针
char const * const：同时修饰值和指针
```

<span style="background-color: #E2F0FF; padding: 0px 3px; margin:2px; border-radius:3px ">只要const在*前面，那么指针就不可以变化，值可以变</span> 

#### 实验

- `const char *`

  ```c
  const char * p;
  // *p = 'c'; 报错
  char k = 'k';
  p = &k;
  ```

  可以察觉，const指定了不可以修改值，但可以又该指针地址。

-  `char const *`

  ```c
  char const * p;
  // *p = 'c'; 报错
  char k = 'k';
  p = &k;
  ```

  说明，const修饰的值。

- `char * const` 

  ```C
  char * const p;
  *p = 'c';
  char k = 'k';
  // p = &k; 报错
  ```

  const修饰的是指针，也即是说修饰的是p的本身。

- `char const * const`

  ```c
  char const * const  p;
  *p = 'c';
  char k = 'k';
  p = &k;
  ```

  

