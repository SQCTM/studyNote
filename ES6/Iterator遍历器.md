# Iterator遍历器

[TOC]

## 概念

- **4种数据集合**

  - Array
  - Object
  - Map
  - Set

- **遍历器**

  是一种接口，为各种不同的数据结构提供统一的访问机制

  - **作用**

    - 为各种数据结构，提供一个统一的、简便的访问接口
    - 使得数据结构的成员能够按某种次序排列
    - ES6新的遍历命令`for...of`循环，Iterator接口主要供`for...of`消费

  - **遍历过程**

    1. 创建一个指针对象，指向当前数据结构的起始位置。遍历器对象本质是一个**指针对象**
    2. 第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员
    3. 第二次调用指针对象的next方法，指针就指向数据结构的第二个成员
    4. 不断调用指针对象的next方法，直到它指向数据结构的结束位置

    每一次调用next方法，都会返回数据结构的当前成员的信息，即返回一个包含`value`和`done`两个属性的**对象**：

    - `value`属性是**当前成员的值**
    - `done`属性是一个布尔值，**表示遍历是否结束**



## 默认Iterator接口

一种数据结构只要部署了Iterator接口，这种数据结构就是“可遍历的”。默认的Iterator接口部署在数据结构的`Symbol.iterator`属性中，即一个数据结构只要具有`Symbol.iterator`属性就可以认为是“可遍历的”

- **Symbol.iterator属性**

  `Symbol.iterator`属性是一个表达式，返回`Symbol`对象的`iterator`属性，这是一个预定义好的、类型为 Symbol 的特殊值

  它返回的是当前数据结构默认的**遍历器生成函数**，执行这个函数就会返回一个遍历器

  ```javascript
  const obj = {
    [Symbol.iterator] : function () {
      return {
        next: function () {
          return {
            value: 1,
            done: true
          };
        }
      };
    }
  };
  /*
      上面代码中，对象obj是可遍历的，因为具有Symbol.iterator属性
      执行这个属性，会返回一个遍历器对象，该对象的根本特征就是具有next方法
      每次调用next方法都会返回一个代表当前成员的信息对象，具有value和done两个属性
  */
  ```

  - **注意点**
    - 普通对象部署数组的`Symbol.iterator`方法，并无效果
    - 如果`Symbol.iterator`方法对应的不是遍历器生成函数（即会返回一个遍历器对象），解释引擎将会报错

- **原生具备Iterator接口的数据结构**

  - Array
  - Map
  - Set
  - String
  - TypedArray
  - 函数的 arguments 对象
  - NodeList 对象



## 调用Iterator接口的场合

有一些场合会默认调用 Iterator 接口（即`Symbol.iterator`方法）

- **解构赋值**

  对数组和 Set 结构进行解构赋值时，会默认调用`Symbol.iterator`方法

  ```javascript
  let set = new Set().add('a').add('b').add('c');
  
  let [x,y] = set;
  // x='a'; y='b'
  
  let [first, ...rest] = set;
  // first='a'; rest=['b','c'];
  ```

- **扩展运算符**

  ```javascript
  // 例一
  var str = 'hello';
  [...str] //  ['h','e','l','l','o']
  
  // 例二
  let arr = ['b', 'c'];
  ['a', ...arr, 'd']
  // ['a', 'b', 'c', 'd']
  
  /*
    这提供了一种简便机制，可以将任何部署了Iterator接口的数据结构转为数组
    只要某个数据结构部署了Iterator接口，就可以对它使用扩展运算符，将其转为数组
  */
  let arr = [...iterable];
  ```

- **yield\***

  `yield*`后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口

  ```javascript
  let generator = function* () {
    yield 1;
    yield* [2,3,4];
    yield 5;
  };
  var iterator = generator();
  
  iterator.next() // { value: 1, done: false }
  iterator.next() // { value: 2, done: false }
  iterator.next() // { value: 3, done: false }
  iterator.next() // { value: 4, done: false }
  iterator.next() // { value: 5, done: false }
  iterator.next() // { value: undefined, done: true }
  ```

- **其他场合**

  由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口，例如：

  - `for...of`
  - `Array.from()`
  - `Map()`, `Set()`, `WeakMap()`, `WeakSet()`（比如`new Map([['a',1],['b',2]])`）
  - `Promise.all()`
  - `Promise.race()`



## for...of循环