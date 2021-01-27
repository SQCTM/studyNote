# 和为S的两个数字

[TOC]

## 题目

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的



## 示例

- **输入**

  [1,2,4,7,11,15],15

- **返回值**

  [4,11]

- **说明**

  对应每个测试案例，输出两个数，小的先输出



## 解法

- **思路**

  设置两个指针，一个从头开始遍历，一个从尾开始遍历，两个数字**相距越远乘积越小**

- **代码**

  时间复杂度：O(n)；空间复杂度：O(1)

  ```javascript
  function FindNumbersWithSum(array, sum)
  {
      // write code here
      if(array.length == 0)
          return [];
      var i = 0, j = array.length-1;
      while(i < j){
          var s = array[i]+array[j]
          if(s == sum)
              return [array[i], array[j]];
          else if(s < sum)
              i++;
          else
              j--
      }
      return [];
  }
  ```

  