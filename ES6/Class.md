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

- **严格模式**

  类和模块的内部**默认是严格模式**，所以不需要使用`use strict`指定运行模式

- **不存在变量提升**

  类使用在前定义在后会报错，这样规定的**原因与继承有关**

  ```javascript
  {
    let Foo = class {};
    class Bar extends Foo {
    }
  }
  /*
    上面的代码不会报错，因为Bar继承Foo的时候，Foo已经有定义了
    如果存在class的提升上面代码就会报错
    因为class会被提升到代码头部，而let命令是不提升的，所以导致Bar继承Foo的时候，Foo还没有定义
  */
  ```

- **this的指向**

  类的方法内部如果含有`this`，它默认指向类的实例

- **name属性**

  本质上ES6的类只是ES5的构造函数的一层包装，所以函数的许多特性都被`Class`继承，包括`name`属性

  ```javascript
  class Point {}
  Point.name // "Point"
  ```



## 取值函数和存值函数

在类的内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为

```javascript
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

存值函数和取值函数是设置在属性的Descriptor**描述对象**上的



## 私有方法和私有属性

- **私有方法**

  ES6不提供，只能通过变通方法模拟实现

  ```javascript
  //1.命名
  class Widget {
    // 公有方法
    foo (baz) {
      this._bar(baz);
    }
    // 私有方法
    _bar(baz) {
      return this.snaf = baz;
    }
  
  }
  
  //2.使得bar()实际上成为了当前类的私有方法
  class Widget {
    foo (baz) {
      bar.call(this, baz);
    }
  
  }
  
  function bar(baz) {
    return this.snaf = baz;
  }
  
  //3.利用Symbol值的唯一性，将私有方法的名字命名为一个Symbol值
  const bar = Symbol('bar');
  const snaf = Symbol('snaf');
  
  export default class myClass{
    // 公有方法
    foo(baz) {
      this[bar](baz);
    }
    // 私有方法
    [bar](baz) {
      return this[snaf] = baz;
    }
  
  };
  ```

- **私有属性**

  目前有一个**提案**，为class加了私有属性。在属性名之前使用`#`表示

  ```javascript
  class IncreasingCounter {
    #count = 0;
    get value() {
      console.log('Getting the current value!');
      return this.#count;
    }
    increment() {
      this.#count++;
    }
  }
  const counter = new IncreasingCounter();
  counter.#count // 报错
  counter.#count = 42 // 报错
  ```

  也可以这样写私有方法



## 静态方法和静态属性

- **静态方法**

  如果在一个方法前加上`static`关键字，就表示该方法**不会被实例调用**，而是**直接通过类来调用**

  ```javascript
  class Foo {
    static classMethod() {
      return 'hello';
    }
  }
  
  Foo.classMethod() // 'hello'
  
  var foo = new Foo();
  foo.classMethod()  // TypeError: foo.classMethod is not a function
  ```

  - **注意点**
    - 如果静态方法包含`this`关键字，这个`this`**指的是类**，而不是实例
    - 父类的静态方法，可以被子类继承
    - 父类的静态方法可以在子类中从`super`对象上调用

- **静态属性**

  指的是Class本身的属性，即`Class.propName`，而不是定义在实例对象（`this`）上的属性

  ```javascript
  //目前只有这个方法可行，因为ES6明确规定，Class内部只有静态方法，没有静态属性
  class Foo {
  }
  
  Foo.prop = 1;
  Foo.prop // 1
  ```

  在实例属性前加`static`还只是提案



## new.target属性

该属性一般用在构造函数之中，**返回new命令作用于的那个构造函数**。如果构造函数不是通过new命令或`Reflect.construct()`调用的，则会返回`undefined`，因此这个属性可以**用来确定构造函数是怎么调用的**

```javascript
//以下代码确保构造函数只能通过new命令调用
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```

