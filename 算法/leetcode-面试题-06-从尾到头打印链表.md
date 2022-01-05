---
title: leetcode-面试题-06 从尾到头打印链表
date: 2015-04-01 00:00:06
tags: 算法
---


<br>




[面试题06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

难度:  <font color="green">**简单**</font>


<img src="leetcode-面试题-06-从尾到头打印链表/0.png" width = 90% height = 50% />


<br>

---

<br>

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reversePrint(head *ListNode) []int {

    res := make([]int,0)

    if head == nil {
        return res
    }
    
    temp := reversePrint(head.Next)
    res = append(res,temp...)

    res = append(res,head.Val)
    return res

}
```
