---
title: ReactNative环境安装
date: 2016-12-17 13:04:53
tags: [reactNative]
---

## ReactNative环境安装
2016年12月17日13:04:16

在安装rn，初始化rn应用，以及npm install rn工程的时候，要尽量注意做到使用`npm`，不要使用`cnpm`代替，可能会出现未知的错误。

### 1.安装node、配置镜像加速

node安装过程略。

建议一定要设置镜像来加速，不然速度会很慢：

```bash
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```

### 2.安装rn和yarn

```
npm install -g yarn react-native-cli
```

> Yarn是Facebook提供的替代npm的工具，可以加速node模块的下载。React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。

如果出现错误，考虑修改权限

```
sudo chown -R `whoami` /usr/local
```

### 3.选择安装

```
brew install watchman
```

> Watchman是由Facebook提供的监视文件系统变更的工具，安装此工具，packager可以快速捕捉文件的变化从而实现实时刷新，提高效率。

```
brew install flow
```

> Flow是一个静态的JS类型检查工具,它只是Facebook自家的代码规范。新手可以选择跳过。

### 4.新建测试项目

```
react-native init AwesomeProject
cd AwesomeProject
react-native run-ios
```

