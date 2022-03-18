---
title: 2183. 统计可以被 K 整除的下标对数目
tags:
  - Acwing
  - 2022寒假
  - 每日一题
  - gcd
  - 暴力
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
date: 2022-02-22 10:47:18
---


{% note info %}
**摘要**
Title: 2183. 统计可以被 K 整除的下标对数目
Tag: gcd，暴力
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://leetcode-cn.com/problems/count-array-pairs-divisible-by-k/)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>2183. 统计可以被 K 整除的下标对数目</font>

* ## <font size=4 face=粗体>题意</font>

  >给你一个下标从 0 开始、长度为 n 的整数数组 nums 和一个整数 k ，返回满足下述条件的下标对 (i, j) 的数目：
  >0 <= i < j <= n - 1 且
  >nums[i] * nums[j] 能被 k 整除。
 

* ## <font size=4 face=粗体>思路</font>

  可能因为k的最大因数个数是常数级别的，所以使第二重循环遍历接近常数
  其实就是将两个数的其中一个数，转化为和k的最小公倍数，也就是k的因子

* ## <font size=4 face=粗体>代码</font>

  ```python
  class Solution:
      def countPairs(self, nums: List[int], k: int) -> int:
          d, res = Counter(), 0
          for num in nums:
              g = gcd(num, k)
              for i in d:
                  if i * g % k == 0:
                      res += d[i]
              d[g] += 1
          return res
  ```