---
date: 2022-07-12T15:11:35+08:00
author: "Rustle Karl"

title: "Terraform 基础教程"
url:  "posts/tools/docs/terraform/README"  # 永久链接
tags: [ "Tools", "README" ]  # 标签
categories: [ "Tools 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 安装

https://learn.hashicorp.com/tutorials/terraform/install-cli

### Ubuntu

```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
```

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
```

```bash
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
```

```bash
sudo apt-get update && sudo apt-get install terraform
```

### Windows

```bash
choco install terraform
```

```bash
terraform -help
```

```bash
terraform -help plan
```

## 简单示例

```bash
mkdir learn-terraform-docker-container
```

```bash
cd learn-terraform-docker-container
```

```bash
code main.tf
```

```ini
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = ">= 2.13.0"
    }
  }
}

provider "docker" {
  host    = "npipe:////.//pipe//docker_engine"
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

初始化：

```bash
terraform init
```

执行：

```bash
terraform apply
```

销毁：

```bash
terraform destroy
```

```bash

```

```bash

```


## 二级

### 三级

```bash

```

```bash

```


## 二级

### 三级

```bash

```

```bash

```


