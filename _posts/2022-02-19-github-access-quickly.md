---
layout: post
title: github访问加速
subtitle:
tags: [github]
---

### 修改系统Hosts文件

#### 手动方式

1. 修改hosts文件

      各系统hosts文件位置：

   - Windows 系统：`C:\Windows\System32\drivers\etc\hosts`
   - Linux 系统：`/etc/hosts`

      通过[该网址](https://www.ipaddress.com/) 或 **raw.hellogithub.com/hosts** 查找github各域名对应的DNS解析地址。

2. 激活生效

      各系统激活方式：

   - Windows 系统：刷新DNS缓存，在cmd中执行：`ipconfig /flushdns`
   - Linux 系统：`sudo nscd restart` 或 `sudo /etc/init.d/nscd restart`
   - Ubuntu：`sudo /etc/init.d/network-manager restart`
   - CentOS：`sudo /etc/init.d/network restart`

#### 自动方式

使用[SwitchHosts](https://github.com/oldj/SwitchHosts) 工具管理 hosts。

### 参考资料：

[1] [GitHub520](https://github.com/521xueweihan/Github520)

[2] [GitHub中国加速访问 #3](https://github.com/chenxuhua/issues-blog/issues/3)

[3] [github访问加速](https://zhuanlan.zhihu.com/p/75994966?utm_source=wechat_session)