---
title: babel-cli入门使用步骤
date: 2017-03-22
tags: [babel]
---

# babel

## babel-cli

项目中遇到了一个es6写的文件代码，在某些老的android设备的webview运行环境中不支持es6，比如`class`，`扩展运算符`等，使用这个解决方案：用`babel-cli`和`babel-preset-es2015`插件，把代码转为es5的代码。下面是具体步骤：


### 1.全局安装babel-cli

`$ npm install --global babel-cli`

`babel`提供命令行工具，用来在命令行转码，参考：[官方文档](http://babeljs.cn/docs/usage/cli/)

基本用法如下：

```bash
# 转码结果输出到标准输出
$ babel example.js

# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
$ babel example.js --out-file compiled.js
# 或者
$ babel example.js -o compiled.js

# 整个目录转码
# --out-dir 或 -d 参数指定输出目录
$ babel src --out-dir lib
# 或者
$ babel src -d lib

# -s 参数生成source map文件
$ babel src -d lib -s
```

### 2.项目中新建.babelrc 配置文件

`.babelrc`它是`babel`的配置文件，存放在项目的根目录下面，用来设置转码规则和插件。基本格式如下：

```json
{
    "presets": [
      "es2015",
      "react",
      "stage-2"
    ],
    "plugins": []
  }
```

presets字段设定`转码规则`，官方提供以下的规则集，你可以根据需要安装。

```bash
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015

# react转码规则
$ npm install --save-dev babel-preset-react

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```
### 3.安装es2015插件

`$ npm install --save-dev babel-preset-es2015`

修改配置文件为：

```json
{
    "presets": [
      "es2015"
    ],
    "plugins": []
  }
```

### 4.在命令行中使用命令转码


```bash
$ babel example.js -o compiled.js
```

## 关于babel-node

babel-cli工具自带一个`babel-node`命令，提供一个支持ES6的REPL环境。它支持Node的REPL环境的所有功能，而且可以直接运行ES6代码。
它不用单独安装，而是随`babel-cli`一起安装。然后，执行babel-node就进入PEPL环境。

```bash
$ babel-node
> (x => x * 2)(1)
2
```

babel-node命令可以直接运行ES6脚本。将上面的代码放入脚本文件es6.js，然后直接运行。

```bash
$ babel-node es6.js
2
```
babel-node也可以安装在项目中。


```bash
$ npm install --save-dev babel-cli
```

然后，改写`package.json`。

```bash
{
  "scripts": {
    "script-name": "babel-node script.js"
  }
}
```

上面代码中，使用babel-node替代node，这样script.js本身就不用做任何转码处理。

