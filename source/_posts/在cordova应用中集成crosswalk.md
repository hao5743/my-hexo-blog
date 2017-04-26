---
title: 在cordova应用中集成crosswalk
date: 2017-04-11 15:06:20
tags: [cordova, crosswalk, ionic]
---

# 在cordova应用中集成crosswalk

`Crosswalk`是一款开源的Web引擎，其基于 Chromium/Blink 的应用运行环境，对于混合开发的轻量级应用尤为受欢迎。

`crosswalk`是hybrid应用的运行时环境，它用来代替系统自带的`webview`，以保证应用行为的一致性（css一致，es6支持等），兼容性（完美支持WebRTC，WebAudio，Flexbox布局等）和提高流畅性（相比老旧安卓设备，因为它们自身的webview比较老旧）。

参考链接：
[crosswalk官网](https://crosswalk-project.org/)
[官方npm插件](https://www.npmjs.com/package/cordova-plugin-crosswalk-webview/)
[crosswalk官网 - cordova应用步骤](https://crosswalk-project.org/documentation/cordova.html)

### 优点
* `webview`不再依赖于安卓版本，因为每个Android版本WebView的表现都有差别，可以最大限度降低Android碎片化的影响，得到一致的，可预测的行为。
* 兼容性更好，使用最新的Web技术及API，保证WebRTC, WebAudio, Web Components等
* 性能更好，与旧版本系统的老webview相比

### 缺点
* 增大内存占用率，增加大约30MB
* 增大APK包的大小，大约17MB
* 增加安装后的磁盘占用空间，大约50MB
* `Crosswalk WebView`的本地存储(IndexedDB, LocalStorage, etc)和系统webview的相互独立

### 安装crosswalk插件

ionic2或者cordova应用中安装：

```bash
$ cordova plugin add cordova-plugin-crosswalk-webview --save
```

构建：

```bash
$ cordova build android
```


