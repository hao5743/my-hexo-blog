---
title: node版本管理 n和nvm说明
date: 2017-02-24 10:33:54
tags: [node]
---


项目中我们有时需要使用不同版本 的node来调试或测试程序，最本的方法当然是手动下载不同的版本，并卸载之前的版本，这样可以达到目的，但是无疑是麻烦时间的，在node中有“node版本管理器”来实现类似的功能需要。这里介绍比较流行的两个，“`n`”和“`nvm`”.
先说说n。
n是node一个模块，可以用来管理和切换node版本，其作者是TJ Holowaychuk（出名的Express框架作者），使用非常之简单。
安装 方法：

```bash
$ sudo npm install -g n  
```
常用命令：

```bash
$n //查看已安装版本  
$n latest  //安装最新版本并使用  
$n latest -d   //下载最新版但不使用，-d参数表示为仅下载  
$n stable  //安装最新稳定版本并使用  
$n <version>  //安装某个版本并使用，如$n 6.2.2  
$n rm <version ...> //删除某些版本  
$n ls    //查看可用版本  
$n --latest    //查看最新版本  
$n --stable    //查看最新稳定版  
$n -h    //查看帮助信息，更多命令在这里查看  
[plain] view plain copy print?在CODE上查看代码片派生到我的代码片
$ n use 0.10.21 some.js   //<span style="font-family: Arial, Helvetica, sans-serif;">以指定的版本来执行脚本</span>  
```


下面说说`nvm`。
问题来了，既然有这么简单好用的n，那么nvm为什么还会大肆流行呢？这个问题一会回答，先说说nvm。
nvm全称Node Version Manager，不同于 n，nvm 不是一个 npm package，而是一个独立软件包。这意味着我们需要单独使用它的安装逻辑：

```bash
$ wget -qO- https://raw.github.com/creationix/nvm/v0.4.0/install.sh | sh 
``` 
独立的安装包，因此当你机器上没有安装node时仍可以使用它。
常用命令：

```bash
$ nvm install 0.10    
$ nvm use 0.10         //使用指定的版本   
$ nvm ls               //查看当前已经安装的版本  
.nvm  
->  v0.10.24  
$ nvm current         //查看正在使用的版本  
v0.10.24  
$ nvm run 0.10.24 myApp.js      //以指定版本执行脚本  
$ rm -rf ~/.nvm     //卸载nvm  
```

对比说明：
我们在使用 n 管理 node 版本前，首先需要一个 node 环境。我们或者用 Homebrew 来安装一个 node，或者从官网下载 pkg 来安装，总之我们得先自己装一个 node —— n 本身是没法给你装的。
然后我们可以使用 n 来安装不同版本的 node。
在安装的时候，n 会先将指定版本的 node 存储下来，然后将其复制到我们熟知的路径 /usr/local/bin，非常简单明了。当然由于 n 会操作到非用户目录，所以需要加 sudo 来执行命令。
两者区别:
安装简易度。nvm 安装起来显然是要麻烦不少；n 这种安装方式更符合 node 的惯性思维。见仁见智吧。
系统支持。注意， nvm 不支持 Windows。
对全局模块的管理。n 对全局模块毫无作为，因此有可能在切换了 node 版本后发生全局模块执行出错的问题；nvm 的全局模块存在于各自版本的沙箱中，切换版本后需要重新安装，不同版本间也不存在任何冲突。
关于 node 路径。n 是万年不变的 /usr/local/bin；nvm 需要手动指定路径。

参考地址：
[http://taobaofed.org/blog/2015/11/17/nvm-or-n/](http://taobaofed.org/blog/2015/11/17/nvm-or-n/)     管理 node 版本，选择 nvm 还是 n？
[http://it.taocms.org/03/3079.htm](http://it.taocms.org/03/3079.htm)      利用n和nvm管理Node的版本



