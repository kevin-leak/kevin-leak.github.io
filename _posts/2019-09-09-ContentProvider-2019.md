---
layout:     post                    # 使用的布局（不需要改）
title:      ContentProvider分析                # 标题 
subtitle:   全面分析android存储           #副标题
date:       2019-09-09              # 时间
author:     kevin-leak                      # 作者
header-img: img/post/android/bg-2019-09.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - android
---

> 文章结构
>
> contentProvider的用法
>
> contentProvider的原理分析
>
> contentProvider衍生的类
>
> contentProvider的注意点



### 原理

<img src="../img/post/android/picture/contentProvider_theory.png" />



### 作用

contentProvider 的作用是为应用提供不同数据的接口，用过contentProvider统一获取资源。





### 使用

URL：contentProvider 用来处理资源定位

ContentUris：解析URI中的id

UriMatcher：根据自己设定的URI，来匹配客户端发来的URI











