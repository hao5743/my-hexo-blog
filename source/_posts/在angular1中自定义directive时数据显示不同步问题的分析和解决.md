---
title: 在angular1中自定义directive时数据显示不同步问题的分析和解决
date: 2016-12-05 16:33:38
tags: [angular1, javascript]
categories: angular
---

# 在angular1中自定义directive时数据显示不同步问题的分析和解决

2016-12-05 16:33:38

在angular1项目中，有的时候我们需要写`自定义directive`来完成某些应用逻辑，但是如果你有过一些的angular1项目的开发经验，肯定会因为一些莫名其妙数据双向绑定，回调函数无法调用执行等问题，困扰过。下面是一个简单的案例。
### 问题引出
这里我们写一个简单的`directive`来说明问题。

directive.js

```

angular.module('app.directive')
.directive('ayTest',function(){
    return {
        restrict: 'EA',
        scope: {
            name:'='
        },
        template:`
            <div class="padding mystyle">
                {{vm.name}}
            </div>
        `,
        controller:function($scope){
            var vm = $scope.vm = {};
            vm.name = $scope.name;

        }
    }

});
```

A.controller.js

```javascript
	var vm = $scope.vm = {};
    vm.name = 'Tom1';
    $timeout(function(){
        vm.name = 'Tom2';
    },1000);
    $timeout(function(){
        vm.name = 'Tom3';
    },2000);
```

A.html

```
   <ay-test name="vm.name"></ay-test>
```

这个directive可以实现以我们自己的格式展示‘name’的功能，我们可以通过在template中填写自己独有的style或者嵌套div来完善name展示模块。

在这我们希望`name`能够实现双向绑定，即当在A页面中`name`值发生变化的时候，directive也自动获得更改。

但是，在这里并不能实现这个功能，当`name`显示为`Tom1`之后，就不会再变化了。

###为什么会这样呢？

我们都知道自定义directive时，有3个绑定符号，分别是

+ `@` 单向绑定属性值
+ `=` 双向绑定属性
+ `&` 绑定用户函数

这里为了实现信息的双向绑定，已经使用了`=`，为什么还无法完成双向绑定呢？对于大神们，当然能一眼看出问题的所在，但小白可能会摸索比较长的时间~~~~

*问题出在`vm.name = $scope.name;`这句代码上*。因为name是一个基本字符串，赋值时，相当于对vm.name创建了一个新值，并赋值为`$scope.name`的值。这样，虽然controller中的`$scope.name`和页面上的`{{name}}`仍存在双向绑定，但是和我们的`vm.name`却无任何关系了。

我们应该还知道下面的这个问题：

```
//controller.js
$socpe.username = 'Mary';
//B.html
<input ng-model='username' />
```
如果你按照上面的方式来编码，十有八九会出现意外情况。

出现这样错误的根本原因，其实还是对JavaScript变量的值访问、引用访问，开发中的值传递、引用传递没有理解清楚。下面说一些这方面的问题。

### Js基本类型、值传递
下面是笔者摘自《JavaScript高级程序设计》中的几段话，读者可以参照理解。这里并不是笔者自己懒，而书中的表达更准确，而且也很容易理解~~(如果你说不知道这本书，推荐快去读一下，如果你做Js相关开发，会让你受益匪浅，我能给你电子版，联系`hao5743@163.com` )

> 5种基本数据类型：Undefined、Null、Boolean、Number和String，**5种基本数据类型是按值访问**的，因为可以操作保存在变量中的实际的值。
> 
> 引用类型的值是保存在内存中的对象。于其他语言不同，JavaScript不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。**引用类型的值是按引用访问**的。
>
> --*参见《JavaScript高级程序设计》P69 4.1 基本类型和引用类型的值*
>
>
> 如果从一个变量向另一个变量复制基本类型的值，会在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上。
>
> --*参见《JavaScript高级程序设计》P69 4.1.2 复制变量值*
>
> **ECMAScript中所有函数的参数都是按值传递的**。也就是说，把函数外部的值赋值给函数内部的参数，就和把值从一个变量复制到另一个变量一样。基本类型值的传递如同基本类型变量的复制一样，二引用类型值的传递，则如同引用类型变量的复制一样。有不少开发人员在这一点可能会感到困惑，因为访问变量有按值和按引用两种方式，而参数只能按值传递。
>
> --*参见《JavaScript高级程序设计》P70 4.1.3 传递参数*

