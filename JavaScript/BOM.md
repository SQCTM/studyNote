# BOM

浏览器对象模型

[TOC]

## window对象

- **框架**

  - 每个框架都有自己的window对象以及所有原生构造函数及其他函数的副本。每个框架都保存在 frames集合（伪数组）中，可以通过位置或通过名称来访问
  - top对象始终指向外围的框架，也就是整个浏览器窗口；parent对象表示包含当前框架的框架；self对象则回指window（没有框架的情况下parent等于top

- **窗口**

  - **窗口位置**

    - **获取**

      ```JavaScript
      var leftPos = (typeof window.screenLeft == "number") ?                   
          window.screenLeft : window.screenX; 
      var topPos = (typeof window.screenTop == "number") ?                   
          window.screenTop : window.screenY;
      
      //跨浏览器取得窗口左边和上边的位置
      ```

    - **移动**

      - `moveTo()`接收新位置的x和y坐标值
      - `moveBy()`接收在水平和垂直方向上移动的像素数

  - **窗口大小**

    - **innerWidth、innerHeight**表示该容器中页面视图区的大小（减去边框宽度）
    - **outerWidth 和 outerHeight**返回浏览器窗口本身的尺寸（无论是从外层的window对象还是从某个框架访问），Opera中这两个属性的值表示页面视图容器的大小
    - 在Chrome中，四个属性返回相同的值，即视口大小而非浏览器窗口大小

    

- **间歇调用和超时调用**

  - **超时调用setTimeout()**

    ```JavaScript
    setTimeout(function() {      
        alert("Hello world!"); 
    }, 1000); 
    ```

    - 第一个参数可以是字符串也可以是函数，但不建议传字符串，传递字符串可能导致性能损失
    - 第二个参数是表示等待多长时间的**毫秒数**，但经过该时间后指定的代码不一定会执行。JS是一个单线程序的解释器，一定时间内只能执行一段代码。有一个JS任务队列，任务会按照将它们添加到队列的顺序执行。setTimeout()的第二个参数告诉JS再过多长时间把当前任务添加到队列中。如果队列是空的，那么添加的代码会立即执行；如果队列不是空的，那么它就要等前面的代码执行完了以后再执行
    - 结果返回一个数值ID，表示超时调用。**超时调用ID**是计划执行代码的唯一标识符
    - **clearTimeout()方法**取消尚未执行的超时调用计划，将超时调用ID作为参数传入即可

  - **间歇调用setInterval()**

    ```JavaScript
    setInterval (function() {      
        alert("Hello world!");  
    }, 10000); 
    ```

    - 结果返回一个**间歇调用ID**

    - **clearInterval()方法**取消尚未执行的间歇调用，将间歇调用ID作为参数传入即可

    - 若执行代码时间超过间隔时间，执行完后会立马触发下一次执行（累积效应、连续触发），因此建议使用超时调用来实现间歇调用

      ```JavaScript
      var num = 0; 
      var max = 10; 
       
      function incrementNumber() {     
          num++; 
       
          //如果执行次数未达到 max 设定的值，则设置另一次超时调用     
          if (num < max) {        
              setTimeout(incrementNumber, 500);    
          } 
          else {       
              alert("Done");   
          } 
      } 
       
      setTimeout(incrementNumber, 500);
      ```

  - **tips**

    - 两个函数原理都是把函数中的代码提出放入一个事件队列中，待整个代码执行完后才扫描事件队列
    - 若设置的毫秒数小于10，系统会自动调成10，且10毫秒几乎没有间隔感

- **系统对话框**

  - **alert()**

    警告框，只有确认按钮

  - **confirm()**

    确认框，有确认按钮和取消按钮

  - **prompt()**

    提示框，提示用户输入文本，有文本框、确认按钮和取消按钮

  - 系统对话框不包含HTML，样式也不由CSS决定

  - 对话框是**阻断式**的，显示这些对话框时代码会停止执行，而关掉对话框后代码又会恢复执行



## location对象

- 提供了与当前窗口中加载的文档有关的信息，还提供了一些导航功能

- 既是window对象的属性，也是 document 对象的属性

