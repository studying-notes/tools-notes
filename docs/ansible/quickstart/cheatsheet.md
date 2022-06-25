---
date: 2022-06-24T13:41:55+08:00
author: "Rustle Karl"

title: "Ansible 小抄"
url:  "posts/ansible/quickstart/cheatsheet"  # 永久链接
tags: [ "Ansible", "README" ]  # 标签
categories: [ "Ansible 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

> https://docs.ansible.com/ansible/latest/user_guide/cheatsheet.html#ansible-playbook

## 执行剧本

```shell
ansible-playbook -i /path/to/my_inventory_file -u my_connection_user -k /path/to/my_ssh_key -f 3 -T 30 -t my_tag -m /path/to/my_modules -b -K my_playbook.yml
```

- -i 清单路径
- -u 用户
- -k 密钥
- -f 并行数量
- -T 超时
- -t 标签过滤
- -m 导入本地模块
- -b 管理员权限执行
- -K 交互输入密码

## 二级

### 三级

```shell

```

```ini

```

```yaml

```
