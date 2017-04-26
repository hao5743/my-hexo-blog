---
title: Angular使用iframe加载外部网站
date: 2017-04-26 09:32:53
tags: [Angular]
---

## 问题
项目中，需要使用iframe来加载一个外部网页，但src被Angular的默认安全策略所禁止，无法加载网页。

## 解决
在Angular中，可以使用iframe用来加载一个外部链接地址，但是src需要使用`@angular/platform-browser`中的`DomSanitizer模块`中的`sanitizer.bypassSecurityTrustResourceUrl(url)`方法来特殊处理，因为Angular中有默认的安全规则会阻止链接的加载。

一个例子：

html:

```html
<!--这样无法正常加载网页，被安全策略禁止-->
<iframe [src]="link" width="100%" height="100%" frameborder="0" align="center"></iframe>
<!--需要这样使用-->
<iframe id='MainIframe' [src]="trust(link)" width="100%" height="100%" frameborder="0" align="center"></iframe>
<!--或直接处理-->
<iframe id='MainIframe' [src]="sanitizer.bypassSecurityTrustResourceUrl(link)" width="100%" height="100%" frameborder="0" align="center"></iframe>
        
```

ts:

```ts

//引入DomSanitizer模块
import { DomSanitizer } from '@angular/platform-browser';

...

link:string = 'http://www.baidu.com';

constructor(private sanitizer: DomSanitizer) {
    console.log('Hello IframeFull Component');
}

...

trust(url) {
    return this.sanitizer.bypassSecurityTrustResourceUrl(url);
}
```

> 测试时间：2017-04-26 
> 使用Angular版本： 2.2.1

