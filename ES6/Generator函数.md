# Generator函数

[TOC]

## 基本概念

一种异步编程解决方案，语法行为与传统函数完全不同。它如同一个状态机，封装了多个内部状态。执行Generator函数会返回一个遍历器对象，返回的遍历器对象，可以依次遍历Generator函数内部的每一个状态

- **语法**

  - `function`关键字与函数名之间有一个星号
  - 函数体内部使用yield表达式，**定义不同的内部状态**

  ```javascript
  function* helloWorldGenerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
  }
  var hw = helloWorldGenerator();
  /*
    1.上面代码定义的Generator函数内部有两个yield表达式（hello和world），即该函数有三个状态：
      hello，world和return语句（结束执行）
    2.调用Generator函数后，该函数并不执行，返回的是一个指向内部状态的指针对象，即Iterator遍历器对象
    3.下一步必须调用遍历器对象的next方法，使得指针移向下一个状态
      每次调用next方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield表达式或return语句为止
      即Generator函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行
  */
  hw.next()  // { value: 'hello', done: false }
  hw.next()  // { value: 'world', done: false }
  hw.next()  // { value: 'ending', done: true }
  hw.next()  // { value: undefined, done: true }
  ```

- **yield表达式**

  yield表达式就是**暂停标志**

  - **注意点**

    - yield表达式后面的表达式，只有当调用`next`方法、内部指针指向该语句时才会执行，因此等于为 JavaScript提供了**手动的“惰性求值”**的语法功能

      ```javascript
      function* gen() {
        yield 123+456;
      }
      //上面代码中yield后面的表达式123+456不会立即求值，只有在next方法将指针移到这一句时才会求值
      ```

    - yield表达式只能用在Generator函数里面，用在其他地方都会报错

    - yield表达式如果用在另一个表达式之中，必须放在圆括号里面

      ```javascript
      function* demo() {
        console.log('Hello' + yield); // SyntaxError
        console.log('Hello' + yield 123); // SyntaxError
        console.log('Hello' + (yield)); // OK
        console.log('Hello' + (yield 123)); // OK
      }
      ```

    - yield表达式用作函数参数或放在赋值表达式的右边，可以不加括号

      ```javascript
      function* demo() {
        foo(yield 'a', yield 'b'); // OK
        let input = yield; // OK
      }
      ```

  - **与return的区别**

    - 函数每次遇到`yield`暂停执行，下一次再从该位置继续向后执行，而`return`语句不具备位置记忆的功能
    - 一个函数里面只能执行一次`return`语句，但可以执行多个`yield`表达式

- **与Iterator接口的关系**

  由于Generator函数就是遍历器生成函数，因此可以把它赋值给对象的`Symbol.iterator`属性，从而使得该对象具有Iterator接口

  ```javascript
  var myIterable = {};
  myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
  };
  
  [...myIterable] // [1, 2, 3]
  ```

  Generator函数执行后返回一个遍历器对象。该对象本身也具有`Symbol.iterator`属性，执行后返回自身

  ```javascript
  function* gen(){
    // some code
  }
  
  var g = gen();
  
  g[Symbol.iterator]() === g  // true
  
  //上面代码中，gen是一个Generato 函数，调用它会生成一个遍历器对象g。它的Symbol.iterator属性也是一个遍历器对象生成函数，执行后返回它自己
  ```



## next方法

- **遍历器对象的next方法运行逻辑**

  1. 遇到`yield`表达式就暂停执行后面的操作，并将跟在`yield`后面表达式的值作为返回对象的`value`属性值
  2. 下一次调用`next`方法时再继续往下执行，直到遇到下一个`yield`表达式
  3. 如果没有再遇到新的`yield`表达式，就一直运行到函数结束，直到`return`语句为止，并将`return`语句后面表达式的值作为返回对象的`value`属性值
  4. 如果该函数没有`return`语句，则返回的对象的`value`属性值为`undefined`

- **方法的参数**

  yield表达式本身没有返回值，或者说总是返回`undefined`。`next`方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值
  
  ```javascript
  function* f() {
    for(var i = 0; true; i++) {
      var reset = yield i;
      if(reset) { i = -1; }
    }
  }
  
  var g = f();
  g.next() // { value: 0, done: false }
  g.next() // { value: 1, done: false }
  g.next(true) // { value: 0, done: false }
  /*
    如果next方法没有参数，每次运行到yield表达式，变量reset的值总是undefined
    当next方法带一个参数true时，变量reset就被重置为这个参数（即true）,i就变为-1
  */
  ```



