---
title: 研究了oh-my-zsh在mac中环境变量的配置办法
date: 2016-12-28 17:29:54
tags: [macOS, bash]
---

# 研究了oh-my-zsh在mac中环境变量的配置办法
2016-12-28 17:29:54


## mac环境变量保存的地方
当没有zsh的时候，mac中的环境变量保存在
####  1./etc/profile   （建议不修改这个文件 ）
 全局（公有）配置，不管是哪个用户，登录时都会读取该文件。
 
####  2./etc/bashrc    （一般在这个文件中添加系统级环境变量）
 全局（公有）配置，bash shell执行时，不管是何种方式，都会读取此文件。
 
#### 3.~/.bash_profile  （一般在这个文件中添加用户级环境变量）
 每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行

如果要修改环境变量，一般修改`~/.bash_profile`就行了。

## 查看环境变量

```
$ printenv 
```

查看当前使用的shell

```
$ echo $SHELL
    /bin/zsh
```

## zsh安装后

安装zsh后，默认情况下就不会自动读取`~/.bash_profile`了。
在用户目录下应该有`.oh-my-zsh`目录，和`.zshrc`配置文件

1. `.oh-my-zsh`目录： 它是zsh的安装文件夹，可以自己更改

2. `.zshrc`: 里面是zsh默认配置，可以用于设置环境变量（export），alias命令别名，设置主题等

	> 但是zsh不建议直接操作这个默认配置，如果用户需要自定义配置，推荐去这里`./oh-my-zsh/custom/custom.zsh`修改。注意：`custom`文件夹里的所有配置都会被zsh自动读取并配置。

3. `./oh-my-zsh/custom/my_custom.zsh`:用户设置自定义系统变量、自定义命令等等

	一般情况下，我们在`./oh-my-zsh/custom/my_custom.zsh`中配置一个快捷键
	
	```
	alias zshconfig="subl ~/.oh-my-zsh/custom/my_custom.zsh"
	
	```

## 综上，如果要配置环境变量的步骤

1. 执行`zshconfig`,会自动使用你定义的命令，打开配置文件
2. 在该文件中添加你想要添加的环境变量，比如`export ANDROID_HOME=/Development/android-sdk/`
3. 重启cmd，生效！OK。配置完成


## 备份一个我自己的配置文件

`./oh-my-zsh/custom/my_custom.zsh`如下：

```
alias zshconfig="subl ~/.oh-my-zsh/custom/my_custom.zsh"
alias code=\''/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code'\'
alias subl="'/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl'"
alias nano="subl"
alias sl="subl"
alias py="python"
alias nw="/Applications/nwjs.app/Contents/MacOS/nwjs"
export ANDROID_HOME=/Development/android-sdk/
```

