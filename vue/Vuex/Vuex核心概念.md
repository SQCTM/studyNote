# Vuex核心概念

[TOC]

## 什么是Vuex

是一个专为 Vue.js 应用程序开发的**状态管理模式**，它把组件的共享状态抽取出来，以一个**全局单例**模式管理

Vuex 应用的核心就是 **store**（仓库），包含着应用中大部分的**状态 (state)**

- **Vuex 和全局对象的区别**

  - Vuex 的状态存储是**响应式**的。当组件从 store 中读取状态时，若 store 中的状态发生变化，则相应的组件也会得到高效更新
  - 不能直接改变 store 中的状态。改变 store 中的状态的**唯一途径**就是显式地**提交 (commit) mutation**，这样可以方便地跟踪每一个状态的变化

- **核心组成部分**

  - **State**
  - **Getter**
  - **Mutation**
  - **Action**
  - **Module**

  ![vuex](F:\前端笔记\studyNote\images\vuex.png)



## 基本例子

创建一个 store，需要提供一个初始 `state` 对象和`mutation `方法

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

可以通过 `store.state` 来获取状态对象，以及通过 `store.commit` 方法触发状态变更

```js
store.commit('increment')

console.log(store.state.count) // -> 1
```

Vuex 的状态存储是响应式的，为了在组件中用 `this.$store` 访问，Vuex 提供了一个从根组件向所有子组件，以 `store` 选项的方式“注入”该 store 实例的机制

```javascript
new Vue({
  el: '#app',
  store: store,   // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
})

//简写
new Vue({
  el: '#app',
  store
})
```

通过在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过`this.$store `访问到

```js
//从组件的方法提交一个变更
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
  }
}
```



## 核心组成部分

### State

- **mapState辅助函数**

  此辅助函数把vuex里的数据映射到该组件计算属性中，返回的是一个对象

  ```js
  // 在单独构建的版本中辅助函数为 Vuex.mapState
  import { mapState } from 'vuex'
  
  export default {
    // 几种写法
    computed: mapState({
      // 箭头函数可使代码更简练
      count: state => state.count,
  
      // 传字符串参数 'count' 等同于 `state => state.count`
      countAlias: 'count',
  
      // 为了能够使用 `this` 获取局部状态，必须使用常规函数
      countPlusLocalState (state) {
        return state.count + this.localCount
      }
    })
  }
  
  //当映射的计算属性的名称与 state 的子节点名称相同时，可以给 mapState 传一个字符串数组
  computed: mapState(['count'])  // 映射 this.count 为 store.state.count
  
  //使用展开运算符
  computed: {
    localComputed () { /* ... */ },
    // 使用对象展开运算符将此对象混入到外部对象中
    ...mapState({
      // ...
    })
  }
  ```

  

### Getter

在 store 中定义`getters`，可以认为是 **store 的计算属性**，返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。Getter 的第一个参数为 state，也可以接受其他 getter 作为第二个参数

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)  
      //store.getters.doneTodos: [{ id: 1, text: '...', done: true }]
    },
    doneTodosCount: (state, getters) => {
      return getters.doneTodos.length  //1
    }
  }
})
```

- **访问**

  - **通过属性访问**

    getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的

    `this.$store.getters`

  - **通过方法访问**

    可以通过让 getter 返回一个函数，来实现给 getter 传参

    ```js
    getters: {
      // ...
      getTodoById: (state) => (id) => {
        return state.todos.find(todo => todo.id === id)
      }
    }
    
    store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
    ```

- **mapGetters辅助函数**

  ```js
  import { mapGetters } from 'vuex'
  
  export default {
    // ...
    computed: {
    // 使用对象展开运算符将 getter 混入 computed 对象中
      ...mapGetters([
        'doneTodosCount',
        'anotherGetter',
        // ...
      ])
    }
  }
  
  //将一个 getter 属性另取一个名字，使用对象形式
  ...mapGetters({
    // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
    doneCount: 'doneTodosCount'
  })
  ```



### Mutation

更改 store 中的状态的唯一方法是提交 mutation

 mutation 类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。回调函数即进行状态更改的地方，它会接受 state 作为第一个参数

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})

//调用
this.$store.commit('increment')
```

- **提交载荷**

  向 `store.commit` 传入**额外的参数**，即 mutation 的载荷

  ```javascript
  mutations: {
    increment (state, n) {
      state.count += n
    }
  }
  store.commit('increment', 10)
  
  //在大多数情况下，载荷应该是一个对象
  mutations: {
    increment (state, payload) {
      state.count += payload.amount
    }
  }
  store.commit('increment', {
    amount: 10
  })
  ```

- **对象风格的提交方式**

  直接使用包含 `type` 属性的对象

  ```js
  store.commit({
    type: 'increment',
    amount: 10
  })
  mutations: {
    increment (state, payload) {
      state.count += payload.amount
    }
  }
  ```



### Action



### Module