- **注意点**

  - Class内部调用`new.target`，返回当前Class
  - 子类继承父类时，`new.target`会返回子类

  利用这些特点可以写出不能独立实例化，只能被继承的类

  ```javascript
  class Shape {
    constructor() {
      if (new.target === Shape) {
        throw new Error('本类不能实例化');
      }
    }
  }
  
  class Rectangle extends Shape {
    constructor(length, width) {
      super();
      // ...
    }
  }
  
  var x = new Shape();  // 报错
  var y = new Rectangle(3, 4);  // 正确
  ```



## 继承

Class可以通过`extends`关键字实现继承

- **注意点**

  - 子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错

    因为子类自己的`this`对象必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工加上子类自己的实例属性和方法。如果不调用`super`方法，子类就得不到`this`对象

    ```javascript
    class Point { /* ... */ }
    
    class ColorPoint extends Point {
      constructor() {
      }
    }
    
    let cp = new ColorPoint(); // ReferenceError
    ```

  - 如果子类没有定义`constructor`方法，这个方法会被默认添加

    ```javascript
    class ColorPoint extends Point {
    }
    
    // 等同于
    class ColorPoint extends Point {
      constructor(...args) {
        super(...args);
      }
    }
    ```

  - 在子类的构造函数中，只有调用`super`之后才可以使用`this`关键字，否则会报错

    因为子类实例的构建，基于父类实例，只有`super`方法才能调用父类实例

    ```javascript
    class Point {
      constructor(x, y) {
        this.x = x;
        this.y = y;
      }
    }
    
    class ColorPoint extends Point {
      constructor(x, y, color) {
        this.color = color; // ReferenceError
        super(x, y);
        this.color = color; // 正确
      }
    }
    ```

  - 父类的静态方法会被子类继承

    ```javascript
    class A {
      static hello() {
        console.log('hello world');
      }
    }
    
    class B extends A {
    }
    
    B.hello()  // hello world
    ```

    

### Object.getPrototypeOf()方法

用来从子类上获取父类。可以使用这个方法判断，一个类是否继承了另一个类

```javascript
Object.getPrototypeOf(ColorPoint) === Point  // true
```



### super关键字

既可以当作**函数**使用，也可以当作**对象**使用。在这两种情况下，它的用法完全不同：

- **函数**

  作为函数调用时**代表父类的构造函数**，子类的构造函数必须执行一次`super`函数

  - **注意点**

    - super虽然代表了父类A的构造函数，但是**返回的是子类B的实例**，即super内部的this指的是B的实例，因此`super()`在这里相当于`A.prototype.constructor.call(this)`

    - 作为函数时，`super()`**只能用在子类的构造函数之中**，用在其他地方就会报错

      ```javascript
      class A {}
      
      class B extends A {
        m() {
          super(); // 报错
        }
      }
      ```

- **对象**

  在普通方法中指向**父类的原型对象**；在静态方法中指向**父类**

  - **注意点**

    - 由于`super`指向父类的原型对象，所以定义在父类实例上的方法或属性是无法通过`super`调用的

      ```javascript
      class A {
        constructor() {
          this.p = 2;
        }
      }
      
      class B extends A {
        get m() {
          return super.p;
        }
      }
      
      let b = new B();
      b.m // undefined
      
      //如果属性定义在父类的原型对象上，super就可以取到
      class A {}
      A.prototype.x = 2;
      
      class B extends A {
        constructor() {
          super();
          console.log(super.x) // 2
        }
      }
      
      let b = new B();
      ```

    - 在子类普通方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类实例

      ```javascript
      class A {
        constructor() {
          this.x = 1;
        }
        print() {
          console.log(this.x);
        }
      }
      
      class B extends A {
        constructor() {
          super();
          this.x = 2;
        }
        m() {
          super.print();
        }
      }
      
      let b = new B();
      b.m() // 2
      ```

    - 用在静态方法中`super`将指向父类，在子类的静态方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类，而不是子类的实例

