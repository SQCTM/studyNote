# let和const

[TOC]

## let

- **所声明的变量只在let命令所在的代码块内有效**

  ```javascript
  {
      let a = 10;
      var b = 1;
  }
  
  console.log(a);  //Reference Error: a is not defined
  console.log(b);  //1
  ```

  - **for循环中的let声明**

    - 循环中let声明的i只在本轮循环有效，每一次循环的i其实都是一个新的变量

      ```javascript
      //var声明
      var a = [];
      for(var i = 0; i < 10; i++) {
          a[i] = function() {
              console.log(i);
          }
      }
      a[6](); //10
      //以var声明的i在全局范围内都有效，所以全局只有一个变量i，所以每次循环变量i的值都会发生改变
      
      //let声明
      var a = [];
      for(let i = 0; i < 10; i++) {
          a[i] = function() {
              console.log(i);
          }
      }
      a[6](); //6
      //以let声明的变量i只在本轮循环有效，每一次循环的i都是一个新的变量。JS引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮的循环的基础上进行计算
      ```

    - for循环中设置循环变量的那部分是一个父作用域，循环体内部是一个单独的子作用域

      ```javascript
      for(let i = 0; i < 3; i++) {
          let i = 'abc';
          console.log(i);  //输出三次'abc'
      }
      ```

      循环内部变量i和循环变量i不在一个作用域，而是有各自单独的作用域

- **不存在变量提升**

  ```javascript
  //var
  console.log(foo); //输出undefined
  var foo = 2;
  
  //let
  console.log(bar);  //报错ReferenceError
  let bar = 2;
  ```

- **暂时性死区**

  只要块级作用域内存在let或const命令，它所声明的变量就”绑定“这个区域，不再受外部的影响。本质是只要进入当前作用域所要使用的变量就已经存在，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量

  ```javascript
  var tmp = 123;
  if(true) {
      tmp = 'abc';  //报错ReferenceError
      let tmp;
  }
  ```

  在let命令声明变量之前，该变量都是不可用的，即暂时性死区

  - **typeof操作出现问题**

    一般情况下如果一个变量没有被声明过使用typeof不会报错，但如果在let声明变量前对该变量使用typeof反而会报错，因为在let声明前都属于该变量的死区，只要用到该变量就会报错

    ```javascript
    typeof x;  //报错ReferenceError
    let x;
    
    typeof y;  //undefined
    ```

  - **在let声明执行完成前获取变量报错**

    ```javascript
    var a = a;  //不报错
    
    let x = x;  //报错Reference Error: x is not defined
    ```

    在变量x声明语句还没有执行完成前就尝试获取x的值，x依然处于暂时性死区，所以会报错

- **不允许重复声明**

  ```javascript
  function () {
      //报错
      let a =10;
      var a = 1;
      //报错
      let b = 10;
      let b = 1;
  }
  ```



## 块级作用域

- **外层作用域无法读取内层作用域的变量**

  ```javascript
  {
      {let insane = 'Hello world';}
      console.log(insane);  //报错
  };
  ```

- **内层作用域可以定义外层作用域的同名变量**

  ```javascript
  {
      let insane = 'Hellow world';
      {let insane = 'Hello world';}
  };
  ```

- **有了块级作用域不再需要立即执行匿名函数**

  ```javascript
  //立即执行匿名函数
  (function() {
      var tmp = 'Hello world';
      //...
  }());
  
  //块级作用域
  {
      let tmp = 'Hello world';
      //...
  }
  ```

- **允许在块级作用域内声明函数**

  ES5中函数只能在顶层作用域和函数作用域中声明，不能在块级作用域声明；ES6明确允许在块级作用域之中声明函数，但**在块级作用域之外不可引用**，具体行为方式如下：

  - 允许在块级作用域内声明函数
  - 函数声明类似于var， 即会提升到全局作用域或函数作用域的头部
  - 同时函数声明还会提升到所在块级作用域的头部

  ```javascript
  function f() { console.log("I am outside!"); }
  (function f() {
      if(false) {
          function f() { console.log("I am inside"); }
      }
      
      f();  //Uncaught TypeError: f is not a function
  })
  
  //实际运行代码
  function f() { console.log("I am outside!"); }
  (function f() {
      var f = undefined;
      if(false) {
          function f() { console.log("I am inside"); }
      }
      
      f(); 
  })
  ```

  因此应该避免在块级作用域内声明函数，如果确实需要，也**应该写成函数表达式形式**，而不是函数声明语句

​       *ES6的块级作用域允许声明函数的规则**只在使用大括号的情况下成立**，如果没有使用大括号会报错



## const

