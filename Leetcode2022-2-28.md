---
title: 503. 下一个更大元素 II
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
date: 2022-02-26 22:50:03
---


{% note info %}
**摘要**
Title: 503. 下一个更大元素 II
Tag: 单调栈
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://leetcode-cn.com/problems/next-greater-element-ii/)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>503. 下一个更大元素 II</font>

* ## <font size=4 face=粗体>题意</font>

  >给定一个循环数组 nums （ nums[nums.length - 1] 的下一个元素是 nums[0] ），返回 nums 中每个元素的 下一个更大元素 。
  >数字 x 的 下一个更大的元素 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1 。

* ## <font size=4 face=粗体>思路</font>

  遇到**循环数组**
   * 可以采用将数组**拉直**，即两个原数组拼接
   * 可以采用下标除余

  答案先把数组倒过来，再取前半部分

* ## <font size=4 face=粗体>代码</font>

  枚举元素

  ```python
  class Solution:
      def nextGreaterElements(self, nums: List[int]) -> List[int]:
          nums = [*nums * 2]
          stk, res = [], []
          for num in reversed(nums):
              while stk and stk[-1] <= num:
                  stk.pop()
              if stk:
                  res.append(stk[-1])
              else:
                  res.append(-1)
              stk.append(num)
          return res[::-1][:len(res) // 2]
  ```

  枚举下标

  ```python
  class Solution:
      def nextGreaterElements(self, nums: List[int]) -> List[int]:
          nums = [*nums * 2]
          stk, res = [], [0] * len(nums)
          for i in range(len(nums) - 1, -1, -1):
              while stk and stk[-1] <= nums[i]:
                  stk.pop()
              res[i] = stk[-1] if stk else -1
              stk.append(nums[i])
          return res[:len(res) // 2]
  ```