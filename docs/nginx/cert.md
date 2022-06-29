---
date: 2022-02-14T09:27:04+08:00
author: "Rustle Karl"

title: "生成开发用 HTTPS 证书"
url:  "posts/tools/docs/nginx/cert"  # 永久链接
tags: [ "nginx", "HTTPS" ]  # 标签
categories: [ "Nginx 学习笔记" ]  # 分类

toc: true  # 目录
draft: false  # 草稿
---

```shell
apt install -y libnss3-tools
```

```shell
git clone https://github.com/FiloSottile/mkcert 
```

```shell
cd mkcert
```

```shell
go build -ldflags "-X main.Version=$(git describe --tags)"
```

```shell
choco install -y mkcert
```

```shell
mkdir ~/.cert
```

```shell
mkcert -key-file key.pem -cert-file cert.pem "localhost" "ubuntu"
```

```shell
mkcert -install
```
