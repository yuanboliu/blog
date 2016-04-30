---
layout:     post
title:      Hadoop 从入门到放弃
subtitle:   part-1
date:       2016-04-21 10:20:00
author:     Yuanbo
header-img: img/post-bg-unix-linux.jpg
tags:
    - Learning Hadoop
    - Under blogging
---

# 前言
Hadoop 发展至今已有10多年, 积累了几十万行代码以及浩瀚的文档. 这给初学者带来一个很严重的问题, 代码级别贡献的准入门槛会变得非常高. 本系列文章《Hadoop 从入门到放弃》主要探讨如何能更简单地了解Hadoop源码的发展历程, 更好地理解Hadoop架构.个人认为, 最好的学习方式莫过于分享自己的学习经验, 这样能不断逼迫自己深入研究.  

我从事Hadoop相关工作, 目前主要focus在Hadoop HDFS上. 给自己预设的目标是尽可能参与到Apache Hadoop的开发上来, 为开源社区做出自己的一份贡献. 但是摆在我眼前的一道难题就是, Hadoop源码太多, 初学者基本上很难找到切入点, 这大大削弱了我的参与热情. 于是我给自己找到另外一条学习Hadoop之路, 就从Hadoop HDFS的起点开始研究, 按照Hadoop HDFS的roadmap重新实现. 这必然是一件比较漫长的路, 不过站在巨人的肩膀上, 我相信我会走得很快.

# 正文
Hadoop起源于Google的三篇论文, 首先我们简单回顾一下关于GFS的那篇论文, 并提取其中的一些关键设计点.
  
  - 组件失效被认为是常态, 这里的组件应该包含硬件故障以及软件故障
  - 处理的数据量非常大, 超过单台机器的处理能力
  
第一点, 软件以及硬件故障不是特例, 而是常态. 这就要求系统具有很强的容错性. 第二点, 数据量超大, 这就要求系统应该具有一定的分布式结构, 具有扩展性, 对数据的处理(增删改查), 应当具有原子性, 一致性等特点. 那么, GFS是如何解决上述问题的呢? 一个典型的分布式系统一般由一台master节点和一系列worker节点组成. master节点一般保存元数据, 并协调worker节点共同工作. worker节点负责具体工作内容. 具体到一个能做数据读写的GFS, 其场景架构如下:

![image](/img/post-start-hadoop/read-scenario.png)
![image](/img/post-start-hadoop/write-scenario.png)