---
date: 2022-06-24T11:10:32+08:00
author: "Rustle Karl"

title: "临时命令 (ad hoc commands) 简介"
url:  "posts/ansible/quickstart/intro_adhoc"  # 永久链接
tags: [ "Ansible", "README" ]  # 标签
categories: [ "Ansible 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

> https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html

临时命令用于不需要经常重复、命令简单的情况下。比如重启服务器。

## 常用示例

### 重启服务器

```shell
ansible all -a "/sbin/reboot"
```

默认情况下，Ansible 仅使用 5 个并发进程。如果您的主机数量多于设置的值，Ansible 会与它们通信，但需要更长的时间。用 10 个并行重启 [all] 服务器：

```shell
ansible all -a "/sbin/reboot" -f 10
```

重启可能需要管理员权限。官方文档有说明，暂略。

### 管理文档

#### 复制

```shell
ansible all -m ansible.builtin.copy -a "src=/etc/hosts dest=/tmp/hosts"
```

#### 改变文件所有权

```shell
ansible all -m ansible.builtin.file -a "dest=/tmp/hosts mode=600"
```

```shell
ansible all -m ansible.builtin.file -a "dest=/tmp/hosts mode=600 owner=develop group=develop"
```

#### 递归删除文件夹

```shell
ansible all -m ansible.builtin.file -a "dest=/path/to/directory state=absent"
```

### 包管理

#### 安装包

```shell
ansible fedora -m ansible.builtin.yum -a "name=acme state=present"
```

指定版本号：

```shell
ansible fedora -m ansible.builtin.yum -a "name=acme-1.5 state=present"
```

最新版：

```shell
ansible webservers -m ansible.builtin.yum -a "name=acme state=latest"
```

### 管理服务


```shell
ansible webservers -m ansible.builtin.service -a "name=httpd state=started"
```

```shell
ansible webservers -m ansible.builtin.service -a "name=httpd state=restarted"
```

```shell
ansible webservers -m ansible.builtin.service -a "name=httpd state=stopped"
```

## 二级

### 三级


```shell

```

```shell

```


## 二级

### 三级

```shell

```

```shell

```

```shell

```
