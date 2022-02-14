---
layout: post
title: 配置SSHD服务与远程登陆
subtitle: 在Linux环境下
tags: [环境配置]
---

### SSH服务端

- 安装openssh-server

```shell
sudo apt install ssh openssh-server
```

- 配置 sshd_config

**vi /etc/ssh/sshd_config**

```
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2
```

- 重启sshd

```shell
systemctl restart sshd
```



### SSH客户端
- 安装openssh-client

```
sudo apt-get install openssh-client
```

- 生成密钥

```shell
ssh-keygen -t rsa
```

其中id_rsa为私钥，id_rsa.pub为公钥。

公钥放在SSH服务端所在机器上，私钥放在SSH客户端所在机器上。通过[SSH免密登陆原理](https://zhuanlan.zhihu.com/p/397692994)可以知道拥有私钥的机器可以免密登陆拥有对应公钥的机器。

- 上传公钥到SSH服务端所在机器

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub username@ip
```

在SSH服务端所在机器上的~/.ssh/authorized_keys中可以看到公钥内容。



### 远程登陆

```shell
ssh username@ip -p port
```



### 参考资料：

[1] [开发板网络配置与 SSHD](https://yoc.docs.t-head.cn/icebook/Chapter1-%E5%87%86%E5%A4%87%E5%B7%A5%E4%BD%9C/5-%E5%BC%80%E5%8F%91%E6%9D%BF%E7%BD%91%E7%BB%9C%E9%85%8D%E7%BD%AE%E4%B8%8ESSH.html)

[2] [https://man.openbsd.org/ssh.1](https://man.openbsd.org/ssh.1)