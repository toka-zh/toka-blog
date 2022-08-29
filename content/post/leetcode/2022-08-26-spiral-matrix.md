---
title: "螺旋矩阵II"
date: 2022-08-26T11:43:58+08:00
tags: ["LeetCode"]
categories: ["LeetCode"]
---

## 59.螺旋矩阵II

[力扣题目链接](https://leetcode.cn/problems/spiral-matrix-ii/)

给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。
 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

    输入：n = 3
    输出：[[1,2,3],[8,9,4],[7,6,5]]





**示例 2：**

    输入：n = 1
    输出：[[1]]


**提示：**

* 1 <= n <= 20



## 思路

首先找规律,提取出可以进行循环的部分
1. 正方形的每条边长度相等
2. 遍历每条边的元素只需单独遍历 行 或 列
3. 每向内一层,横边的 纵坐标 变化,竖边的 横坐标 变化
4. 同层间边的起始值为等差数列,并且与层数相关

## 解法

```go
func generateMatrix(n int) [][]int {
    matrix := make([][]int, n)
    start := 1
    
    for i := 0; i < n; i++ {
        matrix[i] = make([]int, n)
    }
    for i := 0; i < (n+1)/2; i++ { // 层数
        edge := n - 1 - 2*i
        for j := 0; j < edge; j++ { // 每层的四边
            // up
            matrix[i][i+j] = start + j
            // right
            matrix[i+j][n-1-i] = start + edge*1 + j
            // // down
            matrix[n-1-i][n-1-i-j] = start + edge*2 + j
            // // left
            matrix[n-1-i-j][i] = start + edge*3 + j
        }
        start += edge * 4
    }
    
    if n%2 == 1 {
        matrix[(n)/2][(n)/2] = start
    }
    
    return matrix
}
```

