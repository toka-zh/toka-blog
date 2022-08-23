---
title: "2.移除元素"
date: 2022-08-20T05:30:58+08:00
tags: ["leetcode"]
categories: ["LeetCode"]
draft: true
---

## 27. 移除元素

[力扣题目链接](https://leetcode.cn/problems/remove-element/)

## 解法
不考虑暴力解法

### 双指针-头尾指针
```go
func removeElement(nums []int, val int) int {
    start := 0
    end := len(nums)-1
    for start<=end{
        if nums[start] == val{
            nums[start],nums[end] = nums[end],nums[start]
            end --
        }else{
            start++
        }
    }
    return end+1
}
```

### 双指针-快慢指针
```go
func removeElement(nums []int, val int) int {
    ptr := 0
    for i:=0;i<len(nums);i++{
        if nums[i] != val{
            nums[ptr] = nums[i]
            ptr++
        }
    }
    return ptr
}

```