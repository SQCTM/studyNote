# WebComponent

[TOC]

WebComponent是一套技术的组合，能提供给开发者组件化开发的能力，具体涉及到了Custom elements(自义定元素)、Shadow DOM(影子DOM)和HTML templates(HTML模板)



## 组件化

**对内高内聚，对外低耦合**。对内各个元素彼此紧密结合、相互依赖，对外和其它组件的联系最少且接口简单



## 阻碍前端组件化的因素

CSS和DOM是阻碍组件化的一个因素。在大型项目中维护会比较困难，如果在页面中嵌入第三方内容时，还需要确保第三方的内容样式不会影响到当前内容，同样也要确保当前的DOM不会影响到第三方的内容

- **解决**

  WebComponent提供了对局部视图封装能力，可以让DOM、CSSOM和JS运行在局部环境中，这样就使得局部的CSS和DOM不会影响到全局



## WebComponent组件化的实现

1. **使用template属性来创建模板**

   利用DOM可以查找到模板的内容，但是模板元素是不会被渲染到页面上，即DOM树中的template节点不会出现在布局树中，所以可以用template来自定义一些基础的元素结构，这些基础的元素结构是可以被重复使用的

   ```HTML
   <template id="testComponent">
       <style>
           p {
               background-color:black;
           }
           div {
               width: 200px;
               background-color: blue;
               border: 3px solid green;
           }
       </style>
       <script>
         function foo() {
             console.log('inner log')
         }
       </script>
   </template>
   ```

2. **创建一个相关的类**

   在该类的**构造函数**中要完成三件事：

   1. **查找模板内容**
   2. **创建影子DOM**
   3. **再将模板添加到影子DOM上**

   *影子DOM的作用是将模板中的内容与全局DOM和CSS进行隔离，这样就可以实现元素和样式的私有化。可以把影子DOM看成是一个作用域，其内部的样式和元素是不会影响到全局的，且在全局环境下要访问影子DOM内部的样式或者元素也是需要通过约定好的接口的，使用DOM接口无法直接查询到影子DOM内部元素

   ```html
   <script>
       class TestComponent extends HTMLElement {
           constructor() {
               super();
               //获取组件模板
               const content = document.querySelector('#testComponent').content;
               //创建影子DOM节点
               const shadowDOM = this.attachShadow({mode: 'open'});
               //将模板添加到影子DOM上
               shadowDOM.appendChild(content.cloneNode(true))
           }
       }
       //自义定元素
       customElement.define('test-component', TestComponent);
   </script>
   ```

3. **像正常使用HTML元素一样直接使用**

   ```HTML
   <test-component></test-component>
   ```

***影子DOM的JS脚本不会被隔离**，比如在影子DOM定义的JS函数依然可以被外部访问，这是因为JS语言本身已经可以很好的实现组件化了



## 浏览器如何实现影子DOM

- **影子DOM作用**

  - 影子DOM中的元素对整个页面是不可见的
  - 影子DOM的CSS不会影响到整个网页的CSSOM，影子DOM内部的CSS只对内部的元素起作用

- **实现**

  影子DOM都可以看做是一个独立的DOM，它有一个**shadow root作为根节点**，可以将要展示的样式或元素添加到影子DOM的根节点上

  浏览器为了实现影子DOM特性在代码内部做了大量的条件判断，当通过DOM接口去查找元素时渲染引擎会去判断shadow root元素是否是影子DOM，如果是就直接跳过该元素的查询操作，所以这样通过DOM API就无法直接查询到影子DOM的内部元素

  当生成布局树时渲染引擎会判断shadow root元素是否是影子DOM，如果是那么在影子DOM内部元素节点选择CSS样式的时候会直接使用影子DOM内部的CSS属性，所以这样最终渲染出来的效果就是影子DOM内部定义的样式

