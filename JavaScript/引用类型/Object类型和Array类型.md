# Object类型和Array类型

[TOC]

## Object类型

```javascript
var person = {     
    "name": "Nicholas",     
    "age": 29,    
    5: true      //这个数值属性名会自动转换为字符串
}; 

//获取对象属性有两种方法：
alert(person["name"]);       //"Nicholas" 
alert(person.name);         //"Nicholas" 
//通常情况下都使用第二种方法。如果是必须使用变量来访问或属性名中包含特殊字符，则用第一种方法。
```



## Array类型

- 数组是数据的有序列表

- 数组的每一项可以保存任何类型的数据

- 数组的大小可以动态调整。通过设置数组的length属性，可以从数组末尾移除项或向数组中添加项

  ```javascript
  var colors = ["red", "blue", "green"];   
  alert(colors[2]);      //green 
  colors.length = 2; 
  alert(colors[2]);      //undefined 
  ```

  

### 检测数组

- **instanceof**

  缺点：适用于只有一个全局执行环境的条件下。如果存在两个以上不同全局执行环境，从而可能存在两个以上不同版本的Array构造函数。不同环境之间传递数组就可能出现问题。

- **数组构造函数**

  ```javascript
  var arr = new Array();
  console.log(arr.constructor == Array);   //true
  ```

  *缺点和instanceof相同

- **Array.isArray()**

  缺点：对浏览器兼容性有要求。要求支持ECMAScript5。

- **Object.prototype.toString.call()**

  ```javascript
  var arr = new Array();
  alert(Object.prototype.toString.call(arr) == "[Object Array]");  //true
  ```

  

### 转换方法

- **toLocaleString**

- **toString**

- **valueOf**

- ```javascript
  var colors = ["red", "blue", "green"];    // 创建一个包含 3 个字符串的数组 
  alert(colors.toString());     // red,blue,green 
  alert(colors.valueOf());      // red,blue,green 
  alert(colors);                // red,blue,green 
  //最后一个直接将数组传递给了alert，由于alert要接收字符串参数，所以它会自动在后台调用toString()方法
  ```



### 栈方法

- 先进后出

- **push()**

  接收任意数量参数，把它们逐个添加到数组末尾，并**返回修改后数组的长度**

- **pop()**

  从数组末尾移除最后一项，减少数组的length值，然后**返回被移除的项**



### 队列方法

- 先进先出

- **shift()**

  移除数组中的第一个项并返回该项，同时将数组长度减1

- **unshift()**

  在数组前端添加任意个项并返回新数组的长度

- 结合使用shift()和push()方法，可以像队列一样使用数组；结合使用unshift()和pop()，可以从相反方向来模拟队列



### 重排序方法

- **reverse()**

  反转数组项的顺序。

  ```javascript
  var values = [1, 2, 3, 4, 5];
  values.reverse(); 
  console.log(values);        //5,4,3,2,1 
  ```

- **sort()**

  - **默认情况下**按**升序**排列数组项

  - sort()会调用每个数组项的toString()转型方法，然后比较得到的**字符串**

    ```JavaScript
    var values = [0, 1, 5, 10, 15]; 
    values.sort(); 
    console.log(values);   //0,1,10,15,5 
    ```

  - sort()方法可以接收一个**比较函数作为参数**

    ```javascript
    //升序
    function compare(value1, value2) {     
        if (value1 < value2) {         
            return -1;     
        } 
        else if (value1 > value2) {         
            return 1;     
        } 
        else {         
            return 0;     
        } 
    } 
    
    //降序
    function compare(value1, value2) {     
        if (value1 < value2) {         
            return 1;     
        } 
        else if (value1 > value2) {         
            return -1;     
        } 
        else {         
            return 0;     
        } 
    } 
    
    //即冒泡排序
    ```

  - 更加简单的比较函数

    ```javascript
    //升序
    function compare(value1, value2){     
        return value1 - value2; 
    } 
    
    //降序
    function compare(value1, value2){     
        return value2 - value1; 
    } 
    ```

  - 性能（设数组长度为n

    - 最好情况（不用重新排序：比较**n次**
    - 最坏情况（排序完全相反：比较**n<sup>2</sup>次**

- 两种方法的返回值都是经过排序之后的数组



### 操作方法

