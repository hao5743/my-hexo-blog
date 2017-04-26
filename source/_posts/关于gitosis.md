---
title: 关于gitosis
date: 2016-12-30 10:29:00
tags: [git]
---

# 关于gitosis

在工作中，我们有自己的git服务器，但是随着git项目的增多和项目成员的增多，我们开始使用了`Gitosis`软件。使用它可以方便的管理与git相关的如成员公钥、权限、项目成员等问题，这里我只对我项目中用到一些简单配置做了整理和记录，并非全面的介绍gitosis。这里提供一份更为详尽介绍的[文档](https://git-scm.com/book/zh/v1/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-Gitosis)。

Git是非常著名的分布式版本控制系统。

**Gitosis则是方便通过Git与ssh架设中央服务器的软件。**


### 安装

已经配置安装好，故没有从头介绍，如需要看这个[安装步骤](https://git-scm.com/book/zh/v1/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-Gitosis)

### 说明

gitosis安装好之后的几个目录说明：

` /home/git `目录: gitosis默认把这里作为存储所有 Git 仓库的根目录

`/home/git/repositories`目录: 这里是存放需要管理的git项目（无需直接操作这个目录，要用gitosis来管理） 

### 管理

远程克隆gitosis到本地，方便管理

```bash
# 在你的本地计算机上
git@gitserver:gitosis-admin.git
```

如果你无法克隆，说明你没有权限，也就是`gitosis.conf`中`[group gitosis-admin]`没有存储你的公钥。你需要联系管理员给你管理权限。

### 创建者或管理员新增一个管理员权限

```bash
# 在创建者或管理员的机器上（如有略过）
$ git clone git@gitserver:gitosis-admin.git
$ cd gitosis-admin.git
# 被添加者上传自己公钥（如有略过）
$ scp ~/.ssh/id_rsa.pub 用户名@主机:/tmp
# 管理员拷贝公钥到keydir,并重命名（如有略过）
$ cp /tmp/id_rsa.pub keydir/tom.pub

# 开始配置
# 编辑gitosis.conf,在[group gitosis-admin]下members中添加成员tom
$ vim gitosis.conf
# ...编辑操作...wq...
# [group gitosis-admin]
# members = admin tom
# writable = gitosis-admin

# 提交更改（有可能需要先git add）
$ git commit
$ git push

# OK！tom用户现在已经有gitosis的管理权限了，
# 它可以克隆gitosis-admin.git它的个人电脑上，并执行管理操作

```
目录说明：

`gitosis.conf`文件: 此处用于项目配置，包括gitosis管理员、git项目读写权限

`keydir`目录: 此处存放着相关用户的公钥


