---
title: 2180. 统计各位数字之和为偶数的整数个数
tags:
  - Leetcode
  - 2022寒假
  - 每日一题
  - 模拟
categories:
  - [ACM]
  - [2022寒假每日一题]
  - [Python]
toc: true
quicklink: true
math: true
sidebar: true
copyright: true
reward: true
date: 2022-02-21 20:34:41
---


{% note info %}
**摘要**
Title: 2180. 统计各位数字之和为偶数的整数个数
Tag: 模拟
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://leetcode-cn.com/problems/count-integers-with-even-digit-sum/)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>2180. 统计各位数字之和为偶数的整数个数</font>

* ## <font size=4 face=粗体>题意</font>

  >给你一个正整数 num ，请你统计并返回 小于或等于 num 且各位数字之和为 偶数 的正整数的数目。
  >正整数的 各位数字之和 是其所有位上的对应数字相加的结果。

* ## <font size=4 face=粗体>思路</font>

  数据范围小，模拟  

* ## <font size=4 face=粗体>代码</font>

  ```python
  class Solution:
      def countEven(self, num: int) -> int:
          res = 0
          for i in range(1, num + 1):
              s = str(i)
              cnt = 0
              for j in s:
                  cnt += int(j)
              if cnt % 2 == 0:
                  res += 1
          return res
  ```