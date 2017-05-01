---
layout:     post
title:      Hadoop 源码剖析
subtitle:   前言
date:       2017-04-20 10:20:00
author:     Yuanbo
header-img: img/hadoop-bed-time.jpg
tags:
    - Hadoop Analysis
---
Hadoop 发展至今已经有十余年的历史，作为一个优秀的开源框架，它有很多值得人们学习和借鉴的地方。但对于初学者来说，其代码研究门槛较高。如何看Hadoop源代码，甚至是给Hadoop项目贡献代码，需要一定的指导，本系列博客旨在解决此类问题。

我的Hadoop之路大概经历了这样一个过程，首先是运维Hadoop集群，然后在Hadoop上层构建对外的服务，最后开始贡献Hadoop源代码，前前后后大概经历了2年多的时间。两年的时间里，我积累了一些Hadoop源码学习的经验，希望通过博客分享给大家，同时也希望把这件事情当做一个锻炼自己的机会，训练自己语言组织以及逻辑思维的能力。

之前也接触过一些Hadoop源码分析的文章，虽然看得不多，但这些文章给我留下的印象是对于一些特性详细的罗列了代码的调用过程。对于有经验的来讲，这样的文章读起来并不费劲，因为他有一些概念是了解的，对于Hadoop设计风格也是有一定积累。但是对于初学者来说，这种文章对于增强Hadoop源码理解是很难有帮助的。我希望开辟这样一条讲解之路：开一个repository，从零搭建Hadoop，每一个commit对应着一篇博文讲解。

这是一个巨大的工程，因此在开始之前需要做一些澄清：
1. Hadoop源码重新构建不依赖于任何一个Hadoop git版本，直接分析trunk
2. 重新构建的repository不完全copy trunk中的代码，按照讲解进度编写
3. 对于git，idea，jira等工具不做介绍，假定读者已经完全掌握
4. 已读过Google 三篇论文，对于Hadoop项目有一个大概的框架


本质上来说，Hadoop项目构建了几种不同角色的常驻进程，这些进程内部包含着各式各样的线程处理不同的事物，进程之间实现了几种类型的通信。在这样一个架构下，一步步添加新的feature进来，最终形成现在的Hadoop项目。重新构建Hadoop，就是要重走这样的构建流程，最终，帮助读者熟悉Hadoop源码，了解其中的“套路”，从而贡献Hadoop源码。


GitHub地址：https://github.com/yuanboliu/hadoop-analysis