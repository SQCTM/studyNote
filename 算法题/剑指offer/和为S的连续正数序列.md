# 和为S的连续正数序列

[TOC]

## 题目

小明很喜欢数学，有一天他在做数学作业时，要求计算出9~16的和。他马上就写出了正确答案是100。但是他并不满足于此，他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久，他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你，你能不能也很快的找出所有和为S的连续正数序列？Good Luck！



## 示例

- **输入**

  9

- **返回值**

  [[2,3,4],[4,5]]



## 解法

- **思路**

  - 设定两个指针，左指针i为上边界，右指针j为下边界，从`i=1;j=2`开始，左指针始终小于sum的一半

  - 如果和大于sum，左指针向后移位，如果小于，右指针向后移位

  - 如果两个指针碰在一起，则跳出

- **代码**

  ```javascript
  //滑动窗口方法
  function FindContinuousSequence(sum)
  {
      // write code here
      if(sum < 2)
          return [];
      var result = [];
      var i = 1, j = 2, s = 3;
      while(i <= Math.floor(sum/2)){  //左指针始终小于sum的一半
          if(s < sum){  //和小于sum，右指针向后移位
              j++;
              s+=j;
          }
          else if(s > sum){  //和大于sum，左指针向后移位
              s-=i;
              i++;
          }
          else{  //相等
              var temp = [];
              for(let k = i; k <= j; k++)
                  temp.push(k);
              result.push(temp);
              //继续找下一批
              s-=i;
              i++;
          }
          if(i == j)
              break;
      }
      return result;
  }

