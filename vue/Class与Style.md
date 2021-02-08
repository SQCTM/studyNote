# Class与Style

[TOC]

## Class语法

- **对象语法**

  - **写法1**

    ```html
    <div
      class="static"
      v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
    ```
  
    ```javascript
  data: {
      isActive: true,
      hasError: false
    }
    ```
    
    ```html
    <!--结果渲染为-->
    <div class="static active"></div>
      
    <!--hasError 的值为 true 时-->
    <div class="static active text-danger"></div>
    ```
  - **写法2**
  
     ```html
    <div v-bind:class="classObject"></div>
    ```
  
    ```javascript
    data: {
      classObject: {
        active: true,
        'text-danger': false
      }
    }
    ```
    
  - **写法3**
  
     ```html
    <div v-bind:class="classObject"></div>
    ```
  
    ```javascript
    data: {
      isActive: true,
      error: null
    },
    computed: {
      classObject: function () {
        return {
          active: this.isActive && !this.error,
          'text-danger': this.error && this.error.type === 'fatal'
        }
      }
    }
    ```
  
- **数组语法**

  ```html
  <div v-bind:class="[activeClass, errorClass]"></div>
  ```

  ```javascript
  data: {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
  ```

  ```html
  <!--结果渲染为-->
  <div class="active text-danger"></div>
  ```

- **使用在组件上**

  ```javascript
  Vue.component('my-component', {
    template: '<p class="foo bar">Hi</p>'
  })
  ```

  ```html
  <my-component class="baz boo"></my-component>
  
  <!--结果渲染为-->
  <p class="foo bar baz boo">Hi</p>
  ```

  

## 内联样式Style

- **对象语法**

  - **写法1**

    ```html
    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
    ```

    ```javascript
    data: {
      activeColor: 'red',
      fontSize: 30
    }
    ```

  - **写法2**

    ```html
    <div v-bind:style="styleObject"></div>
    ```

    ```javascript
    data: {
      styleObject: {
        color: 'red',
        fontSize: '13px'
      }
    }
    ```

  - **写法3**

    也可以结合计算属性

- **数组语法**

  可以将多个样式对象应用到同一个元素上

  ```html
  <div v-bind:style="[baseStyles, overridingStyles]"></div>
  ```

- **自动加前缀**

  当 `v-bind:style` 使用需要添加[浏览器引擎前缀](https://developer.mozilla.org/zh-CN/docs/Glossary/Vendor_Prefix)的 CSS property 时，vue 会自动侦测并添加相应的前缀

- **多重值**

  2.3.0+起可以为 `style` 绑定中的 property 提供一个包含多个值的数组，常用于提供多个带前缀的值

  例如：`<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>`

  这样写只会渲染数组中**最后一个被浏览器支持的值**