---
title: 彻底解决Ionic项目中的跨域问题
date: 2016-12-03 18:38:17
tags: [ionic, CORS]
categories: ionic
---


在`ionic`项目中，如果你使用`ionic serve`或者`ionic run`,并且开启了动态加载（live reload），且访问了远端服务器的API，那么你就可能会遇到 [跨域资源共享(CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) 问题。出现类似下面的错误提示：
```
XMLHttpRequest cannot load http://api.ionic.com/endpoint.
No 'Access-Control-Allow-Origin' header is present on the requested resource.
Origin 'http://localhost:8100' is therefore not allowed access.
```
那么，什么是CORS？ 为什么在这里会发生这个错误呢？
## 什么是CORS？
CORS = Cross origin resource sharing [跨域资源共享](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
`origin`是你当前所在页面的主机地址。
如果你在浏览`http://ionicframework.com/blog/handling-cors-issues-in-ionic`页面，它的`origin`就是`ionicframework.com`。在此页面上，如果你要向`http://cors.api.com/api`发送一个[AJAX](https://developer.mozilla.org/zh-CN/docs/AJAX/Getting_Started)请求,您的主机源将由Origin标头指定，该标头会自动包含在浏览器的CORS请求中。因为`ionicframework.com`和主机`api.com`并不匹配，我们从`ionicframework.com`的请求必须要经过服务器的批准，才能够访问该资源，以Http Options请求头的形式。

如果我们遇到了上面的错误，说明服务器没有给我们的地址`ionicframework.com `赋予访问该资源的权限。

下面来看一下，当通过`ionic serve`,`ionic run`,`ionic run -l`三种不同方式运行app时，你的`origin`分别是什么？

### 在浏览器中运行时
在执行`ionic serve`时，发生了什么？

+ 启动了一个本地web服务器
+ 你的浏览器自动打开了指向本地服务器地址的一个页面

这时你可以看到你的app加载到了你的浏览器上，地址是`http://localhost:8100`(如果你选择了localhost)。

这时，你的`origin`是`localhost:8100`。

任何发送到主机而不是`localhost:8100`的AJAX请求，都将以`localhost:8100`作为其`origin`，因此需要经过CORS预检请求，来查看是否有权限访问该资源。

### 在设备上运行时
当执行`ionic run`时，又发生了些什么呢？

+ 应用程序文件被复制到设备（或模拟器）。
+ app开始运行，然后启动一个设备（或模拟器）上的浏览器来打开这些文件，类似`file://some/path/www/index.html`

这样，你的`origin`将不存在了，因为你正在运行于一个文件（`file://`）URI。理所当然，你的任何向外请求将不需要通过CORS预检。

### 在设备上通过自动加载（livereload）运行时
执行`ionic run -l`时，又将发生什么呢？

+ 首先启动一个本地web服务器
+ 设备（或模拟器）上app开始运行，启动一个浏览器来开地址`http://192.168.1.1:8100`（你的本地IP地址）上的文件。

这时，你的`origin`是`192.168.1.1:8100`。

任何发送到主机而不是`192.168.1.1:8100`的AJAX请求，都将以`192.168.1.1:8100`作为其`origin`，因此需要经过CORS预检请求，来查看是否有权限访问该资源。


## 在Ionic中处理CORS问题

CORS问题仅仅发生在，当我们以`ionic serve`或`ionic run -l`来运行或测试app的时候。

有2种方法来解决这个问题.

第一种，比较容易的方法，是设置远端服务器，让它可以接受所有源的请求。第二种，有时候我们没有权限去修改远端服务器，此时就需要一个不指定源的请求。

我们可以使用代理服务器来实现这个方案。`ionic cli`给我们提供了一个很方便的配置代理服务器的方法，下面我们看一下如何来实现。

## Ionic CLI代理服务器

下面是关于代理（proxy）的一个定义：
>In computer networks, a proxy server is a server (a computer system or an application) that acts as an intermediary for requests from clients seeking resources from other servers.
>
>在计算机网络中，代理服务器是一个客户端请求的中介服务器（计算机系统或应用），它用于帮助客户端寻找位于其他服务器上的资源。

要解决我们在ionic中遇到的CORS问题，我们需要设置一个代理服务器，它接受我们的请求，并向API端点发出新的请求，然后接受响应，并将其转发回我们的应用程序。

由于代理服务器需要向目标发送新的请求，因此请求中将不会有`origin`(源)，也就不再需要CORS了。 需要非常注意一点是，**`origin`是浏览器自动帮我们添加在请求头中的**。

Ionic CLI具有为客户端请求设置代理服务器的能力，用来帮助您解决CORS问题，下面来看一下Ionic CLI设置代理的方法。

## 设置代理服务器

再次说明一下，仅仅在`ionic serve`或`ionic run -l`的时候需要设置代理。

首先，我们需要在`ionic.project`（注意，ionic2 cli新版本已经将此文件重命名为`ionic.config.json`）配置文件中设置代理。 这将告诉Ionic本地服务器监听这些路径并将这些请求转发到目标网址。

在app中，我们需要替换包含endpoint的URLS，并设置为我们的代理服务器地址。（In our app, we will need to replace our endpoint URLS to be set to the proxy server address for when we are running serve or run -l.）
>什么是`endpoint`？
>
> 在项目中 endpoint有时也写作baseUrl，比如这个`http://cors.api.com/api/book/2/detail`的endpoint是`http://cors.api.com/api`，再比如`http://localhost:8101/api/user/123`的endpoint是`http://localhost:8101/api`。
> 
> 笔者没有想到好的中文来替代这个英文单词，所以没有进行翻译，希望小伙伴们能够理解它的意思就好啦~~~~~

我们可以通过设置一些*gulp任务*，用gulp中的替换模块(replace)来替换出这些URLS，来简化这一步骤。

比较推荐的方法是设置一个Angular Constant来同一设置的endpoint，方便统一进行修改和维护。

下面是操作步骤。我们需要设置一个Angular Service ApiEndpoint来获取数据。

## 配置代理地址

配置代理有两个需要注意的地方。

* `path`：你在本地Ionic服务器上访问它们的路径
* `proxyUrl`：你最终希望通过API调用达到的proxyUrl

修改`ionic.config.json`(旧版本Ionic CLI是`ionic.project`)：

```
{
  "name": "proxy-example",
  "app_id": "",
  "proxies": [
    {
      "path": "/api", 
      "proxyUrl": "http://cors.api.com/api"
    }
  ]
}
```

如上所述，当您请求访问`http://localhost:8100/api`的ionic服务器时，它会替你代理请求到`http://cors.api.com/api`上。这样，就不需要CORS了。

## 设置Angular Constant

很容易将你的ApiEndpoint设置为Angular Constant。下面，我们已经将ApiEndpoint指定为我们的代理url。然后，我们就可以使用这个url作为一个常数。

```
angular.module('starter', ['ionic', 'starter.controllers', 'starter.services'])
.constant('ApiEndpoint', {
  url: 'http://localhost:8100/api'
})

//For the real endpoint, we'd use this
// constant('ApiEndpoint', {
//  url: 'http://cors.api.com/api'
// })
```

## 设置Angular Service

```
angular.module('starter.services', [])

//NOTE: We are including the constant `ApiEndpoint` to be used here.
.factory('Api', function($http, ApiEndpoint) {
  console.log('ApiEndpoint', ApiEndpoint)

  var getApiData = function() {
    return $http.get(ApiEndpoint.url + '/tasks')
      .then(function(data) {
        console.log('Got some data: ', data);
        return data;
      });
  };

  return {
    getApiData: getApiData
  };
})
```

## 使用Gulp实现URL自动切换

首先，需要安装`replace module`

```bash
npm install --save replace
```
然后，我们需要修改`gulpfile.js`，并添加2个任务，来添加代理地址、移出代理地址。

```
// `npm install --save replace`
var replace = require('replace');
//注意下面的文件地址，它是包含你endpoint或baseurl的文件
var replaceFiles = ['./www/js/app.js'];

gulp.task('add-proxy', function() {
  return replace({
    regex: "http://cors.api.com/api",
    replacement: "http://localhost:8100/api",
    paths: replaceFiles,
    recursive: false,
    silent: false,
  });
})

gulp.task('remove-proxy', function() {
  return replace({
    regex: "http://localhost:8100/api",
    replacement: "http://cors.api.com/api",
    paths: replaceFiles,
    recursive: false,
    silent: false,
  });
})

```

## 结束语

这个教程展示给你了在运行`ionic serve`或`ionic run -l`的时候，一个处理CORS问题的方法。

我们知道，如果按这个方法处理，当我们需要在`ionic serve`和`ionic run -l`切换的时候，将ApiEndpoint替换出来可能会比较麻烦。因此我们推荐使用gulp在程序启动时自动完成这个过程。

其实，最简单的处理CORS问题的方法，是要求你的api提供服务器来允许所有的`origin`。但是，有时候我们无法这么做。

使用Angular Constant和replace module会给我们一个愉快的解决方法来处理CORS问题。

如果你需要一个具体的例子，可以看一下这个[示例工程](https://github.com/driftyco/ionic-proxy-example)。

上面说的就是所有的当你访问一个API服务器时，关于处理CORS问题的全部。

如果你有疑问或想法，请在下面给我们评论，或者通过[twitter](https://twitter.com/ionicframework) or [github](https://github.com/driftyco/ionic-cli).

------

>文章翻译自：[Handling CORS issues in Ionic，By Josh on February 24, 2015](http://blog.ionic.io/handling-cors-issues-in-ionic/)
>
>译者：shaochong
>
>说明：如果有翻译的不合适的地方，您可以联系（hao5743@163.com）我修改~








