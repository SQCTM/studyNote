# DOM

[TOC]

## 节点层次

DOM可以将任何HTML或XML文档描绘成一个由多层节点构成的结构

```html
<html>     
    <head>         
    <title>Sample Page</title>    
    </head>     
    <body>         
        <p>Hello World!</p>     
    </body> 
</html> 

//文档节点是每个文档的根节点，此例中文档节点只有一个子节点，<html>元素即文档元素。每个文档只能有一个文档元素。在HTML页面中，文档元素始终都是<html>元素
```

![image-20200809162615658](C:\Users\aa\AppData\Roaming\Typora\typora-user-images\image-20200809162615658.png)



### Node类型

所有节点类型都继承自Node类型，因此**所有节点类型都共享着相同的基本属性和方法**

#### 属性

- **nodeType**

  表明节点的类型，节点类型由12个数值常量来表示，判断节点类型时可以将其直接与数字值进行比较

- **nodeName**：元素的标签名

- **nodeValue**：节点的值

- **childNodes**

  保存着一个含该节点所有子节点的NodeList对象，有length属性，是基于DOM结构动态执行查询的结果

  - **NodeList**

    NodeList是一个**类数组对象**（伪数组），用于保存一组有序的节点，可以通过位置来访问这些节点

  - **访问NodeList**

    - 方括号：`var firstChild = someNode.childNodes[0]`
    - item()方法：`var firstChild = someNode.childNodes.item(0)`

  - 使用Array.prototype.slice()方法可以将NodeList对象转换为数组

- **parentNode**

  - 指向文档树中的父节点
  - 包含在childNodes 列表中的所有节点都具有相同的父节点，因此它们的parentNode属性都指向同一个节点

- **previousSibling和nextSibling**

  - childNodes列表中的每个节点相互之间都是同胞节点
  - previousSibling指上一个同胞节点；nextSibling指下一个同胞节点
  - 第一个同胞节点的previousSibling属性为null，而列表中后一个节点的nextSibling属性也为null

- **firstChild和lastChild**

  - firstChild为父节点的第一个子节点；lastChild为父节点的最后一个子节点
  - 只有一个子节点时，firstChild和lastChild指向同一个节点；没有子节点时，firstChild和lastChild的值均为null

- **ownerDocument**

  指向表示整个文档的文档节点

![image-20200809171318438](C:\Users\aa\AppData\Roaming\Typora\typora-user-images\image-20200809171318438.png)



#### 方法

- **hasChildNodes()**

  在节点包含一或多个子节点时返回true，否则返回false

- **appendChild()**

  向childNodes列表的末尾添加一个节点，更新完成后，返回新增的节点

  *如果传入到appendChild()中的节点已是文档的一部分，那就将该节点**从原来的位置转移到新位置**，因为任何DOM节点也不能同时出现在文档中的多个位置上

- **insertBefore()**

  - 把节点放在childNodes列表中某个特定的位置上
  - 接受两个参数：要插入的节点和作为参照的节点；
  - 插入的节点被插在参照节点的**前一个位置**
  - 结果返回插入的节点

- **replaceChild()**
  接受两个参数：要插入的节点和要被替换的节点；被替换的节点将由这个方法返回并从文档树中被移除，同时由要插入的节点占据其位置

- **removeChild()**

  移除节点，结果返回被移除的节点

  *被移除的节点仍为文档所有，只是在文档中没有了自己的位置

- **cloneNode()**

  复制节点；接受一个参数为布尔值：

  - 若参数为true，即进行**深复制**，是**复制节点及其整个子节点树**
  - 若参数为false或不传参，即进行**浅复制**，是**只复制节点本身**

- **normalize()**

  处理文档树中的文本节点，在调用该方法的节点中：

  - 删除空文本节点
  - 合并相邻的文本节点为一个文本节点



### Document类型

Document类型表示文档

- **document对象**

  - 在浏览器中，document对象是HTMLDocument（继承自Document 类型）的一个实例，**表示整个HTML页面**
  - document对象是window对象的一个属性，因此**可以将其作为全局对象来访问**

- **Document节点特征属性**

  - nodeType的值为9
  - nodeName的值为"#document"
  - nodeValue的值为null
  - parentNode的值为null
  - ownerDocument的值为null

