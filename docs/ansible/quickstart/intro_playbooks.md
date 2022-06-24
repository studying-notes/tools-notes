---
date: 2022-06-24T13:22:45+08:00
author: "Rustle Karl"

title: "Playbooks 简介"
url:  "posts/ansible/quickstart/intro_playbooks"  # 永久链接
tags: [ "Ansible", "README" ]  # 标签
categories: [ "Ansible 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 示例

```yaml
- name: Update web servers
  hosts: webservers
  remote_user: root

  tasks:
  - name: Ensure apache is at the latest version
    ansible.builtin.yum:
      name: httpd
      state: latest
  - name: Write the apache config file
    ansible.builtin.template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf

- name: Update db servers
  hosts: databases
  remote_user: root

  tasks:
  - name: Ensure postgresql is at the latest version
    ansible.builtin.yum:
      name: postgresql
      state: latest
  - name: Ensure that postgresql is started
    ansible.builtin.service:
      name: postgresql
      state: started
```

## 执行

```shell
ansible-playbook playbook.yml -f 10
```
