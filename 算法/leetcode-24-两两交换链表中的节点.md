---
title: leetcode-24 两两交换链表中的节点
date: 2015-03-01 00:00:24
tags: 算法
---

<br>


[24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)


难度:  <font color="orange">**中等**</font>


<img src="leetcode-24-两两交换链表中的节点/0.png" width = 100% height = 50% />

<br>

---

<br>


**和链表反转类似，都有三个指针，分别指向前后和当前节点。不同点是两两交换后，移动节点步长为２**



[此题还可使用递归思路来求解](https://leetcode-cn.com/problems/swap-nodes-in-pairs/comments/)

<br>

视频讲解:

[[LeetCode] 24. 两两交换链表中的节点(Swap Nodes in Pairs)](https://www.bilibili.com/video/BV1VC4y1s75E)


```go
/**
 * Definition for singly-linked list.

 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func swapPairs(head *ListNode) *ListNode {
	if head == nil || head.Next == nil {
		return head
	}

	prev, cur := head, head

	head = cur.Next

	for ; cur != nil && cur.Next != nil; cur = cur.Next {
		next := cur.Next

		if prev != nil {
			prev.Next = next
		}

		cur.Next, next.Next, prev = next.Next, cur, cur

	}

	return head

}

```


[三道题套路解决递归问题](https://lyl0724.github.io/2020/01/25/1/)