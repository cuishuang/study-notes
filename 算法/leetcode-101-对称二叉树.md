---
title: leetcode-101 对称二叉树
date: 2015-03-01 00:01:41
tags: 算法
categories: Tree
---

<br>




[101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

难度:  <font color="green">**简单**</font>


<img src="leetcode-704-二分查找/0.png" width = 90% height = 50% />


<br>

---

<br>


```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSymmetric(root *TreeNode) bool {

    if root == nil {
        return true
    }

    return isSameNode(root.Left,root.Right)    
}


func isSameNode(left *TreeNode,right *TreeNode) bool {

    if left == nil && right == nil {
        return true
    }

    if left == nil || right == nil {
        return false
    }


    if left.Val != right.Val {
        return false
    }

    return isSameNode(left.Left,right.Right) && isSameNode(left.Right,right.Left)

}

```