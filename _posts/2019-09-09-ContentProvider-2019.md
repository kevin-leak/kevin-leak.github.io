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



### 原理介绍

<img src="https://www.crabglory.club/img/post/android/picture/contentProvider_theory.png" />

ContentResolve 可以通过Context中利用getContentResolve()获取；

获取对象后，通过其方法进行操作，然后底层利用Binder进行通信；

ContentProvider获取参数并进行解析，进行实际的数据操作；

利用context.getContentResolve().notifyChage通知客户端数据发生变化。

具体简单介绍：

<span style="background-color: #F9B6E5; padding:0px 3px; margin:2px; border-radius:3px ">三个主类</span> 

ContentResolve 、 ContentProvider，Binder

- ContentResolve ：用来对数据操作的，同时内部实现了**ContentObserve**，实现对数据变化的监视
- ContentProvider：抽象类，因为数据的类型有很多，ContentProvider将数据统一个接口
- Binder：用来组件之间通信的一个类，可以跨进程。



<span style="background-color: #F9B6E5; padding:0px 3px; margin:2px; border-radius:3px ">通信参数</span> 

- URI：资源定位符
- ContentValue：复写了大量的put(key, value)方法，用来储存数据
- Selection：选择数据的哪一行



<span style="background-color: #F9B6E5; padding:0px 3px; margin:2px; border-radius:3px ">操作辅助类</span> 

- Cursor

  用来梳理ContentResolve中query获取的数据集，封装了各种取数据的方式。

- UriMatcher

  配合ContentProvider中getType方法，用addUri来预设需要的URI，在getType中用match来进行适配，返回合适的类型

- ContentUris

  主要是addAppendId和getAppendId，来设置信息。



### 具体使用















