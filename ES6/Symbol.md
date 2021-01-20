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

  