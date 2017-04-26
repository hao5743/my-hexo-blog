---
title: TypeScript - 类
date: 2017-02-22 16:04:45
tags: [typescript]
---


# 类

 从ECMAScript 2015，也就是ECMAScript 6开始，JavaScript程序员将能够使用基于类的面向对象的方式。 使用TypeScript，我们允许开发者现在就使用这些特性，并且编译后的JavaScript可以在所有主流浏览器和平台上运行，而不需要等到下个JavaScript版本。

## 创建一个类

```ts
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```
## 继承

* 使用`extend`关键字来创建子类
* 子类构造函数中使用`super()`，执行父类的构造方法

```ts
class Animal {
    name:string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```
## public、private、protected修饰符

* 在ts中，成员类型默认为`public`

* 当成员被标记成`private`时，它就不能在声明它的类的外部访问

* `protected`修饰符与`private`修饰符的行为相似，但**`protected`成员在派生类中仍然可以访问**

* 构造函数可以被标记成`protected`，这是这个类不可以在类外进行实例化，但是能被继承并在子类中实例化。

```ts
 class Person {
    protected name: string;
        protected constructor(theName: string) { this.name = theName; }
    }
    
    // Employee can extend Person
    class Employee extends Person {
        private department: string;
    
        constructor(name: string, department: string) {
            super(name);
            this.department = department;
        }
    
        public getElevatorPitch() {
            return `Hello, my name is ${this.name} and I work in ${this.department}.`;
        }
    }
    
    let howard = new Employee("Howard", "Sales");
    let john = new Person("John"); // Error: The 'Person' constructor is protected
```
## readonly修饰符

* `readnoly`修饰符，设置属性为只读的
* 只读属性必须在声明时或构造函数里被初始化

## 参数属性
使用参数属性可以方便地在一个地方创建并初始化一个成员,可以简化代码

```ts
class Animal {
    constructor(private name: string) { }
    move(distanceInMeters: number) {
        console.log(this.name);
    }
}
```
上面仅在构造函数里使用`private name: string`参数来创建和初始化name成员,把声明和赋值合并至一处。

等价于

```ts
class Animal {
    private name:string
    constructor(theName: string) { 
        this.name=theName;
    }
    move(distanceInMeters: number) {
        console.log(this.name);
    }
}
```

## 存取器 getters/setters

```ts
class Person{
    private _name:string;
    get name():string{
        return this._name;
    }

    set name(name:string){
        this._name=name;
    }
}

let p = new Person();
p.name='tom';        //调用set
console.log(p.name); //调用get
```
要注意几点：

* 使用`tsc file.ts --target es5`编译，因为它只支持ECMAScript 5或更高
* 只带有 `get`不带有`set`的存取器自动被推断为readonly

用在哪里？
可以用在存取时需要做一些额外操作，而不是直接赋值的地方。

## 静态属性
* `static`可以创建类的静态成员，这些属性存在于类本身，而不是类的实例上
* 使用`类名.静态成员名`格式访问类的静态成员

## 抽象类
* 抽象类一般不会直接实例化，而是作为派生类的基类。
* 与接口不同的地方：抽象类可以包含成员的实现细节。
* `abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法。

```ts
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```
注意：
**抽象类中的抽象方法不包含具体实现**，而且**它必须要在派生类中实现**

## 高级技巧
### 构造函数
在创建ts的一个类时，声明了几个东西：

* 类的`实例类型`
* 创建了一个叫做`构造函数的值`


a.ts
```ts
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter: Greeter;
greeter = new Greeter("world");
console.log(greeter.greet());
```

上面的代码使用`tsc a.ts` 编译后得到的js代码：

```js
let Greeter = (function () {
    function Greeter(message) {
        this.greeting = message;
    }
    Greeter.prototype.greet = function () {
        return "Hello, " + this.greeting;
    };
    return Greeter;
})();

let greeter;
greeter = new Greeter("world");
console.log(greeter.greet());
```

### 把类当做接口使用
类定义会创建两个东西：`类的实例类型`和`一个构造函数`。 因为**类可以创建出类型**，所以你能够在允许使用接口的地方使用类。

```ts
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```



