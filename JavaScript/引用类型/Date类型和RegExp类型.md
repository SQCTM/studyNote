# Date类型和RegExp类型

[TOC]

## Date类型

- Date 类型使用自 UTC（国际协调时间）1970年 1月 1日零时开始经过的毫秒数来保存日期。
- 传参
  - 不传参数时，自动获取当前日期和时间
  - 创建指定日期的对象应传入表示该日期的毫秒数
  - 可以传入字符串后台自动调用Date.parse()；传入多个表示日期的Number类型的参数后台自动调用Date.UTC()
  - 传入boolean或null，得到1970-1-1早上八点
  - 传入undefined或无效字符串得到Invalid Date（无效时间）



### 简单方法

- **Date.parse()**

  - 接收一个表示日期的字符串参数，然后根据这个字符串返回相应日期的毫秒数。

  - 接收格式
    - “月/日/年”，如 6/13/2004
    - “英文月名 日,年”，如 January 12,2004
    - “英文星期几 英文月名 日 年 时:分:秒 时区”，如 Tue May 25 2004 00:00:00 GMT-0700
    - ISO 8601 扩展格式 YYYY-MM-DDTHH:mm:ss.sssZ（例如 2004-05-25T00:00:00）。只有兼容ECMAScript 5的实现支持这种格式
    - 还有其他的也行，识别功能很强大。但是最好日期写前面，时间写后面。反过来容易出错。 
  - Date.parse()的转换速度比较慢
- **Date.UTC()**

  - 返回表示日期的毫秒数
  - 参数
    - 分别是年份、**基于0**的月份、月中的哪一天 （1 到 31）、小时数（0 到 23）、分钟、秒以及毫秒数
    - 只有前两个参数（年和月）是必需的
    - 若没有提供月中的天数，则假设天数为1；如果省略其他参数，则都假设为 0

- **Data.now()**

  - 适用于ECMAScript 5

  - 返回表示调用这个方法时的日期和时间的毫秒数

  - 简化了使用 Data 对象分析代码的工作

    ```javascript
    //取得开始时间 
    var start = Date.now(); 
     
    //调用函数 
    doSomething(); 
     
    //取得停止时间
    var stop = Date.now(),     
        result = stop – start;
    
    //用一元操作符+也是一样的效果
    ```

    

### 继承的方法

- **toLocaleString()**

  会按照与浏览器 设置的地区相适应的格式返回日期和时间。

- **toString()**

  通常返回带有时区信息的日期和 时间，其中时间一般以军用时间（即小时的范围是 0 到 23）表示。

- **valueOf()**

  不返回字符串，而是返回日期的毫秒表示。方便使用比较操作符（小于或大于）来比较日期值。



*返回日期毫秒表示的还有getTimer()、Number()和一元操作符+。转换速度valueOf()和getTimer()稍快，Number()和+稍慢，但是也差距很小



### 格式化的方法

-  toDateString()——以特定于实现的格式显示星期几、月、日和年
-  toTimeString()——以特定于实现的格式显示时、分、秒和时区
- toLocaleDateString()——以特定于地区的格式显示星期几、月、日和年
- toLocaleTimeString()——以特定于实现的格式显示时、分、秒  
- toUTCString()——以特定于实现的格式完整的 UTC日期
- toGMTString()——与toUTCString()，但toUTCString()更精准



## RegExp类型

即正则表达式。语法：`var expression = / pattern / flags ; `

- 模式（pattern）部分可以是任何简单或复杂的正则表达式。

- flags则是每个正则表达式都可带有一或多个标志，用以标明正则表达式的行为。 正则表达式的匹配模式支持下列3个标志：

  - **g**：表示**全局**模式，即模式将被应用于所有字符串，而非在发现第一个匹配项时立即停止
  - **i**：表示**不区分大小写**模式
  - **m**：表示**多行**模式，即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项

- 一个正则表达式就是一个模式与上述3个标志的组合体

  

### 定义正则表达式

- **字面量形式**

  ```javascript
  /* * 匹配第一个"bat"或"cat"，不区分大小写 */ 
  var pattern1 = /[bc]at/i; 
  ```

- **RegExp 构造函数**

  ```javascript
  /* * 与字面量形式定义的pattern1完全等价，只不过是使用构造函数创建的 */ 
  var pattern2 = new RegExp("[bc]at", "i");
  ```

- **两者区别**

  字面量始终会**共享同一个RegExp实例**，而使用构造函数创建的每一个新RegExp实例都是一个新实例。



### 元字符的转义

- **一般转义**

  ```javascript
  /* * 匹配第一个"bat"或"cat"，不区分大小写 */ 
  var pattern1 = /[bc]at/i; 
  
  /* * 匹配第一个" [bc]at"，不区分大小写 */ 
  var pattern2 = /\[bc\]at/i;    //对两个方括号进行转义
  
  /* * 匹配所有以"at"结尾的 3 个字符的组合，不区分大小写 */ 
  var pattern3 = /.at/gi; 
  
  /* * 匹配所有".at"，不区分大小写 */ 
  var pattern4 = /\.at/gi;    //对.进行转义
  ```

- **双重转义**

   使用RegExp构造函数定义时，不能把正则表达式字面量传递给RegExp构造函数，所有元字符都必须双重转义

  eg: `var pattern = new RegExp("\\[bc\\]at", "i"); `



### RegExp实例

#### 属性

