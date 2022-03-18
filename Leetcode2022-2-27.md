---
title: 496. 下一个更大元素 I
tags:
  - Leetcode
  - 每日一题
  - 单调栈
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
date: 2022-02-26 21:58:21
---


{% note info %}
**摘要**
Title: 496. 下一个更大元素 I
Tag: 单调栈
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://leetcode-cn.com/problems/next-greater-element-i/)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>496. 下一个更大元素 I</font>

* ## <font size=4 face=粗体>题意</font>

  >nums1 中数字 x 的 下一个更大元素 是指 x 在 nums2 中对应位置 右侧 的 第一个 比 x 大的元素。
  >给你两个 没有重复元素 的数组 nums1 和 nums2 ，下标从 0 开始计数，其中nums1 是 nums2 的子集。
  >对于每个 0 <= i < nums1.length ，找出满足 nums1[i] == nums2[j] 的下标 j ，并且在 nums2 确定 nums2[j] 的 下一个更大元素 。如果不存在下一个更大元素，那么本次查询的答案是 -1 。
  >返回一个长度为 nums1.length 的数组 ans 作为答案，满足 ans[i] 是如上所述的 下一个更大元素 。

* ## <font size=4 face=粗体>思路</font>

  求每一个数的右边离他最近的最大数是什么，即从后往前遍历，用单调栈先把小的弹出去

* ## <font size=4 face=粗体>代码</font>

  ```python
  class Solution:
      def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
          stk = []
          d = Counter()
          for num in reversed(nums2):
              while stk and stk[-1] <= num:
                  stk.pop()
              d[num] = stk[-1] if stk else -1
              stk.append(num)
          return [d[num] for num in nums1]
  ```