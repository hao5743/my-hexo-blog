---
title: 渐进式Web应用程序在现代移动环境中的优势
date: 2016-12-08 19:16:19
tags: [ionic, PWA]
categories: [ionic]
---
渐进式Web应用程序在现代移动环境中的优势
The Advantages of Progressive Web Apps in the Modern Mobile Environment

> 文章翻译自：[http://blog.ionic.io/the-advantages-of-pwas/](http://blog.ionic.io/the-advantages-of-pwas/)
2016/12/1，作者：[Justin](http://twitter.com/@Justinwillis96)，译者：shaochong

我们一直在谈论Progressive Web Apps（下文简称PWA）- 它是用现代化的web API如`service workers`，`web manifest`和`web push`来构建的web应用程序，让人们在浏览器中就可以体验到和本地程序一样棒的使用体验。
> 我来解释几个概念（译者注）
> `service workers`是啥玩意？你可以这么理解，它运行于你当前页面主线程外的另一个线程中，用来加速你的APP，以它独特的技术来建立高效的离线体验，拦截网络请求，并且会根据当前的网络是否可用、服务器的内容是否更新来采取合适的策略。他们还允许通知推送和后台同步API。更多请参见MDN [Service Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)。
> `web manifest`又是啥玩意？它提供了一个文本文件的应用程序（如名称，作者，图标和描述）的信息，其目的是安装Web应用程序到设备的主屏幕，提供更快速、更丰富的用户体验。更多参见MDN [Web App Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest)。
> `web push`是什么鬼？Web Push API让web应用有了可以接受来自服务端推送来的消息的能力，无论web应用是否在前台运行，甚至是否加载完成。这让开发者可以实现异步通知和更新，及时的数据更新会让用户有更好的参与感。更多请参见[Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)

### 全新的移动环境
在我们谈论PWA的优势之前，有一件很重要的事情，那就是要搞清楚现在的移动环境和我们过去的开发环境有什么不同的地方。随着发展中国家越来越多的人能够通过他们的移动设备访问互联网，我们也要准备好为他们构建app，就像为在旧金山使用iphone7的人一样。
一，这些用户主要是通过2G或这3G来上网，它们网速很糟糕。
二，他们的设备通常有1-2G的RAM，慢速四核CPU，4GB的磁盘存储空间，运行android5.0或更高。
三，移动数据上网费用还是很昂贵的。到2016年，印度1GB移动数据平均成本为3.81美元，而最低工资为每小时31美分。这是一个很具有挑战性的应用场合，但是通过PWA我们仍然可以构建很棒、用户体验很好的应用程序。
### 为什么PWAs在现代移动环境中有着无与伦比的优势
和本地原生应用相比，PWA有三个关键的特点，让它能够构建功能更完善、用户体验更好的应用程序。

+ 高效的使用磁盘空间
+ 高效的使用网络
+ 不需要一次性安装大型的安装包

Service workers可以帮我们完成这三个工作。
举个例子来说，service workers 能够让我们动态的缓存资源到本地设备，所以即使是在慢速、糟糕的网络环境中，我们仍能够提供很好的使用体验。这里的关键点是动态缓存。这意味着在慢速糟糕的网络环境中，我们可以优先缓存那些对程序来说最重要的资源，而无需去下载一个大的APK文件。比如，对于一个电商app，你可能不希望将app中的每一个产品页都缓存到本地。我们仅仅需要将那些之前浏览过的产品页和app启动所必须的资源文件缓存到本地。
此外，由于是网站的工作方式，用户不必一次性的下载大的app文件。他们只需要下载他们所访问的PWA页面，并且通过使用service worker的动态缓存，我们可以选择仅缓存我们PWA应用的重要页面，因此这些页面只需要下载一次即可。我们可以通过Flipkart PWA来看看实际效果。在我的Nexus6上，他占用了大约351KB的存储空间。而在同一台设备上，Twitter的应用程序占用了69.36MB。如你所见，差距很大，在那些只有4G存储空间的设备上，差距将更加明显。
### Ionic对于PWA的计划
目前在ionic中支持对PWA的开箱即用，而且在不久的将来我们将会使它的支持更加强大。我们现在默认提供一个service worker和web manifest，让你的ionic应用具有PWA应用的能力，比如离线缓存，浏览器提示添加到主屏幕等。
此外，由于ionic是使用纯web技术来构建的，所以它可以很好的在浏览器中工作。展望未来，我们已经致力于通过代码拆分（code splitting），树形目录（treeshaking）和属性重命名（property renaming）来使我们的捆绑包更小，这可以让你的ionic2应用运行的更快。我们知道，在移动网络上，加载时间是很重要的，相信通过这些改变，我们可以更好的满足这一点。
使用Progressive Web Apps和现代化网络的强大功能，即使在低端，便宜的设备上，我们也能提供出色的用户体验。 PWA为移动应用程序提供了全新的范例，这对新的移动环境至关重要。 下次当你有一个极好的构思和想法，而且想要把它构建为移动app的时候，考虑将其构建为一个PWA应用，这可以让你的app触及到下一个十亿级的用户群体。

>译者：shaochong
>
>E-mail：hao5743@163.com
>
>博客：[http://hao5743.github.io/](http://hao5743.github.io/)

