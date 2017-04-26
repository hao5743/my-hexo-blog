---
title: ionic state命令将要废弃
date: 2016-12-29 11:29:00
tags: [ionic]
---

# ionic state命令将要废弃

参考自官方网站

[Discuss: Deprecate/Remove state command #904](https://github.com/driftyco/ionic-cli/issues/904)

[cordova说明](https://cordova.apache.org/docs/en/latest/platform_plugin_versioning_ref/index.html)

> The state command was created to manage Cordova plugins before it was supported in Cordova. Cordova now manages plugins and platforms in config.xml: https://cordova.apache.org/docs/en/latest/platform_plugin_versioning_ref/index.html so there is a decent amount of confusion between the Ionic and Cordova behavior. As Cordova platforms and plugins are not an Ionic concern, let's deprecate the state command and document the existing Cordova command workflow, ie plugin add --save.

state命令是之前用来管理cordova插件的。现在Cordova自身已经开始通过`config.xml`来支持插件和平台管理（[官方说明](https://cordova.apache.org/docs/en/latest/platform_plugin_versioning_ref/index.html)），这会让我们对ionic和cordova的不同的管理方式感到困惑。因为cordova的平台和插件是ionic所不关心的，所以建议停止使用`state`命令，而使用cordova的管理方式，比如`plugin add --save`

## 对于插件我们应该这样管理

重装所有插件

```bash
rm -rf plugins
# 更新插件,执行时会自动安装在config.xml中的插件
cordova prepare

```

从工程中彻底删除一个插件，--save命令会将删除或更改影响到`config.xml`,与`package.json`无关

```bash
cordova plugin reomve ionic-plugin-keyboard --save
cordova plugin add ionic-plugin-keyboard --save
```

platform add的时候会自动检测`config.xml`，并安装记录的插件

```
cordova platform add android
```

