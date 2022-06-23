---
date: 2022-06-23T15:07:23+08:00
author: "Rustle Karl"

title: "源代码片段分析"
url:  "posts/hugo/docs/snippet"  # 永久链接
tags: [ "Hugo", "README" ]  # 标签
categories: [ "Hugo 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 允许访问工作目录之外的文件

`tpl/os/os.go`

```go
// ReadDir lists the directory contents relative to the configured WorkingDir.
func (ns *Namespace) ReadDir(i interface{}) ([]_os.FileInfo, error) {
	path, err := cast.ToStringE(i)
	if err != nil {
		return nil, err
	}

    //list, err := afero.ReadDir(ns.deps.Fs.WorkingDir, path)
    // 暴力改成直接读取目录
	list, err := ioutil.ReadDir(path)
	if err != nil {
		return nil, fmt.Errorf("failed to read directory %q: %s", path, err)
	}

	return list, nil
}
```

```shell

```
