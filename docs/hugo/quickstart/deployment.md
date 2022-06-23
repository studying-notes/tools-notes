---
date: 2022-06-23T14:53:15+08:00
author: "Rustle Karl"

title: "正式部署的几种方法"
url:  "posts/hugo/quickstart/deployment"  # 永久链接
tags: [ "Hugo", "README" ]  # 标签
categories: [ "Hugo 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 本地方式

### 从源码编译

```shell
git clone https://github.com/gohugoio/hugo.git
```

```shell
go build && cp hugo /usr/local/bin
```

### 从商店获取

```shell
snap install hugo --channel=extended
```

## Docker 方式

```shell
docker pull klakegg/hugo
```
