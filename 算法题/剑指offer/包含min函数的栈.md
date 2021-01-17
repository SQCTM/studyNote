# 包含min函数的栈

[TOC]

## 题目

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））



## 解法

- **思路**

  首先需要一个正常栈用于栈的正常操作，然后需要一个辅助栈专门用于获取最小值

  时间复杂度：O(1)；空间复杂度：O(n)，开辟了一个辅助栈

  ![包含min函数的栈](F:\前端笔记\studyNote\images\包含min函数的栈.png)

- **代码**

  ```javascript
  var stack = [], minStack = [];
  function push(node)
  {
      // write code here
      stack.push(node);
      var l = minStack.length;
      if(l == 0)
          minStack.push(node);
      else{
          if(node <= minStack[l-1])
              minStack.push(node);
          else
              minStack.push(minStack[l-1]);
      }
          
  }
  function pop()
  {
      // write code here
      stack.pop();
      minStack.pop();
  }
  function top()
  {
      // write code here
      return stack[stack.length - 1];
  }
  function min()
  {
      // write code here
      var l = minStack.length;
      return minStack[l-1];
  }
  ```

  