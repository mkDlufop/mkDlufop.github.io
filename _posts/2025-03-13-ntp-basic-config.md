---
layout: post
title: 搭建本地 NTP 服务器
subtitle: NTP 服务器的基本配置及验证
tags: [ntp]
---

## 一、服务端配置

### 1、安装 ntp（服务端）和 ntpdate（客户端）

```bash
yum install ntp ntpdate // 有些版本的 ntp 已包含 ntpdate
```

### 2、修改配置文件：/etc/ntp.conf

```bash
// 允许该网段内设备同步时间
// nomodify：禁止客户端修改服务端配置
// notrap：禁止客户端使用陷阱功能
restrict 192.168.0.0 mask 255.255.0.0 nomodify notrap

server ntp.sjtu.edu.cn // 指定上层时间源

// 设置本地时间作为时间源
// stratum 设置为 10，表示该时间源的优先级较低
server 127.127.1.0
fudge 127.127.1.0 stratum 10
```

### 3、防火墙配置

```bash
// ntp 服务使用 UDP 123 端口
// 放行 UDP 123 端口
firewall-cmd --add-port=123/udp --permanent
firewall-cmd --reload
```

### 4、启动和验证

```bash
systemctl start ntpd // 启动
systemctl enable ntpd // 开机自启动
systemctl status ntpd // 检查状态
netstat -tulnp | grep ntp // 确认123端口监听
ntpstat // 检查 ntpd 服务的运行状态
```

> 注意：ntpdate 和 ntp 服务都可以用来从外部时间服务器同步时间，但 ntpdate 会造成时间的突变。
> ntpdate 和 ntp 服务都使用的 123 端口。
> ntp 采用平滑的方式来同步时间，但缺点就是慢，而且对于超过 17 分钟的时间间隔会拒绝更新。
> 所以从外部时间服务器同步时间时，一般先停掉ntpd服务（systemctl stop ntpd），再用 ntpdate 将时间做一次
> 突变校准（ntpdate 0.centos.pool.ntp.org），最后重新启动 ntpd 服务。

## 二、客户端配置

### 1、安装 ntpdate（客户端）

```bash
yum install ntp ntpdate // 有些版本的 ntp 已包含 ntpdate
```

### 2、客户端同步时间

```bash
ntpdate 服务端IP // 同步时间，加上 -u 可以绕过防火墙
hwclock -w // 将系统时间写入硬件时间
```

### 3、验证

```bash
ntpq -pn // 验证客户端时间同步
ntpdate -q 服务端IP // 查看与时钟源的差异
```
