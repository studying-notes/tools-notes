---
date: 2022-06-23T15:09:35+08:00
author: "Rustle Karl"

title: "问题记录"
url:  "posts/hugo/docs/question"  # 永久链接
tags: [ "Hugo", "README" ]  # 标签
categories: [ "Hugo 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

# 外置 exFAT 格式硬盘编译错误

```
hugo
Start building sites …
Total in 15540 ms
Error: Error copying static files: chtimes your\path\to\site\: The parameter is incorrect.
```

在 StackOverflow 找到了一样的问题，唯一的回答是可能网站源码放在了 exFAT 格式的移动硬盘中，跟我的情况一模一样。

```
https://stackoverflow.com/questions/64069079/blogdownserve-site-error-copying-static-files
```

推测是该硬盘格式无法获取某些/个文件属性。

```
hugo.exe --noTimes
```

通过禁用获取时间属性启动。
