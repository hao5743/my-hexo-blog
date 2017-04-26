---
title: RxJs常用operator总结
date: 2017-03-07
tags: [javascript, rxjs]
---

# RxJs中的operator

## 创建操作符
常见的有`Observable.forEvent()`,  `Observable.create()`, `Observable.interval()`。

### create
`Observable.create()`：自定义创建一个`observable`

```ts
const {Observable} = require('rxjs/Observable');
require('rxjs/add/observable/of');
require('rxjs/add/operator/map');
//create创建操作符
let observable = Observable.create(observer=>{
    observer.next(3);
    observer.next(2);
});

observable.subscribe(data=>{
    console.log(data);
});
```

### interval

`interval(time:number)`:创建一个递增序列，每隔指定时间发送一条数据

```ts
let numbers = Rx.Observable.interval(1000);
numbers.subscribe(x => console.log(x));  // 0 1 2 3 4 5 6 ......
```

### of
`public static of(values: ...T, scheduler: Scheduler): Observable<T>`: 从数组创建一个序列，一次将数据发射出去，然后发射完成命令

```ts
var numbers = Rx.Observable.of(10, 20, 30);
var letters = Rx.Observable.of('a', 'b', 'c');
var result = numbers.concat(letters);
result.subscribe(x => console.log(x));  //10 20 30 a b c
```


## 变换操作符
对每一次返回数据做一次变换操作，把原本的数据流变成需要的数据流，最常使用的变换操作是`observable.map()`

比如，数据获取接口经常会有一些规范去包裹数据，比如

```
{
    "err_code":0,
    "data":{"name":"Operators"}
}
```
如果我们只需要data字段，就可以使用map来实现：

```ts
observable.map(res=>{
    return res.data;
}).subscribe(data=>{
    //do sth with data
});
```

## 过滤操作符filter
用于过滤掉数据流中一些不需要处理的数据，常用的是`observable.filter()`
比如，前端获取数据的时候，接口可能会因为各种原因无法返回最终需要的数据，可能由于异常返回的数据为空，或者返回一个错误代码和错误描述告知前端，但是并不需要处理这些错误信息，就需要过滤掉这些错误数据。

```ts
//过滤操作符
observable.filter(response=>{
    return !!response.data && response.status === 200;
}).map(response=>{
    return response.data;
}).subscribe(data=>{
    //do sth with data
});
```

## 组合操作符
很多业务场景需要依赖两个甚至更多的接口数据，并且在这些接口数据都成功获取后，再进行关联合并。要满足这种场景，就需要把各个数据流汇汇合组成新的数据流，这时就要用到`组合操作符`，比如`Observable.forkJoin()`,`Observable.prototype.concatMap()`


```ts
//组合操作符 Observable.forkJoin
let getFirstDataObs = Observable.create(observer => {
    observer.next('a1');
    observer.next('a2');
    observer.complete();
});
let getSecondDataObs = Observable.create(observer => {
    observer.next('b1');
    observer.next('b2');
    observer.complete();
});

let joinedObservable = Observable.forkJoin(
    getFirstDataObs, getSecondDataObs
);

joinedObservable.subscribe(datas => {
    //data[0] 是getFirstDataObs 的数据
    //data[1] 是getSecondDataObs 的数据
    //只有在他两个都执行complete()之后，才会返回数据，且返回最后发送的数据
    console.log(datas);  //[ 'a2', 'b2' ]
});
```

如果某次请求数据需要依赖前一次请求的结果，也就是说两次请求必须有先后顺序，这时候可以使用`Observalbe.prototype.concatMap()`,示例：

```ts
//concatMap
let getFirstDataObs = Observable.create(observer=>{
    setTimeout(res=> { observer.next(1);}, 500);
    setTimeout(res=> { observer.next(2);}, 1000);
    setTimeout(res=> { observer.next(3);}, 1500);
    setTimeout(res=> { observer.complete();}, 2000);
});

let createSecondDataObs  = (firstData)=>{
    return Observable.create(observer=>{
        observer.next(firstData*10+1);
        observer.next(firstData*10+2);
        observer.next(firstData*10+3);
        observer.complete();
    });
};

let concatedObs = getFirstDataObs.concatMap(firstData=>{
    return createSecondDataObs(firstData);
}).subscribe(secondData=>{
    //do sth with second data
    console.log(secondData); 
});
```

