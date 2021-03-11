# CSS实现居中

[TOC]

## 水平居中

### 行内元素

`text-align: center实现令块级元素内部的行内元素水平居中`

`text-align`属性规定了**元素中的文本的水平对齐方式**，具**有继承性**

此方法对行内元素(inline)，行内块(inline-block)，行内表(inline-table)，inline-flex元素水平居中都有效

```html
<div style="border: solid red 1px;text-align: center;width: 500px">
	<span style="border: solid blue 1px;">
	  水平居中
	<span>
</div>

<!--或-->
        
<div style="border: solid red 1px;text-align: center;width: 500px">
	水平居中
</div>
```



### 块级元素

将**固定宽度**的块级元素的`margin-left`和`margin-right`设成`auto`

```html
<div style="border: solid red 1px;">
	<p style="border: solid blue 1px;width: 200px; margin: 0 auto">
	一个固宽块级元素
	</p>
</div>
```



### 多块级元素

- **利用inline-block**
  如果一行中有两个以上块级元素，利用`display: inline-block`将它们变成行内块，使它们可以并行在同一行；再利用父容器的`text-align: center`使多块级元素水平居中

  ```html
  <div style="border: solid red 1px;text-align: center;">
    <div class="inline-block">块1</div>
    <div class="inline-block">块2</div>
  </div>
  
  <style type="text/css">
  .inline-block{
  	display: inline-block;
  	margin: 10px 5px;
  	border: solid blue 1px;
  }
  </style>
  ```

- **利用弹性布局**
  设置`display: flex`设定弹性布局，其中`justify-content`属性用于设置弹性盒子元素在主轴(横向)方向上的对齐方式

  ```html
  <div class="flex">
  	<div>块1</div>
  	<div>块2</div>
  </div>
  
  <style type="text/css">
  .flex{
  	display: flex;
  	justify-content: center;
  	border: solid red 1px;
  	width: 500px;
  }
  .flex>div{
  	border: solid blue 1px;
  	margin: 10 5px;
  }
  </style>
  ```



## 垂直居中

### 单行行内元素

设置内联元素的高度`height`和行高`line-height`相等即可实现

`line-height`属性设置行间的距离即**行高**，具**有继承性**

```html
<div style="height: 100px; line-height: 100px;border: solid red 1px;width: 500px;">
	<span style="border: solid blue 1px;">
	  垂直居中
	</span>
</div>

<!--或-->

<div style="height: 100px; line-height: 100px;border: solid red 1px;width: 500px;">
	垂直居中
</div>
```



### 多行元素

- **利用表格布局**

  `vertical-align: middle`

  ```html
  <div style="display: table; height: 100px; border: solid red 1px;width: 100px;">
  	<div style="display: table-cell;vertical-align: middle;border: solid blue 1px;">
    	  多行元素垂直居中
  	</div>
  </div>
  ```

- **利用弹性布局**
  弹性布局中`flex-direction: column`定义主轴方向为纵向，再结合`justify-content: center;`即可实现

  ```html
  <div class="flex">
  	<div style="border: solid blue 1px;">
    	  多行元素垂直居中
  	</div>
  </div>
  
  <style type="text/css">
  .flex{
  	border: solid red 1px;
  	display: flex;
      flex-direction: column;
      justify-content: center;
      height: 200px;
      width: 100px;
  }
  </style>
  ```

  



### 块级元素

- **固定宽高**

  以父元素为参照，相对其进行定位，设`top：50%`，并设置`margin-top`向上偏移自身高度的一半即可

  ```html
  <div class="box">
  	<div>垂直居中</div>
  </div>
  
  <style type="text/css">
  .box{
  	border: solid red 1px;
  	height: 100px;
  	position: relative; 
  }
  .box>div{
  	border: solid blue 1px;
  	height: 40px;
  	position: absolute; 
  	top: 50%;
  	margin-top: -20px;
  }
  </style>
  ```

- **未知宽高**

  也是先以父元素为参照进行相对定位，设`top：50%`，再借助CSS3中的`transform`属性向Y轴上移50%，实现垂直居中；部分浏览器可能存在兼容性问题

  ```html
  <div class="box">
  	<div>垂直居中</div>
  </div>
  
  <style type="text/css">
  .box{
  	border: solid red 1px;
  	height: 100px;
  	position: relative; 
  }
  .box>div{
  	border: solid blue 1px;
  	position: absolute; 
  	top: 50%;
  	transform: translateY(-50%);  
  }
  </style>
  ```



## 水平垂直居中

### 固定宽高

除了结合上述的水平居中和垂直居中的方法外，还有一个方法：

```html
<div class="box">
	<div>水平垂直居中</div>
</div>

<style type="text/css">
.box{
	border: solid red 1px;
	height: 150px;
	position: relative; 
}
.box>div{
	border: solid blue 1px;
	width: 100px;
	height: 20px;
	position: absolute; 
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	margin: auto;
}
</style>
```



### 未知宽高

利用transform属性

```html
<div class="box">
	<div>水平垂直居中</div>
</div>

<style type="text/css">
.box{
	border: solid red 1px;
	height: 150px;
	position: relative; 
}
.box>div{
	border: solid blue 1px;
	position: absolute; 
	top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
</style>
```



### flex布局

结合`justify-content: center;`，再设置`align-items`属性定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式

```html
 <div class="box">
	<div style="border: solid blue 1px;">水平垂直居中</div>
</div>

<style type="text/css">
.box{
	border: solid red 1px;
	height: 150px;
	display: flex;
	justify-content: center;
    align-items: center;
}
</style>
```

