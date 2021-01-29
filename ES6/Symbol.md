# Symbol

[TOC]

## 概述

ES6引入的一种新的原始数据类型，**表示独一无二的值**，是JavaScript语言的第七种数据类型

- **引入Symbol原因**

  ES5的对象属性名都是字符串，这容易造成属性名的冲突，为了**保证对象的每个属性名都是独一无二的**而引入Symbol

- **Symbol值的生成**

  Symbol值由**Symbol函数**生成

  ```javascript
  //无参数
  let s = Symbol();
  console.log(typeof s);   // "symbol"
  console.log(s);   //Symbol()
  
  //有参数
  let s1 = Symbol('foo');
  let s2 = Symbol('bar');
  console.log(s1);   // Symbol(foo)
  console.log(s2);   // Symbol(bar)
  console.log(s1.toString());  // "Symbol(foo)"
  console.log(s2.toString());  // "Symbol(bar)"
  ```

  - **注意点**
  
    - 即使参数相同Symbol函数的返回值也不同
  
      ```javascript
      // 没有参数的情况
      let s1 = Symbol();
      let s2 = Symbol();
      s1 === s2 // false
      
      // 有参数的情况
      let s1 = Symbol('foo');
      let s2 = Symbol('foo');
      s1 === s2 // false
      ```
  
    - 不能与其他类型的值进行运算，会报错
  
      ```javascript
      let sym = Symbol('My symbol');
      
      "your symbol is " + sym
      // TypeError: can't convert symbol to string
      `your symbol is ${sym}`
      // TypeError: can't convert symbol to string
      ```
  
      



## Symbol值的转换

- **可以显式转为字符串**

  ```javascript
  let sym = Symbol('My symbol');
  
  String(sym) // 'Symbol(My symbol)'
  sym.toString() // 'Symbol(My symbol)'
  ```

- **转为布尔值**

  ```javascript
  let sym = Symbol();
  Boolean(sym) // true
  !sym  // false
  
  if (sym) {
    // ...
  }
  ```

- **注意点**

  不可以转化为数值

  ```javascript
  Number(sym) // TypeError
  sym + 2 // TypeError
  ```

  

## Symbol值用作对象属性名

由于每一个Symbol值都是不相等的，这意味着Symbol值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。这对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或覆盖

```javascript
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

- **注意点**

  - Symbol值作为对象属性名时，不能用点运算符

    ```javascript
    const mySymbol = Symbol();
    const a = {};
    
    a.mySymbol = 'Hello!';
    a[mySymbol]  // undefined
    a['mySymbol']  // "Hello!"
    ```

  - 在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中

    ```javascript
    let s = Symbol();
    let obj = {
      [s]: function (arg) { ... }
    };
    
    obj[s](123);
    //如果s不放在方括号中，该属性的键名就是字符串s，而不是Symbol值
    ```

  

## Symbol属性名的遍历

Symbol值作为属性名，遍历对象时，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回

- **获取方法**

  - **Object.getOwnPropertySymbols()**

    ```javascript
    const obj = {};
    const foo = Symbol('foo');
    obj[foo] = 'bar';
    
    for (let i in obj) {
      console.log(i); // 无输出
    }
    
    Object.getOwnPropertyNames(obj) // []
    Object.getOwnPropertySymbols(obj) // [Symbol(foo)]
    ```

  - **Reflect.ownKeys()**

    此方法可以返回所有类型的键名，包括常规键名和 Symbol 键名

    ```javascript
    let obj = {
      [Symbol('my_key')]: 1,
      enum: 2,
      nonEnum: 3
    };
    
    Reflect.ownKeys(obj)  //  ["enum", "nonEnum", Symbol(my_key)]
    ```

  

## Symbol方法

- **Symbol.for()**

  接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值

  ```javascript
  let s1 = Symbol.for('foo');
  let s2 = Symbol.for('foo');
  
  s1 === s2  // true
  //上面代码中，s1和s2都是Symbol值，但它们都是由同样参数的Symbol.for方法生成的，所以实际上是同一个值
  ```

  - **与Symbol()区别**

    `Symbol.for()`会被登记在全局环境中供搜索，`Symbol()`不会。`Symbol.for()`不会每次调用就返回一个新的 Symbol类型的值，而是会先检查给定的`key`是否已经存在，如果不存在才会新建一个值

  - **注意点**

    `Symbol.for()`为Symbol值登记的名字是**全局环境**的，可以用在不同的iframe或service worker中取到同一个值

- **Symbol.keyFor()**

  返回一个已登记的Symbol类型值的key，未登记的Symbol值返回undefined

  ```javascript
  let s1 = Symbol.for("foo");
  Symbol.keyFor(s1) // "foo"
  
  let s2 = Symbol("foo");
  Symbol.keyFor(s2) // undefined
  ```

  

## 其它应用

- **消除魔术字符串**

  魔术字符串指的是，在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或数值

  ```javascript
  function getArea(shape, options) {
    let area = 0;
  
    switch (shape) {
      case 'Triangle': // 魔术字符串
        area = .5 * options.width * options.height;
        break;
      /* ... more code ... */
    }
  
    return area;
  }
  
  getArea('Triangle', { width: 100, height: 100 }); // 魔术字符串
  //上面代码中，字符串Triangle就是一个魔术字符串。它多次出现与代码形成“强耦合”，不利于将来的修改和维护
  
  //一般消除方法
  const shapeType = {
    triangle: 'Triangle'
  };
  
  function getArea(shape, options) {
    let area = 0;
    switch (shape) {
      case shapeType.triangle:
        area = .5 * options.width * options.height;
        break;
    }
    return area;
  }
  
  getArea(shapeType.triangle, { width: 100, height: 100 });
  
  //使用Symbol修改
  const shapeType = {
    triangle: Symbol()
  };
  ```

- **定义常量**

  常量使用Symbol值最大的好处，就是其他任何值都不可能有相同的值了

  ```javascript
  //例子1
  const log = {};
  
  log.levels = {
    DEBUG: Symbol('debug'),
    INFO: Symbol('info'),
    WARN: Symbol('warn')
  };
  console.log(log.levels.DEBUG, 'debug message');
  console.log(log.levels.INFO, 'info message');
  
  //例子2
  const COLOR_RED    = Symbol();
  const COLOR_GREEN  = Symbol();
  
  function getComplement(color) {
    switch (color) {
      case COLOR_RED:
        return COLOR_GREEN;
      case COLOR_GREEN:
        return COLOR_RED;
      default:
        throw new Error('Undefined color');
      }
  }
  ```

- **定义非私有但只用于内部的方法**

  ```javascript
  let size = Symbol('size');
  
  class Collection {
    constructor() {
      this[size] = 0;
    }
  
    add(item) {
      this[this[size]] = item;
      this[size]++;
    }
  
    static sizeOf(instance) {
      return instance[size];
    }
  }
  
  let x = new Collection();
  Collection.sizeOf(x) // 0
  
  x.add('foo');
  Collection.sizeOf(x) // 1
  
  Object.keys(x) // ['0']
  Object.getOwnPropertyNames(x) // ['0']
  Object.getOwnPropertySymbols(x) // [Symbol(size)]
  ```

- **模块的Singleton模式**

  Singleton模式指的是调用一个类，任何时候返回的都是同一个实例