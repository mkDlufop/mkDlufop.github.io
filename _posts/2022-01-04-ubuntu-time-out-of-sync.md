---
layout: post
title: 双系统下时间显示不对（Windows+Linux）
subtitle: 
tags: [Linux]
---

### 前置知识

Windows与类Unix系统默认的时间管理方式：

**Windows**：本地时间（local time）=计算机硬件时间=BIOS中显示的时间。

**类Unix系统**：本地时间（local time）=计算机硬件时间+系统设置的时区数=BIOS时间+系统设置的时区数。类Unix系统会把BIOS时间当成UTC，所以待系统启动后，系统显示的时间为BIOS时间+系统设置的时区数。

### 双系统下时间显示不对的原因

Windows与类Unix系统默认的时间管理方式不同。假设现在是13点，Ubuntu设置的时区在中国，那么Ubuntu里会显示13点，此时BIOS里的时间是5点。然后切换到Windows，会发现时间不对。因为Windows以BIOS中显示的时间为本地时间，所以此时Windows中显示的时间为5点。在Windows里同步时间，此时时间会显示为13点，BIOS的时间会被改为13点。之后再切换到Ubuntu下，会发现显示的时间变成了21点。

### 解决方案

方案一：

1，先在Ubuntu下更新时间：

```shell
sudo apt install ntpdate
sudo ntpdate time.windows.com
```

2，将系统时间同步到硬件上：

```shell
sudo hwclock --localtime --systohc // --systohc 系统时钟和硬件时钟同步
```

方案二：

在Ubuntu中把计算机硬件的时间改成系统显示时间，即禁用Ubuntu中的UTC。

```shell
timedatectl set-local-rtc 1 --adjust-system-clock
```

