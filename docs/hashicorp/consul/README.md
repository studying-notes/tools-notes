---
date: 2022-07-15T12:14:13+08:00
author: "Rustle Karl"

title: "Consul 简介"
url:  "posts/tools/docs/hashicorp/consul/README"  # 永久链接
tags: [ "Tools", "README" ]  # 标签
categories: [ "Tools 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 关键特性

- **服务发现** 解决在分布式环境中，如何找到可用的服务地址的问题，支持通过 DNS 和 HTTP 查询服务地址。
- **健康检查** 定时监控服务是否正常，对于异常的服务会主动下线。
- **Key/Value 存储** 配置中心解决方案，是一种key/valjue存储结构，区别就是key是以目录结构形式组织的，可以用来存储系统配置信息。
- **多数据中心** : 支持多数据中心部署。

## 默认端口

- 8500 http 端口，用于 http 接口和 web ui
- 8300 server rpc 端口，同一数据中心 consul server 之间通过该端口通信
- 8301 serf lan 端口，同一数据中心 consul client 通过该端口通信
- 8302 serf wan 端口，不同数据中心 consul server 通过该端口通信
- 8600 dns 端口，用于服务发现

## 架构

consul 主要有 server 和 client 两种组件组成。

server 负责核心数据的存储和处理请求，server 可以部署多个实例 ( 通常推荐 3 -5 个 )，server 只有一个 leader 实例，就是主节点，主节点是通过选举产生的，主节点负责数据的写入处理，同时将数据同步至其他 server 节点 client 负责跟 server 通信，处理转发服务注册、服务发现请求到 server 节点，client 还负责服务的健康检查，client 节点可以部署多个实例，甚至每个微服务节点都部署一个 client 实例。

## 命令介绍

server 负责核心数据的存储和处理数据的读写，通过 client 可操作 server 提供的接口。

consul 将 server 和 client 两个组件的实现都融合在一个叫 consul 的命令程序中，所以我们安装 consul 后，只有一个 consul 命令。

在 consul 中，无论 server 还是 client 都叫做 agent，通过命令参数区分，我们运行的是 server 还是 client。

### 基本命令

#### 查询集群节点

```bash
consul members
```

打印出集群中所有的节点信息，可以通过 status 状态查看节点是否正常运行。

#### 重新加载配置文件

```bash
consul reload
```

#### 关闭节点

```bash
consul leave
```

#### 查询所有注册的服务

```bash
consul catalog services
```

## 生产环境部署

生产环境节点配置，主要分 server 节点和 client 节点，至少部署一个 server 节点，为了高可用，通常建议部署 3 -5 个 server 节点，client 节点任意，client 节点可以跟服务节点数量一致，每台服务的节点部署一个 client。

## 服务注册

consul 支持两种方式注册，通过配置文件形式或通过 http 接口进行注册。

### 使用配置文件注册

基于配置文件的服务注册方式，是官方推荐的方式，因为这种方式对我们的微服务应用无侵入，就是不需要写代码调用 consul 的接口去注册。

#### 定义一个服务

首选定义一个存放 consul 配置文件的目录，例如 /etc/consul.d：

```bash
#首选创建一个配置文件目录，确保 consul 命令有权限访问这个目录即可
mkdir  /etc/consul.d
```

为方便管理，我们通常一个微服务定义一个配置文件，服务定义配置文件是 json 格式。

例如：定义一个名字叫 web 的服务。

文件：/etc/consul.d/web.json

```json
{
    "service": {
        "name": "web",
        "tags": [
            "rails"
        ],
        "port": 80
    }
}
```

服务配置文件名，可以随便取，通常以服务名命名。

配置文件参数说明：

- name - 服务名
- tags - 可以为服务打上标签，是个字符串数组，查询服务的时候可以用来过滤
- port - 服务的端口
- address - 服务的ip地址，可选，一般不用填写，注册的时候agent会加上。

#### 注册服务

本地开发模式，启动 consul 的时候，通过 config-dir 参数指定配置文件目录，就会自动注册配置文件目录下的所有服务。

```bash
#以开发模式启动consul
consul  agent -dev -config-dir=/etc/consul.d/
```

如果 consul agent 已经启动，并且启动的时候已经通过参数 config-dir 指定了同样的配置目录，只要执行下面命令，重新加载配置值文件即可注册服务。

```bash
#重新加载配置
consul reload
```

#### 查询注册的服务

通过下面命令查询，当前注册的所有服务，会发现 web 服务已经注册成功。

```bash
consul catalog services
```

```bash
consul
web
```

#### 反注册

就是删除注册的服务，输入下面命令，会删除注册的服务信息，同时删掉本地的服务的配置文件。

```bash
consul services deregister -id=web
```

```bash

```


### 使用接口注册服务

https://www.consul.io/api-docs/catalog

## 服务查询

consul 支持两种接口查询我们注册服务信息，http api 或者 DNS 查询。

### 通过 http 接口查询

#### 查询指定的服务

```bash
curl http://localhost:8500/v1/catalog/service/web
```

#### 仅查询可用的服务地址

```bash
curl 'http://localhost:8500/v1/catalog/service/web?passing'
```

在 url 后面增加一个 passing 参数代表，查询可用的服务地址。

#### 查询注册中心的服务目录

通过这个接口，我们可以查询 consul 目前注册了那些服务。

```bash
http 'http://localhost:8500/v1/catalog/services'
```

### 通过 DNS 查询

consul 支持通过 dns 查询服务信息，dns 只能返回可用的服务地址

```bash
dig @127.0.0.1 -p 8600 web.service.consul
```

参数说明:

- `@127.0.0.1` - 指定 dns 服务器地址，就是我们 consul 的 agent 地址，本地安装了 agent 使用 127.0.0.1 即可
- `p` - 指定 dns 服务的端口，consul 默认使用的是 8600

web.service.consul 是需要查询的域名。consul 服务的域名规则：

```bash
服务名.service.consul
```

consul 默认会为服务配置本地域名。

```bash

```

## 服务健康检查

分布式环境下，微服务实例的重启、微服务异常等等，都会导致服务不可用，例如：我们向 consul 注册了一个 web 服务，web 服务启动了 10 个实例，现在有 3 个实例不可用，consul 怎么知道那些服务实例不可用？

consul 健康检查机制，就是解决上述问题的方案，健康检查机制运行在 consul client 中，会定期的根据服务健康检查配置，去检测服务是否正常，如果服务异常，就将服务的实例标记为不用，

如果恢复了，就标记为可用。

健康检查基本配置格式，是在服务定义配置中，增加 checks 字段，配置健康检查。


```json
{
 "service": {
     "name": "web",
     "tags": ["rails"],
     "port": 80,
     "checks" : [
          {
           }
      ]
  }
}
```

consul 健康检查有多种方式，具体的配置参考下面。

### 基于 http 请求

定时以 GET 请求方式，请求指定 url，http 请求返回状态码 200 表示正常，其他状态代表异常。

```json
{
	"service": {
		"name": "web",
		"tags": ["rails"],
		"port": 80,
		"checks": [{
			"id": "api", // 健康检查项的id，唯一
			"name": "HTTP API on port 5000", // 检查项的名字
			"http": "https://localhost:5000/health", // 定期访问的Url,通过这个url请求结果确定服务是否正常
			"tls_skip_verify": false, // 关闭tls验证
			"method": "POST", // 设置http请求方式，默认是GET
			"header": { // 可以自定义请求头，可以不配置
				"x-foo": ["bar", "baz"]
			},
			"interval": "10s", // 定期检查的时间间隔，这里是10秒
			"timeout": "1s" // 请求超时时间，1秒
		}]
	}
}
```

### 基于 grpc 请求

```json
{
	"service": {
		"name": "web",
		"tags": ["rails"],
		"port": 80,
		"checks": [{
			"id": "mem-util", // 检查项目id
			"name": "Service health status", // 检查项名字
			"grpc": "127.0.0.1:12345", // grpc地址，ip+port
			"grpc_use_tls": true,
			"interval": "10s" // 10秒检测一次
		}]
	}
}
```

### 基于命令

consul 支持定期执行一个命令或脚本，来检测服务是否正常，consul 通过监测命令退出状态判断服务是否正常，命令退出状态 0 代表正常，其他代表异常。

```json
{
	"service": {
		"name": "web",
		"tags": ["rails"],
		"port": 80,
		"checks": [{
			"id": "mem-util", // 检查项id
			"name": "Memory utilization", // 检查项名字
			"args": ["/usr/local/bin/check_mem.py", "-limit", "256MB"], // 这里是我们要执行的命令，第一个参数是命令或者脚本名，后面跟着任意个参数
			"interval": "10s", // 每10秒执行一次命令
			"timeout": "1s" // 命令执行超时时间
		}]
	}
}
```

```json
{
	"service": {
		"name": "web",
		"tags": ["rails"],
		"port": 80,
		"checks": [{
			"args": ["curl", "localhost"], // 执行curl命令，数组第2到第N个元素，代表命令参数
			"interval": "10s" // 每10秒执行一次命令
		}]
	}
}
```

为了安全考虑，如果健康检查使用执行命令方式，在启动 consul 的时候支持下面两种参数：

- `-enable-script-checks` 允许通过配置文件和 http api 注册的服务，执行命令检查健康状态
- `-enable-local-script-checks` 禁止通过 http api 注册的服务，执行命令检查健康状态，只允许通过配置文件注册的服务，执行命令。

建议生产环境使用 `-enable-local-script-checks` 参数启动 consul agent。

```bash
consul agent -dev -enable-local-script-checks -config-dir=/etc/consul.d
```

本地测试，增加 -dev 参数，代表开发模式，生产环境，去掉 -dev 参数。

因为只有 consul 的 client 才会执行健康检查任务，可以在 client 设置这个参数就可以。

## 键值存储

consul 提供一个键值存储（k/v）数据库,我们使用这个特性，存储我们的应用配置、应用元数据和实现分布式锁。

consul 支持命令和 http api 的方式读写键值对。

存储的 key/value 都不限制类型，但是不能大于 512k。

consul 的键（key）是以目录树的形式组织的，例如：

```bash
/tizi/consul/users
/tizi/consul/conns
/tizi/maxusers
```

key 以这种目录树结构组织，方便我们实现前缀搜索，例如查询 /tizi 为前缀的键值数据。

### 命令方式

#### 更新或者创建键值数据

```bash
consul kv put redis/config/connections 5
```

#### 读取数据

```bash
consul kv get redis/config/connections
```

#### 查询 key 的详细信息

```bash
consul kv get -detailed redis/config/connections
```

#### 删除指定 key 的数据

```bash
consul kv delete redis/config/connections
```

### HTTP 方式

#### 读取数据

```bash
curl "http://127.0.0.1:8500/v1/kv/redis/config/connections"
```

value 的值是 base64 编码的，读取的时候需要编码处理。

## 可视化后台

http://localhost:8500/ui/

## 监控服务变化

当 consul 中注册的服务信息发生变化的时候，我们除了定时通过接口去查询最新的服务信息之外，consul 还提供了 watch 机制，通过监控 consul 数据的变化，主动通知

确实也是不错的选择，但更多还是用 Prometheus。

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```



