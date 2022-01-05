---
title: leetcode-42 接雨水
date: 2015-03-01 00:00:42
tags: 算法
---

<br>

![如图](leetcode-42接雨水/1.png)
 <br>
 
 最朴素的一种解法,使用动态规划~
 >维护一个一维的dp数组，这个DP算法需要遍历两遍数组，第一遍遍历dp[i]中存入i位置左边的最大值，然后开始第二遍遍历数组，第二次遍历时找右边最大值，然后和左边最大值比较取其中的较小值，然后跟当前值A[i]相比，如果大于当前值，则将差值存入结果
 
 直接贴代码:
 
 
 ```go
func trap(height []int) int {

	rs := 0
	mx := 0
	n := len(height)
	dp := make([]int,n)

	for i := 0; i < n; i++ {
		dp[i] = mx
		mx = max(mx,height[i])
	}

	mx = 0

	for i := n-1; i >= 0; i-- {
		dp[i] = min(dp[i],mx)
		mx = max(mx,height[i])
		if dp[i] - height[i] > 0 {
			rs += dp[i] - height[i]
		}
	}
	return rs

}


// golang max int
func max(first int, args... int) int {
	for _ , v := range args{
		if first < v {
			first = v
		}
	}
	return first
}


// golang min int
func min(first int, args... int) int {
	for _ , v := range args{
		if first > v {
			first = v
		}
	}
	return first
}
 
 ```
 
 
 运行结果:
 
 ![如图](leetcode-42接雨水/2.png)
  <br>
  
  
  ## 总结:
显然,这还不是最优解法
