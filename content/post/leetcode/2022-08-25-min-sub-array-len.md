---
title: "长度最小的子数组"
date: 2022-08-24T15:17:58+08:00
tags: ["LeetCode"]
categories: ["LeetCode"]
---

## 209.长度最小的子数组

[力扣题目链接](https://leetcode.cn/problems/minimum-size-subarray-sum/)


给定一个含有 `n` 个正整数的数组和一个正整数 `target` 。

找出该数组中满足其和 `≥ target` 的长度最小的 连续子数组 `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度。如果不存在符合条件的子数组，返回 `0` 。
 

**示例 1：**

    输入：target = 7, nums = [2,3,1,2,4,3]
    输出：2
    解释：子数组 [4,3] 是该条件下的长度最小的子数组。


**示例 2：**

    输入：target = 4, nums = [1,4,4]
    输出：1


**示例 3：**

    输入：target = 11, nums = [1,1,1,1,1,1,1,1]
    输出：0


**提示：**

* 1 <= target <= 109
* 1 <= nums.length <= 105
* 1 <= nums[i] <= 105


**进阶：**

* 如果你已经实现 O(n) 时间复杂度的解法, 请尝试设计一个 O(n log(n)) 时间复杂度的解法。



## 思路

1. 数组有序
2. 负数平方可能大于正数

可以首尾设置指针,比较 指向数 的 平方数 大小,将较大的插入到结果集前段
或者 可以比较数的绝对值后,再求平方
没有什么太大区别,第二种可以稍微减少计算量(没错 力扣分奴在此)

## 解法

O(n log(n))暴力遍历即可

### O(n)

```go
func sortedSquares(nums []int) []int {
    result := make([]int,len(nums))
    j := len(nums)-1
    i := 0
    idx := j
    for i<=j {
        if nums[j] + nums[i] < 0{
            result[idx] = nums[i] * nums[i]
            i++
        }else{
            result[idx] = nums[j] * nums[j]
            j--
        }
        idx --
    }
    return result
}
```