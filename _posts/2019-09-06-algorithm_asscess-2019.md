
---
layout:     post                    # 使用的布局（不需要改）
title:      算法的评估方式以及呈现方式                  # 标题 
subtitle:   时间、空间复杂度、对数器          #副标题
date:       2019-09-06              # 时间
author:     kevin-leak                      # 作者
header-img: img/post/algorithms/bg-2019-09-06.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - algorithms
---




一个算法评估，应该在于他用来解决什么问题？对于问题有什么要求？而这个算法是否满足这个特性。

而根据计算机的特性特性，可以归纳为：**时间复杂度，空间复杂度**

需要特别注意的是，并不是时间越短的越好，还是要注意一开始说的，要根据问题要求。

那么我们要解决的问题是：

那么我们要处理的就是对于时间和空间如何就的问题，



可以参照阅读：[知乎文章](https://zhuanlan.zhihu.com/p/50479555) 有代码实例



## 时间复杂度

时间复杂度是由：程序执行的次数来衡量的。

介绍一个概念：

常数时间操作：指的是一个程序指令的执行与数据量无关，每次执行都是固定时间内完成。

而时间复杂度就是：一个算法流程中， 常数操作数量的指标 

如果把常数操作当做n的话

一个算法的流程执行假设为：n^2 + 2n + 1;

那么我们说他的时间复杂度是：O(n^2)；

剩余的即为fn，时间复杂度为O(f(n))

总结来说，时间复杂度表示为常数操作流程表达式的，最高指数

举一个例子：

```java
int i = 1;
while(i<n)
{
    i = i * 2;
}
```

假设说，循环x次.

我们这样，模拟一下应该是这样开始的： 

x = 1 ：1 

x = 2 : 1 * 2 

x = 3: 1 * 2 * 2

最后，i > n 的时候，应该就是：$$1 * 2^x > n$$

那么x 等于：$$log_2 n < x $$

时间复杂度为：$$O(logn)$$

### 递归算法的复杂度

master公式的使用
$$T(N) = a*T(N/b) + O(N^d)$$ 

- log(b,a) > d -> 复杂度为 $$O(N^{log(b,a)})$$ 
- log(b,a) = d -> 复杂度为 $$O(N^{d} * logN)$$ 
- log(b,a) < d -> 复杂度为 $$O(N^d) $$

具体的例子，看一下[归并排序](./1. sort/2. merge sort.md)的时间复杂度



## 空间复杂度

空间复杂度比较简单，他是直接忽略字节大小，对于统计变量的开辟，以及数组之类的长度

举个例子：new Array[n]，那么空间复杂度就是：O(n);





## 代码显示复杂度

### 如何测试一个时间复杂度?

#### 毫秒为单位

java 的表示方式：

```java
long current = System.currentTimeMillis();
// 要测试的代码
long after = System.currentTimeMillis();

System.out.println(after-current);
```

c语言的标示，windows

```c
#include <time.h>
int main()
{
    time_t  before = time(NULL);
    // 代码
    time_t  after = time(NULL);
    double k = after - before;
    cout << k << endl;
    return 0;
}
```

参照 [博客](<https://blog.csdn.net/u011630575/article/details/45567599>)

### 纳秒为单位

java 的过程

```java
long current = System.currentTimeMillis();

long affter = System.currentTimeMillis();

System.out.println(affter-current);
```



## 如何显示一个排序过程？

这一块是看到了算法书中的图形化界面，我肯了一下源码，实现还是比较花时间的

这里给个github的网址，可以自己当做一个工具包使用。

```
https://github.com/kevin-wayne/algs4
```



## 如何判断一个算法是否正确？——对数器

对数器。

```java
import java.util.Arrays;

public class Logarithm {

    /**
     * @param size  设置的数组大小
     * @param value 设置的取值范围，负数正数一起
     * @return
     */
    public static int[] generateRandomArray(int size, int value) {

        int[] arr = new int[(int) ((size + 1) * Math.random())];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (value * Math.random());
        }
        return arr;
    }

    // 可以直接用一些库函数来进行测试
    public static void rightMethod(int[] arr) {
        Arrays.sort(arr);
    }

    private static int[] copyArray(int[] arr1) {
        int[] arr = new int[arr1.length];
        for (int i = 0; i < arr1.length; i++){
            arr[i] = arr1[i];
        }
        return arr;
    }

    private static void printArray(int[] arr3) {
        for (int i : arr3) {
            System.out.print(" " + i);
        }
        System.out.println();
    }

    private static boolean isEqual(int[] arr1, int[] arr2) {
        if ((arr1 == null && arr2 != null) || (arr1 != null && arr2 == null))
            return false;
        if (arr1 == null && arr2 == null)
            return true;
        if (arr1.length != arr2.length)
            return false;
        for (int i = 0; i < arr1.length; i++) {
            if (arr1[i] != arr2[i])
                return false;
        }
        return true;
    }
}
```

测试单元

```java
public static void main(String[] args) {
    int testTime = 5000000;
    int size = 10;
    int value = 100;
    boolean succeed = true;
    for (int i = 0; i < testTime; i++) {
        int[] arr1 = generateRandomArray(size, value);
        int[] arr2 = copyArray(arr1);
        int[] arr3 = copyArray(arr1);
        // 要测试的算法
        MergeSort.sort(arr1);
        rightMethod(arr2);
        if (!isEqual(arr1, arr2)) {
            succeed = false;
            printArray(arr3);
            break;
        }
    }
    System.out.println(succeed ? "Nice!" : "error----");
    int[] arr = generateRandomArray(size, value);
    printArray(arr);
    // 要测试的算法
    MergeSort.sort(arr);
    printArray(arr);

}
```

