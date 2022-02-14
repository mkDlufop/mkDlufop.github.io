---
layout: post
title: /dev/sda*:clean,*/*files,*/*blocks
subtitle: 
tags: [Linux]

---

### 前言

系统版本：Ubuntu 20.04.3 LTS

系统睡眠后进入登陆页面输入密码登陆，密码输入框无法选中，导致密码无法输入，随后多次强制重启。多次强制重启后，系统还是处于黑屏状态，最上面有一行：/dev/sda\*:clean, */\*files, */\*blocks	(以上\*为数字)。所幸可以通过ctrl+alt+f1~f8进入到tty界面，只是无法进入GUI界面。

### 解决方案

通过查询资料总结了以下三种方案。

方案一：重装Nvidia驱动。

方案二：清理存储空间。

**方案三：重装GUI。**

```shell
1，sudo apt install --reinstall ubuntu-desktop

2，reboot
```


由于所用到的物理机无Nvidia显卡且剩余存储空间足够大，故采用方案三。

### 参考资料

[1] https://askubuntu.com/questions/882385/dev-sda1-clean-this-message-appears-after-i-startup-my-laptop-then-it-w

[2] https://askubuntu.com/questions/1198488/dev-sda2-clean-files-blocks
