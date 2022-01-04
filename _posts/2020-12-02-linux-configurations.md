---
layout: post
title: Linux基本配置
subtitle: 
tags: [Linux]
---

##### APT相关

1. 更新命令：apt update && apt -y full-upgrade

   #apt update (更新软件仓库的目录) 

   ######       安装任何软件之前必须先更新目录

   #apt full-upgrade (下载软件并安装)

2. 关于更新源

   cat /etc/apt/sources.list

3. 安装软件

   库安装：apt install package_name

   安装DEB安装包：

   apt install ./name.deb或dekg -i name.deb

4. sudo passwd root 修改root密码

##### RPM相关

1. yum update	# 升级所有包的同时升级软件和系统内核

   yum upgrade	# 只升级所有的包，不升级软件和系统内核

2. 授予sudo权限（centos上，默认情况下组wheel的成员被赋予sudo访问权限）

   usermod -aG wheel mkDlufop

##### 关闭sudo

 1. 打开visudo: sudo visudo

 2. 修改 %sudo   ALL=(ALL:ALL) ALL 为 %sudo   ALL=(ALL:ALL) NOPASSWD:ALL，它的作用是

    Allow members of group sudo to execute any command **without password**。
    
    > 有安全风险，请谨慎使用。


##### 虚拟机辅助工具

1. 虚拟机增强工具： apt -y install open-vm-tools-desktop fuse && reboot


##### 输入法

1. apt install ibus ibus-pinyin

   设置：ibus-setup

2. 微软字体：

   apt install ttf-mscorefonts-installer

   文泉译微米黑:

   apt install fonts-wqy-microhei

3. 

##### 软件安装

1. 办公软件

   apt install libreoffice

   或

   https://www.onlyoffice.com/download-desktop.aspx

   apt install ./onlyoffice desktopeditors amd64,deb

2. 安装java

   add-apt-repository ppa:webupd8team/java
   apt update

   apt install java-common oracle-java8-installer
   apt install oracle-java8-set default

3. 安装zsh

   apt install zsh
   
   chsh -s $(which zsh) # 切换默认shell为zsh
   
   