- **其他属性**

  - **文档的子节点**（属性指向）

    - **documentElement属性**

      始终指向HTML页面中的\<html>元素：` document.documentElement`

    - **body属性**

      直接指向\<body>元素：`document.body`

    - **doctype属性**

      指向<!DOCTYPE>：`document.doctype`

      - 通常将<!DOCTYPE>标签看成一个与文档其他部分不同的实体
      - 由于浏览器对document.doctype的支持不一致，因此这个属性的用处很有限

  - **文档信息 **（属性指向）

    -  **title属性**

      包含着\<title>元素中的文本，即显示在浏览器窗口的标题栏或标签页上的文本：` document.title`

      *修改title属性的值不会改变\<title> 元素

    -  **URL属性**

      包含页面完整的URL（即地址栏中显示的 URL）。不可以设置

    - **domain属性**

      包含页面的域名

      - URL与domain属性是相互关联的。

        eg：document.URL等于http://www.wrox.com/WileyCDA/，则document.domain等于www.wrox.com 

      - 可以设置，但有限制：

        - 不能将这个属性设置为URL中不包含的域

        - 如果域名一开始是“松散的”（loose），那么不能将它再设 置为“紧绷的”（tight）

          eg：将document.domain设置为"wrox.com"之后，就不能再设置为"p2p.wrox.com"

    - **referrer属性**

      保存着链接到当前页面的那个页面的URL。在没有来源页面的情况下，referrer属性中可能会包含空字符串。不可以设置

- **方法**

  - **getElementById()**

    根据ID名查找元素并返回，没找到则返回null。只返回文档中第一次出现的元素

  - **getElementsByTagName()**

    根据标签名查找元素，返回一个**HTMLCollection对象**，作为动态集合。 `document.getElementsByTagName("*")`指取得文档中所有元素

    **HTMLCollection对象**：

    - 和NodeList类似
    - 有length属性
    - 可以用方括号和item()访问
    - 有**namedItem()方法**，通过元素的name特性取得集合中的项

  - **getElementsByClassName()**

    通过class类名查找元素，与getElementsByTagName()类似

  - **getElementsByName()**

    通过name特性查找元素，与getElementsByTagName()类似

  *这些查找元素的方法也可以在Element元素上调用

- **特殊集合**

  - document.anchors，包含文档中所有带 name 特性的\<a>元素
  - document.forms，包含文档中所有的\<form>元素
  - document.images，包含文档中所有的\<img>元素
  - document.links，包含文档中所有带href特性的\<a>元素

  *这些集合都是HTMLCollection对象

- **文档写入**

  - **write()**

    将文本写入到输出流中，原样写入

  - **writeln()**

    将文本写入到输出流中，会在字符串的末尾添加一个换行符（\n）

  - **open()**

    打开网页输出流

  - **close()**

    关闭网页输出流

  



### Element类型

HTMLElement类型直接继承自Element并添加了一些属性，所有 HTML元素都是由 HTMLElement 或者其更具体的子类型来表示的

- **特征属性**

  - nodeType的值为1
  - nodeName的值为元素的标签名（Element也可以使用tagName属性访问标签名
  - nodeValue的值为null
  - parentNode可能是Document或Element

- **特性**

  - **标准特性**

    - **id**，元素在文档中的唯一标识符
    -  **title**，有关元素的附加说明信息，一般通过工具提示条显示出来
    -  **lang**，元素内容的语言代码，很少使用
    - **dir**，语言的方向，值为"ltr"（left-to-right，从左至右）或"rtl"（right-to-left，从右至左） ， 也很少使用
    - **className**，类名（class在js中是保留字

  - **自定义特性**（HTML5规范）

    HTML5中新增dataset(数据集)(对象)属性，用来存放自定义属性，自定义格式必须为：**data-属性名**，获取自定义属性方法即：元素.dataset.属性名

  - **特性方法**

    - **getAttribute()**

      取得特性，也可以取得自定义特性，取得的是**字符串**，取不到则返回null

    - **setAttribute()**

      设置特性，设置的特性名会被统一转换为**小写**形式

    - **removeAttribute()**

      删除特性，只能删除非标准特性，标准特性一旦被设置就无法删除

  - **特性与属性**

    - 所有特性都是属性，但给DOM元素添加的自定义属性不会自动成为元素特性
    - 有两个特性用属性访问和用getAttribute()得到结果不同：
      - style特性，属性访问返回一个对象，getAttribute()访问返回CSS文本
      - onclick特性，属性访问返回一个函数，getAttribute()访问返回相应代码字符串

- **attributes 属性 **

  - Element 类型是使用attributes属性的唯一一个 DOM节点类型

  - attributes 属性中包含一个NamedNodeMap，是一个动态集合，元素的每一个特性都由一个Attr 节点表示，每个节点都保存在NamedNodeMap对象中

  - **NamedNodeMap对象方法**

    - getNamedItem(name)：返回nodeName属性等于name的节点
    - removeNamedItem(name)：从列表中移除nodeName属性等于name的节点，与 removeAttribute()方法的效果相同，唯一区别是removeNamedItem()返回表示被删除特性的 Attr节点
    - setNamedItem(node)：向列表中添加节点，以节点的nodeName属性为索引
    - item(pos)：返回位于数字pos位置处的节点

    *这些方法不方便，不常用

