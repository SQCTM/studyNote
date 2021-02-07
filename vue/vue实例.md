# vue实例

[TOC]

## 创建实例

一个 Vue 应用由一个通过`new Vue`创建的**根 Vue 实例**，以及可选的嵌套的、可复用的组件树组成

```javascript
var vm = new Vue({
  // 选项
})
```

当创建一个 Vue 实例时，可以传入一个**选项对象**，通过使用这些选项来创建想要的行为



## 响应式数据

当一个 Vue 实例被创建时，它将`data`对象中的所有的 property 加入到 Vue 的**响应式系统**中。当这些 property 的值发生改变时，视图将会产生“响应”，视图会进行重渲染，匹配更新为新的值

```javascript
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

vm.a == data.a // => true

// 设置 property 会影响到原始数据
vm.a = 2
data.a // => 2
```

- **注意点**

  - 只有**当实例被创建时就已经存在**于`data`中的 property 才是**响应式**的

    如果在实例创建后才添加 property，那么对该 property 的改动将不会触发任何视图的更新，所以最好提前在 data 对象中设置一些初始值

  - 使用 `Object.freeze()`阻止响应式

    冻结data对象，阻止修改现有的 property，响应系统无法再追踪变化

    ```javascript
    var obj = {
      foo: 'bar'
    }
    
    Object.freeze(obj)
    
    new Vue({
      el: '#app',
      data: obj
    })
    ```

    ```html
    <div id="app">
      <p>{{ foo }}</p>
      <!-- 这里的 `foo` 不会更新！ -->
      <button v-on:click="foo = 'baz'">Change it</button>
    </div>
    ```

    

## 非自定义属性与方法的调用

都有前缀`$`，以便与用户定义的 property 区分开来

```javascript
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```



## 生命周期

![vue_lifecycle](F:\前端笔记\studyNote\images\vue_lifecycle.png)

### 生命周期钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程，在这个过程中可以运行一些叫做生命周期钩子的**函数**

钩子函数执行顺序如下：

1. **beforeCreate**
2. **created**
3. **beforeMount**
4. **mounted**
5. **beforeDestroy**
6. **destroyed**

> **注意**
>
> 不要在选项 property 或回调上使用箭头函数
>
> 比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`
>
> 因为箭头函数没有`this`，`this`会作为变量一直向上级词法作用域查找，直至找到为止，经常导致报错