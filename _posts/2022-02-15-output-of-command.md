---
layout: post
title: 以变量的形式获取一个命令的输出
subtitle: 使用命令替换和进程替换
tags: [Shell]
---

#### 命令替换（*command substitution*）

形式：$( cmd )

结果：cmd的输出结果会替换掉 $( cmd )

例子：echo "It is $(uname)" # 输出：It is Linux



#### 进程替换（*process substitution*）

形式：<( cmd )

结果：cmd的输出结果会输出到一个临时文件中，临时文件名为<( cmd )

例子：diff <(ls foo) <(ls bar) # 输出会显示文件夹foo和bar中文件的区别



#### 参考资料：

[1] [MIssing-semester：Shell 工具和脚本](https://missing-semester-cn.github.io/2020/shell-tools/)