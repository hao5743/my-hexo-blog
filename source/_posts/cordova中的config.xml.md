---
title: cordova中的config.xml
date: 2017-04-26 10:43:15
tags: [cordova, ionic2]
---

### 概述 

`Config.xml`存储着cordova应用的全局配置信息。

本文重点解释了config.xml中的几个常用配置，比如：`access` `allow-navigation` `allow-intent` 等。

更多信息请查看[官方参考文档](https://cordova.apache.org/docs/en/6.x/config_ref/index.html)。


### widget、name、description、author
* widget：`config.xml`中的根元素
* name：指定app名，它会显示在设备的主屏幕上
* description：app描述
* author：作者信息

### content
指定app的根页面，默认是`index.html`
示例：

```
<widget ...>
   <content src="startPage.html"></content>
</widget>
```

### access
配置允许app发起的网络请求（images, XHRs, etc）地址

注意：没有配置`access`标签时，，只有`file://`urls被允许访问。但一般情况，app包含一个默认的`<access origin="*">`标签。

示例：

```
<!--允许访问 google.com:-->
<access origin="http://google.com" />

<!--允许访问安全地址 google.com (https://):-->
<access origin="https://google.com" />

<!--允许访问 google.com 的所有子域名, 比如 mail.google.com and docs.google.com:-->
<access origin="http://*.google.com" />

<!--不阻止任何网络请求，比如 google.com and developer.mozilla.org:-->
<access origin="*" />
<!--This is the default value for newly created CLI projects.-->
```

更多请查看[network-request-whitelist](https://cordova.apache.org/docs/en/6.x/reference/cordova-plugin-whitelist/index.html#network-request-whitelist)


### allow-navigation
控制内部的`WebView`可以加载的`URL`，只适用于顶级导航。

默认情况下，只允许`file://`类型的URLS访问。如果要加载其他的urls，需要配置`<allow-navigation>`标签

```
<!-- 允许访问 example.com -->
<allow-navigation href="http://example.com/*" />

<!-- 允许使用通配符，协议，主机，地址都可以使用 -->
<allow-navigation href="*://*.example.com/*" />

<!-- 使用通配符，允许所有网址访问，包括HTTP and HTTPS and file
     *不推荐使用* -->
<allow-navigation href="*" />

<!-- 上面的和下面这三条等价 -->
<allow-navigation href="http://*/*" />
<allow-navigation href="https://*/*" />
<allow-navigation href="data:*" />
```

更多请查看[cordova-plugin-whitelist](https://cordova.apache.org/docs/en/6.x/reference/cordova-plugin-whitelist/index.html#navigation-whitelist)

### allow-intent
定义app可以要求操作系统打开的链接地址，默认，任何外部请求链接都被禁止。

示例：

```
<!-- 允许在浏览器中打开网址 -->
<allow-intent href="http://*/*" />
<allow-intent href="https://*/*" />

<!-- 允许在浏览器中打开 example.com -->
<allow-intent href="http://example.com/*" />

<!-- 允许通配符的使用，在协议、主机、地址 -->
<allow-intent href="*://*.example.com/*" />

<!-- 允许在短信应用中打开sms:链接 -->
<allow-intent href="sms:*" />

<!-- 允许在电话应用中打开tel:链接 -->
<allow-intent href="tel:*" />

<!-- 允许在地图应用中打开geo:链接 -->
<allow-intent href="geo:*" />

<!-- 允许在已安装的app中打开未被识别的url地址
     *NOT RECOMMENDED* -->
<allow-intent href="*" />
```
查看更多[cordova-plugin-whitelist](https://cordova.apache.org/docs/en/6.x/reference/cordova-plugin-whitelist/index.html#intent-whitelist)

### engine
Specifies details about what platform to restore during a prepare.

### plugin
Specifies details about what plugin to restore during a prepare. This element is automatically added to a project's config.xml when a plugin is added using the --save flag.

###  variable
Persists the value of a CLI variable to be used when restoring a plugin during a prepare. This element is added to config.xml when a plugin that uses CLI variables is added using the --save flag.

示例：

```
<plugin name="cordova-plugin-device" spec="^1.1.0">
    <variable name="MY_VARIABLE" value="my_variable_value" />
</plugin>
```

### preference
Sets various options as pairs of name/value attributes. Each preference's name is case-insensitive. Many preferences are unique to specific platforms, and will be indicated as such.

配置详情：[https://cordova.apache.org/docs/en/6.x/config_ref/index.html#preference](https://cordova.apache.org/docs/en/6.x/config_ref/index.html#preference)

### feature
If you use the CLI to build applications, you use the plugin command to enable device APIs. This does not modify the top-level config.xml file, so the element does not apply to your workflow. If you work directly in an SDK and using the platform-specific config.xml file as source, you use the tag to enable device-level APIs and external plugins. They often appear with custom values in platform-specific config.xml files. See the API Reference for details on how to specify each feature

更多请参考：[feature](https://cordova.apache.org/docs/en/6.x/config_ref/index.html#feature)

### platform
When using the CLI to build applications, it is sometimes necessary to specify preferences or other elements specific to a particular platform.

```
<platform name="android">
   <preference name="Fullscreen" value="true" />
</platform>
```

### hook
Represents your custom script which will be called by Cordova when certain action occurs (for example, after plugin is added or platform prepare logic is invoked). This is useful when you need to extend default Cordova functionality. 

更多请参考：[https://cordova.apache.org/docs/en/6.x/config_ref/index.html#hook](https://cordova.apache.org/docs/en/6.x/config_ref/index.html#hook)

```
<hook type="after_plugin_install" src="scripts/afterPluginInstall.js" />
```

> date：2017-04-26 10:37:40
> 版本有变化时，配置可能发生变化，详细信息请参考[官方文档](https://cordova.apache.org/docs/en/6.x/config_ref/index.html)





