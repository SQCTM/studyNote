# 列表渲染v-for

[TOC]

## v-for指令

通常基于一个**数组**来渲染一个**列表**

<br>

### 基本语法

`(item, index) in items`  或 `item of items`

- `items` 是**源数据数组**
- `item` 则是**被迭代的数组元素**的别名
- `index`参数是**可选的**，代表**当前项的索引**

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```javascript
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

渲染结果：

<ul style="border: 1px solid #eee;padding: 25px 35px;">
  <li>Parent - 0 - Foo</li>
  <li>Parent - 1 - Bar</li>
</ul>
<br>


### 使用对象

`(value, name, index) in object`

- `object`是**源数据对象**
- `value`是被迭代对象属性的**键值**
- `name`是**可选的**，代表被迭代对象属性的**键名**
- `index`是**可选的**，代表**当前属性的索引**

```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

```javascript
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```

渲染结果：

<div style="border: 1px solid #eee;padding: 25px 35px;">
  <div>0. title: How to do lists in Vue</div>
  <div>1. author: Jane Doe</div>
  <div>2. publishedAt: 2016-04-10</div>
</div>
**注意：**在遍历对象时会按`Object.keys()`的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下都一致

<br>

### 使用整数

```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

渲染结果：

<div style="border: 1px solid #eee;padding: 25px 35px;">
  1 2 3 4 5 6 7 8 9 10
</div>
<br>


### 在\<template>元素上使用

类似于 `v-if`，最终的**渲染结果将不包含 `<template>` 元素**

```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

<br>

## 维护状态

当 Vue 正在更新使用 `v-for` 渲染的元素列表时，默认使用“**就地更新**”的策略：如果数据项的顺序被改变，Vue 将不会移动 DOM 元素，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染

这个默认的模式是高效的，但是**只适用于不依赖子组件状态或临时 DOM 状态的列表渲染输出**



### key属性

给元素加上`key`这一attribute，能使vue跟踪每个节点的身份，从而重用和重新排序现有元素

```html
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
```

建议使用`v-for`时都加上`key`，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升

- **注意**
  - `key` 是 Vue 识别节点的一个通用机制，`key` 并不仅与 `v-for` 特别关联
  - 不要使用对象或数组之类的非基本类型值作为 `v-for` 的 `key`，要使用**字符串**或**数值类型**的值

<br>

## v-for不能与v-if一起使用

当它们处于同一节点，`v-for` 的优先级比 `v-if` 更高，所以 `v-if` 将分别重复运行于每个 `v-for` 循环中

如果想要有条件地跳过循环的执行，那么可以将 `v-if` 置于外层元素 (或 `<template>`) 上

<br>

## 在组件上使用v-for

2.2.0+ 的版本里，当在组件上使用 `v-for` 时，**必须加上`key`** ，并且任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域，为了**把迭代数据传递到组件里需要使用 prop**

```html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```





