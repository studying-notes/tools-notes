---
date: 2022-07-18T10:35:21+08:00
author: "Rustle Karl"

title: "部署 Vault"
url:  "posts/tools/docs/hashicorp/vault/deploy"  # 永久链接
tags: [ "Tools", "README" ]  # 标签
categories: [ "Tools 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

https://learn.hashicorp.com/tutorials/vault/getting-started-deploy?in=vault/getting-started

## 创建配置文件

config.hcl

```ini
storage "raft" {
  path    = "./vault/data"
  node_id = "node1"
}

listener "tcp" {
  address     = "127.0.0.1:8200"
  tls_disable = "true"
}

api_addr = "http://127.0.0.1:8200"
cluster_addr = "https://127.0.0.1:8201"
ui = true
```

## 创建数据目录

```bash
mkdir -p ./vault/data
```

## 启动服务

```bash
vault server -config=config.hcl
```

为命令行设置服务地址：

```bash
export VAULT_ADDR='http://127.0.0.1:8200'
```

## 初始化密钥

```bash
vault operator init
```

生成 5 个密钥和一个 Token。

## 解封

解封至少需要 3 个密钥。

```bash
vault operator unseal
```

上面的操作至少重复 3 次，输入 3 个不同密钥。

## 登录

```bash
vault login
```

输入 Token。

## 清除数据

```bash
pgrep -f vault | xargs kill
```

```bash
rm -r ./vault/data
```

## 显示详情

```bash
vault secrets list -detailed
```

## 创建存储路径

```bash
vault secrets enable -path=secret kv
```

## 添加数据

```bash
vault kv put -mount=secret foo bar=baz
```

## 三级

```bash

```

## 三级

```bash

```


