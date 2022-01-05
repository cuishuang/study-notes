---
title: leetcode-112 路径总和
date: 2015-03-01 00:01:52
tags: 算法
categories: Tree
---



[112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

难度:  <font color="green">**简单**</font>


<img src="leetcode-112-路径总和/0.png" width = 90% height = 50% />


<br>

---


<br>


- 递归求解即可

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
func hasPathSum(root *TreeNode, targetSum int) bool {

	if root == nil {
		return false
	}

	if root.Left == nil && root.Right == nil {
		return targetSum == root.Val
	}

	return hasPathSum(root.Left, targetSum-root.Val) || hasPathSum(root.Right, targetSum-root.Val)

}

```
