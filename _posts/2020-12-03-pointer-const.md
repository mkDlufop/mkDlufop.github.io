---
layout: post
title: 指针与const
subtitle: 
tags: [c]
---

> 以下仅适用于c99

#### 指针是const

+ 表示一旦得到了某个变量的地址，不能再指向其他变量。

  ```c
  int* const q = &i; // q是const
  ```



#### 所指是const

+ 表示不能通过这个指针去修改那个变量（并不能使得那个变量成为const）

```c
const int *p = &i;
```



> 判断哪个被const了的标志是const在*的前面还是后面，const在\* 后面表示指针是const。
>
> int i;
>
> const int* p1 = &i;
>
> int const* p2 = &i;
>
> int *const p3 = &i;