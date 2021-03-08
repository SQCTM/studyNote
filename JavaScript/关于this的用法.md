# 关于this的用法

[TOC]

## 什么是this

`this`是 JavaScript 语言的一个关键字，它是函数运行时，在函数体内部自动生成的一个对象，即函数运行时所在的**环境对象**，只能在函数体内部使用



## 不同使用场合

函数的不同使用场合，`this`有不同的值



### 纯粹的函数调用

属于全局性调用，因此`this`就代表**全局对象**

```javascript
var x = 1;
function test() {
   console.log(this.x);
}
test();  // 1

//函数中的函数也是
var x = 1;
function test()
{
    var x = 2;
    (function(){
        console.log(this.x);
    })();
}
test();   //1
```



### 作为对象方法的调用

函数作为某个对象的方法调用时，`this`就指向这个**上级对象**

```javascript
function test() {
  console.log(this.x);
}

var obj = {};
obj.x = 1;
obj.m = test;

obj.m(); // 1
```



### 作为构造函数调用

`this`指向通过构造函数产生的新对象

```javascript
var x = 2;

function test() {
  this.x = 1;
}

var obj = new test();
x  // 2
obj.x // 1
```



### apply、call和bind方法调用

`apply()`方法的作用是改变函数的调用对象。它的第一个参数就表示改变后调用这个函数的对象。因此，这时this指的就是这第一个参数
当`apply()`的参数为空时，默认调用全局对象

```javascript
var x = 0;
function test() {
　console.log(this.x);
}

var obj = {};
obj.x = 1;
obj.m = test;
obj.m.apply() // 0
obj.m.apply(obj); //1
```
`call()`和`bind()`方法同理



### ES6箭头函数

箭头函数没有自己的`this`，所以**内部的this就是外层代码块的this**