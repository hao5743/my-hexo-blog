---
title: Angular笔记 - 属性型指令
date: 2017-02-22 16:52:47
tags: [Angular]
---

## 简介
属性型指令用于改变DOM元素的外观或行为。

在Angular中有三种类型的指令：

1. 组件 — 拥有模板的指令
2. 属性型指令 — 改变元素外观或行为，例如内置的 `NgStyle` 指令可以同时修改元素的多个样式
3. 结构型指令 — 通过添加和移除dom元素改变视图结构，例如，`NgFor` 和 `NgIf`。

## 一个简单的属性型指令
我们这里实现了一个简单的属性型指令，说一下几个重点的内容

功能：当用户鼠标悬浮在元素上时，改变它的背景颜色

代码：

highlight.directive.ts

```ts
import { Directive, ElementRef, HostListener, Input, Renderer } from '@angular/core';
@Directive({
  selector: '[myHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef, private renderer: Renderer) { }
  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }
  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }
  private highlight(color: string) {
    this.renderer.setElementStyle(this.el.nativeElement, 'backgroundColor', color);  
  }
}
```
a.html

```html
<h1>My First Attribute Directive</h1>
<p myHighlight>Highlight me!</p>
```

说明：

* `@Directive`装饰器需要一个 CSS 选择器，以便从模板中识别出关联到这个指令的 HTML，这里指令的选择器是`[myHighlight]`，Angular 将会在模板中找到所有带`myHighlight属性`的元素。
* 需要把 Angular 的`ElementRef`和`Renderer`注入进构造函数。`ElementRef`是一个服务，它赋予我们通过它的`nativeElement`属性直接访问 DOM 元素的能力。 `Renderer`服务允许通过代码设置元素的样式。
* 需要把这个类添加到 NgModule 元数据的`declarations数组`中

> EXCEPTION: Template parse errors:
  Can't bind to 'myHighlight' since it isn't a known property of 'p'.
  
  错误原因:你记着设置`@NgModule`的`declarations数组`了吗？它很容易被忘掉

* `@HostListener`装饰器引用属性型指令的宿主元素，在这个例子中就是`<p>`


## 高级点的属性型指令

对上面的那个指令添加功能：

1. 通过绑定从外部设定这个颜色 
2. 从外部设定一个默认颜色，如果没有传入颜色，则使用这个默认颜色
3. 这样使用这个指令
    
    ```html
    <p [myHighlight]="color" [defaultColor]="'violet'">
        Highlight me too!
    </p>
    ```
    
代码：

highlight.directive.ts

```ts
import { Directive, ElementRef, HostListener, Input, Renderer } from '@angular/core';
@Directive({
  selector: '[myHighlight]'
})
export class HighlightDirective {
  private _defaultColor = 'red';
  constructor(private el: ElementRef, private renderer: Renderer) { }
  @Input() set defaultColor(colorName: string){
    this._defaultColor = colorName || this._defaultColor;
  }
  @Input('myHighlight') highlightColor: string;
  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || this._defaultColor);
  }
  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }
  private highlight(color: string) {
    this.renderer.setElementStyle(this.el.nativeElement, 'backgroundColor', color);
  }
}
```

a.html

```html
<div>
  <input type="radio" name="colors" (click)="color='lightgreen'">Green
  <input type="radio" name="colors" (click)="color='yellow'">Yellow
  <input type="radio" name="colors" (click)="color='cyan'">Cyan
</div>
<p [myHighlight]="color">Highlight me!</p>
<p [myHighlight]="color" [defaultColor]="'violet'">
  Highlight me too!
</p>
```

说明：

* 新的`highlightColor`属性被称为输入属性，因为数据是从绑定表达式流入指令中。 注意，在定义这个属性的时候，我们调用了`@Input()`装饰器，`@Input`向类添加元数据，使`highlightColor`属性能以`myHighlight`为别名进行绑定
* 你可以通过重命名属性名到myHighlight来移除这个区别，像这样：`@Input() myHighlight: string;`
* `defaultColor`属性是一个 `setter` 函数，它代替了硬编码的默认颜色 “red”,它不需要 getter 函数。这里，把字符串字面量`'violet'`绑定到了`defaultColor`上。


## 关于 输入属性的源和目标
Angular 在绑定的源和目标之间的区别：

* 如果属性出现在了模板表达式等号 (=) 的右侧，它就是一个`源`。

* 如果它出现在了方括号 ([ ]) 中，并且出现在等号 (=) 的左侧，它就是一个`目标`， 就像在绑定到HighlightDirective的myHighlight属性时所做的那样。

例如：

```html
<p [myHighlight]="color">Highlight me!</p>
```
`[myHighlight]="color"`中的 `'color' `是绑定源。 源属性不需要声明。

`[myHighlight]="color"`中的 `'myHighlight'` 是绑定目标。 必须把它定义为一个输入属性，否则，Angular 就会拒绝绑定，并给出一个明确的错误。

Angular 这样区别对待目标属性有充分的理由。 作为目标的组件或指令需要保护。

