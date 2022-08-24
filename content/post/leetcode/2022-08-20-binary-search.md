---
title: "1.二分查找"
date: 2022-08-20T05:24:55+08:00
tags: ["leetcode"]
categories: ["LeetCode"]
draft: true
---

## 704二分查找

[力扣题目链接](https://leetcode.cn/problems/binary-search/)

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

示例 1:

    输入: nums = [-1,0,3,5,9,12], target = 9
    输出: 4
    解释: 9 出现在 nums 中并且下标为 4

示例 2:

    输入: nums = [-1,0,3,5,9,12], target = 2
    输出: -1
    解释: 2 不存在 nums 中因此返回 -1

 
提示:
1. 你可以假设 nums 中的所有元素是不重复的。
2. n 将在 [1, 10000]之间。
3. nums 的每个元素都将在 [-9999, 9999]之间。

## 解法

### 递归解法
递归写法方便之处在于思路简单,比较易于切入,但由于递归写法需要不断压入函数到运行堆栈,所以最终需要转为性能较高的非递归写法

```go
package main

func search(nums []int, target int) int {
    return binarySearch(nums,0,len(nums)-1,target)
}

func binarySearch(nums []int, start, end, target int) int{
    if start > end || start == end {
        return -1
    }
	
	// 中位指针
    middle := start + (end - start) /2
	
	// 判断中位值
    if target < nums[middle]{
        // 目标值 < 中位值
        // 目标值在中位值左边
        return binarySearch(nums,start,middle-1,target)
    }else if target > nums[middle]{
        // 目标值 > 中位值
        // 目标值在中位值右边
        return binarySearch(nums,middle+1,end,target)
    }else{
        // 目标值 = 中位值
		// target == nums[middle]
        // 返回中位值位置
        return middle
    }
}
```

### 非递归解法
从递归解法`return -1`的条件可以判断转为循环的条件
循环体则和递归判断一样,根据不同的条件替换不同的值

```go
package main

func search(nums []int, target int) int {
    start := 0
    end := len(nums)-1
    for start <= end{
        middle := (end + start) / 2
        if target < nums[middle]{
                end = middle-1
        }else if target > nums[middle]{
                start = middle+1
        }else if target == nums[middle]{
			return middle
		}
    
    }
    return -1
}
```
