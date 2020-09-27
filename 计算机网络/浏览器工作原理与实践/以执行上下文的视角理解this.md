# 以执行上下文的视角理解this

[TOC]

## 三种this

- **全局执行上下文中的this**

  全局执行上下文的this是指向window对象的，这是this和作用域的唯一交点

- **函数执行上下文中的this**
  
  默认情况下调用一个函数其执行上下文中的this指向window对象，但可以设置this值指向其他对象
  
  - **函数中this值设置方法**
    
    - **通过函数call方法设置**
    
      ```javascript
      let bar = {
          myName: "123",
          test1: 1
      }
      function foo() {
          this.myName = "456";
      }
      foo.call(bar);  //this指向bar，bar中变量myName值变为456
      console.log(bar.myName);  //456  
      ```
    
      *除了call，**bind**和**apply**方法也可以设置函数中this值
    
    - **通过对象调用方法设置**
    
      ```javascript
      //例子1
      var myObj = {
          name: "123",
          showThis: function() {
              console.log(this);
          }
      }
      myObj.showThis();  //this指向myObj
      //使用对象来调用其内部一个方法，该方法的this指向对象本身
      
      //例子2
      var myObj = {
          name: "123",
          showThis: function() {
              this.name = "456";
              console.log(this.name);
          }
      }
      var foo = myObj.showThis;
      foo();  //456，this指向window
      //在全局环境中调用一个函数，函数内部的this指向的是全局变量window
      ```
    
    - **通过构造函数设置**
    
      ```javascript
      function CreateObj() {
          this.name = '123';
      }
      var myObj = new CreateObj();
      console.log(myobj);  //{name: "123"}
      ```
    
      构造函数时js引擎流程：
    
      - 创建一个临时对象
      - 调用CreateObj.call方法，并将创建的临时对象作为call方法的参数；CreateObj函数执行上下文创建时this值指向临时对象
      - 执行CreateObj函数，函数执行上下文中this指向临时对象
      - 返回临时对象
  
- **eval中的this**



## this的设计缺陷以及应对方案

- **嵌套函数中的this不会从外层函数中继承**

  ```javascript
  var myObj = {
      name: "123",
      showThis: function() {
          console.log(this);  //指向myObj对象
          function bar() {
              console.log(this)  //指向全局window对象
          }
          bar();
      }
  }
  myObj.showThis();
  
  //解决方法1
  var myObj = {
      name: "123",
      showThis: function() {
          console.log(this.name);  
          var self = this;
          function bar() {
              self.name = "456";  
          }
          bar();
      }
  }
  myObj.showThis();  //123
  console.log(myObj.name); //456
  //声明一个变量self用来保存this，本质是把this体系转换为作用域的体系
  
  //解决方法2
  var myObj = {
      name: "123",
      showThis: function() {
          console.log(this.name); 
          var bar = () => {
              this.name = "456";  
              console.log(this.name)
          }
          bar();
      }
  }
  myObj.showThis();  //123 456
  console.log(myObj.name);  //456
  //ES6中的箭头函数不会创建其自身的执行上下文,因此bar函数中this指向myObj对象
  ```

- **普通函数中的this默认指向全局对象window**

  有时候不方便。但在js**严格模式下**默认执行一个函数，该函数执行上下文中的**this值是undefined**
  
  