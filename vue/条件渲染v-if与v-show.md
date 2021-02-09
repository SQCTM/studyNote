# 条件渲染v-if与v-show

[TOC]

## v-if指令

用于条件性地渲染一块内容，此块内容只会在指令的表达式返回`truthy`[^1]值的时候被渲染

- **v-else-if**

  2.1.0 新增，充当 `v-if` 的"else-if 块"，可以连续使用，必须紧跟在带`v-if`或者`v-else-if`的元素之后

- **v-else**

  与`v-if`搭配使用，`v-else` 元素必须紧跟在带`v-if`或`v-else-if`的元素后面，否则它将不会被识别

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

<br>

### 在\<template>元素上使用

想切换多个元素时可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 `v-if`，最终的**渲染结果将不包含 `<template>` 元素**

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

<br>

### 用key管理可复用元素

Vue 会尽可能高效地渲染元素，通常会**复用已有元素**而不是从头开始渲染

```html
<!-- 例如 -->
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
<!-- 上面的代码中切换loginType将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，<input>不会被替换掉，仅仅是替换了它的placeholder -->
```

`key`是具有唯一值的一个attribute，它可以**让两个元素不会发生复用完全独立**

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
<!-- 上面代码中，每次切换时输入框都将被重新渲染，但<label>元素仍会被高效地复用，因为它们没有添加key -->
```

<br>

## v-show指令

用于根据条件展示元素的选项

```html
<h1 v-show="ok">Hello!</h1>
```

- **特点**

  `v-show`的元素**始终会被渲染并保留在 DOM 中**。`v-show` 只是简单地切换元素的CSS property `display`

- **注意**

  `v-show` 不支持 `<template>` 元素，也不支持 `v-else`

<br>

## v-if与v-show对比

- **v-if**
  - 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建
  - 是**惰性的**，如果在初始渲染时条件为假则什么也不做，直到条件第一次变为真时才会开始渲染条件块
  - 切换开销更高
- **v-show**
  - 不管初始条件是什么，元素总是会被渲染，只是简单地基于 CSS 进行切换
  - 初始渲染开销更高





<br>



[^1]: `truthy` (真值)指的是在布尔值上下文中转换后的值为真的值，除了`Falsy`假值(`false`、`0`、`""`、`null`、`undefined` 和 `NaN`)以外所有的值都是真值

