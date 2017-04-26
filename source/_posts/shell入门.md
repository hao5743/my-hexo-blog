---
title: shell入门
date: 2017-04-24 11:05:22
tags: [shell]
---

# shell

## 运行第一个简单的shell

新建test.sh,内容为:

```bash
#! /bin/bash
echo 'hello shell'
```

添加运行权限：

```bash
$ chmod +x test.sh
```

运行：

```bash
$ ./test.sh
或者
$ /bin/bash test.sh
```

## 使用变量