- **join()**

  `arrayObject.join(separator)`

  把数组所有元素转换成字符串并用指定分隔符连接，若不传参则默认是逗号

  ```javascript
  var colors = ["red", "green", "blue"]; 
  console.log(colors.join(","));       //red,green,blue 
  console.log(colors.join("||"));      //red||green||blue 
  ```

- **concat()**

  - 会先创建当前数组的一个副本，然后将接收到的参数添加到副本的末尾，最后返回新构建的数组

  - 若没有传参，则只是复制当前数组并返回副本

  - 若传入的参数是数组，则concat()会将传入数组的每一项都添加到结果数组中

    *push()则是将整个数组作为最后一项

- **slice()**

  - 即可以接收一个参数也可以接收两个参数

  - 只有一个参数时，则返回从参数指定位置开始到当前数组末尾的所有项；有两个参数时，则返回起始和结束位置之间的项，但不包括结束位置的项

    ```javascript
    var colors = ['red','green','blue','yellow','purple'];
    var colors1 = colors.slice(1);
    var colors2 = colors.slice(1,4);
    
    console.log(colors1);   // green,blue,yellow,purple
    console.log(colors2);   // green,blue,yellow
    ```

  - 不传参则返回数组副本

  - 若参数是负数，则**加上数组长度**来取值，相当于倒着取

  - 若参数超出范围或者两个参数弄反了则**返回空数组**

- **splice()**
  
  - **删除**
  
    可以删除任意数量的项。参数为**要删除的第一项的位置**和**要删除的项数**。
  
    例：`splice(0, 2)   //删除数组中的前两项`
  
  - **插入**
  
    向指定位置插入任意数量的项。参数为**起始位置**、**0**（要删除的项数）、**要插入的项**。
  
    例：`splice(2, 0, 'red', 'green')   //从位置2开始插入两项字符串` 
  
  - **替换**
  
    可以向指定位置插入任意数量的项且同时删除任意数量的项。参数为**起始位置**、**要删除的项数**、**要插入的项**。插入的项数不必与删除的项数相等。
  
    例：`splice(2, 1, 'red', 'green')  //删除数组位置2的项，再从位置2开始插入两项字符串`



### 位置方法

- 一共有**两种**，**indexOf()**从数组的开头开始向后查找，**lastIndexOf()**则从数组末尾开始向前查找
- 两个方法都接收**两个参数**：**要查找的项**和**查找起点位置索引**（第二个可选
- 若找到了返回要**查找的项在数组中的位置**，没找到则返回**-1**
- 在查找比较时会使用**全等操作符**，要求查找的项必须严格相等



### 迭代方法

- 一共有**五种**方法，每种方法都接收**两个参数**：在数组**每一项上运行的函数**和**运行该函数的作用域对象**（第二个可选，影响this值

- 传入方法的函数接收三个参数：**数组项的值**、**该项在数组中的位置**和**数组对象本身**

- 五种方法作用相同只是**返回值不同**：

  - **every()**：如果传入函数对**每一项**都返回true，则返回true
  - **filter()**：返回对传入函数**会返回true的项组成的数组**
  - **forEach()**：没有返回值
  - **map()**：返回每次传入函数调用的**结果组成的数组**
  - **some()**：如果传入函数对**任意一项**返回true，则返回true

- 五种方法都不会修改数组中包含的值

- 例：判断数组中有没有大于2的项

  ```javascript
  var num = [1,2,3,4,5];
  var result = num.some(function(item, index, array) {
      return (item > 2);
  });
  console.log(result);   // true
  ```

  

### 归并方法

- 一共有**两种**方法，两种方法都会迭代数组所有项。**reduce()**从数组第一项开始逐个遍历到最后；**reduceRight()**则从数组的最后一项开始向前遍历到第一项

- 每种方法都接收**两个参数**：在数组**每一项上调用的函数**和**作为归并基础的初始值**（第二个可选

- 传入的函数接收**四个参数**：**前一个值**、**当前值**、**项的索引**和**数组对象**。这个**函数返回的任何值都会作为第一个参数自动传给下一项**。

- 第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数是数组的第二项。

- 例：利用reduce求数组所有项之和

  ```javascript
  var num = [1,2,3,4,5];
  var sum = num.reduce(function(prev, cur, index, array) {
      return prev + cur;
  });
  console.log(sum); // 15
  //第一次执行回调函数，prev是1，cur是2，结果返回3作为第一个参数传给下一项；因此第二次执行回调函数，prev是3，cur是3。
  ```

