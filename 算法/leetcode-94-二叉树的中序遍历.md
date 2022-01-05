---
title: leetcode-94 二叉树的中序遍历
date: 2015-03-01 00:01:34
tags: 算法
categories: Tree
---

<br>


---

姊妹篇:

[leetcode-144 二叉树的前序遍历](http://www.dashen.tech/2015/03/01/leetcode-144-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E9%81%8D%E5%8E%86/)

[leetcode-145 二叉树的后序遍历](http://www.dashen.tech/2015/03/01/leetcode-145-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86/)


---



<br>

[94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

难度:  <font color="orange">**中等**</font>


![pic](leetcode-94-二叉树的中序遍历/1.png)


<br>


---


<br>

**二叉树的前序、中序、后序三种遍历方式,所谓的"前中后",指的是对根节点的遍历顺序.**

[可参考](https://blog.csdn.net/qq_33243189/article/details/80222629)
[或点击查看此篇](https://blog.csdn.net/dingqianyi2007/article/details/79585153)


<font color="purple">即对于中序排序,对每个节点始终按照"左中右"的规则进行遍历;</font>


> 根据访问结点操作发生位置命名：<br>
① NLR：前序遍历(PreorderTraversal亦称（先序遍历））<br>
——访问结点的操作发生在遍历其左右子树之前。<br>
② LNR：中序遍历(InorderTraversal)<br>
——访问结点的操作发生在遍历其左右子树之中（间）。<br>
③ LRN：后序遍历(PostorderTraversal)<br>
——访问结点的操作发生在遍历其左右子树之后。<br>
注意：
由于被访问的结点必是某子树的根，<br>所以N(Node）、L(Left subtree）和R(Right subtree）又可解释为根、根的左子树和根的右子树。NLR、LNR和LRN分别又称为先根遍历、中根遍历和后根遍历。

<br>

对于任意一个编号为n的节点，如果它有子节点，它的左子节点编号为2n,右节点的编号为2n+1。（这条性质很重要，决定了二叉树可以用数组来表示）<br>

[二叉树的顺序存储,力荐](https://www.cnblogs.com/yw-ah/p/5872516.html)






---


<br>

### 迭代解法:

<br>

```go
package main

import "fmt"

//Definition for a binary tree node.
type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func main() {

	var a = TreeNode{}
	rs := inorderTraversal(&a)

	fmt.Println(rs)

}

func inorderTraversal(root *TreeNode) []int {
	//声明返回
	var result []int

	//判断根
	if root == nil {
		return result
	}

	//寻找左--放入栈
	var stack []*TreeNode

	//放入根
	stack = append(stack, root)

	//准备工作寻找左--并放入栈
	p := root.Left
	for p != nil {
		//压入栈
		stack = append(stack, p)

		//继续左
		p = p.Left
	}

	//开始填充结果
	for len(stack) != 0 {
		//栈顶出栈
		topNode := stack[len(stack)-1]
		stack = stack[:len(stack)-1]

		//写入当前
		result = append(result, topNode.Val)

		//判断当前的右
		if topNode.Right == nil {
			continue
		}

		//当前右入栈
		stack = append(stack, topNode.Right)

		//获取当前的右的左
		p := topNode.Right.Left
		for p != nil {
			//压入栈
			stack = append(stack, p)

			//继续左
			p = p.Left
		}
	}

	//返回结果
	return result

}

```

![pic](leetcode-94-二叉树的中序遍历/2.png)



---

<br>

### 递归解法:

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
func inorderTraversal(root *TreeNode) []int {


    res := make([]int,0)

    if root == nil {
        return res
    }

    temp := inorderTraversal(root.Left)
    res = append(res,temp...)

    res = append(res,root.Val)

    temp = inorderTraversal(root.Right)
    res = append(res,temp...)

    return res

}
```