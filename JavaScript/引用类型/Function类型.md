# Function类型

[TOC]

## 函数的创建

- **声明语句**

  ```javascript
  function sum (num1, num2) {     
      return num1 + num2; 
  }
  ```

  *函数声明语句最好不要在语句块中使用

- **表达式语句**

  ```javascript
  //创建匿名函数
  var sum = function(num1, num2){     
      return num1 + num2; 
  };
  
  //创建命名函数
  var result = (function sum(num1, num2){
      return num1+num2;
  })
  ```

  - 表达式语句创建函数主要用于：
    - 赋值给变量
    - 赋值给对象的属性
    - 作为参数传给另一个函数
  - 命名函数：
    - 有**name属性**，值即为**函数名**的字符串，而匿名函数name值为undefined
    - **函数名是一个指向函数对象的指针**，用表达式语句创建的命名函数可在函数内部调用函数名，但不能在函数外部调用（声明语句创建的函数内部外部都可以调用
    - 常用于递归

- **箭头函数**（ES6）

  - 是匿名函数，不同的是，name属性值为**空字符串**
  - 很多地方可以简写
  - 不会创建上下文执行环境
  - 没有arguments对象

- **函数生成器**（ES6）

  - 可以保存函数执行状态

  - yield返回的不是函数，是**迭代器对象**，需要被赋值给变量或对象属性

  - 返回的迭代器对象有**next()**方法：

    - 第一次调用next()，从生成器最顶部开始执行代码直至第一个yield，将结果以对象的形式返回。再次调用时，则从当前yield开始执行至下一个yield。
    - 返回的对象中有**value**和**done**两项属性，value即返回值，done则是判断是否执行至最后一个yield，否则是false，是则是true

    ```javascript
    function* func(){
       console.log("one");
       yield '1';
       console.log("two");
       yield '2'; 
       console.log("three");
       return '3';
    }
    
    var f=new func();
    f.next();
    // one
    // {value: "1", done: false}
     
    f.next();
    // two
    // {value: "2", done: false}
     
    f.next();
    // three
    // {value: "3", done: true}
     
    f.next();
    // {value: undefined, done: true}
    ```

- **函数构造器**（不推荐）

  - 即new Function()
  - 函数体要用以字符串的形式传入
  - 数据的安全性和函数的性能有很大问题
  - 只在全局作用域下创建，易命名污染



## 函数的内部属性

- **arguments**

  - 是一个类数组对象，包含着传入函数中的所有参数

  - 有一个callee属性，是一个指针，指向拥有这个 arguments 对象的函数。

    ```javascript
    //下面的阶乘函数函数的执行与函数名 factorial 紧紧耦合在了一起
    function factorial(num){     
        if (num <=1) {         
            return 1;     
        } 
        else {         
            return num * factorial(num-1)     
        } 
    }
    
    //为了消除这种紧密耦合的现象，可以使用arguments.callee
    function factorial(num){     
        if (num <=1) {         
            return 1;     
        } 
        else {        
            return num * arguments.callee(num-1)    
        } 
    }
    ```

    *在严格模式下，访问 arguments.callee 会导致错误

- **this**

  引用的是函数执行的环境对象

  ```javascript
  window.color = "red"; 
  var o = { color: "blue" }; 
   
  function sayColor(){     
      alert(this.color); 
  } 
  
  //当在网页的全局作用域中调用函数时，this对象引用的就是window
  sayColor();     //"red" 
   
  o.sayColor = sayColor; 
  //this引用的变成了对象o
  o.sayColor();   //"blue"
  ```

- **caller**（ES5）

  属性中保存着**调用当前函数的函数的引用**， 如果是在全局作用域中调用当前函数，它的值为null。

  ```javascript
  function outer(){     
      inner(); 
  } 
   
  function inner(){   
      alert(inner.caller);   
      // alert(arguments.callee.caller); 
  } 
   
  outer();
  
  // inner.caller指向outer()。警告框中会显示outer()函数的源代码。
  ```

  *严格模式下，不能为函数的 caller 属性赋值，否则会导致错误。 



## 函数的属性与方法

### 属性

- **length**

  表示函数**形参的个数**

- **prototype**

  -  是保存引用类型所有实例方法的真正所在
  - ES5中，prototype 属性是**不可枚举的**，因此使用 for-in 无法发现



### 方法

- 每个函数都包含两个**非继承而来**的方法：**apply()**和 **call()**：

  - 用途都是在特定的作用域中调用函数，实际上等于设置函数体内this对象的值

  - **apply()**

    两个参数：一个是在其中运行函数的作用域，另一个是参数数组。第二个参数可以是 Array 的实例，也可以是 arguments 对象。

    ```javascript
    function sum(num1, num2){     
        return num1 + num2; 
    } 
     
    function callSum1(num1, num2){     
        return sum.apply(this, arguments);        // 传入 arguments 对象 
    } 
     
    function callSum2(num1, num2){     
        return sum.apply(this, [num1, num2]);    // 传入数组 
    } 
     
    alert(callSum1(10,10));   //20 
    alert(callSum2(10,10));   //20 
    ```

  - **call()**

    与 apply()方法的作用相同，它们的区别仅在于接收参数的方式不同。

    ```javascript
    function sum(num1, num2){     
        return num1 + num2; 
    } 
     
    function callSum(num1, num2){   
        return sum.call(this, num1, num2); 
    } 
     
    alert(callSum(10,10));   //20
    ```

  - 两个方法能够扩充函数赖以运行的作用域

    ```javascript
    window.color = "red"; 
    var o = { color: "blue" }; 
     
    function sayColor(){     
        alert(this.color);
    } 
     
    sayColor();                //red 
     
    sayColor.call(this);       //red 
    sayColor.call(window);     //red 
    sayColor.call(o);          //blue 
    
    //在不给函数传递参数的情况下，使用哪个方法都无所谓
    ```

- **bind()**（ES5）

  创建一个函数的实例，其this值会被绑定到传给bind()函数的值

  ```javascript
  window.color = "red"; 
  var o = { color: "blue" }; 
   
  function sayColor(){    
      alert(this.color); 
  }  
  var objectSayColor = sayColor.bind(o);
  objectSayColor();    //blue
  ```

- 每个函数继承的 toLocaleString()、toString()和valueOf()方法始终都**返回函数的代码**

