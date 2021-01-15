# Set和Map数据结构

[TOC]

## Set

一种数据结构，类似于数组，但成员的**值是唯一的**，没有重复

- **基本用法**

  Set本身是一个构造函数，用来生成Set数据结构

  ```javascript
  const s = new Set();
  [2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
  for (let i of s) {
    console.log(i);
  }
  // 2 3 5 4
  //结果表明Set结构不会添加重复的值
  
  //Set函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化
  const set = new Set([1, 2, 3, 4, 4]);
  [...set]  // [1, 2, 3, 4]
  ```

  - **注意点**

    - 向Set加入值的时候，**不会发生类型转换**，所以`5`和`"5"`是两个不同的值

    - Set 内部判断两个值是否不同时使用的算法叫“Same-value-zero equality”，类似于精确相等运算符（`===`），主要的区别是向Set加入值时认为NaN等于自身，而精确相等运算符认为NaN不等于自身

      ```javascript
      let set = new Set();
      let a = NaN;
      let b = NaN;
      set.add(a);
      set.add(b);
      console.log(set);  // {NaN}
      //表明在Set内部，两个NaN是相等的
      
      console.log(NaN === NaN);  //false
      //表明精确相等运算符认为NaN不等于自身
      ```

    - 两个对象总是不相等的

      ```javascript
      let set = new Set();
      
      set.add({});
      set.size // 1
      
      set.add({});
      set.size // 2
      ```

    - `Array.from`方法可以将Set结构转为数组

      这就提供了去除数组重复成员的一种方法：`Array.from(new Set(array))`

    - Set结构没有键名，只有键值（或者说键名和键值是同一个值）

- **属性**

  - `Set.prototype.constructor`：构造函数，默认就是Set函数
  - `Set.prototype.size`：返回Set实例的成员总数