- **创建元素**

  document.createElement()方法，标签名在HTML中不分大小写



### Text类型

- **特征属性**

  -  nodeType的值为3
  - nodeName的值为"#text"
  - nodeValue的值为节点所包含的文本
  - parentNode是一个 Element
  - 不支持（没有）子节点
  - 还有一个length属性，保存着节点中的字符数目

- **操作节点方法**

  - appendData(text)：将text添加到节点的末尾
  - deleteData(offset, count)：从offset指定的位置开始删除count个字符
  - insertData(offset, text)：在offset指定的位置插入text
  - replaceData(offset, count, text)：用text替换从offset指定的位置开始到offset+count为止处的文本
  - splitText(offset)：从offset指定的位置将当前文本节点分成两个文本节点
  - substringData(offset, count)：提取从 offset 指定的位置开始到offset+count为止处的字符串

- **方法**

  - **document.createTextNode()**

    创建文本节点

  - **normalize()**

    规范化文本节点，删除空白节点，合并相邻文本节点（如果两个文本节点是相邻的同胞节点，那么这两个节点中的文本就会连起来显示，中间不会有空格

  - **splitText()**

    分割文本节点，将一个文本节 点分成两个文本节点，接收一个参数表示分割位置。原来的文本节点将包含从开始到指定位置之前的内容，新文本节点将包含剩下的文本。这个方法会返回的新文本节点与原节点的parentNode相同



### Comment类型

注释在DOM中是通过Comment类型来表示的。Comment类型与Text类型继承自相同的基类，因此它拥有除splitText()之外的所有字符串操作方法

- **特征属性**

  - nodeType的值为8
  - nodeName的值为"#comment"
  - nodeValue的值是注释的内容
  - parentNode可能是Document或Element
  - 不支持（没有）子节点

- **创建注释节点**

   document.createComment()，不常用



### CDATASection类型

CDATASection类型只针对基于XML的文档，表示的是CDATA区域。CDATASection 类型继承自 Text 类型，因此拥有除 splitText()之外的所有字符串操作方法

- **特征属性**

  - nodeType的值为4
  - nodeName的值为"#cdata-section"
  -  nodeValue的值是CDATA区域中的内容
  - parentNode可能是Document或Element
  - 不支持（没有）子节点

- **创建CDATA区域**

  document.createCDataSection()



### DocumentType类型

DocumentType类型在Web浏览器中并不常用，仅有Firefox、Safari、Chrome 4.0和Opera支持它。Document-Type 包含着与文档的doctype有关的所有信息

*DocumentType对象不能动态创建，而只能通过解析文档代码的方式来创建

- **特征属性**
  - nodeType的值为10
  - nodeName的值为doctype的名称
  - nodeValue的值为null
  - parentNode是Document
  - 不支持（没有）子节点
- **其他属性**
  - **name**：文档类型的名称
  - **entities**：由文档类型描述的**实体**的NamedNodeMap对象
  - **notations**：由文档类型描述的**符号**的NamedNodeMap对象



### DocumentFragment类型

文档片段，可以包含和控制节点。不能把文档片段直接添加到文档中，但可以将它作为一个“仓库”来使用，即可以在里面保存将来可能会添加到文档中的节点。添加到文档片段的节点不属于文档树，文档片段本身也不是文档树的一部分

- **特征属性**

  - nodeType的值为11
  - nodeName的值为"#document-fragment"
  - nodeValue的值为null
  - parentNode的值为null

- **创建片段**

  document.createDocumentFragment()

*插入节点浏览器会进行文档树重绘，多次插入则多次重绘，这样性能会下降。可以将要插入的节点都存入文档片段中再一起插入

*但IE浏览器使用此功能性能反而可能下降



### Attr类型

元素的特性在DOM中以Attr类型来表示，特性就是存在于元素的 attributes 属性中的节点

*特性不是文档树的一部分

- **特征属性**

  - nodeType的值为 2
  - nodeName的值是特性的名称
  -  nodeValue的值是特性的值
  - parentNode的值为 null
  - 在HTML中不支持（没有）子节点；在XML中子节点可以是Text或EntityReference

- **其他属性**

  - **name**：特性名称
  - **value**：特性值
  - **specified**：布尔值，用以区别特性是在代码中指定的还是默认的

- **创建节点**

  document.createAttribute()

