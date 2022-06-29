---
date: 2020-10-06T22:49:26+08:00  # 创建日期
author: "Rustle Karl"

title: "Nginx 基础入门"
url:  "posts/tools/docs/nginx/README"  # 永久链接
tags: [ "nginx", "README" ]  # 标签
categories: [ "Nginx 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

> 新手常犯错误：直接双击 `nginx.exe` 运行。这样会导致修改配置后重启、停止无效，必须手动关闭任务管理器内的所有 `nginx` 进程。

正确的做法是：在 `nginx` 目录，打开命令行工具，命令方式启动/关闭/重启 `nginx`。

## 启动

```shell
start nginx
```

## 重启

```shell
nginx -s reload
```

## 停止

### 暴力终止

```shell
nginx -s stop
```

### 优雅退出

```shell
nginx -s quit
```
