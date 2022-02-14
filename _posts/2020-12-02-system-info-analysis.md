---
layout: post
title: 系统信息分析
subtitle: 
tags: [Linux]
---

# 系统信息查看命令

```shell
cat /etc/issue # 查看发行版信息

uname -a # 查看内核/操作系统/CPU信息

head -n 1 /etc/issue # 查看操作系统版本

cat /proc/cpuinfo # 查看CPU信息，CPU的信息在启动的过程中被装载到虚拟目录/proc下的cpuinfo文件中

hostname # 查看计算机名

lspci -tv # 列出所有PCI设备

lsusb -tv # 列出所有USB设备

lsmod # 列出加载的内核模块

env # 查看环境变量
```

[系统性能分析工具集1](https://yoc.docs.t-head.cn/icebook/Chapter2-%E5%8A%9F%E8%83%BD%E6%BC%94%E7%A4%BA/%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/) [2](https://yoc.docs.t-head.cn/icebook/Chapter5-%E5%86%85%E6%A0%B8%E5%BC%80%E5%8F%91%E4%B8%8E%E8%B0%83%E8%AF%95/10-%20Linux%E7%B3%BB%E7%BB%9F%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7%E9%9B%86.html)

### 资源/硬盘占用情况

```shell
free -h # 查看内存信息

free -m # 查看内存使用量和交换区使用量

df -h # 查看各分区使用情况

du -sh <目录名> # 查看指定目录的大小

grep MemTotal /proc/meminfo # 查看内存总量

grep MemFree /proc/meminfo # 查看空闲内存量

uptime # 查看系统运行时间、用户数、负载

cat /proc/loadavg # 查看系统负载
```



### 网络情况查看

```shell
iptables -L # 查看防火墙设置 

route -n # 查看路由表 

netstat -lntp # 查看所有监听端口 

netstat -antp # 查看所有已经建立的连接 

netstat -s # 查看网络统计信息
```



### 磁盘和分区情况

```shell
mount | column -t # 查看挂接的分区状态

fdisk -l # 查看所有分区

swapon -s # 查看所有交换分区

hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备) 

dmesg | grep IDE # 查看启动时IDE设备检测状况
```



### 用户查看

```shell
w # 查看活动用户

id <用户名> # 查看指定用户信息

last # 查看用户登录日志

cut -d: -f1 /etc/passwd # 查看系统所有用户

cut -d: -f1 /etc/group # 查看系统所有组

crontab -l # 查看当前用户的计划任务
```



### 进程查看

```shell
ps -ef # 查看所有进程

top # 实时显示进程状态
```



###　系统服务情况

```shell
chkconfig –list # 列出所有系统服务

chkconfig –list | grep on # 列出所有启动的系统服务
```

