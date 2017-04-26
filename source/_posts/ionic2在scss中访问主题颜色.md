---
title: ionic2在scss中访问主题颜色
date: 2017-04-26 09:37:20
tags: [ionic2]
---

通过在`scss`中引用主题颜色，可以增加程序的可维护性，当程序的主题颜色更改时，引用的地方随之自动更改。

使用`background-color: map-get($colors, primary)`来引用在主题中定义的颜色。

示例：

```css
.progress-inner {
        min-width: 15%;
        white-space: nowrap;
        overflow: hidden;
        height: 4px;
        border-radius: 10px;
        background-color: map-get($colors, primary);
    }
```



