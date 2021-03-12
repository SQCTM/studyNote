# CSS实现绘制三角形

[TOC]

## 原理

三角形可以通过调整盒子的边框`border`属性的四个方向的宽度、线条样式以及颜色来实现
将盒子的边框调到足够宽就会发现，盒模型的边框是4条**像梯形一样的线**

```html
<div class="test"></div>

<style type="text/css">
	.test{
		background-color: red;
		width: 100px;
		height: 100px;
		border-left: solid blue 20px;
		border-top: solid yellow 20px;
		border-right: solid pink 20px;
		border-bottom: solid black 20px;
	}
</style>
```

![盒子边框](F:\前端笔记\studyNote\images\盒子边框.png)



## 实心三角形

### 等腰非直角

```html
<div class="box"></div>    <!--原理-->
<div class="up"></div>     <!--向上-->
<div class="down"></div>   <!--向下-->
<div class="left"></div>   <!--向左-->
<div class="right"></div>  <!--向右-->

<style type="text/css">
	div{
        width: 0px;
		height: 0px;
		margin: 10px;	
		display: inline-block;
	}
	.box{
		border-bottom: solid 40px red; 
		border-right: solid 20px pink;
		border-left: solid 20px blue;
	}
	.up{
		border-bottom: solid 40px red; 
		border-right: solid 20px transparent;
		border-left: solid 20px transparent;
	}
	.down{
		border-top: solid 40px red; 
		border-right: solid 20px transparent;
		border-left: solid 20px transparent;
		
	}
	.left{
		border-top: solid 20px transparent; 
		border-bottom: solid 20px transparent;
		border-left: solid 40px red;
	}
	.right{
		border-top: solid 20px transparent; 
		border-bottom: solid 20px transparent;
		border-right: solid 40px red;
	}
</style>
```



### 等腰直角

**例子1**：

```html
<div class="box"></div>    <!--原理-->
<div class="up"></div>     <!--向上-->
<div class="down"></div>   <!--向下-->
<div class="left"></div>   <!--向左-->
<div class="right"></div>  <!--向右-->

<style type="text/css">
	div{
        width: 0px;
		height: 0px;
		border-style: solid;
		border-width: 30px;
		margin: 10px;
		display: inline-block;
	}
	.box{
		border-color: pink yellow blue orange;
	}
	.up{
		border-color: transparent transparent red transparent; 
	}
	.down{
		border-color: red transparent transparent transparent; 
		
	}
	.left{
		border-color: transparent transparent transparent red; 
	}
	.right{
		border-color: transparent red transparent transparent; 
	}
</style>
```



**例子2**：

```html
<div class="box"></div>    <!--原理-->
<div class="up"></div>     <!--向上-->
<div class="down"></div>   <!--向下-->
<div class="left"></div>   <!--向左-->
<div class="right"></div>  <!--向右-->

<style type="text/css">
	div{
        width: 0px;
		height: 0px;
		margin: 10px;	
		display: inline-block;
	}
	.box{
		border-bottom: solid 30px red; 
		border-right: solid 30px pink;
		border-left: solid 30px blue;
	}
	.up{
		border-bottom: solid 30px red; 
		border-right: solid 30px transparent;
		border-left: solid 30px transparent;
	}
	.down{
		border-top: solid 30px red; 
		border-right: solid 30px transparent;
		border-left: solid 30px transparent;
		
	}
	.left{
		border-top: solid 30px transparent; 
		border-bottom: solid 30px transparent;
		border-left: solid 30px red;
	}
	.right{
		border-top: solid 30px transparent; 
		border-bottom: solid 30px transparent;
		border-right: solid 30px red;
	}
</style>
```



## 有边框的三角形

两个三角形重叠

```html
<div class="box">
	<div></div>
</div> 

<style type="text/css">
	div{
        width: 0px;
		height: 0px;	
	}
	.box{
		border-left: 30px solid transparent;
		border-right: 30px solid transparent;
		border-bottom: 30px solid black;
		position: relative;
	}
	.box>div{
		border-left: 28px solid transparent;
		border-right: 28px solid transparent;
		border-bottom: 28px solid red;
		position: absolute;
		left: -28px;
		top: 1px;
	}
</style>
```





