---
date: 2022-07-04T23:03:02+08:00
author: "Rustle Karl"

title: "s3fs 在本地挂载 S3 文件系统"
url:  "posts/tools/docs/s3fs"  # 永久链接
tags: [ "Tools", "README" ]  # 标签
categories: [ "Tools 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 安装

```bash
apt install -y s3fs
```

## 配置

```bash
echo null:null > ~/.passwd-s3fs
chmod 600 ~/.passwd-s3fs
```

## 挂载

```bash
mkdir /mnt/s3
```

```bash
s3fs test /mnt/s3 -o passwd_file=.passwd-s3fs -o url=http://192.168.0.16:8333 -o use_path_request_style
```

## 随机生成文件测试

```bash
seq 100 | xargs -i dd if=/dev/zero of={}.dat bs=10M count=1
```

S3 是压缩储存的，上面的命令储存后比较小。

用随机数据源：

```bash
seq 100 | xargs -i dd if=/dev/urandom of={}.dat bs=10M count=1
```

大了很多。
