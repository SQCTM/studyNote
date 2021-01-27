# Promise对象

[TOC]

## 含义

是异步编程的一种解决方案，一个Promise对象保存着一个异步操作，从它那可以获取异步操作的消息

- **特点**

  - **对象的状态不受外界影响**

    Promise对象代表一个异步操作，有三种**状态**：

    - `pending`（进行中）
    - `fulfilled`（已成功）
    - `rejected`（已失败）

    只有异步操作的结果可以决定当前是哪一种状态，任何其他操作都无法改变这个状态

  - **一旦状态改变就不会再次变**

    Promise对象的**状态改变**只有两种可能：

    - 从`pending`变为`fulfilled`，进行中变为已成功
    - 从`pending`变为`rejected`，进行中变为已失败

    只要这两种情况发生，状态就凝固不会再改变，会一直保持这个结果，这就称为 `resolved`（已定型）

    改变发生后再对`Promise`对象添加回调函数会**立即得到这个结果**。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听是得不到结果的

- **作用**

  可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口使得控制异步操作更加容易

- **缺点**

  - 无法取消，一旦新建它就会立即执行，无法中途取消
  - 如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部
  - 当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）



## 基本用法

- **创造Promise实例**

  ```javascript
  const promise = new Promise(function(resolve, reject) {
    // ... some code
  
    if (/* 异步操作成功 */){
      resolve(value);
    } else {
      reject(error);
    }
  });
  ```

  Promise构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由JavaScript引擎提供，不用自己部署

  - `resolve`

    将Promise对象的**状态从未完成变为成功**（即从pending变为resolved），在异步操作成功时调用，并**将异步操作的结果作为参数传递出去**

  - `reject`

    将Promise对象的**状态从未完成变为失败**（即从pending变为rejected），在异步操作失败时调用，并**将异步操作报出的错误作为参数传递出去**

- **实例生成后**

  可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函

  ```javascript
  promise.then(function(value) {
    // success
  }, function(error) {
    // failure
  });
  ```

  `then`方法可以接受两个回调函数作为参数：

  1. 第一个回调函数是Promise对象的状态变为`resolved`时调用
  2. 第二个回调函数是Promise对象的状态变为`rejected`时调用（可选，不一定要提供）

  两个方法都接收promise对象传出的值作为参数

  

## 一些方法

- **Promise.prototype.then()**

- **Promise.prototype.catch()**

- **Promise.prototype.finally()**

- **Promise.all()**

  用于将多个Promise实例包装成一个新的Promise实例

  ```javascript
  const p = Promise.all([p1, p2, p3]);
  //方法的参数必须具有Iterator接口，如数组
  ```

  以上面代码为例，p的状态由p1、p2、p3决定，分成两种情况：

  - 只有p1、p2、p3的状态都变成`fulfilled`，p的状态才会变成`fulfilled`，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

  - 只要p1、p2、p3之中有一个被`rejected`，p的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给p的回调函数。

- **Promise.race()**

  用于将多个Promise实例包装成一个新的Promise实例

  与`Promise.all()`的区别是，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的Promise实例的返回值，就传递给p的回调函数

- **Promise.resolve()**

  将现有对象转为Promise对象，参数有四种情况：

  - **参数是一个Promise 实例**

    不做任何修改、原封不动地返回这个实例

  - **参数是一个thenable对象**

    thenable对象指的是具有then方法的对象，`Promise.resolve()`方法会将这个对象转为Promise对象，然后就立即执行thenable对象的`then()`方法

  - **参数不是具有then()方法的对象，或根本就不是对象**

    返回一个新的Promise对象，状态为resolved，回调函数会立即执行，`Promise.resolve()`方法的参数同时传给回调函数

  - **不带有任何参数**

    直接返回一个resolved状态的Promise对象

- **Promise.reject()**

  返回一个新的Promise实例，该实例的状态为rejected，`Promise.reject()`方法的参数会原封不动地作为`reject`的理由，变成后续方法的参数

- **Promise.try()**

  不管函数`f`是否包含异步操作，都用then方法指定下一步流程，用catch方法处理`f`抛出的错误

  ```javascript
  Promise.resolve().then(f)
  ```

  事实上，`Promise.try`就是模拟`try`代码块，就像`promise.catch`模拟的是`catch`代码块

  - **缺点**

    如果`f`是同步函数，那么它会在本轮事件循环的末尾执行

    ```javascript
    const f = () => console.log('now');
    Promise.resolve().then(f);
    console.log('next');
    // next
    // now
    ```

  - **改进**

    两种写法

    ```javascript
    //1.用async函数来写
    const f = () => console.log('now');
    (async () => f())();
    console.log('next');
    // now
    // next
    
    //注意：async () => f()会吃掉f()抛出的错误。所以，如果想捕获错误，要使用promise.catch方法
    (async () => f())()
    .then(...)
    .catch(...)
    
    //2.使用new Promise()
    const f = () => console.log('now');
    (
      () => new Promise(
        resolve => resolve(f())
      )
    )();
    console.log('next');
    // now
    // next
    ```

    

## 应用

- **加载图片**

  ```javascript
  const preloadImage = function (path) {
    return new Promise(function (resolve, reject) {
      const image = new Image();
      image.onload  = resolve;
      image.onerror = reject;
      image.src = path;
    });
  };
  //一旦图片加载完成，Promise的状态就发生变化
  ```

- **Generator 函数与 Promise 的结合**

  ```javascript
  function getFoo () {
    return new Promise(function (resolve, reject){
      resolve('foo');
    });
  }
  
  const g = function* () {
    try {
      const foo = yield getFoo();
      console.log(foo);
    } catch (e) {
      console.log(e);
    }
  };
  
  function run (generator) {
    const it = generator();
  
    function go(result) {
      if (result.done) return result.value;
  
      return result.value.then(function (value) {
        return go(it.next(value));
      }, function (error) {
        return go(it.throw(error));
      });
    }
  
    go(it.next());
  }
  
  run(g);
  //上面代码的 Generator 函数g之中，有一个异步操作getFoo，它返回的就是一个Promise对象。函数run用来处理这个Promise对象，并调用下一个next方法
  ```

  