---
layout: post
title: Shell中常用的工具
subtitle: Shell名词解释 & find grep..
tags: [Shell]
---

#### Shell

- **Shell 是操作系统内核与用户之间交互的一个接口。[Most operating system shells fall into one of two categories – command-line and graphical. Command-line shells provide a command-line interface (CLI) to the operating system, while graphical shells provide a graphical user interface (GUI). Other possibilities, although not so common, include a voice user interface and various implementations of a text-based user interface (TUI) that are not CLI. The relative merits of CLI- and GUI-based shells are often debated.](https://en.wikipedia.org/wiki/Shell_(computing)#Command-line_shells:~:text=Most%20operating%20system%20shells%20fall,based%20shells%20are%20often%20debated.)**

- **[常见的 Command-line shells 有：sh、bash、dash、zsh、fish。常见的 Graphical shells 有：微软Windows操作系统的Windows shell（Windows shell 提供桌面环境、开始菜单和任务栏）。](https://en.wikipedia.org/wiki/Shell_(computing)#Command-line_shells:~:text=.%5B11%5D-,Microsoft%20Windows,-%5Bedit%5D)**

- **[传统意义上的 Shell 指的是命令行式的 Shell（即 Command-line shells ）。](https://baike.baidu.com/item/shell/99702#:~:text=%E4%BC%A0%E7%BB%9F%E6%84%8F%E4%B9%89%E4%B8%8A%E7%9A%84shell%E6%8C%87%E7%9A%84%E6%98%AF%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%BC%8F%E7%9A%84shell)[命令行式的 Shell 是用一个叫 command interpreter（命令解析器）的程序来实现 CLI 。](https://en.wikipedia.org/wiki/List_of_command-line_interpreters#:~:text=In%20computing%2C%20a%20command%2Dline%20interpreter%2C%20or%20command%20language%20interpreter%2C%20is%20a%20blanket%20term%20for%20a%20certain%20class%20of%20programs%20designed%20to%20read%20lines%20of%20text%20entered%20by%20a%20user%2C%20thus%20implementing%20a%20command%2Dline%20interface.)Shell既是一种命令语言，又是一种程序设计语言。**

> 只要我们能想到的，就一定有方便的办法能够办到。因此当你想要完成某件事却又不知道应该做什么的时候，请向 Google 求助。

> [**重要的是你要知道有些问题使用合适的工具就会迎刃而解，而具体选择哪个工具则不是那么重要。**](https://missing-semester-cn.github.io/2020/shell-tools/#:~:text=%E9%87%8D%E8%A6%81%E7%9A%84%E6%98%AF%E4%BD%A0%E8%A6%81%E7%9F%A5%E9%81%93%E6%9C%89%E4%BA%9B%E9%97%AE%E9%A2%98%E4%BD%BF%E7%94%A8%E5%90%88%E9%80%82%E7%9A%84%E5%B7%A5%E5%85%B7%E5%B0%B1%E4%BC%9A%E8%BF%8E%E5%88%83%E8%80%8C%E8%A7%A3%EF%BC%8C%E8%80%8C%E5%85%B7%E4%BD%93%E9%80%89%E6%8B%A9%E5%93%AA%E4%B8%AA%E5%B7%A5%E5%85%B7%E5%88%99%E4%B8%8D%E6%98%AF%E9%82%A3%E4%B9%88%E9%87%8D%E8%A6%81%E3%80%82)



#### find

```shell
# 查找所有名称为 src 的文件夹(不区分大小写使用 -iname )
find . -name src -type d
# 查找所有文件夹路径中包含 test 的 python 文件
find . -path '*/test/*.py' -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在 500k 至 10M 的 tar.gz 文件
find . -size +500k -size -10M -name '*.tar.gz'

# 删除全部扩展名为 .tmp 的文件
find . -name '*.tmp' -exec rm {} \;
# 查找全部的 PNG 文件并将其转换为 JPG
find . -name '*.png' -exec convert {} {}.jpg \;

# 递归地查找文件夹中所有的 HTML 文件，并将它们压缩成 zip 文件, -d '\n' 保证文件名中有空格时命令也可以正确执行
# xargs 的 -d 选项默认使用空格来切分数据，-d '\n' 表明使用换行符来切分数据
find . -type f -name "*.html" | xargs -d '\n'  tar -cvzf html.zip
# 递归的查找文件夹中最近使用的文件，按照最近的使用时间列出文件
find . -type f -mmin -60 -print0 | xargs -0 ls -lt | head -10
```

替代品：[fd](https://github.com/sharkdp/fd)  [locate vs find](https://unix.stackexchange.com/questions/60205/locate-vs-find-usage-pros-and-cons-of-each-other)

#### grep

```shell
# 输出匹配结果的前后 5 行
grep -C 5
```

替代品：[rg](https://github.com/BurntSushi/ripgrep)

#### 查找Shell命令

敲 Ctrl + R 后输入子串对命令历史记录进行搜索，反复按下 Ctrl + R 会在所有搜索结果中循环。在 zsh 中可以使用上下方向键完成这项工作。

如果在命令的开头加上一个空格，它就不会被加进 shell 记录中。这在输入包含密码或敏感信息的命令时很有用。完成这个功能需要在 .bashrc 中添加 HISTCONTROL=ignorespace 或者向 .zshrc 添加 setopt HIST_IGNORE_SPACE 。如果忘记加空格，可以通过编辑 bash_history 或 .zhistory 手动删除那一项。

#### 文件夹导航

[fasd](https://github.com/clvv/fasd)  [autojump](https://github.com/wting/autojump)

#### 概览目录结构

[tree](https://linux.die.net/man/1/tree)  [broot](https://github.com/Canop/broot) 

#### 文件管理器

[nnn](https://github.com/jarun/nnn)  [ranger](https://github.com/ranger/ranger)

#### 通知工具

notify-send # 使用当前桌面环境的通知系统创建一条通知

#### 查看日志

```shell
# 显示此次开机后 systemd 的所有日志
journalctl -b
```

```shell
# 显示内核的日志
dmesg
```



#### 参考资料：

[1] [MIssing-semester：Shell 工具和脚本](https://missing-semester-cn.github.io/2020/shell-tools/)

