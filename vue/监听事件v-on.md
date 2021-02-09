# 监听事件v-on

[TOC]

## v-on指令

监听 DOM 事件，并在触发时运行一些 JavaScript 代码

- **事件处理方法**

  ```html
  <div id="example-2">
    <!-- greet是在下面定义的方法名 -->
    <button v-on:click="greet">Greet</button>
  </div>
  ```

  ```javascript
  var example2 = new Vue({
    el: '#example-2',
    data: {
      name: 'Vue.js'
    },
    // 在methods对象中定义方法
    methods: {
      greet: function (event) {
        alert('Hello ' + this.name + '!');  //this在方法里指向当前 Vue 实例
        if (event) {   //event是原生DOM事件
          alert(event.target.tagName);  //显示BUTTON
        }
      }
    }
  })
  ```

- **特殊变量$event**

  需要在内联语句处理器中访问原始的 DOM 事件时，可以用特殊变量 `$event` 把它传入方法

  ```html
  <button v-on:click="warn('Form cannot be submitted yet.', $event)">
    Submit
  </button>
  ```

  ```javascript
  methods: {
    warn: function (message, event) {
      // 这样可以访问原生事件对象
      if (event) {
        event.preventDefault()
      }
      alert(message)
    }
  }
  ```



## 事件修饰符

由点开头的指令后缀来表示

- `.stop`：阻止事件继续传播
- `.prevent`：阻止默认行为
- `.capture`：使用事件捕获模式，即内部元素触发的事件先在此处理，然后才交由内部元素进行处理
- `.self`：只当在 event.target 是当前元素自身时触发处理函数，即事件不是从内部元素触发的
- `.once`：事件只会触发一次，2.1.4 新增
- `.passive`：默认行为立即触发，2.3.0 新增，尤其能够提升移动端的性能

```html
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待onScroll完成  -->
<!-- 这其中包含event.preventDefault()的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

- **注意点**
  - 使用修饰符时顺序很重要。例如：用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击
  - `.once` 可以被用到自定义的组件事件中，其它只能用到只能对原生的 DOM 事件起作用的修饰符
  - 不要同时使用 `.passive` 和 `.prevent` ，因为 `.prevent` 会被忽略，同时浏览器可能会向你警告。`.passive` 会告诉浏览器你不想阻止事件的默认行为



## 按键修饰符

```html
<!-- 只有在key是Enter时调用vm.submit() -->
<input v-on:keyup.enter="submit">
```

可以直接将 `KeyboardEvent.key` 暴露的任意有效按键名转换为 kebab-case 来作为修饰符

```html
<input v-on:keyup.page-down="onPageDown">
<!-- 在上述示例中，处理函数只会在$event.key等于PageDown 时被调用 -->
```



## 系统修饰键

2.1.0 新增，可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`：Mac 系统对应 command 键 (⌘)；Windows 系统对应 Windows 徽标键 (⊞)
- `.exact`：2.5.0 新增，控制由精确的系统修饰符组合触发的事件

```html
<!-- Ctrl + Click -->
<div v-on:click.ctrl="doSomething">Do something</div>

<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button v-on:click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button v-on:click.exact="onClick">A</button>
```

- **注意**

  修饰键与常规按键不同，比如在和 `keyup` 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 `ctrl` 的情况下释放其它按键，才能触发 `keyup.ctrl`



## 鼠标按钮修饰符

2.2.0 新增

- `.left`
- `.right`
- `.middle`



## v-on优点

- ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试
- 当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除