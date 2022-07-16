---
date: 2022-07-13T09:51:22+08:00
author: "Rustle Karl"

title: "vagrant 虚拟机自动化管理工具"
url:  "posts/tools/docs/vagrant/README"  # 永久链接
tags: [ "Tools", "README" ]  # 标签
categories: [ "Tools 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 安装

https://www.vagrantup.com/downloads

### Windows

```bash
choco install vagrant
```

### Ubuntu

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

```bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

```bash
sudo apt update && sudo apt install -y vagrant
```

## 常用虚拟机

```bash
choco install virtualbox
```

```bash
apt install -y virtualbox
```

```bash

```

## 基础操作

### 搜索镜像

```bash
vagrant cloud search ubuntu
```

### 添加镜像

```bash
vagrant box add hashicorp/bionic64
vagrant box add centos/7
vagrant box add bento/ubuntu-22.04
vagrant box add generic/fedora36
vagrant box add ubuntu/focal64
vagrant box add ubuntu/jammy64
```

### 列出镜像

```bash
vagrant box list
```

### 列出过期镜像

```bash
vagrant box outdated
```

### 清理过期镜像

```bash
vagrant box prune
```

### 删除镜像

```bash
vagrant box remove NAME
```

### 更新镜像

```bash
vagrant box update
```

```bash

```

```bash

```

## KVM 插件

```bash
vagrant plugin install vagrant-libvirt
```

这个有很多问题，统一用 virtualbox 即可。

在 Windows 下也会出现各种问题，还是 Linux 最好。

## 示例

```bash
mkdir nomad-demo
```

```bash
cd nomad-demo
```

```bash
curl -O https://raw.githubusercontent.com/hashicorp/nomad/master/demo/vagrant/Vagrantfile
```

添加网络代理。

```bash
vagrant up --provider virtualbox
```

```bash
vagrant ssh
```


## 二级

### 三级

```bash

```

```bash

```


## 二级

### 三级

```bash

```

```bash

```


## 二级

### 三级

```bash

```

```bash

```