结果输出：`11 12 13 21 22 23 31 32 33`

通过`Observable.prototype.concatMap()`方法，`getSecondDataObs()`的数据流被紧接在`getFirstDataObs()`的数据流后，并且最终被数据流被subscribe()所捕获。


## 工具操作符

在Observable.prototype中有很多有用的工具方法，统称为工具操作符，如`delay`,`timeout`,`debounce`,`take`,`takeUntil`,`takeLast`,`takeWhile`,`skip`等等
### timeout
`timeout`:当observable在指定时间内数据没有完全返回时，发出一个错误

```ts
//工具操作符timeout
let observable = Observable.create(observer => {
   observer.next(3);
   setTimeout(function() {
       observer.complete();
   }, 3000);
});

observable.timeout(2000).subscribe(data=>{
   //do sth with data
   console.log(data);
},err=>{
   handleError(err);
});
//当observable超过2000ms没有返回全部数据流（即返回complete时），便会抛出一个err错误，被handleError捕获。
```

### delay

`delay`: 返回一个新序列，但是每个序列数据延迟指定时间
![delay](http://ol1ftyec4.bkt.clouddn.com/2017-03-07-delay.png)

```ts
//延迟1000ms
observable.delay(1000).subscribe(data=>{
    console.log(data);
});
```

### debounceTime

`public debounceTime(time: number)`: 和`delay`一样，也会延迟发送数据，但仅发送这一段时间内最新的**一条**消息，如果原序列在这段时间内没有发送消息，就不发送。比如可以用在，前台界面搜索框延迟500ms后自动发起api请求进行查询，或后台数据返回的太过频繁。
![debounceTime](http://ol1ftyec4.bkt.clouddn.com/2017-03-07-debounceTime-1.png)

```ts
Observable.fromEvent(inputSelector,'keyup')
    .debounceTime(500)
    .switchMap(event=>getValue(event.target.value))
    .subscribe(callback);
```

### debounce

`public debounce(durationSelector: function(value: T): SubscribableOrPromise): Observable`：和`debounceTime`类似，但它的发送时机不是由延迟时间决定，而是由另一个`observable`决定,

```ts
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = clicks.debounce(() => Rx.Observable.interval(1000));
result.subscribe(x => console.log(x));
```

### take

`public take(count:number)`: 挑选出指定的前几个数据，作为新的序列，并complete
![take](http://ol1ftyec4.bkt.clouddn.com/2017-03-07-take.png)

```ts
var interval = Rx.Observable.interval(1000);
var five = interval.take(5);  //取出前5个数据
five.subscribe(x => console.log(x)); //0 1 2 3 4
``` 

### takeLast

`takeLast(count:number)`：挑选出原序列的最后count个数据，在complete时一起发送出去
![takeLast](http://ol1ftyec4.bkt.clouddn.com/2017-03-07-takeLast.png)

```ts
let interval = Rx.Observable.interval(1000).take(5);
five.takeLast(2).subscribe(x => console.log(x)); //挑选出最后2个，打印：3 4
```

### takeUntil

`public takeUntil(notifier: Observable): Observable<T>`： 筛选原序列中数据，直到notifier发射出一个数据为止.(Lets values pass until a second Observable, notifier, emits something. Then, it completes.)

```ts
var interval = Rx.Observable.interval(1000);
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = interval.takeUntil(clicks);  //一直从原序列中取数据，直到点击事件发生为止
result.subscribe(x => console.log(x));
```

### takeWhile

`public takeWhile(predicate: function(value: T, index: number): boolean): Observable<T>` ：满足某个条件时不再从原序列中取数据
![takeWhile](http://ol1ftyec4.bkt.clouddn.com/2017-03-07-takeWhile.png)


### skip

`public skip(count: Number): Observable`：跳过序列中的前`count`个数据

![skip](http://ol1ftyec4.bkt.clouddn.com/2017-03-07-skip.png)

### switchMap
`public switchMap(project: function(value: T, ?index: number): ObservableInput, resultSelector: function(outerValue: T, innerValue: I, outerIndex: number, innerIndex: number): any): Observable`: 将原序列某个数据转换为另一个observable的若干个序列数据，当元序列有新数据到来时，会截断旧数据

[switchMap官方说明](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-switchMap)

![switchMap](http://ol1ftyec4.bkt.clouddn.com/2017-03-08-switchMap.png)



## 结束

