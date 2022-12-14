---
title: "移除链表元素"
date: 2022-08-29T18:23:58+08:00
tags: ["Leetcode"]
categories: ["Leetcode"]
---

## 203.移除链表元素

[力扣题目链接](https://leetcode.cn/problems/remove-linked-list-elements/)

给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。

**示例 1：**
![img](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

    输入：head = [1,2,6,3,4,5,6], val = 6
    输出：[1,2,3,4,5]


**示例 2：**

    输入：head = [], val = 1
    输出：[]


**示例 3：**

    输入：head = [7,7,7,7], val = 7
    输出：[]


**提示：**

    列表中的节点数目在范围 [0, 104] 内
    1 <= Node.val <= 50
    0 <= val <= 50

## 解法
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func removeElements(head *ListNode, val int) *ListNode {
    for head != nil && head.Val == val{
        head = head.Next
    }

    ptr := head

    for ptr != nil && ptr.Next != nil{
        if ptr.Next.Val == val{
            ptr.Next = ptr.Next.Next
        }else{
            ptr = ptr.Next
        }
    }
    return head
}
```