### 解决方案

理解了上面的话，那么你就能轻易用不同的方案解决这个问题了。

#### 方案1 直接使用`$scope.name`展示数据

directive.js

```

angular.module('app.directive')
.directive('ayTest',function(){
    return {
        restrict: 'EA',
        scope: {
            name:'='
        },
        //这里直接使用name，不再使用vm.name
        template:`
            <div class="padding mystyle">
                {{name}}
            </div>
        `,
        controller:function($scope){
            var vm = $scope.vm = {};

        }
    }

});
```
不再使用vm的方式，而是直接绑定到页面，可以解决问题。

#### 方案2 使用引用类型打包基本类型再传递

如果，你在angular1开发中，一直遵循了好的实践，那么有可能`vm.name`你可能已经使用习惯了。也许你会问，如果我还想使用`vm.name`的方式，我想遵循官方给出的最佳实践，怎么解决这个问题呢？
> （什么是最佳实践？如果你是angular开发者，而还不知道的话，那么一定要去读读，原版[在这里](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md)，英文差的[看这里](http://www.tuicool.com/articles/JbuErq)）

当然有方法啦！看下面
controller.js

```
    var vm = $scope.vm = {};
    vm.name = {};
    //将数据封装到到内部属性data上
    vm.name.data = 'Tom1';
    $timeout(function(){
        vm.name.data = 'Tom2';
    },1000);
    $timeout(function(){
        vm.name.data = 'Tom3';
    },2000);
```
A.html

```
    <ay-test name="vm.name"></ay-test>
```

directive.js

```
.directive('ayTest',function(){
    return {
        restrict: 'EA',
        scope: {
            name:'='
        },
        template:`
            <div class="padding">
                {{vm.name.data}}
            </div>
        `,
        controller:function($scope){
            var vm = $scope.vm = {};
            //注意，虽然这里重新赋值了name,但是我们的vm.name.data还是同一份，所以仍能实现绑定
            vm.name = $scope.name;

        }
    }

});
```

#### 方案3 使用`$scope.$watch`

如果你知道`$scope.$watch`，并且了解何时使用它，那么你应该知道这里也可能使用它来解决。看代码。

controller.js

```
    var vm = $scope.vm = {};
    //并没有进行数据封装
    vm.name = 'Tom1';
    $timeout(function(){
        vm.name = 'Tom2';
    },1000);
    $timeout(function(){
        vm.name = 'Tom3';
    },2000);
```
A.html

```
    <ay-test name="vm.name"></ay-test>
```

directive.js

```
angular.module('app.directive')
.directive('ayTest',function(){
    return {
        restrict: 'EA',
        scope: {
            name:'='
        },
        template:`
            <div class="padding">
                {{vm.name}}
            </div>
        `,
        controller:function($scope){
            var vm = $scope.vm = {};
            //添加数据监测
            $scope.$watch('name',function(newValue,oldValue){
                vm.name = $scope.name;
            });

        }
    }
});

```
这个方法其实相当于多加了一层的监测，`vm.name`和页面上的`{{vm.name}}`绑定，通过`$scope.$watch`，我们让`vm.name`和`$scope.name`两个变量的值保持一致性。

#### 哪个方法好呢
个人推荐第一种方法，因为他所做的工作最少，效率相对较高（虽然高不了很多）。

### 最后的话

本文首先提出了在编写自定义directive时有时候会出现的数据无法同步的问题。随后分析了问题，并给出了几种解决方案。

其实，问题的根本首先在于对JavaScript变量`值访问`、`引用访问`和`值传递`等概念的理解，其次在于对angular1`scope层级`、`双向绑定`、`脏值检测`等概念的理解。

有时，还能需要处理其他情况，比如默认值处理，分条件展示，错误值处理、数据回显、回调函数处理等具体问题，可以参考上面3种思路来具体实现。

很简单一个问题，个人只是做了一下分析，有错误的地方请大神及时指出，感激不尽~~

>Author：shaochong
>
>Email：hao5743@163.com
>
>文章地址：[https://hao5743.github.io/2016/12/05/Data-bind-problem-in-Angular-when-define-your-own-directives/](https://hao5743.github.io/2016/12/05/Data-bind-problem-in-Angular-when-define-your-own-directives/)
>
>博客：[http://hao5743.github.io/](http://hao5743.github.io/)


