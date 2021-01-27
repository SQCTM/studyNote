# Promise对象

[TOC]

## 含义

是异步编程的一种解决方案，一个Promise对象保存着一个异步操作，从它那可以获取异步操作的消息

- **特点**

  - **对象的状态不受外界影响**

    `Promise`对象代表一个异步操作，有三种**状态**：

    - `pending`（进行中）
    - `fulfilled`（已成功）
    - `rejected`（已失败）

    只有异步操作的结果可以决定当前是哪一种状态，任何其他操作都无法改变这个状态

  - **一旦状态改变就不会再次变**

    `Promise`对象的**状态改变**只有两种可能：

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





## 应用

