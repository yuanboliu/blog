---
layout:     post
title:      Hadoop 从入门到放弃-part 2
subtitle:   part-2
date:       2016-04-21 10:20:00
author:     Yuanbo
header-img: img/post-bg-unix-linux.jpg
tags:
    - Learning Hadoop
---

# 正文

> "工欲善其事， 必先利其器"

Hadoop HDFS项目首先是一个Java Maven项目， 其次才是Hadoop项目， 那么花些时间了解一下Maven是很有必要的。本文主要聚焦于Maven modules之间的依赖关系。Maven之间依赖主要依靠以下两种方式实现
 
- 在子module中声明parent，即常见的parent标签
- 在父模块中声明modules，即常见的modules标签

这两者有什么区别呢，第一点关于parent声明，可以实现子模块继承父模块的相关属性，并且在子模块中还能重写部分属性值。第二点声明modules，则在子模块间有依赖时，可以确定编译顺序，并且在子模块A依赖于B的时候，解决A找不到B的问题。默认情况下，parent声明和modules声明保持一致。但是在一些复杂的组织下，这两者并不完全等价。

我们来看看Hadoop的组织架构
![image](/blog/img/in-post/post-start-hadoop/module-dependency.png)

（注：此项目结构是part-1实现需要用到的结构，并不是完整的Hadoop项目结构）

是不是觉得上面的结构很绕？一个简单理解的方式是，module定义的是项目的目录结构以及编译顺序，parent定义的是属性依次加载顺序（后者覆盖前者，如dependency，property，build等属性）。我们从module的角度来看，项目结构如下
```
    hadoop-main
    └ hadoop-project
    └ hadoop-project-dist
    └ hadoop-hdfs-project
        └ hadoop-hdfs
```
从parent角度（属性继承链）来看，此项目的结构如下：
```
    hadoop-main
    └ hadoop-project
      └ hadoop-project-dist
        └ hadoop-hdfs
      └ hadoop-hdfs-project
        
```
这样我们就大致了解了Hadoop项目模块之间的关系。准备提交一个commit