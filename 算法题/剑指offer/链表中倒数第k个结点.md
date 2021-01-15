# 链表中倒数第k个结点

[TOC]

## 题目

输入一个链表，输出该链表中倒数第k个结点



## 实例

- **输入**

  {1,2,3,4,5},1

- **返回值**

  {5}



## 解法

- **思路**

  设置两个指针，两个指针中间相距k-1个节点。第一个指针先移动，移到了第k个节点时，第二个指针还在第一个节点，这时两个一起移动。当第一个移到了最后一个节点时，第一个指针则是在倒数第k个节点

- **代码**

  ```javascript
  /*function ListNode(x){
      this.val = x;
      this.next = null;
  }*/
  function FindKthToTail(head, k)
  {
      // write code here
      if(head == null || k <= 0)
          return null;
      var p1 = head, p2= head;
      for(var i = 0; i < k; i++){
          if(p2)
              p2 = p2.next;
          else
              return null;
      }
      while(p2){
          p1 = p1.next;
          p2 = p2.next;
      }
      return p1;
  }
  ```

  