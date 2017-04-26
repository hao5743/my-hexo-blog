---
title: es6 扩展运算符 三个点（...） 
date: 2017-02-17 14:46:18
tags: [es6]
---

# 它是什么
es6中引入扩展运算符（...），它用于把一个数组转化为用逗号分隔的参数序列，它常用在不定参数个数时的函数调用，数组合并等情形。因为typeScript是es6的超集，所以**typeScript也支持扩展运算符**。

# 用在哪儿
### 可变参数个数的函数调用

```js
function push(array, ...items) {  
  array.push(...items);  
}  

function add(...vals){
  let sum=0;
  for(let i=0;i<vals.length;i++){
    sum+=vals[i];
  }
  return sum;
}

let arr = [1,2,3,4,5,6];
let sum = add(...arr);
console.log(sum);  //21
```


### 更便捷的数组合并

```js
let arr1 = [1,2];
let arr2 = [5,6];
let newArr = [20];
//es5 旧写法
newArr = newArr.concat(arr1).concat(arr2); //[20,1,2,5,6]
console.log(newArr);
//es6 使用扩展运算符
newArr = [20,...arr1,...arr2];  //[20,1,2,5,6]
console.log(newArr);

```

### 替代es5的apply方法

```js
// ES5 的写法  
function f(x, y, z) {  
// ...  
}  
var args = [0, 1, 2];  
f.apply(null, args);  
// ES6 的写法  
function f(x, y, z) {  
// ...  
}  
var args = [0, 1, 2];  
f(...args);  
```

求最大值Math.max()

```js
// ES5 的写法  
Math.max.apply(null, [14, 3, 77])  
// ES6 的写法  
Math.max(...[14, 3, 77])  
//  等同于  
Math.max(14, 3, 77);  
```

通过push函数，将一个数组添加到另一个数组的尾部

```js
// ES5 的写法  
var arr1 = [0, 1, 2];  
var arr2 = [3, 4, 5];  
Array.prototype.push.apply(arr1, arr2);  
// ES6 的写法  
var arr1 = [0, 1, 2];  
var arr2 = [3, 4, 5];  
arr1.push(...arr2);  
```

新建Date类型

```js
// ES5  
new (Date.bind.apply(Date, [null, 2015, 1, 1]))  
// ES6  
new Date(...[2015, 1, 1]);  
```

### 与解构赋值结合，生成新数组

```js
// ES5  
a = list[0], rest = list.slice(1)  
// ES6  
[a, ...rest] = list 
下面是另外一些例子。  
const [first, ...rest] = [1, 2, 3, 4, 5];  
first // 1  
rest // [2, 3, 4, 5]  
const [first, ...rest] = [];  
first // undefined  
rest // []:  
const [first, ...rest] = ["foo"];  
first // "foo"  
rest // [] 
```

### 将字符串转为真正的数组

```js
[...'hello']  
// [ "h", "e", "l", "l", "o" ]  
```

### 将实现了 Iterator 接口的对象转为数组

```js
var nodeList = document.querySelectorAll('div');  
var array = [...nodeList];  
```


-------
end 2017-02-17 14:46:14

