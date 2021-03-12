# CSS flex弹性布局

[TOC]

## 设置弹性布局

弹性布局用来为盒状模型提供最大的灵活性

```css
.box{
  display: flex;
}

/*行内元素使用 Flex 布局*/
.box{
  display: inline-flex;
}

/*Webkit 内核的浏览器必须加上-webkit前缀*/
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
```

**注意**：设为 Flex 布局后，子元素的`float`、`clear`和`vertical-align`属性将失效



## 基本概念

采用 Flex 布局的元素，称为 Flex **容器**；它的所有子元素自动成为容器成员，称为 Flex **项目**![flex布局](F:\前端笔记\studyNote\images\flex布局.png)

容器默认存在两根轴：

- **水平的主轴**（`main axis`）
  主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`
- **垂直的交叉轴**（`cross axis`）
  交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`



## 容器属性

- `flex-direction`

  决定主轴的方向

  - `row`：默认值，主轴为水平方向，起点在左端
  - `row-reverse`：主轴为水平方向，起点在右端
  - `column`：主轴为垂直方向，起点在上沿
  - `column-reverse`：主轴为垂直方向，起点在下沿

- `flex-wrap`
  默认情况下，项目都排在一条轴线上。该属性定义，如果一条轴线排不下，如何换行

	- `nowrap`：默认，不换行
	- `wrap`：换行，第一行在上方
	- `wrap-reverse`：往上换行，即第一行在下方
	
- `flex-flow`
  是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`

- `justify-content`
  定义了项目在主轴上的对齐方式，当主轴为从左到右时：

  - `flex-start`：默认值，左对齐
  - `flex-end`：右对齐
  - `center`： 居中
  - `space-between`：两端对齐，项目之间的间隔都相等。
  - `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍

- `align-items`
  定义项目在交叉轴上如何对齐

  - `flex-start`：交叉轴的起点对齐
  - `flex-end`：交叉轴的终点对齐
  - `center`：交叉轴的中点对齐
  - `baseline`: 项目的第一行文字的基线对齐
  - `stretch`：默认值，如果项目未设置高度或设为auto，将占满整个容器的高度

- `align-content`
       定义了**多根轴线**的对齐方式，如果项目只有一根轴线，该属性不起作用

  - `flex-start`：与交叉轴的起点对齐
  - `flex-end`：与交叉轴的终点对齐
  - `center`：与交叉轴的中点对齐
  - `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布
  - `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
  - `stretch`：默认值，轴线占满整个交叉轴



## 项目属性

- `order`
  值为数值，定义项目的排列顺序。数值越小，排列越靠前，默认为0
  
- `flex-grow`
  有剩余空间时，定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
  
- `flex-shrink`
  定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
  
  **注意**：负值对该属性无效
  
- `flex-basis`
   定义了在分配多余空间之前，项目占据的主轴空间；浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小

  它可以设为跟`width`或`height`属性一样的值，则项目将占据固定空间

-  `flex`
    `flex-grow`, `flex-shrink`和`flex-basis`的简写，默认值为`0 1 auto`（后两个属性可选），该属性有两个**快捷值**：
    - `auto` (1 1 auto) 
    - `none` (0 0 auto)

  **注意**：建议**优先使用这个属性**，而不是单独写三个分离的属性，因为浏览器会推算相关值

- `align-self`

  - 允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性
  - 默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`
  - 该属性可能取6个值，除了`auto`，其他都与`align-items`属性完全一致