- 将URL解析为独立的片段，让开发人员可以通过不同的属性访问这些片段

  | 属性名   | 例子                 | 说明                                                         |
  | -------- | -------------------- | ------------------------------------------------------------ |
  | hash     | "#contents"          | 返回URL中的hash（#号后跟零或多个字符），如果URL 中不包含散列，则返回空字符串 |
  | host     | "www.wrox.com:80"    | 返回服务器名称和端口号（如果有）                             |
  | hostname | "www.wrox.com"       | 返回不带端口号的服务器名称                                   |
  | href     | "http:/www.wrox.com" | 返回当前加载页面的完整URL。而location对象的 toString()方法也返回这个值 |
  | pathname | "/WileyCDA/"         | 返回URL中的目录和（或）文件名                                |
  | port     | "8080"               | 返回URL中指定的端口号。如果URL中不包含端口号，则 这个属性返回空字符串 |
  | protocol | "http:"              | 返回页面使用的协议。通常是http:或https:                      |
  | search   | "?q=javascript"      | 返回URL的查询字符串。这个字符串以问号开头                    |

  - 除了hash之外，给其它属性赋值时，浏览器刷新留下一条历史记录
  - location.search返回的是字符串

- **位置操作方法**

  - **assign()**

    立即打开新URL并在浏览器的历史记录中生成一条记录

  - **replace()**

    接受一个参数，即要导航到的URL；结果会导致浏览器位置改变，但不会在历史记录中生成新记录

  - **reload()**

    - 重新加载当前显示的页面
    - 若调用reload()时不传递任何参数，页面就会以有效的方式重新加载，即页面就会从浏览器缓存中重新加载；若传递参数true，则强制从服务器重新加载
    - 位于reload()调用之后的代码可能会也可能不会执行，这要取决于网络延迟或系统资源等因素。因此最好将 reload()放在代码的后一行。 



## navigator对象

识别客户端浏览器的事实标准， navigator 对象的属性通常用于检测显示网页的浏览器类型

- **插件**

  - 对于非IE浏览器，可以使用**plugins数组**，属性有：

    - name：插件的名字
    - description：插件的描述
    - filename：插件的文件名
    - length：插件所处理的 MIME类型数量

    ```JavaScript
    //检测插件（在 IE中无效） 
    function hasPlugin(name){    
        name = name.toLowerCase();    
        for (var i=0; i < navigator.plugins.length; i++){       
            if (navigator. plugins [i].name.toLowerCase().indexOf(name) > -1){             return true;        
            }     
        } 
     
        return false; 
    } 
     
    //检测 Flash 
    alert(hasPlugin("Flash")); 
     
    //检测 QuickTime 
    alert(hasPlugin("QuickTime"));
    ```

  - IE浏览器检测插件的唯一方式就是使用专有的**ActiveXObject类型**

    ```JavaScript
    //检测 IE中的插件 
    function hasIEPlugin(name){     
        try {         
            new ActiveXObject(name);        
            return true;    
        } catch (ex){       
            return false; 
        } 
    } 
     
    //检测 Flash 
    alert(hasIEPlugin("ShockwaveFlash.ShockwaveFlash")); 
     
    //检测 QuickTime 
    alert(hasIEPlugin("QuickTime.QuickTime")); 
    ```



## screen对象

表明客户端的能力，其中包括浏览器窗口外部的显示器的信息，如像素宽度和高度等。用处不大



## history对象

- 保存着用户上网的历史记录，从窗口被打开的那一刻算起

- 每个浏览器窗口、每个标签页乃至每个框架，都有自己的history对象与特定的window对象关联

- 开发人员无法得知用户浏览过的URL。但借由用户访问过的页面列表，可以在不知道实际URL的情况下实现后退和前进：

  - **go()**

    - 可以在用户的历史记录中任意跳转，可以向后也可以向前
    - 接受一个参数，表示向后或向前跳转的页面数的一个整数值；负数表示向后，正数表示向前

  -  **back()和 forward()**

    ```JavaScript
    //后退一页 
    history.back(); 
     
    //前进一页 
    history.forward();
    ```

- **length 属性**

  保存着历史记录的数量。这个数量包括所有历史记录，即所有向后和向前的记录。对于加载到窗口、标签页或框架中的第一个页面而言，history.length 等于0