- **方法**

  - **操作数据**

    - `add(value)`：添加某个值，返回Set结构本身
    - `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功
    - `has(value)`：返回一个布尔值，表示该值是否为Set的成员
    - `clear()`：清除所有成员，没有返回值

  - **遍历成员**

    - **方法**

      - `keys()`：返回键名的遍历器对象

      - `values()`：返回键值的遍历器对象

      - `entries()`：返回键值对的遍历器对象

        ```javascript
        let set = new Set(['red', 'green', 'blue']);
        for (let item of set.entries()) {
          console.log(item);
        }
        // ["red", "red"]
        // ["green", "green"]
        // ["blue", "blue"]
        ```

      - `forEach()`：使用回调函数遍历每个成员

        用于对每个成员执行某种操作，没有返回值；可以有第二个参数，表示绑定处理函数内部的`this`对象

        ```javascript
        let set = new Set([1, 4, 9]);
        set.forEach((value, key) => console.log(key + ' : ' + value))
        // 1 : 1
        // 4 : 4
        // 9 : 9
        ```

    - **遍历顺序**

      Set的遍历顺序就是插入顺序

    - **遍历的应用**

      - 扩展运算符（`...`）可以用于 Set 结构
      - 数组的`map`和`filter`方法可以间接用于Set结构

      ```javascript
      let set = new Set([1, 2, 3]);
      set = new Set([...set].map(x => x * 2));
      // 返回Set结构：{2, 4, 6}
      
      let set = new Set([1, 2, 3, 4, 5]);
      set = new Set([...set].filter(x => (x % 2) == 0));
      // 返回Set结构：{2, 4}
      ```



## WeakSet

与 Set 类似，是不重复的值的集合，

- **与Set区别**

  - **成员只能是对象**

    不能是其他类型的值

  - **成员对象都是弱引用**

    即垃圾回收机制不考虑WeakSet对该对象的引用。如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于WeakSet之中

    因为WeakSet里的引用都不计入垃圾回收机制。因此WeakSet适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在WeakSet里面的引用就会自动消失

  - **WeakSet不可遍历**

    由于WeakSet内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此不可遍历

    因为不可遍历，**WeakSet没有size属性**，只有`add`、`delete`、`has`方法

- **应用**

  可以用来**储存DOM节点**，这样可以不用担心这些节点从文档移除时，会引发内存泄漏



## Map

类似于对象，是键值对的集合

- **与对象区别**

  对象只能用字符串在当键，而Map的键的范围不限于字符串，各种类型的值（包括对象）都可以当作键

  即Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的Hash结构实现

- **语法**

  包括数组在内，任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构都可以当作Map构造函数的参数

  ```javascript
  const map = new Map([
    ['name', '张三'],
    ['title', 'Author']
  ]);
  //等同于
  const items = [
    ['name', '张三'],
    ['title', 'Author']
  ];
  const map = new Map();
  items.forEach(
    ([key, value]) => map.set(key, value)
  );
  ```

  - **注意点**

    - 如果对同一个键多次赋值，后面的值将覆盖前面的值

    - 只有对同一个对象的引用，Map结构才将其视为同一个键

      ```javascript
      const map = new Map();
      map.set(['a'], 555);
      map.get(['a']) // undefined
      //表面是针对同一个键，但实际上是两个不同的数组实例，内存地址是不一样的
      ```

    - 如果Map的键是一个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map 将其视为一个键，比如`0`和`-0`就是一个键，布尔值`true`和字符串`true`则是两个不同的键,`undefined`和`null`也是两个不同的键。虽然`NaN`不严格相等于自身，但Map将其视为同一个键

    - 如果读取一个未知的键，则返回`undefined`

- **属性**

  - `size`：返回 Map 结构的成员总数

- **方法**

  - **操作数据**

    - `set(key,value)`：添加成员，返回整个Map结构
    - `get(key)`：根据键值获取对应的value，如果找不到就返回undefined
    - `has(key)`：判断某个键是否存在，返回一个布尔值
    - `delete(key)`：删除某个键，删除成功返回true，失败返回false
    - `clear()`：清除所有成员，没有返回值

  - **遍历成员**

    - **方法**

      - `Map.prototype.keys()`：返回键名的遍历器对象

      - `Map.prototype.values()`：返回键值的遍历器对象

      - `Map.prototype.entries()`：返回所有成员的遍历器对象

      - `Map.prototype.forEach()`：遍历Map的所有成员

        可以接受第二个参数，用来绑定this

    - **应用**

      - Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）
      - 结合数组的`map`方法、`filter`方法，可以实现 Map 的遍历和过滤（Map 本身没有`map`和`filter`方法）

- **与其他数据结构的互相转换**

  - **Map转为数组**

    ```javascript
    const myMap = new Map()
      .set(true, 7)
      .set({foo: 3}, ['abc']);
    [...myMap]  // [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
    ```

  - **数组转为Map**

    ```javascript
    new Map([
      [true, 7],
      [{foo: 3}, ['abc']]
    ])
    // Map {
    //   true => 7,
    //   Object {foo: 3} => ['abc']
    // }
    ```

  - **Map转为对象**

    ```javascript
    //如果所有Map的键都是字符串可以无损地转为对象
    //如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名
    function strMapToObj(strMap) {
      let obj = Object.create(null);
      for (let [k,v] of strMap) {
        obj[k] = v;
      }
      return obj;
    }
    const myMap = new Map()
      .set('yes', true)
      .set('no', false);
    strMapToObj(myMap)   // { yes: true, no: false }
    ```

  - **对象转为Map**

    ```javascript
    //1.通过Object.entries()
    let obj = {"a":1, "b":2};
    let map = new Map(Object.entries(obj));
    
    //2.自己手动实现
    function objToStrMap(obj) {
      let strMap = new Map();
      for (let k of Object.keys(obj)) {
        strMap.set(k, obj[k]);
      }
      return strMap;
    }
    
    objToStrMap({yes: true, no: false})
    // Map {"yes" => true, "no" => false}
    ```

  - **Map转为JSON**

    ```javascript
    //情况1.Map的键名都是字符串可以选择转为对象JSON
    function strMapToJson(strMap) {
      return JSON.stringify(strMapToObj(strMap));
    }
    let myMap = new Map().set('yes', true).set('no', false);
    strMapToJson(myMap)
    // '{"yes":true,"no":false}'
    
    //情况2.Map的键名有非字符串可以选择转为数组JSON
    function mapToArrayJson(map) {
      return JSON.stringify([...map]);
    }
    let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
    mapToArrayJson(myMap)
    // '[[true,7],[{"foo":3},["abc"]]]'
    ```

  - **JSON转为Map**

    ```javascript
    //正常情况：json所有键名为字符串
    function jsonToStrMap(jsonStr) {
      return objToStrMap(JSON.parse(jsonStr));
    }
    
    jsonToStrMap('{"yes": true, "no": false}')
    // Map {'yes' => true, 'no' => false}
    
    //特殊情况：整个JSON是一个数组，且每个数组成员本身，又是一个有两个成员的数组
    function jsonToMap(jsonStr) {
      return new Map(JSON.parse(jsonStr));
    }
    
    jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
    // Map {true => 7, Object {foo: 3} => ['abc']}
    ```



## WeakMap

与Map结构类似，也是用于生成键值对的集合

- **与Map区别**
  - **只接受对象作为键名**（null除外），不接受其他类型的值作为键名
  - 键名所指向的对象，**不计入垃圾回收机制**
  - **不可遍历**，只有四个方法可用：`get()`、`set()`、`has()`、`delete()`
- **用途**
  - DOM节点作为键名
  - 部署私有属性