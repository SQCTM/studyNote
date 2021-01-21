# 最小的k个数

[TOC]

## 题目

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4



## 示例

- **输入**

  [4,5,1,6,2,7,3,8],4

- **返回值**

  [1,2,3,4]



## 相关知识

### 快速排序

快速排序将一个数组分成两个数组，再对两个数组独立排序，是个**递归**算法。在数组中找一个基准数，将比基准数小的放在基准数左边、比基准数大的放在基准数右边

1. 初始化：设置两个变量low=0, high = array.length - 1；默认数组第一个数array[low]为基准数key
2. 从数组后面开始向前搜索（high--），找到第一个小于key的数array[high]，交换array[high]与array[low]数据
3. 再从数组前面开始向后搜索（low++)，找到第一个大于key的数array[low]，交换array[low]与array[high]数据
4. 循环步骤2-3，直到 low=high，该位置就是基准位置
5. 返回基准位置
6. 步骤1-5第一趟找到的基准位置，作为下一趟的分界点
7. 递归调用分界点前和分界点后的子数组排序重复步骤1-7
8. 最终得到排序好的数组



## 解法

- **排序**

  - **思想**

    直接从小到大排序，然后取前k个数据

  - **代码**

    时间复杂度：O(nlogn)；空间复杂度：O(1)

    ```javascript
    function GetLeastNumbers_Solution(input, k)
    {
        // write code here
        var min = new Array();
        if(k > input.length)
            return min;
        input.sort();
        for(let i = 0; i < k; i++){
            min.push(input[i]);
        }
        return min;
    }
    ```

- **partition快排法**

  - **思想**

    1. 对数组进行快排返回基准数位置key

    2. 利用二分法对返回的基准数位置进行判断：

       - 如果key == k-1，找到答案
       - 如果key < k-1，说明答案在
       - 如果key > k-1，说明答案在

    3. 然后再判断p，利用二分法：

       - 如果[l,p), p，也就是p+1个元素（因为下标从0开始），如果p+1 == k, 找到答案

         对。1, r)区间内

         如果p+1 > k , 说明答案在[l, p)内

  - **代码**

    ```javascript
    function partition(a, low, high)
    {
        var key = a[low];  //数组第一个数为基准数
        while(low < high){
            while(a[high] >= key && low < high)  //从后开始搜索
                high--;
            [a[low], a[high]] = [a[high], a[low]]; //交换
            while(a[low] <= key && low < high)  //从前开始搜索
                low++;
            [a[low], a[high]] = [a[high], a[low]]; //交换
        }
        return low; //low=high=基准数位置
    }
    
    function GetLeastNumbers_Solution(input, k)
    {
        // write code here
        var min = new Array();
        if(k > input.length || k == 0)
            return min;
        var low = 0, high = input.length-1;
        while(low <= high){
            var key = partition(input, low, high);//基准数位置
            if(key == k-1){
                min = input.slice(0, k);
                min.sort();
                return min;
            }
            else if(key > k-1){
                high = key; 
            } 
            else{
                low = key+1;
            }
        }
        return min;
    }
    ```

    

  