- **global**：布尔值，表示是否设置了g标志。 
- **ignoreCase**：布尔值，表示是否设置了i标志。 
- **lastIndex**：整数，表示**开始搜索下一个匹配项的字符位置**，从0算起。 
- **multiline**：布尔值，表示是否设置了m标志。 
- **source**：正则表达式的字符串表示，按照**字面量形式**而非传入构造函数中的字符串模式返回

```javascript
var pattern2 = new RegExp("\\[bc\\]at", "i"); 
 
alert(pattern2.global);         //false 
alert(pattern2.ignoreCase);     //true 
alert(pattern2.multiline);      //false 
alert(pattern2.lastIndex);      //0 
alert(pattern2.source);         //"\[bc\]at" 
```



#### 方法

-  **exec()**

  - 该方法是专门为**捕获组**而设计的
  - 参数：接受一个，即要应用模式的字符串
  - 返回包含第一个匹配项信息的**数组**；在没有匹配项的情况下返回null
  - 返回的数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串。如果模式中没有捕获组，则该数组只包含一项
  - 返回的数组是Array的实例，但包含两个额外的属性：**index** 和 **input**
    - index 表示匹配项在字符串中的位置
    - input 表示应用正则表达式的字符串

  ```javascript
  var text = "mom and dad and baby"; 
  var pattern = /mom( and dad( and baby)?)?/gi; 
   
  var matches = pattern.exec(text); 
  alert(matches.index);     // 0 
  alert(matches.input);     // "mom and dad and baby" 
  alert(matches[0]);        // "mom and dad and baby" 
  alert(matches[1]);        // " and dad and baby" 
  alert(matches[2]);        // " and baby" 
  ```

  - 是否设置全局标志

    - 设置全局标志，在同一个字符串上多次调用exec()，每次也只会返回一个匹配项，但每次调用则都会在字符串中继续查找下一个新的匹配项，lastIndex值在每次调用后都会增加
    - 不设置全局标志，在同一个字符串上多次调用 exec()将始终返回第一个匹配项的信息，lastIndex值始终保持不变

    

- **test()**

  接受一个字符串参数，在模式与该参数匹配的情况下返回 true，否则返回 false。

  ```javascript
  var text = "000-00-0000"; 
  var pattern = /\d{3}-\d{2}-\d{4}/; 
   
  if (pattern.test(text)){     
      alert("The pattern was matched."); 
  } 
  ```



*RegExp实例继承的toLocaleString()和toString()方法都会返回正则表达式的字面量，与创建正则表达式的方式无关。

```javascript
var pattern = new RegExp("\\[bc\\]at", "gi"); 
alert(pattern.toString());             // /\[bc\]at/gi 
alert(pattern.toLocaleString());       // /\[bc\]at/gi 
```



### RegExp构造函数属性 

|   长属性名   | 短属性名 |                            说  明                            |
| :----------: | :------: | :----------------------------------------------------------: |
|    input     |    $_    |           近一次要匹配的字符串。Opera未实现此属性            |
|  lastMatch   |    $&    |              近一次的匹配项。Opera未实现此属性               |
|  lastParen   |    $+    |            近一次匹配的捕获组。Opera未实现此属性             |
| leftContext  |    $`    |               input字符串中lastMatch之前的文本               |
|  multiline   |    $*    | 布尔值，表示是否所有表达式都用多行模式。IE和Opera未实现此属性 |
| rightContext |    $'    |               Input字符串中lastMatch之后的文本               |

```javascript
var text = "this has been a short summer"; 
var pattern = /(.)hort/g; 
 
/*  * 注意：Opera 不支持 input、lastMatch、lastParen 和 multiline 属性  * Internet Explorer 不支持 multiline 属性  */   

if (pattern.test(text)){     
    alert(RegExp.input);            // this has been a short summer     
    alert(RegExp.leftContext);      // this has been a                 
    alert(RegExp.rightContext);     // summer     
    alert(RegExp.lastMatch);        // short     
    alert(RegExp.lastParen);        // s     
    alert(RegExp.multiline);        // false 
}

/* 例子使用的长属性名都可以用相应的短属性名来代替。但由于这些短属性名大都不是有效的ECMAScript标识符，因此必须通过方括号语法来访问它们 */   
if (pattern.test(text)){     
    alert(RegExp.$_);              // this has been a short summer     
    alert(RegExp["$`"]);           // this has been a                 
    alert(RegExp["$'"]);           // summer     
    alert(RegExp["$&"]);           // short     
    alert(RegExp["$+"]);           // s     
    alert(RegExp["$*"]);           // false 
} 

```



*除以上外，还有 RegExp.$1、RegExp.$2 … RegExp.$9，分别用于存储第一、第二 …… 第九个匹配的捕获组，这九种属性。

```javascript
var text = "this has been a short summer"; 
var pattern = /(..)or(.)/g;        
if (pattern.test(text)){     
    alert(RegExp.$1);       //sh     
    alert(RegExp.$2);       //t 
}
```



### 模式的局限性 

- 匹配字符串开始和结尾的\A 和\Z 锚[^1]
- 向后查找(lookbehind) [^2]
- 并集和交集类 
- 原子组(atomic grouping) 
- Unicode支持（单个字符除外，如\uFFFF） 
- 命名的捕获组[^3]
- s（single，单行）和 x（free-spacing，无间隔）匹配模式 
- 条件匹配 
- 正则表达式注释 



[^1]:但支持以插入符号（^）和美元符号（$）来匹配字符串的开始和结尾。 
[^2]:但完全支持向前查找（lookahead）。 
[^3]: 但支持编号的捕获组。 

