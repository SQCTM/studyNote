# DOM扩展

[TOC]

## Selectors API

一个致力于让**浏览器原生支持CSS查询**的标准

- **核心方法**

  - **querySelector()**

    接收一个 CSS选择符，返回与该模式匹配的**第一个元素**，如果没有找到则返回null

    - 通过Document类型调用时，会在文档元素的范围内查找匹配的元素
    - 通过Element类型调用时，只会在该元素后代元素的范围内查找匹配的元素

  - **querySelectorAll()**

    返回一个NodeList的实例，包含所有匹配的元素，如果**没有找到匹配的元素则NodeList是空的**

  *两个方法都是如果传入了浏览器不支持的选择符或者选择符中有语法错误，则会抛出错误

  *两个返回的**NodeList都是静态**的，且**运行速度慢很多**

- **Element类型方法**

  - **matchesSelector()**

    接收一个参数CSS选择符，如果调用元素与该选择符匹配则返回 true；否则返回 false



## Element Traversal API

为DOM元素定义了额外的属性，让开发人员能够更方便地从一个元素跳到另一个元素。之所以会出现这个扩展是因为浏览器处理DOM元素间空白符的方式不一样（元素中若只有空白符也可以算作文本节点

**为Element元素添加的5个属性**：

- childElementCount：返回子元素（不包括文本节点和注释）的个数
- firstElementChild：指向第一个子元素；firstChild 的元素版
- lastElementChild：指向后一个子元素；lastChild 的元素版
- previousElementSibling：指向前一个同辈元素；previousSibling 的元素版
- nextElementSibling：指向后一个同辈元素；nextSibling 的元素版



## HTML5

- **与类相关的扩充**

  - **getElementsByClassName()**

  - **classList**

    是新集合类型DOMTokenList的实例，是一个元素所有类名的集合

    - 有length属性
    
    - 访问可以用item()，也可以用方括号语法
    
    - **相关方法**：
      - add(value)：将给定的字符串值添加到列表中。如果值已经存在，就不添加了
      - contains(value)：表示列表中是否存在给定的值，如果存在则返回 true，否则返回 false
      - remove(value)：从列表中删除给定的字符串
      -  toggle(value)：如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它
      
      *这些方法都只能传入一个字符串且不能包含空格
      
    - 支持classList属性的浏览器有Firefox 3.6+和Chrome

  *用for循环多次添加不同class浏览器只会渲染一次，因为渲染和js解析器运行是同步的，在同一时间内只有一个在工作

- **焦点管理**

  辅助管理DOM焦点的功能

  - **document.activeElement属性**

    引用DOM中当前获得了焦点的元素

    - 元素获得焦点的方式有：页面加载、用户输入（通常是 通过按 Tab 键）和在代码中调用focus()方法
    - 默认情况下，文档刚刚加载完成时，属性中保存的是document.body元素的引用；文档加载期间，属性的值为null

  - **document.hasFocus()方法**

    用于确定文档是否获得了焦点。是则返回true；否则返回false

- **HTMLDocument的扩展**

  - **readyState 属性**

    指示文档是否加载完成。若值为loading则是正在加载文档；若值为complete则是已经加载完文档

  - **compatMode的属性**

    检测页面的兼容模式，区分渲染页面的模式是标准的还是混杂的

  - **head属性**

    document.head属性， 引用文档的\<head>元素

    *双重保险：`var head = document.head || document.getElementsByTagName("head")[0]; `

- **字符集属性**

  - **charset属性**

    表示文档中实际使用的字符集， 也可以用来指定新字符集。默认情况下，属性的值为"UTF-16"

  - **defaultCharset属性**

    表示根据默认浏览器及操作系统的设置，当前文档默认的字符集。如果文档没有使用默认的字符集，那charset和defaultCharset属性的值可能会不一 样

- **自定义数据属性**

  dataset(数据集)(对象)属性，用来存放自定义属性。自定义属性的格式必须为：**data-属性名**；获取自定义属性方法即：**元素.dataset.属性名**

  *可以给元素添加一些不可见的数据以便进行其他处理

