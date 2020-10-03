# JavaScript面向对象

[TOC]

## 面对对象的三大特征

- **封装**
- **继承**
- **多态**



### 封装

一种通过接口抽象将具体实现包装并隐藏起来的方法

- **两大部分**

  - 限制对对象内部组件直接访问机制
  - 将数据和方法绑定起来，对外提供方法，从而改变对象状态的机制

- ES6之前JS不存在类的概念，但可以利用JS的函数来模拟类的概念

  ```javascript
  //公有属性
  function Book(name) {
      this.name = name;
  }
  console.log(new Book("Life").name);
  
  //私有属性
  function Book(name) {
      this.getName = ()=>{
          return name;
      };
      this.setName = (newName)=>{
          name = newName;
      };
  }
  let book = new Book("Life");
  book.setName("Time");
  console.log(book.getName()); //Time
  console.log(book.name); //无法访问私有属性name的值
  ```

- **闭包与纯函数**

  - **闭包**

    引用了自由变量的函数，自由变量为这个函数调用提供了一个上下文，上下文不同将对入参相同的函数调用造成不同影响：

    - 函数的行为不同，函数调用改变其上下文中的其它变量
    - 函数的返回值不同

  - **纯函数**

    与闭包相对，函数不允许引用任何自由变量

    - **特性**
      - 函数的调用不允许改变其所属的上下文
      - 相同入参的函数调用一定能得到相同的返回值

  - 闭包的调用是不安全的，因为它可能改变对象的上下文，同时它也不是幂等的，一次调用和多次调用可能产生不同结果；纯函数的调用是安全的，也是幂等的



### 继承

一个对象或者类能够自动保持另一个对象或者类的实现的一种机制

- **原型链继承**

  原型是JS**函数的一个内置属性prototype**，**指向一个对象**，而这个对象的所有属性和方法都会被该函数的所有实例自动继承

  ```javascript
  function Base(name) {
      this.name = name;
  }
  function Child(name) {
      this.name = name;
  }
  Child.prototype = new Base();
  var c = new Child("Life");
  console.log(c.name); //Life
  console.log(c instanceof Base);  //true
  console.log(c instanceof Child);  //true
  ```

  - **重要原则**

    - **设置prototype语句一定要放到两个构造函数之外**

      若放在构造函数中，每次构建过程中原型都会被破坏并重建一次，这不仅资源浪费状态丢失，还会影响JS引擎对instanceof的判断，子类实例化的对象就不是两个类的实例了

    - **设置prototype语句一定要放到实例化任何子类之前**

      若放在之后，原型在实例生成之后发生破坏并重建，则两个类都与实例没有关系了

  - **缺点**

    父类的构造方法如果包含参数就无法被完美地继承，传入后赋值的操作要在子类中重做一遍

- **构造继承**

  ```JavaScript
  function Base1(name) {
      this.name = name;
  }
  function Base2(type) {
      this.type = type;
  }
  function Child(name, type) {
      Base1.call(this, name);
      Base2.call(this, type);
  }
  var c = new Child("Life", "book");
  console.log(c.name);  //Life
  console.log(c instanceof Base1); //false
  console.log(c instanceof Child); //true
  ```

  - **优点**
    - 保留父类对于构造器参数的处理逻辑
    - 实现了**多重继承**
  - **缺点**
    - 使用instanceof方法判断时子类对象并非父类实例
    - 父类的prototype若有额外属性和方法就无法通过构造继承被自动搬到子类里来



### 多态

同样的接口有不同的实现。在JS中没有用来表示接口的关键字，但通过在不同实现类中定义同名的方法就可以做到多态效果，即同名方法在不同类中有不同的实现。由于没有类型和参数的强约束，它的灵活性远大于JAVA等静态语言



## 对象的创建

**使用new关键字时JS引擎步骤**：

- 创建一个对象，以x暂代
- 绑定原型 
- 令this = x并调用构造方法
- 对于构造器中的return语句根据 typeof x === 'object' 的结果来决定它的实际返回：
  - 如果为false，返回会被强制指定为x
  - 如果为true，new遵循构造器的实际return语句来返回