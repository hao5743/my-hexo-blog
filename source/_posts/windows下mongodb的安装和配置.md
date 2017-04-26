---
title: windows下mongodb的安装和配置
date: 2017-03-29 16:20:13
tags: [mongodb]
---

# windows下mongodb的安装和配置

2017年3月29日，系统：win10，mongodb version v3.4.2

## 下载和安装
1. 下载地址：[https://www.mongodb.com/download-center#community](https://www.mongodb.com/download-center#community)
2. 下载符合你系统的版本，然后安装。默认安装到`C:\Program Files\MongoDB`,你也可以自定义安装目录。

## 创建数据目录
MongoDB将数据目录存储在 db 目录下。但是这个数据目录不会主动创建，我们在安装完成后需要创建它。

请注意，**数据目录应该放在根目录下**（(如： C:\ 或者 D:\ 等 )。

这里我们假设创建数据目录在`E:\data\db`

## 命令行运行mogondb服务

假设你的mongodb安装在`C:\Program Files\MongoDB`:

进入安装目录：

```bash
cd C:\Program Files\MongoDB\Server\3.4\bin
```

启动服务：

```bash
mongod.exe --dbpath e:\data\db
```

## 将MongoDB服务器作为Windows服务运行


在命令行中启动服务之后，执行：


```bash
 mongod.exe --logpath "e:\data\dbConf\mongodb.log" --logappend --dbpath "e:/data/db" --port 27017 --serviceName "MongoDB" --install
```

终止命令行中的mongodb服务，打开刚才新建的mongodb服务：

```bash
NET START MongoDB
```

如果出现服务器无法正常启动的问题，是因为mongod.lock这个文件，在服务器异常退出时，该文件会影响下一次启动mongod服务的,我们首先关闭命令行mongodb服务，然后只需要删除该文件就行了：

```
mongod.exe --config e:\data\db\mongod.lock --remove
```

> windows删除服务命令： `sc delete MongoDB`

说明：

请注意，你必须有管理权限才能运行下面的命令。执行以下命令将MongoDB服务器作为Windows服务运行：

```bash
mongod.exe --bind_ip yourIPadress --logpath "C:\data\dbConf\mongodb.log" --logappend --dbpath "C:\data\db" --port yourPortNumber --serviceName "YourServiceName" --serviceDisplayName "YourServiceName" --install
```

下表为mongodb启动的参数说明：

参数	描述

--bind_ip	绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定默认本地所有IP

--logpath	定MongoDB日志文件，注意是指定文件不是目录

--logappend	使用追加的方式写日志

--dbpath	指定数据库路径

--port	指定服务端口号，默认端口27017

--serviceName	指定服务名称

--serviceDisplayName	指定服务名称，有多个mongodb服务时执行。

--install	指定作为一个Windows服务安装。

## MongoDB后台管理 Shell

进入安装目录：

```bash
cd C:\Program Files\MongoDB\Server\3.4\bin
mongo.exe
```

`db` 命令用于查看当前操作的文档（数据库）


插入一些简单的记录并查找它：

```
> db.runoob.insert({x:10})
WriteResult({ "nInserted" : 1 })
> db.runoob.find()
{ "_id" : ObjectId("5604ff74a274a611b0c990aa"), "x" : 10 }
>
```


> 本文章写于，2017年3月29日，测试环境：mongodb版本 v3.4.2，操作系统win10



