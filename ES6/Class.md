# Class

[TOC]

## 基本语法

通过`class`关键字，可以定义类

```javascript
class Point {
  constructor(x, y) {  //构造方法
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
    
  toValue() {
    // ...
  }
}
```

- 类的数据类型就是**函数**，类本身就指向构造函数

  ```javascript
  typeof Point // "function"
  Point === Point.prototype.constructor // true
  ```

- 类的所有方法都定义在类的prototype属性上面

  ```javascript
  //上面定义类的方法等同于
  Point.prototype = {
    constructor() {},
    toString() {},
    toValue() {},
  };
  
  //在类的实例上面调用方法，其实就是调用原型上的方法
  class B {}
  const b = new B();
  b.constructor === B.prototype.constructor // true
  ```

- 类的内部所有定义的方法，都是**不可枚举**的

  ```javascript
  Object.keys(Point.prototype)  // []
  Object.getOwnPropertyNames(Point.prototype)  // ["constructor","toString"]
  
  //这一点与ES5的行为不一致
  var Point = function (x, y) {
    // ...
  };
  Point.prototype.toString = function () {
    // ...
  };
  Object.keys(Point.prototype)  // ["toString"]
  Object.getOwnPropertyNames(Point.prototype)  // ["constructor","toString"]
  //上面代码采用ES5的写法，toString()方法就是可枚举的
  ```

  

### constructor方法

类的默认方法，通过`new`命令生成对象实例时自动调用该方法。一个类必须有`constructor()`方法，如果没有显式定义，一个空的`constructor()`方法会被默认添加

```javascript
class Point {
}
// 等同于
class Point {
  constructor() {}
}
```

`constructor()`方法默认返回实例对象，但可以指定返回另外一个对象

```javascript
class Foo {
  constructor() {
    return Object.create(null);
  }
}

new Foo() instanceof Foo  // false
//上面代码中，constructor()函数返回一个全新的对象，结果导致实例对象不是Foo类的实例
```



### 类的实例对象

- 类生成实例时**必须使用new调用**，否则会报错，ES5中普通的构造函数不用new也不会报错

- 实例的属性除非显式定义在其本身（即定义在`this`对象上），否则都是定义在原型上（即定义在`class`上）

  ```javascript
  //定义类
  class Point {
  
    constructor(x, y) {
      this.x = x;
      this.y = y;
    }
  
    toString() {
      return '(' + this.x + ', ' + this.y + ')';
    }
  
  }
  
  var point = new Point(2, 3);
  point.toString() // (2, 3)
  
  point.hasOwnProperty('x') // true
  point.hasOwnProperty('y') // true
  point.hasOwnProperty('toString') // false
  point.__proto__.hasOwnProperty('toString') // true
  
  //上面代码中，x和y都是实例对象point自身的属性（定义在this变量上），所以hasOwnProperty()方法返回true
  //而toString()是原型对象的属性（定义在Point类上），所以hasOwnProperty()方法返回false
  ```

- 类的所有实例共享一个原型对象



### Class表达式

```javascript
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};

//这个类的名字为MyClass；Me只在Class内部可用，指代当前类
let inst = new MyClass();
inst.getClassName() // Me
Me.name // ReferenceError: Me is not defined

//如果Me内部用不到也可以省略
const MyClass = class { /* ... */ };
```

采用Class表达式，可以写出立即执行的Class

```javascript
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName(); // "张三"
```



### 注意点

- **不存在变量提升**
- **私有方法**
- **私有属性**
- **this的指向**
- **name属性**



## 继承

