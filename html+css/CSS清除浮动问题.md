# CSS清除浮动问题

[TOC]

## 浮动问题

给一个盒子使用CSS的`flloat`浮动属性，导致其父元素盒子高度坍塌，不能被撑开

```html
<div class="parent">
    <div class="son"></div>
    <div class="son"></div>
</div> 

<style type="text/css">
	.parent{
		border: red solid 2px;
		width: 200px;
	}
	.son{
		border: blue solid 2px;
		width: 50px;
		height: 50px;
		float: left;   /*float: right;也一样*/
		margin: 0 5px;
	}
</style>
```

![浮动问题](F:\前端笔记\studyNote\images\浮动问题.png)



## 解决方法

- **方法1**

  给父级元素设定盒子高度`height`

  ```html
  <div class="parent">
      <div class="son"></div>
      <div class="son"></div>
  </div> 
  
  <style type="text/css">
  	.parent{
  		border: red solid 2px;
  		width: 200px;
  		height: 100px;
  	}
  	.son{
  		border: blue solid 2px;
  		width: 50px;
  		height: 50px;
  		float: left;   /*float: right;也一样*/
  		margin: 0 5px;
  	}
  </style>
  ```

- **方法2**

  新建一个div，给其CSS样式加上`clear: both;`属性，并在父级元素结束前引入

  ```html
  <div class="parent">
      <div class="son"></div>
      <div class="son"></div>
      <div style=""></div>
  </div> 
  
  <style type="text/css">
  	.parent{
  		border: red solid 2px;
  		width: 200px;	
  	}
  	.son{
  		border: blue solid 2px;
  		width: 50px;
  		height: 50px;
  		float: left;   /*float: right;也一样*/
  		margin: 0 5px;
  	}
  	.clear{
  		clear: both;
  		border: black solid 2px;
  	}
  </style>
  ```

  - **clear属性**
    该属性的值指出了不允许有浮动对象的边情况
    - **参数值说明**
      - `none`：允许两边都可以有浮动对象
      - `both`：不允许有浮动对象
      - `left`：不允许左边有浮动对象
      - `right`：不允许右边有浮动对象

- **方法3**

  给父级元素CSS样式加上`overflow:hidden`，浏览器会自动检查浮动区域的高度

  ```html
  <div class="parent">
      <div class="son"></div>
      <div class="son"></div>
  </div> 
  
  <style type="text/css">
  	.parent{
  		border: red solid 2px;
  		width: 200px;
  		overflow: hidden;	
  	}
  	.son{
  		border: blue solid 2px;
  		width: 50px;
  		height: 50px;
  		float: left;   /*float: right;也一样*/
  		margin: 0 5px;
  	}
  </style>
  ```

- **方法4**

  给父级元素定义定义伪类:after和`zoom`属性

  ```html
  <div class="parent clear">
      <div class="son"></div>
      <div class="son"></div>
  </div> 
  
  <style type="text/css">
  	.parent{
  		border: red solid 2px;
  		width: 200px;
  	}
  	.son{
  		border: blue solid 2px;
  		width: 50px;
  		height: 50px;
  		float: left;   /*float: right;也一样*/
  		margin: 0 5px;
  	}
  	.clear{  /*解决ie6,ie7浮动问题*/
  		zoom:1;
  		}
      .clear:after{  /*三者缺一不可*/
        display:block;
        clear:both;
        content:"";
      }
  </style>
  ```