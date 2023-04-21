---
title: 2022年10月第二周笔记
author: beni
date: 
categories: [diary, ]
tags: [need-to-sort]
mermaid: true
---


# LC 870

# [memlab 工具，cli网页内存分析工具](https://facebook.github.io/memlab/)

# [hasky git-hook 工具](https://typicode.github.io/husky/#/)
[head](#jetbrains-工具个人使用)

# [jetbrains 工具个人使用](https://zhaiblog.cn/169.html)
    
    有条件的请支持正版

由于自己破解的webstorm2020.1 对angular项目支持不友好，例如组件无法很好跳转，d.ts文件查找困难，网上看到小伙伴升级webstorm可解决该问题。由于自己的穷困潦倒，暂时无法支持正版，所以继续用上熟悉的**开放版**。

下载最新版的ide，出现了无法打开的情况，点击无法启动，没有任何反应。经查询通过cmd 启动webstorm bin下 inspect.bat模式打开，然后会在控制台中会看到具体的报错信息。我的报错信息大概意思是agent错误，因为webstorm在安装的时候会提示是否保留以前的信息，结果就保存了以前的agent信息

<center>新旧agent对比</center>

![错误和正确的agent对比](/assets/img/posts/jetbrains-agent-options.jpg "左侧为旧的agent右侧为正确agent")

ide在启动的时候会去用户配置的 webstorm64.exe.vmoptions 和 APP bin 下 webstorm64.exe.vmoptions 下读取vmoptions，使用下来后者优先级更高，下面就讲讲这个**agent** 

Agent破解：

---

参考[^1]

$$dqdq^2$$


1: qdqwdq  
2: qdqwdqdwqdqd
3: dqdwq
4: qdqdwq

--- 

# 并发学习

Computer Systems: A Programmer's Perspective, 2E

并发：逻辑控制流在时间上重叠 *逻辑控制流*？

构造并发程序的方法：  
* 进程
* IO多路复用 应用程序调度逻辑流。逻辑流被模型化状态机，数据到达文件描述符后，主程序显示地从一个状态切换另一个状态 *怎么理解*?
* 线程 在进程中，由内核调度，混合体，像进程流一样内核调度，像IO多路复用共享一个虚拟地址空间

Posix 线程 Pthreads C 标准接口




# 备忘录，行程，计划，总结 APP？
# 查阅资料 图网站？
# vite 迁移 angular 打包，angular本地打包太卡，占用太多内存，如何处理
# 




