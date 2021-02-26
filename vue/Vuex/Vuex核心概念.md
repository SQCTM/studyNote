# Vuex核心概念

[TOC]

## 什么是Vuex

是一个专为 Vue.js 应用程序开发的**状态管理模式**，它把组件的共享状态抽取出来，以一个**全局单例**模式管理

Vuex 的状态存储是响应式的，Vuex 通过 `store` 选项，提供了一种机制将状态从根组件“注入”到每一个子组件中

```javascript
new Vue({
  el: '#app',
  store,   // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  router,
  components: { App },
  template: '<App/>'
})
```

通过在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过`this.$store `访问到



## 核心组成部分

- **State**
- **Getter**
- **Mutation**
- **Action**
- **Module**

![vuex](F:\前端笔记\studyNote\images\vuex.png)

### State



### Getter



### Mutation



### Action



### Module

