# CSS图片宽高比自适应

```html
<div class="box">
    <div class="img-wrapper">
	  <img src="bear.jpg" class="image">
    </div>
</div> 

<style type="text/css">
	.box{
		width: 330px;
	}
	.img-wrapper{
		overflow: hidden;
		width: 100%;
		height: 0;
		/*让图片宽高比为10:3，图片高度自动撑开为宽度的30%*/
		padding-bottom: 30%;  
	}
	.image{
		width: 100%
	}
</style>
```

让图片的宽和高都为最外层父元素宽度的50%：

```html
<div class="box">
    <div class="img-wrapper">
	  <img src="bear.jpg" class="image">
    </div>
</div> 

<style type="text/css">
	.box{
		width: 330px;
		border: solid black 2px;
	}
	.img-wrapper{
		width: 50%;  /*令图片宽度为最外层父元素宽度的50%*/
		padding-bottom: 50%;  /*使图片的宽高比为1:1，图片高度与父元素高度无关*/
	}
	.image{
		width: 100%
	}
</style>
```

