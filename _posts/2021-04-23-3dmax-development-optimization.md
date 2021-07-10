---
layout: post
title: 3dmax开发卡
subtitle: 优化3dmax开发卡的一些方法
tags: [3dmax]
---

#### 使用一些释放内存的MAXScript函数

打开MAXScript 侦听器，输入以下函数：

gc()	运行垃圾收集例程

freescenebitmaps()	释放分配给位图的内存

clearundobuffer()	清除撤销缓冲区（就是把撤销操作的缓存删除）