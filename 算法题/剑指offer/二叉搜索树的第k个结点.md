# 二叉搜索树的第k个结点

[TOC]

## 题目

给定一棵二叉搜索树，请找出其中的第k小的结点



## 示例

- **输入**

  {5,3,7,2,4,6,8},3

- **返回值**

  {4}

- **说明**

  按结点数值大小顺序第三小结点的值为4 



## 解法

- **非递归**

  ```javascript
  /* function TreeNode(x) {
      this.val = x;
      this.left = null;
      this.right = null;
  } */
  function KthNode(pRoot, k)
  {
      // write code here
      if(!pRoot || k <= 0)
          return null;
      var stack = [];
      var cur = pRoot;
      while(stack.length || cur){
          if(cur){
              stack.push(cur);
              cur = cur.left;
          }
          else{
              cur = stack.pop();
              k--;
              if(k == 0)
                  return cur;
              cur = cur.right;
          }
      }
      return null;
  }
  ```

- **递归**

  ```javascript
  /* function TreeNode(x) {
      this.val = x;
      this.left = null;
      this.right = null;
  } */
  var n;
  function mid_order(pRoot)
  {
      if(!pRoot)
          return null;
      var target = mid_order(pRoot.left);
      if(target)
          return target;
      n--;
      if(n == 0)
          return pRoot;
      target = mid_order(pRoot.right);
      return target;
  }
  function KthNode(pRoot, k)
  {
      // write code here
      if(!pRoot || k < 1)
          return null;
      n = k;
      return mid_order(pRoot);
  }
  ```

  