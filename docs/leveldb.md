---
date: 2022-07-11T10:21:47+08:00
author: "Rustle Karl"

title: "LevelDB 简介"
url:  "posts/tools/docs/leveldb"  # 永久链接
tags: [ "Tools", "README" ]  # 标签
categories: [ "Tools 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

LevelDB 是 Google 开源的高性能的键值数据库, 而且 leveldb 是一种内嵌在程序中的数据库，我们可以把 leveldb 数据库嵌入到我们程序中。

LevelDB 是一个持久化的 key/value 存储，key 和 value 都是任意的字节数组 (byte arrays)，并且在存储时，key 值根据用户指定的 comparator 函数进行排序。

## 1. 基本介绍

## 1.1 特性

- keys 和 values 是任意的字节数组。
- 数据按 key 值排序存储。
- 调用者可以提供一个自定义的比较函数来重写排序顺序。
- 提供基本的 `Put(key,value)`，`Get(key)`，`Delete(key)` 操作。
- 多个更改可以在一个原子批处理中生效。
- 用户可以创建一个瞬时快照(snapshot)，以获得数据的一致性视图。
- 在数据上支持向前和向后迭代。
- 使用 Snappy 压缩库对数据进行自动压缩
- 与外部交互的操作都被抽象成了接口(如文件系统操作等)，因此用户可以根据接口自定义的操作系统交互。

## 1.2 局限性

- 这不是一个 SQL 数据库，它没有关系数据模型，不支持 SQL 查询，也不支持索引。
- 同时只能有一个进程(可能是具有多线程的进程)访问一个特定的数据库。对于同一个进程 LevelDB API 是支持多线程安全读写的。LevelDB 内部会使用特殊的锁来控制并发操作。
- 该程序库没有内置的 client-server 支持，有需要的用户必须自己封装。
- 嵌入式数据库，原生只支持 C++。

```bash

```

```bash

```