- 使用`super`的时候，必须显式指定是作为函数还是作为对象使用，否则会报错

  ```javascript
  class A {}
  
  class B extends A {
    constructor() {
      super();
      console.log(super); // 报错
    }
  }
  
  //如果能清晰地表明super的数据类型就不会报错
  class A {}
  
  class B extends A {
    constructor() {
      super();
      console.log(super.valueOf() instanceof B); // true
    }
  }
  
  let b = new B();
  /*
    上面代码中，super.valueOf()表明super是一个对象，因此就不会报错
    由于super使得this指向B的实例，所以super.valueOf()返回的是一个B的实例
  */
  ```

- 由于对象总是继承其他对象的，所以可以在任意一个对象中，使用`super`关键字

  ```javascript
  var obj = {
    toString() {
      return "MyObject: " + super.toString();
    }
  };
  
  obj.toString(); // MyObject: [object Object]
  ```



### 类的prototype属性和\__proto__属性

Class作为构造函数的语法糖，同时有`prototype`属性和`__proto__`属性，因此同时存在两条继承链：

- 子类的`__proto__`属性表示**构造函数的继承**，总是**指向父类**
- 子类`prototype`属性的`__proto__`属性，表示**方法的继承**，总是指向**父类的`prototype`属性**

```javascript
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```

- **类的继承模式**

  ```javascript
  class A {
  }
  
  class B {
  }
  
  // B 的实例继承 A 的实例
  Object.setPrototypeOf(B.prototype, A.prototype);
  // 等同于
  B.prototype.__proto__ = A.prototype;
  
  // B 继承 A 的静态属性
  Object.setPrototypeOf(B, A);
  // 等同于
  B.__proto__ = A;
  
  const b = new B();
  ```

- **不存在继承的类**

  ```javascript
  class A {
  }
  
  A.__proto__ === Function.prototype // true
  A.prototype.__proto__ === Object.prototype // true
  ```



### 实例的\__proto__属性

子类实例的`__proto__`属性的`__proto__`属性，指向父类实例的`__proto__`属性。即**子类的原型的原型是父类的原型**

```javascript
//ColorPoint继承了Point
var p1 = new Point(2, 3);
var p2 = new ColorPoint(2, 3, 'red');

p2.__proto__ === p1.__proto__ // false
p2.__proto__.__proto__ === p1.__proto__ // true
```

通过子类实例的`__proto__.__proto__`属性，可以修改父类实例的行为

```javascript
p2.__proto__.__proto__.printName = function () {
  console.log('Ha');
};

p1.printName() // "Ha"
```



### 原生构造函数的继承

原生构造函数是指语言内置的构造函数，通常**用来生成数据结构**

- **ECMAScript的原生构造函数**
  - Boolean()
  - Number()
  - String()
  - Array()
  - Date()
  - Function()
  - RegExp()
  - Error()
  - Object()

- **继承**

  **ES5不能继承原生构造函数**，因为子类无法获得原生构造函数的内部属性，通过`apply()`和`call()`方法都不行，原生构造函数会忽略`apply`方法传入的`this`，也就是说，原生构造函数的`this`无法绑定，导致拿不到内部属性

  ES6中`extends`关键字可以用来继承原生的构造函数，因此可以在原生数据结构的基础上定义自己的数据结构

  ```javascript
  class MyArray extends Array {
    constructor(...args) {
      super(...args);
    }
  }
  
  var arr = new MyArray();
  arr[0] = 12;
  arr.length // 1
  
  arr.length = 0;
  arr[0] // undefined
  ```

  - **注意点**

    继承`Object`的子类有一个行为差异，自己写的Object子类无法通过`super`方法向父类`Object`传参，ES6改变了`Object`构造函数的行为，一旦发现`Object`方法不是通过`new Object()`这种形式调用，`Object`构造函数就会忽略参数

