---
layout: post
title: 恢复被覆盖的文件及预防措施
subtitle:
tags: [Linux]
---

> 这是一个由把>错看成>>引发的惨案。一个月黑风高夜，正打算加个环境变量到.zshrc，一个眼花，一个>，一个回车。想要的环境变量加进去了，.zshrc也变得只剩这一行环境变量了。为了找回原内容和避免同样的事情再次发生，开始STFW。

# 恢复被覆盖的文件

## 使用lsof命令

lsof命令可以列出被各种进程打开的文件信息。使用`lsof | grep delete`查找是否有符合条件的文件。若存在符合条件的文件，即可去/proc目录下恢复相应的文件。

## 根据丢失内容的关键词搜索被覆盖文件所在的分区

```shell
$ grep -i -a -B100 -A100 'text in the deleted file' /dev/sda1 > file.txt
# -i 忽略大小写；-a 像处理文本一样处理二进制文件；
# -B100 输出行有text in the deleted file的行，以及该行之前的100行
# -A100 输出行有text in the deleted file的行，以及该行之后的100行
```

# 预防措施

1，**及时备份重要数据，避免不必要的麻烦**。

2，关闭输出重定向覆盖的功能。

```shell
# 禁止覆盖重定向至已经存在的文件
$ set -C
# 关闭以上特性
$ set +C

# 若要在-C 特性下，强制使用覆盖重定向，可使用>|，如：
$ echo 123 >| test.txt

```



# 参考资料：

[1] [Can overwritten files be recovered?](https://unix.stackexchange.com/questions/149342/can-overwritten-files-be-recovered)

[2] [zsh: Disable "file exists:" warning with redirection](https://unix.stackexchange.com/questions/212127/zsh-disable-file-exists-warning-with-redirection)

[3] [Prevent overwriting a file with redirection](https://unix.stackexchange.com/questions/483122/prevent-overwriting-a-file-with-redirection)

[4] [Bash Reference Manual: 4.3.1 The Set Builtin](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#The-Set-Builtin)