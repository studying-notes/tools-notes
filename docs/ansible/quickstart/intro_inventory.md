---
date: 2022-06-24T10:15:33+08:00
author: "Rustle Karl"

title: "Inventory 教程"
url:  "posts/ansible/quickstart/intro_inventory"  # 永久链接
tags: [ "Ansible", "README" ]  # 标签
categories: [ "Ansible 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

> https://cn-ansibledoc.readthedocs.io/zh_CN/latest/user_guide/intro_inventory.html
> https://cn-ansibledoc.readthedocs.io/zh_CN/latest/user_guide/intro_dynamic_inventory.html

## 格式

Inventory 最常用的格式是 YAML 和 INI。 比如：

```ini
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

括号中的标题是组名，用于对主机进行分类，用于确定什么时间、什么目的、相对哪些主机做什么事情。

如下为 YAML 格式的等价示例:

```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
```

## 默认组

默认有两个分组： all 和 ungrouped 。 all 组顾名思义包括所有主机。 ungrouped 则是 all 组之外所有主机。所有的主机要不属于 all 组，要不就属于 ungrouped 组。

尽管 all 和 ungrouped 始终存在，但它们以隐式的方式出现，而不出现在诸如 group_names 的组列表中。

## 多主机组

你可以把一台主机放在多个组中。 

```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
    east:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    west:
      hosts:
        bar.example.com:
        three.example.com:
    prod:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    test:
      hosts:
        bar.example.com:
        three.example.com:
```

可以看到 one.example.com 同时存在 dbservers, east, and prod 组中 您还可以使用嵌套组来简化此清单中的 prod and test 组，优化后结果如下：

```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
    east:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    west:
      hosts:
        bar.example.com:
        three.example.com:
    prod:
      children:
        east:
    test:
      children:
        west:
```

## 增加主机段

如果您有许多具有相似模式的主机，则可以将它们添加为一个范围，而不必分别列出每个主机名：

```ini
[webservers]
www[01:50].example.com

[databases]
db-[a:f].example.com
```

```yaml
  webservers:
    hosts:
      www[01:50].example.com:
```

## 变量定义

你可以直接在 Inventory 清单中定义的 host 或 group 变量。刚开始的时候，你可以直接添加 host 或 group 到 Inventory 文件中。当你越加越多的时候，你可能会考虑将变量和 host group 分离成独立的文件。

### 给单台主机设置变量

在 Playbook 中的示例 INI 文件的写法：

```ini
[atlanta]
host1 http_port=80 maxRequestsPerChild=808
host2 http_port=303 maxRequestsPerChild=909
```

```yaml
atlanta:
  host1:
    http_port: 80
    maxRequestsPerChild: 808
  host2:
    http_port: 303
    maxRequestsPerChild: 909
```

比如给 host 添加非标准 SSH 端口，把端口直接添加到主机名后，冒号分隔即可:

```shell
badwolf.example.com:5309
```

connection 连接远程主机的方式：

```ini
[targets]
localhost              ansible_connection=local
other1.example.com     ansible_connection=ssh        ansible_user=myuser
other2.example.com     ansible_connection=ssh        ansible_user=myotheruser
```

如果你在 SSH 配置文件中定义了非标端口，但 ansible 的配置文件里没有指定，使用 openssh 连接主机可以默认读取不用指定特定端口， 但 paramiko 就不能自动发现了。

### 定义别名

```ini
jumper ansible_port=5555 ansible_host=192.0.2.50
```

```yaml
  hosts:
    jumper:
      ansible_port: 5555
      ansible_host: 192.0.2.50
```

在如上的示例中， 执行 Ansible 对 “”jumper”” 主机执行命令时，会连接 192.0.2.50 的 5555 端口。 这种方式仅适用于通过静态 IP 的主机，或者通过隧道连接的主机。

### 给多台主机设置变量

如果组中的所有主机共享一个变量值，则可以一次将该变量应用于整个组：

```ini
[atlanta]
host1
host2

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com
```

```yaml
atlanta:
  hosts:
    host1:
    host2:
  vars:
    ntp_server: ntp.atlanta.example.com
    proxy: proxy.atlanta.example.com
```

### 继承变量值：嵌套组的组变量设置

您可以设置一个 children 的组变量， `:children` INI 格式或 `children:` YAML 格式， 您可以分别使用 `:vars` or `vars:` 给组定义变量

```ini
[atlanta]
host1
host2

[raleigh]
host2
host3

[southeast:children]
atlanta
raleigh

[southeast:vars]
some_server=foo.southeast.example.com
halon_system_timeout=30
self_destruct_countdown=60
escape_pods=2

[usa:children]
southeast
northeast
southwest
northwest
```

```yaml
all:
  children:
    usa:
      children:
        southeast:
          children:
            atlanta:
              hosts:
                host1:
                host2:
            raleigh:
              hosts:
                host2:
                host3:
          vars:
            some_server: foo.southeast.example.com
            halon_system_timeout: 30
            self_destruct_countdown: 60
            escape_pods: 2
        northeast:
        northwest:
        southwest:
```

```ini

```

```yaml

```

### 三级

```ini

```

```yaml

```
