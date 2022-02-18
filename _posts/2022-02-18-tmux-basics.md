---
layout: post
title: tmux的基本使用
subtitle:
tags: [tmux]
---

### 常用功能

- **会话** - 每个会话都是一个独立的工作区，其中包含一个或多个窗口。

  - `tmux` 开始一个新的会话

  - `tmux new -s NAME` 以指定名称开始一个新的会话

  - `tmux ls` 列出当前所有会话

  - 在 `tmux` 中输入 `<C-b> d` ，将当前会话分离

  - `tmux a` 重新连接最后一个会话。您也可以通过 `-t` 来指定具体的会话
  
- **窗口** - 相当于编辑器或是浏览器中的标签页，从视觉上将一个会话分割为多个部分。

  - `<C-b> c` 创建一个新的窗口，使用 `<C-d>`关闭
  - `<C-b> N` 跳转到第 *N* 个窗口，注意每个窗口都是有编号的
  - `<C-b> p` 切换到前一个窗口
  - `<C-b> n` 切换到下一个窗口
  - `<C-b> ,` 重命名当前窗口
  - `<C-b> w` 列出当前所有窗口

- **面板**\- 像 vim 中的分屏一样，面板使我们可以在一个屏幕里显示多个 shell。

  - `<C-b> "` 水平分割
  - `<C-b> %` 垂直分割
  - `<C-b> <方向>` 切换到指定方向的面板，<方向> 指的是键盘上的方向键
  - `<C-b> z` 切换当前面板的缩放
  - `<C-b> [` 开始往回卷动屏幕。您可以按下空格键来开始选择，回车键复制选中的部分
  - `<C-b> <空格>` 在不同的面板排布间切换



参考资料：

[1] [tmux the terminal multiplexer Cheat Sheet](https://cheatography.com/bechtold/cheat-sheets/tmux-the-terminal-multiplexer/)

[2] [MIssing-semester：命令行环境](https://missing-semester-cn.github.io/2020/command-line/)

​			