---
draft: true # 是否为草稿
title: TipsC
status: public
date: 2020-10-27 20:02:33
tags:
categories:
---

使用for循环输入字符数组时少一位

原因：回车符输入到了字符数组的第一位

解决：在for循环前加入`getchar();`，清除缓冲区中的回车符