*不建议直接访问特性节点



## 操作技术

### 动态脚本

- **两种方法**：

  - **插入外部文件**：

    ```javascript
    var script = document.createElement("script"); 
    script.type = "text/javascript";
    ```

    *动态加载的外部 JavaScript文件能够立即运行

  - **直接插入 JS代码**

    ```JavaScript
    var script = document.createElement("script"); 
    script.type = "text/javascript"; 
    var code = "function sayHi(){alert('hi');}"; 
    try {     
        script.appendChild(document.createTextNode(code));  //IE不支持
    } catch (ex){     
        script.text = "code";  //Safari 3.0之前的版本不支持
    } 
    document.body.appendChild(script); 
    ```

    **缺点**：

    - try-catch语句会破坏作用域，性能下降
    - 执行代码时解析器内部调用了eval()方法，性能下降



### 动态样式

- **两种方法**

  - **外部文件**

    ```javascript
    var link = document.createElement("link"); 
    link.rel = "stylesheet"; 
    link.type = "text/css"; 
    link.href = "style.css"; 
    var head = document.getElementsByTagName("head")[0]; 
    head.appendChild(link); 
    ```

  - **直接插入CSS代码**

    ```JavaScript
    var style = document.createElement("style"); 
    style.type = "text/css"; 
    try{     
        //IE不支持
        style.appendChild(document.createTextNode("body{background-color:red}")); 
    } catch (ex){    
        style.styleSheet.cssText = "body{background-color:red}";  
    } 
    var head = document.getElementsByTagName("head")[0]; 
    head.appendChild(style);
    
    //与动态加载JS脚本同理
    ```

    



### 操作表格

为了方便构建表格，HTML DOM为\<table>、\<tbody> 和\<tr>元素添加了一些属性和方法：

- **\<table>**
  - **属性**
    - caption：保存着对\<caption>元素（如果有）的指针
    - tBodies：是一个\<tbody>元素的 HTMLCollection
    - tFoot：保存着对\<tfoot>元素（如果有）的指针
    - tHead：保存着对\<thead>元素（如果有）的指针
    - rows：是一个表格中所有行的 HTMLCollection。 
  - **方法**
    - createTHead()：创建\<thead>元素，将其放到表格中，返回引用
    - createTFoot()：创建\<tfoot>元素，将其放到表格中，返回引用
    - createCaption()：创建\<caption>元素，将其放到表格中，返回引用
    -  deleteTHead()：删除\<thead>元素
    - deleteTFoot()：删除\<tfoot>元素
    - deleteCaption()：删除\<caption>元素
    - deleteRow(pos)：删除指定位置的行
    - insertRow(pos)：向 rows 集合中的指定位置插入一行。
- **\<tbody>**
  - **属性**
    - rows：保存着\<tbody>元素中行的HTMLCollection
  - **方法**
    - deleteRow(pos)：删除指定位置的行
    - insertRow(pos)：向 rows 集合中的指定位置插入一行，返回对新插入行的引用
- **\<tr>**
  - **属性**
    - cells：保存着\<tr>元素中单元格的 HTMLCollection
  - **方法**
    - deleteCell(pos)：删除指定位置的单元格
    - insertCell(pos)：向 cells 集合中的指定位置插入一个单元格，返回对新插入单元格的引用

eg：

```JavaScript
//创建 table 
var table = document.createElement("table"); 
table.border = 1; table.width = "100%"; 
 
//创建 tbody 
var tbody = document.createElement("tbody"); 
table.appendChild(tbody); 
 
//创建第一行 
tbody.insertRow(0); 
tbody.rows[0].insertCell(0); 
tbody.rows[0].cells[0].appendChild(document.createTextNode("Cell 1,1")); 
tbody.rows[0].insertCell(1); 
tbody.rows[0].cells[1].appendChild(document.createTextNode("Cell 2,1")); 
 
//创建第二行 
tbody.insertRow(1); 
tbody.rows[1].insertCell(0); 
tbody.rows[1].cells[0].appendChild(document.createTextNode("Cell 1,2")); 
tbody.rows[1].insertCell(1); 
tbody.rows[1].cells[1].appendChild(document.createTextNode("Cell 2,2")); 
 
//将表格添加到文档主体中 
document.body.appendChild(table);
```



### NodeList

所有NodeList对象都是在访问DOM文档时实时运行的查询，是动态的

```JavaScript
var divs = document.getElementsByTagName("div"),     
    i,      
    div; 
 
for (i=0; i < divs.length; i++){     
    div = document.createElement("div");     
    document.body.appendChild(div); 
} 

//此代码会无限循环
```

