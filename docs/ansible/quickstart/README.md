---
date: 2022-06-23T19:51:55+08:00
author: "Rustle Karl"

title: "Ansible 入门教程"
url:  "posts/ansible/quickstart/README"  # 永久链接
tags: [ "Ansible", "README" ]  # 标签
categories: [ "Ansible 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

> https://docs.ansible.com/ansible/latest/
> https://cn-ansibledoc.readthedocs.io/zh_CN/latest/

- [安装](#安装)
- [配置](#配置)
- [测试主机](#测试主机)
- [批量执行简单命令](#批量执行简单命令)
- [建立节点清单](#建立节点清单)
- [元组](#元组)
- [创建变量](#创建变量)
- [创建剧本](#创建剧本)

## 安装

```shell
conda config --set auto_activate_base true
```

创建虚拟环境：

```shell
conda create -n ansible python=3.10 -y
```

```shell
conda activate ansible
```

通过 pip 安装

```shell
python -m pip install ansible
```

因为依赖 fcntl，不能在 Windows 运行。

## 配置

```shell
mkdir /etc/ansible
```

```shell
vim /etc/ansible/hosts
```

添加管理主机：

```shell
[myvirtualmachines]
192.168.0.10
192.168.0.18
```

## 测试主机

```shell
ansible all --list-hosts
```

为了管理主机，必须上传访问密钥：

```shell
ssh-keygen -t rsa -C "main"
```

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.0.10
```

```shell
ansible all -m ping
```

```json
192.168.0.10 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
192.168.0.18 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

## 批量执行简单命令

```shell
ansible all -a "apt update && apt upgrade -y"

ansible all -a "apt-get -o Acquire::http::proxy='http://192.168.0.12:7890' upgrade -y"
```

## 建立节点清单

清单列出了节点的基本信息。使用清单文件，Ansible 可以通过单个命令管理大量主机。

上面的示例直接对 /etc/ansible/hosts 文件进行了修改。除此之外，可以在其他地方创建清单，然后指定运行。

清单文件支持 ini 或 yaml 格式。

`inventory.yaml`

```yaml
virtualmachines:
  hosts:
    vm01:
      ansible_host: 192.168.75.128
    vm02:
      ansible_host: 192.168.75.129
    vm03:
      ansible_host: 192.168.75.130
```

```shell
vim /etc/ansible/hosts
```

```shell
[myvirtualmachines]
192.168.75.128
192.168.75.129
192.168.75.130
```

```shell
ssh-keygen -t rsa -C "main"
```

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.75.128
```

验证清单文件：

```shell
ansible-inventory -i inventory.yaml --list
```

网络测试：

```shell
ansible virtualmachines -m ping -i inventory.yaml
```

## 元组

使用以下语法创建一个元组，用于组织清单中的多个组：

```yaml
metagroupname:
  children:
```

示例：

```yaml
leafs:
  hosts:
    leaf01:
      ansible_host: 192.0.2.100
    leaf02:
      ansible_host: 192.0.2.110

spines:
  hosts:
    spine01:
      ansible_host: 192.0.2.120
    spine02:
      ansible_host: 192.0.2.130

network:
  children:
    leafs:
    spines:

webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
    webserver02:
      ansible_host: 192.0.2.150

datacenter:
  children:
    network:
    webservers:
```

## 创建变量

变量为托管节点设置值，例如 IP 地址、FQDN、操作系统和 SSH 用户，因此您在运行 Ansible 命令时不需要传递它们。

```yaml
webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
      http_port: 80
    webserver02:
      ansible_host: 192.0.2.150
      http_port: 443
```

可以创建组变量：

```yaml
webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
      http_port: 80
    webserver02:
      ansible_host: 192.0.2.150
      http_port: 443
  vars:
    ansible_user: my_server_user
```

## 创建剧本

剧本记录了自动化操作的过程，可用于部署服务或者配置主机。

比如：

```yml
- name: My first play
  hosts: virtualmachines
  tasks:
   - name: Ping my hosts
     ansible.builtin.ping:
   - name: Print message
     ansible.builtin.debug:
       msg: Hello world
```

```shell
ansible-playbook -i inventory.yaml playbook.yaml
```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```
