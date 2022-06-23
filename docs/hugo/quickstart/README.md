---
date: 2022-06-23T14:37:55+08:00
author: "Rustle Karl"

title: "软件安装与基本用法"
url:  "posts/hugo/quickstart/README"  # 永久链接
tags: [ "Hugo", "README" ]  # 标签
categories: [ "Hugo 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

- [安装](#安装)
  - [Windows](#windows)
- [基本命令](#基本命令)
  - [新建项目](#新建项目)
  - [添加主题](#添加主题)
  - [新建文件](#新建文件)
- [编译网站](#编译网站)
  - [本地预览](#本地预览)
  - [局域网访问](#局域网访问)
  - [生成静态文件](#生成静态文件)
- [项目结构](#项目结构)
- [URL 转换](#url-转换)
- [扉页](#扉页)

## 安装

### Windows

```shell
choco install hugo-extended -confirm
```

`extended` 是为了支持 Sass/SCSS。

## 基本命令

```shell
hugo [command] [flags]
```

### 新建项目

```shell
hugo new site website
```

### 添加主题

```shell
cd website
```

```shell
git init
```

```shell
git submodule add https://github.com/CaiJimmy/hugo-theme-stack.git themes/stack
```

### 新建文件

都是以 `content` 为相对路径，该路径可以在配置文件中自定义。

```shell
hugo new post/quickstart.md
```

```shell
hugo new post/docker/quickstart.md
```

我已经不用这种方法了，这种方法不够自由。

## 编译网站

### 本地预览

编译生成缓存的静态文件，然后启动本地服务用于预览：

```shell
hugo server
```

```shell
# 包括草稿
hugo server -D
```

```shell
# 指定主题
hugo server -t hello-friend
```

手动结束命令后，将会清除缓存的静态文件。

### 局域网访问

```shell
hugo server --bind 0.0.0.0 --baseURL http://192.168.199.140/ -p 12345
```

- `-bind 0.0.0.0` ：开放全部网络。
- `-baseURL` ： 覆盖 baseURL 参数，不加这个参数的话网站上的相关图片会显示不出来，会访问 localhost。

### 生成静态文件

生成静态文件，输出到默认的 `public` 目录：

```shell
hugo
```

## 项目结构

```shell
.
├─archetypes         # Markdown 文件的模板，通常用来自定义设置扉页等
| └─default.md       # 缺省预设的一个模板
├─content            # 内容目录，存放网站内所有内容文件的源代码
| ├─_index.md        # 网站的主页
| ├─chapter        # 章节目录，内容文件分类存放
| | ├─_index.md      # 章节的主页
| | ├─content      # 章节内的子页面目录
| |   ├─xxx.md       # 子页面内容，通常是 Markdown 文件
| |   └─imgs      # Markdown 文件引用的图片
├─data               # 网站的自定义配置文件，文件类型可以是 yaml|toml|json 等格式
├─layouts            # 布局目录，存放渲染 content 目录的模版文件，模版的文件类型是 html 格式
├─resource           # 主题产生的目录
├─static             # 静态资源目录，存放静态资源文件
├─themes             # 主题目录
| └─xxx          # 安装后的主题名称
| | └─layouts        # 某个主题下的网站模板文件
├─public             # 编译后生成的网站文件
└─config.toml        # 网站的配置文件
```

## URL 转换

```shell
├─content           # 网站源代码目录
| ├─_index.md       # URL转换为：https://example.com/
| ├─posts           # 一个名为 posts 的章节文件夹
| | ├─_index.md     # URL 转换为：https://example.com/posts/
| | ├─first.md      # URL 转换为：https://example.com/posts/first/
| | └─topic         # 章节内的子页面内容目录
| | | ├─second.md   # URL 转换为：https://example.com/posts/topic/second/
| | | └─xxx.jpg     # second.md 文件引用的本地图片
```

## 扉页

扉页用来配置文章的标题、时间、链接、分类等元信息，提供给模板调用。除了网站主页外，其它内容文件都需要扉页来识别文件类型和编译文件。

```yaml
---
date: {{ .Date }}  # 创建日期
author: "Rustle Karl"  # 作者

title: "{{ replace .Name "-" " " | title }}"  # 文章标题
description: "文章描述"
url:  "{{ .Name }}"  # 设置网页链接，默认使用文件名
tags: [ "tag1", "tag2"] # 自定义标签

weight: 20 # 文章在章节中的排序优先级，正序排序
chapter: false  # 将页面设置为章节

draft: false  # 草稿
---
```
