---
layout: post
title: Linux基本配置
subtitle: 
tags: [环境配置,Linux]
---

* TOC
{:toc}

### 包管理工具（package management utility）

#### Debian系

Debian系Linux使用deb软件包格式。Debian系下常见的包管理工具有dpkg、apt、apt-get。

```shell
##### 1,更新
# 更新可用软件包和版本的列表 并 将所有已安装的软件包升级到其最新可用版本
sudo apt update && sudo apt -y full-upgrade 
# 查看更新源
cat /etc/apt/sources.list

##### 2,安装软件
# 库安装
sudo apt install package_name
# 安装deb安装包
sudo apt install ./name.deb或dekg -i name.deb
```

#### Red Hat系

Red Hat系Linux使用rpm软件包格式。Red Hat系下常见的包管理工具有rpm、yum。

```shell
##### 1,更新
# 升级所有包的同时升级软件和系统内核
sudo yum update
# 只升级所有的包，不升级软件和系统内核
sudo yum upgrade

##### 2,安装软件
# 库安装
sudo yum install package_name
# 安装rpm安装包
sudo rpm -i name.rpm
```

### 权限相关

#### sudo权限

添加用户至 sudo 用户组

`sudo adduser mkdlufop sudo`

> centos上，默认情况下组wheel的成员被赋予sudo访问权限：
>
> `usermod -aG wheel mkDlufop`

#### 关闭sudo

 1. 打开visudo: sudo visudo

 2. 修改 %sudo   ALL=(ALL:ALL) ALL 为 %sudo   ALL=(ALL:ALL) NOPASSWD:ALL，它的作用是

    Allow members of group sudo to execute any command **without password**。
    
    > 有安全风险，请谨慎使用。

> **谨慎使用root用户权限执行命令！**
>
> 一些会对系统带来毁灭性破坏的例子：
>
> - `rm -rf /`（删除系统中的所有可以删除的文件，**包括被挂载的其他分区**。**即使不以 `root` 权限执行，也可以删掉自己的所有文件。**）
> - `mkfs.ext4 /dev/sda`（将系统的第一块硬盘直接格式化为 ext4 文件系统。这会破坏其上所有的文件。）
> - `dd if=/dev/urandom of=/dev/sda`（对系统的第一块硬盘直接写入伪随机数。这会破坏其上所有的文件，并且找回文件的可能性降低。）
> - `:(){ :|: & };:`（被称为「Fork 炸弹」，会消耗系统所有的资源。在未对进程资源作限制的情况下，只能通过重启系统解决，所有未保存的数据会丢失。）

#### 修改root密码

`sudo passwd root `

### 时间同步

systemd 自带了一个 systemd-timesyncd 服务，提供了简单的时间同步服务。

为了更快地同步时间，这里选用了中国 NTP 快速授时服务和中国计量科学研究院 NIM 授时服务的 NTP 服务器，编辑 `/etc/systemd/timesyncd.conf`，添加或编辑以下内容：`NTP=cn.ntp.org.cn ntp1.nim.ac.cn`。重启 systemd-timesyncd.service，之后运行 `timedatectl timesync-status` 便可查看时间同步状态。

### 常用软件

**办公软件**

`apt install libreoffice` 或

https://www.onlyoffice.com/download-desktop.aspx

`apt install ./onlyoffice desktopeditors amd64,deb`

**安装java**

```shell
add-apt-repository ppa:webupd8team/java
sudo apt update

apt install java-common oracle-java8-installer
apt install oracle-java8-set default
```

**安装zsh**

```shell
sudo apt install zsh

chsh -s $(which zsh) # 切换默认shell为zsh
```

**输入法**

```shell
# 安装ibus
sudo apt install ibus ibus-pinyin
# 设置 ibus
ibus-setup
```

> 添加中文拼音输入法（ Intelligent Pinyin input method for IBus ）(该方法成功实践于 Ubuntu 系统)
>
> 1. 在设置中添加中文语言支持：
>
>     Settings -> Region & Language -> Manage Installed Languages -> Installed / Remove Languages -> 勾选 Chinese (simplified) -> Apply
>
> 2. 更新系统，确保下载完整语言包：`sudo apt update && sudo apt upgrade`
>
> 3. 重启：`reboot`
>
> 4. 添加中文拼音输入法：
>
>     Settings -> Region & Language -> + -> Chinese -> Chinese (Intelligent Pinyin)

**字体**

```shell
# 微软字体：
sudo apt install ttf-mscorefonts-installer
# 文泉译微米黑:
sudo apt install fonts-wqy-microhei
```

**截图**

[flameshot](https://flameshot.org/#download) : `sudo apt install flameshot`

### 虚拟机相关

**虚拟机辅助工具**

1. 虚拟机增强工具： `sudo apt -y install open-vm-tools-desktop fuse && reboot`

    > 为了让每次开机时共享文件夹自动挂载到指定目录，添加以下内容到 `/etc/fstab` ：
    >
    > `.host:/ /mnt/hgfs   fuse.vmhgfs-fuse    auto,allow_other  0   0`

### 参考资料：
{:.no_toc}

[1] [Linux发行版列表](https://zh.wikipedia.org/wiki/Linux%E5%8F%91%E8%A1%8C%E7%89%88%E5%88%97%E8%A1%A8)

[2] [Linux发行版](https://zh.wikipedia.org/wiki/Linux%E5%8F%91%E8%A1%8C%E7%89%88)

[3] [Linux 101 用户与用户组、文件权限、文件系统层次结构](https://101.lug.ustc.edu.cn/Ch05/#root-user)

[4] [Open VM Tools and Folder Sharing](https://aaronvonawesome.com/posts/open-vm-tools-and-folder-sharing/)