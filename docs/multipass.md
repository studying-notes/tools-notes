---
date: 2022-06-26T09:50:29+08:00
author: "Rustle Karl"

title: "multipass 轻量级虚拟机管理"
url:  "posts/tools/docs/multipass"  # 永久链接
tags: [ "Tools", "README" ]  # 标签
categories: [ "Tools 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 安装

```bash
snap install multipass
```

安装完成稍微等一下，服务启动没这么快

## 查询可用虚拟机列表

```bash
multipass find
```

目前支持的镜像只限于 Ubuntu。

## 创建虚拟机

默认是稳定版的 Ubuntu LTS：

```bash
multipass launch --name ubuntu-vm01
```

可以指定版本：

```bash
multipass launch --name ubuntu-vm01 22.04
```

指定其他硬件配置：

```bash
multipass launch -n vm01 -c 2 -m 1G -d 20G 22.04
```

- -n, --name: 名称
- -c, --cpus: cpu核心数, 默认: 1
- -m, --mem: 内存大小, 默认: 1G
- -d, --disk: 硬盘大小, 默认: 5G

## 查询虚拟机版本

```bash
multipass exec ubuntu-vm01 -- lsb_release -a
```

## 虚拟机列表

```bash
multipass list
```

## 基本操作

### 查询虚拟机信息

```bash
multipass info
```

### 进入虚拟机

```bash
multipass shell ubuntu-vm01
```

root

```bash
multipass exec k3s-master-01 -- sudo bash
```

### 从外部执行命令

```bash
multipass exec vm01 pwd
```

### 停止

```bash
multipass stop ubuntu-vm01
```

### 启动

```bash
multipass start ubuntu-vm01
```

### 删除

```bash
multipass delete ubuntu-vm01
```

### 清理

删除只是标记删除，list 还是能看到，必须清理后才能删除。

```bash
multipass purge
```

### 挂载数据卷

```bash
# 挂载格式
multipass mount 宿主机目录  实例名:虚拟机目录
```

```bash
multipass mount /root/projects k3s-master-01:/root/projects
```

### 卸载数据卷

```bash
#卸载数据卷
multipass umount 容器名
```

### 传输文件

```bash
multipass transfer 主机文件 容器名:容器目录
```

## 容器配置自动化

为了保持开发环境和线上环境一致性 同时节省部署时间 multipass 给我们提供了 --cloud-init 选项进行容器启动初始化配置:

```bash
multipass launch --name lean --disk 2G --mem 256M --cloud-init lean.yaml 22.04
```

lean.yaml

```yaml
#cloud-config
runcmd:
  - curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
  - sudo apt-get install -y nodejs
  - wget https://releases.leanapp.cn/leancloud/lean-cli/releases/download/v0.21.0/lean-cli-x64.deb
  - sudo dpkg -i lean-cli-x64.deb
```

runcmd 可以指定该虚拟机**首次启动**时运行的命令，这里我们复制了之前安装 Node.js 的命令，还加上了安装 lean-cli 的命令（我们通过 lean-cli 将代码部署到云平台）。

容器初始化配置文件遵循 [cloud-init 标准](https://cloudinit.readthedocs.io/en/latest/topics/examples.html)，可以通过 yaml 文件进行用户、文件、软件仓库、 DNS 解析、SSH 密钥、puppet、chef 等各种初始化配置。

凡是用户自定义的 cloud-init 的配置文件,必须以 `#cloud-config` 开头，这是 cloud-init 识别它的方式。

## 全部停止

```bash
multipass stop --all
```

## 全部开始

```bash
multipass start --all
```

## 全部删除

```bash
multipass delete --all
```

```bash
multipass purge
```

```bash

```

## 改变硬件
