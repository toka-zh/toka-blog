---
title: "3.有序数组的平方"
date: 2022-08-24T10:38:58+08:00
tags: ["leetcode"]
categories: ["LeetCode"]
---

## 977.有序数组的平方

[力扣题目链接](https://leetcode.cn/problems/squares-of-a-sorted-array/)


给你一个按 非递减顺序 排序的整数数组 `nums`，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。
 

**示例 1：**

    输入：nums = [-4,-1,0,3,10]
    输出：[0,1,9,16,100]
    解释：平方后，数组变为 [16,1,0,9,100]排序后，数组变为 [0,1,9,16,100]


**示例 2：**

    输入：nums = [-7,-3,2,3,11]
    输出：[4,9,9,49,121]


**提示：**

* 1 <= nums.length <= 104
* -104 <= nums[i] <= 104
* nums 已按 非递减顺序 排序


**进阶：**

* 请你设计时间复杂度为 O(n) 的算法解决本问题


## 思路

## 解法

### 滑动窗口

```go
func minSubArrayLen(target int, nums []int) int {
    minLen := len(nums)+1
    i, j, sum := 0, 0, 0
    for i < len(nums) {
        sum += nums[i]
        for sum >= target{
            if i+1-j <= minLen {
                minLen = i + 1 - j
            }
            sum -= nums[j]
            j++
        }
        i++
    }
    if minLen == len(nums)+1{
        return 0
    }else{
        return minLen
    }
}
```



