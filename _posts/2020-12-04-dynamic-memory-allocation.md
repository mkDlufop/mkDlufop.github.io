---
layout: post
title: 动态内存分配
subtitle: 
tags: [c]
---

#### malloc

> #include<stdlib.h>
>
> void* malloc(size_t size)

+ 向malloc申请的空间大小是以字节为大小的
+ 返回的结果是void*，需要类型转换为自己需要的类型
+ (int*)malloc(n\*sizeof(int))

#### free()

+ 把申请得来的空间还给“系统”
+ 申请过的空间，最终都应该要还
+ 只能还申请来的空间的首地址