- **插入标记**

  - **innerHTML属性**

    - 读取时，返回与调用元素的所有子节点（包括元素、注释和文本节点）对应的HTML标记

    - 写入时，会根据指定的值创建新的DOM树，然后用这个DOM树完全替换调用元素原先的所有子节点（如果设置的值仅是文本而没有HTML标签，那么结果就是设置纯文本

    - **限制**

      在大多数浏览器中，通过innerHTML插入\<script>元素并不会执行其中的脚本（但大多数浏览器都支持以直观的方式通过innerHTML插入\<style>元素

  - **outerHTML属性**

    - 读取时，返回调用它的元素及所有子节点的HTML标签
    - 写入时，会根据指定的HTML字符串创建新的DOM子树，然后用这个DOM子树完全替换调用元素

  - **insertAdjacentHTML()方法**

    元素同辈之间的插入，接受两个参数

    - **第一个参数**

      插入位置，应在以下固定值中选取：

      - "beforebegin"，在当前元素之前插入一个紧邻的同辈元素
      - "afterbegin"，在当前元素之下插入一个新的子元素或在第一个子元素之前再插入新的子元素
      - "beforeend"，在当前元素之下插入一个新的子元素或在后一个子元素之后再插入新的子元素
      -  "afterend"，在当前元素之后插入一个紧邻的同辈元素

      *都必须是小写形式

    - **第二个参数**

      要插入的 HTML文本，如果浏览器无法解析该字符串，就会抛出错误

  - **内存与性能问题**

    - 以上方法替换子节点可能会导致浏览器的内存占用问题，尤其是在IE中问题更加明显。替换子节点时，被替换出的节点会被删除，而其对应的事件处理程序或引用的JS对象在内存中没有被一并删除。如果这种情况频繁出现，页面占用的内存数量就会明显增加
    - 创建和销毁 HTML解析器也会带来性能损失，所以好能够将设置innerHTML或outerHTML的次数控制在合理的范围内

- **scrollIntoView()方法**

  可以在所有HTML元素上调用，通过滚动浏览器窗口或某个容器元素，调用元素就可以出现在视口中

  - 如果传入 true或者不传入参数，那么窗口滚动之后会让调用元素的顶部与视口顶部尽可能平齐
  - 如果传入false，调用元素会尽可能全部出现在视口中，（可能的话调用元素的底部会与视口顶部平齐）不过顶部不一定平齐



## 专有扩展

- **children属性**

  是HTMLCollection的实例，与childNodes属性没有什么区别

- **contains()方法**

  检测传入方法的节点是否是调用方法的节点的**后代节点**。是就返回true；否则返回false

- **插入文本**

  - **innerText 属性**

    操作元素中包含的所有文本内容，包括子文档树中的文本

    - 读取值时，它会按照由浅入深的顺序，将子文档树中的所有文本拼接起来

      ```html
      <div id="content">     
          <p>This is a <strong>paragraph</strong> with a list following it.</p>     <ul>         
          <li>Item 1</li>       
          <li>Item 2</li>       
          <li>Item 3</li>    
          </ul>
      </div>
      
      /*    返回字符串如下
       *    This is a paragraph with a list following it. 
       *    Item 1 
       *    Item 2 
       *    Item 3
       *
       *不同浏览器处理空白符的方式不同，输出的文本可能会也可能不会包含原始HTML代码中的缩进
      */ 
      ```

    -  写入值时，结果会删除元素的所有子节点，插入包含相应文本值的文本节点

      *设置innerText只会生成当前节点的一个子文本节点，而为了确保只生成一个子文本节点， 就必须要**对文本进行HTML编码**

    - 可以通过innerText属性过滤掉HTML标签：`div.innerText = div.innerText; `

    - Firefox不支持 innerText，但支持作用类似的textContent属性

      ```JavaScript
      //以下代码可确保跨浏览器兼容
      function getInnerText(element){   //读取
          return (typeof element.textContent == "string") ?        
              element.textContent : element.innerText; 
      }       //优选使用textContent 
       
      function setInnerText(element, text){   //写入 
          if (typeof element.textContent == "string"){    
              element.textContent = text;   
          } else {      
              element.innerText = text;  
          } 
      }
      ```
      
      *innerText和textContent的区别：
      
      - textContent返回的包含script和style标签内容，innerText不包含
      - innerText返回的值依赖于页面的显示，而textContent依赖于代码的内容
      - innerText会触发回流（从当前节点退回到根节点将整个页面重新排列渲染），而textContent可能回流可能重绘（从当前节点向下至子节点重新绘制），具体看操作，因此**textContent性能更好**
      - 两者设置、获取值时格式化不同，这一点与第二点相关

  - **outerText 属性**

    操作包括元素本身以及元素中包含的所有文本内容

    - 读取值时，它会按照由浅入深的顺序，将元素中的所有文本拼接起来
    - 包含相应文本值的文本节点会**替换整个元素**（包括子节点）

- **滚动**

  - scrollIntoViewIfNeeded(alignCenter)：只在当前元素在视口中不可见的情况下，才滚动浏览器窗口或容器元素，终让它可见。如果当前元素在视口中可见，这个方法什么也不做。 如果将可选的alignCenter参数设置为true，则表示尽量将元素显示在视口中部（垂直方向）
  - scrollByLines(lineCount)：将元素的内容滚动指定的行高，lineCount值可以是正值，也可以是负值
  - scrollByPages(pageCount)：将元素的内容滚动指定的页面高度，具体高度由元素的高度决定

  *scrollIntoView()和 scrollIntoViewIfNeeded()的作用对象是元素的容器；scrollByLines()和 scrollByPages()影响的则是元素自身

  *以上方法都只有Safari和Chrome实现了

