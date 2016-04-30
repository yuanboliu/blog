---
layout:     post
title:      Hadoop 从入门到放弃
subtitle:   part-1
date:       2016-04-21 10:20:00
author:     Yuanbo
header-img: img/post-bg-unix-linux.jpg
tags:
    - Learning Hadoop
---

# 前言
Hadoop 发展至今已有10多年, 积累了几十万行代码以及浩瀚的文档. 这给初学者带来一个很严重的问题, 代码级别贡献的准入门槛会变得非常高. 本系列文章《Hadoop 从入门到放弃》主要探讨如何能更简单地了解Hadoop源码的发展历程, 更好地理解Hadoop架构.个人认为, 最好的学习方式莫过于分享自己的学习经验, 这样能不断逼迫自己深入研究.  

我从事Hadoop相关工作, 目前主要focus在Hadoop HDFS上. 给自己预设的目标是尽可能参与到Apache Hadoop的开发上来, 为开源社区做出自己的一份贡献. 但是摆在我眼前的一道难题就是, Hadoop源码太多, 初学者基本上很难找到切入点, 这大大削弱了我的参与热情. 于是我给自己找到另外一条学习Hadoop之路, 就从Hadoop HDFS的起点开始研究, 按照Hadoop HDFS的roadmap重新实现. 这必然是一件比较漫长的路, 不过站在巨人的肩膀上, 我相信我会走得很快.

# 正文
Hadoop起源于Google的三篇论文, 首先我们简单回顾一下关于GFS的那篇论文, 并提取其中的一些关键设计点.
  
  - 组件失效被认为是常态, 这里的组件失效应该包含硬件故障以及软件故障
  - 处理的数据量非常大, 超过单台机器的处理能力
  
第一点, 软件以及硬件故障不是特例, 而是常态. 这就要求系统具有很强的容错性. 第二点, 数据量超大, 这就要求系统应该具有一定的分布式结构, 具有扩展性, 对数据的处理(增删改查), 应当具有原子性, 一致性等特点. 那么, GFS是如何解决上述问题的呢? 一个典型的分布式系统一般由一台master节点和一系列worker节点组成. master节点一般保存元数据, 并协调worker节点共同工作. worker节点负责具体工作内容. 具体到一个能做数据读写的GFS, 其场景架构如下:

![image](/blog/img/in-post/post-start-hadoop/read-scenario.png)
![image](/blog/img/in-post/post-start-hadoop/write-scenario.png)

此图着重表示机器之间的行为模式, 究其实现细节, 会有很内容, 该论文也有相关技术细节的描述. 在本文中, 我不做展开讨论, 我认为自上而下的探讨有利于我们去繁取简, 方便理解与逐步实现. 单看上图, 我们知道, 需要实现的功能如下:

  - 两种常驻进程, master 进程与 worker 进程
  - 有client端与master端交互, client端与worker端交互, master与client端交互(图中遗漏), 分布式一般使用RPC技术完成远程进程间通讯.
  
Hadoop HDFS源自GFS这篇论文, 我们以此为切入点, 开始还原HDFS的实现历程, 最终达到深入理解HDFS源码的目的

# 后序
我新建了一个仓库, 名为[hadoop-yuanbo](https://github.com/yuanboliu/hadoop-yuanbo), 此仓库主要用于从Apache Hadoop主干上back port源码, 以及添加一部分自己的实现. 大型项目的实现都不是一蹴而就的, 我相信这种自上而下, 由简而繁地重走Apache Hadoop历程, 能快速帮助我理解Hadoop源码, 更快地参与到Apache Hadoop开源社区的贡献上来.