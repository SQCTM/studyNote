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
  
- **字符串的Iterator接口**

  字符串是一个类似数组的对象，也原生具有Iterator接口

  ```javascript
  var someString = "hi";
  typeof someString[Symbol.iterator]
  // "function"
  
  var iterator = someString[Symbol.iterator]();
  
  iterator.next()  // { value: "h", done: false }
  iterator.next()  // { value: "i", done: false }
  iterator.next()  // { value: undefined, done: true }
  ```



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



## 遍历器对象的return()、throw()方法

自己写遍历器对象生成函数时，`next()`方法是必须部署的，`return()`方法和`throw()`方法是否部署是可选的

- **return()方法**

  使用场合是如果`for...of`循环提前退出（通常是因为出错或有break语句），就会调用`return()`方法

  `return()`方法必须返回一个对象，这是Generator语法决定的

- **throw()方法**

  主要是配合Generator函数使用，一般的遍历器对象用不到这个方法



## for...of循环

一个数据结构只要具有iterator接口，就可以用`for...of`循环遍历它的成员。`for...of`循环内部调用的是数据结构的`Symbol.iterator`方法，**循环读取键值**

### 使用范围

没有部署iterator接口的对象不能使用，会报错，但可以使用`for...in`遍历对象键名

- **数组**

  调用数组的遍历器接口只返回具有数字索引的属性

- **Set和Map结构**

  - 遍历的顺序是按照各个成员被添加进数据结构的顺序
  - Set结构遍历时，返回的是一个值；Map结构遍历时，返回的是一个数组，该数组的两个成员分别为当前 Map成员的键名和键值

- **类似数组的对象**

  ```javascript
  // 字符串
  let str = "hello";
  
  for (let s of str) {
    console.log(s); // h e l l o
  }
  
  // DOM NodeList对象
  let paras = document.querySelectorAll("p");
  
  for (let p of paras) {
    p.classList.add("test");
  }
  
  // arguments对象
  function printArgs() {
    for (let x of arguments) {
      console.log(x);
    }
  }
  printArgs('a', 'b');
  // 'a'
  // 'b'
  ```

- **Generator对象**



### 与其它遍历作比较

- **forEach**

  数组内置的方法，缺点是无法中途跳出循环，break或return都无效

- **for...in**

  遍历数组和对象的键名，适用于遍历对象，不适用于数组

  - **缺点**
    - 数组的键名是数字，但是`for...in`循环是以字符串作为键名“0”、“1”、“2”等等。
    - `for...in`循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键
    - 某些情况下，`for...in`循环会以任意顺序遍历键名

- **for...of相比于以上的优点**

  - 有着同`for...in`一样的简洁语法，但是没有`for...in`那些缺点
  - 不同于`forEach`方法，它可以与`break`、`continue`和`return`配合使用
  - 提供了遍历所有数据结构的统一操作接口

