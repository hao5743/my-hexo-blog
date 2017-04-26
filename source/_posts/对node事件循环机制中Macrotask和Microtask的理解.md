---
title: 对node事件循环机制中Macrotask和Microtask的理解
date: 2017-02-27 10:38:23
tags: [javascript, node]
---

## node事件循环机制中的`Microtask`和`Macrotask`?

### 他是什么？
在Nodejs事件循环机制中，有任务两个队列：`Macrotask队列`和`Microtask队列`。在一个事件循环里，这两个队列会分两步执行，第一步会固定地执行一个（且仅一个）Macrotask任务，第二步会执行整个Microtask队列中的所有任务。并且，在执行Microtask队列任务的时候，也允许加入新的Microtask任务，直到所有Microtask任务全部执行完毕，才会结束循环。

Macrotasks一般包括: `setTimeout`, `setInterval`, `setImmediate`, `I/O`, `UI rendering`；
Microtasks一般包括: `process.nextTick`, `Promises`, `Object.observe`, `MutationObserver`。

### node事件循环机制详解
从一个事件循环开始，到结束会经历以下步骤：

1. 检查Macrotask队列，选择其中最早加入（即最老的）的任务X，设置为“目前运行的任务”。如果任务X不存在，那么直接跳到步骤4。
2. 运行任务X，即运行对应的回调函数。
3. 设置“目前运行的任务”为null，从Macrotask队列中移除任务X。
4. 检查Microtask队列：

    1. 选择其中最老的任务a，如果任务a不存在，直接结束Microtask队列。
    2. 设置任务a为“目前运行的任务”，并执行。
    3. 设置“目前运行的任务”为null，从Microtask队列中移除任务a。
    4. 选择下一个最老的任务b，跳到步骤2）。
    5. 直到队列里没有剩余的任务，结束队列。
5. 跳回步骤1，检查下一个Macrotask任务。

### 如何选用Macrotask或Microtask呢？
***如果你想让一个任务立即执行，那么就把它设置为Microtask，除此之外都用Macrotask比较好。***

因为可以看出，虽然Node是异步非阻塞的，但**在一个事件循环中，Microtask的执行方式基本上就是用同步的**。

### 有什么问题？

如果`Microtask`任务列表太长，或不断加入新的`Microtask`，就会导致下一个`Macrotask`任务很久得不到执行，可能导致UI一直无法刷新或IO无法完成。

应该是考虑到了这一点，至少Microtask任务中的`process.nextTick`任务，是被设置了（在一个事件循环中的）最大调用次数的，叫`process.maxTickDepth`，默认是1000。一定程度上避免了上述情况。


### 参考文档：

[Node.js事件循环中的：Macrotask 与 Microtask](https://segmentfault.com/a/1190000007710772)

[Node.js事件驱动实现概览](https://segmentfault.com/a/1190000002914296)

