---
title: JavaScript响应式编程 - RxJs入门
date: 2017-03-03 16:13:48
tags: [javascript, rxjs]
---

在angular2项目的过程中，接触到了Rxjs这个东西，对它进行了一下简单的了解，对它的基本用法进行了学习和总结，介绍了：

* 基本概念
* 如何创建一个Observerble可观察序列
* Observer实例的生命周期
* 如何对Observer实例进行订阅？以及监听他的next、error、complete事件？
* rxjs交互图怎么看？
* map、filter、from等简单的操作符含义

## 它是什么
官方文档：[http://reactivex.io/rxjs/](http://reactivex.io/rxjs/)

`Rx`（`ReactiveX`, `Reactive Extensions`）,它是微软开发和维护的基于`响应式编程`（`Reactive Programming`）范式实现的一套工具库集合，于2012年11月开源，它提供了一系列接口规范来帮助开发者方便的处理异步数据流。Rx系列结合了观察者模式、迭代器模式、函数式编程，它已成为业界响应式编程的优秀实践。

`RxJs`就是Rx在JavaScript层面上的实现，除此之外还有RxJava、Rx.Net、RxSwift等等。

`RxJs`全名`Reactive Extensions for JavaScript`，翻译为`Javascript的响应式扩展`, 它的思路是把随时间不断变化的数据、状态、事件等等转成可被观察的序列(`Observable Sequence`)，然后订阅序列中那些Observable对象的变化，一旦变化，就会执行事先安排好的各种转换和操作。

## 几个名词
`Observable`: `可观察的数据序列`.
`Observer`: `观察者实例`，用来决定何时观察指定数据，只有在Rx.Observable.create创建方法可以获取
`Subscription`: 观察数据序列返回的`订阅实例`.
`Operators`: `Observable的操作符`，包括转换数据序列，过滤等，所有的Operators方法接受的参数是上一次发送的数据变更的值，而方法返回值我们称之为发射新数据变更

`Observable`可以发射三种类型的事件：`next`、`error`、`complete`
`Subscription`可以订阅三种类型的事件：`onNext` `onError` `onComplete`
`Observable`有四个生命周期：创建 、订阅 、 执行 、销毁。


## 使用create创建一个Observable

```js
const Rx = require('rxjs/Rx');
const {Observable} = require('rxjs/Observable');
require('rxjs/add/observable/of');
require('rxjs/add/operator/map');
    
//创建序列源
const source = Observable.create(observer=>{
    observer.next('foo');
    setTimeout(()=> observer.next('bar'), 1000);
});
//订阅
const subscription = source.subscribe(data=>console.log(data));
```

## 捕获error和complete

```js
let source = Observable.create(observer=>{
    observer.next('foo');                         //发射数据
    observer.error('my error');                   //发射错误
    setTimeout(()=> observer.next('bar'), 1000);
    setTimeout(()=>observer.complete(),2000);     //发射完成
});

let source1 = source.map(e=>`hello ${e}`);

let subscription = source1
    .subscribe(
        data=>console.log(data),
        error=>console.log(error),
        ()=>console.log('complete')
        );
```

## rxjs交互图
* 每条线表示一个数据序列
* 每个球表示发送的数据变更，小竖线表示complete
* 方法下面表示经过map、filter等操作符处理过后产生的新序列


![屏幕快照 2017-03-03 14.23.49](http://ol1ftyec4.bkt.clouddn.com/2017-03-07-%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-03-03%2014.23.49.png)


```ts
//序列1
let source = Observable.create(observer => {
    observer.next(1);
    observer.next(2);
    observer.next(3);
});
//序列2
let source2 = source.map(e=>e=e*10);
//订阅序列1
source.subscribe(data=>console.log(data));
//订阅序列2
source2.subscribe(data=>console.log(data));

```

## 操作符

`map`：对要发送的data作统一处理后再返回
`filter`：满足条件时才发射出去
`toPromise`：将observer转为promise
`debounce`：（防抖动）取一段时间内最新的数据
`throttle`：(防抖动) 忽略这段时间，发现新值后再发送

更多，如 mapTo, scan, from ,concat, concatAll, merge, mergeAll等

## 结束

angular2中的HTTP模块，ionic-native中的BLE模块都默认使用了`Observable`

还需要要研究：

* 他的使用场景？
* 与promise相比他的优势劣势是什么？
* 未来的使用趋势如何？

## 参考文章
官方文档：[http://reactivex.io/rxjs/](http://reactivex.io/rxjs/)

[rxjs简单入门](https://yq.aliyun.com/articles/65027)

[使用 RxJS 实现 JavaScript 的 Reactive 编程](http://www.oschina.net/translate/rxjs-streams?lang=chs&page=1#)

[Reactive Programming in JavaScript With RxJS](https://dzone.com/refcardz/rxjs-streams)

