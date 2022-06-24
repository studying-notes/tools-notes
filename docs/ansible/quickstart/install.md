---
date: 2022-06-24T09:35:02+08:00
author: "Rustle Karl"

title: "安装指南"
url:  "posts/ansible/quickstart/install"  # 永久链接
tags: [ "Ansible", "README" ]  # 标签
categories: [ "Ansible 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 查询 pip 版本

```shell
python3 -m pip -V
```

```shell
pip -V
```

确保已经在想安装的 Python 环境中了。


## 安装

```shell
python3 -m pip install --user ansible
```

查询版本：

```shell
ansible --version
```

https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#windows-faq-ansible

Ansible 官方明确无法安装在原生 WIndows 上。

## 配置

### 配置文件位置

1. 指定 ANSIBLE_CONFIG 环境变量
2. 当前目录下的 ansible.cfg
3. ~/.ansible.cfg
4. /etc/ansible/ansible.cfg

```shell

```

```shell

```
