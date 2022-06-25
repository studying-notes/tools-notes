---
date: 2022-06-23T23:12:07+08:00
author: "Rustle Karl"

title: "VMware 问题"
url:  "posts/vmware/docs/question"  # 永久链接
tags: [ "Vmware", "README" ]  # 标签
categories: [ "Vmware 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 无法联网

https://blog.csdn.net/qq_36408196/article/details/103390303

这篇文章列举了五种解决方案，一一尝试，然而还是无法联网。

https://blog.csdn.net/dong__ge/article/details/123581117

根这篇文件里的第一张方法一样，但是还是无用，找不到网络管理程序。

最后放弃，重装虚拟机。

## 宿主机无法与虚拟机互联

将 Windows的 VMnet8 网络接口 IP 改成与虚拟机 IP 相同网段。
