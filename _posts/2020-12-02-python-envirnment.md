---
layout: post
title: python环境搭建
subtitle: 
tags: [python]
---



一、在Linux上安装
1，安装python3
    方法一：
    （1）安装python3：sudo dnf install python3
     (2)验证python的版本：python3 -V
     (3)若要执行python3，输入（进入python环境之后即可使用pip命令）：python3
     方法二：
     参考：https://m.yisu.com/zixun/16103.html
2，安装python2
    方法同安装python3，只不过要把3改成2
3，设置默认的python版本（不带版本号的python命令）
    如果要将python3设置为系统范围内的python命令，需要使用alternatives工具：
    sudo alternatives --set python /usr/bin/python3
    对于python2，输入：
    sudo alternatives --set python /usr/bin/python2
    alternative创建了一个指向指定python版本的python软链接，
    若要移除不带版本号的python命令，输入：
    sudo alternative --auto python
二、在Windows上安装
    参考：https://www.runoob.com/python3/python3-install.html