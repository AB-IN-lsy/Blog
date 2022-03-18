---
title: 1004. 最大连续1的个数 III
tags:
  - Leetcode
  - 每日一题
  - 二分
  - 双指针
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
date: 2022-02-26 19:46:16
---


{% note info %}
**摘要**
Title: 1004. 最大连续1的个数 III
Tag: 二分、双指针
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://leetcode-cn.com/problems/max-consecutive-ones-iii/)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>1004. 最大连续1的个数 III</font>

* ## <font size=4 face=粗体>题意</font>

  >给定一个二进制数组 nums 和一个整数 k ，如果可以翻转最多k 个 0 ，则返回 数组中连续 1 的最大个数 。

 

* ## <font size=4 face=粗体>思路</font>

  * **二分**
    遍历右端点，二分左端点，找到使这个区间0的个数$\le k$的最左边的左端点，可以用前缀和进行$O(1)$的判断
  
  * **双指针**
    **当二分的左端点随着右端点的移动而有序**时，应该考虑可以用双指针来优化
* ## <font size=4 face=粗体>代码</font>

  * **二分**

    ```python
    class Solution:
        def longestOnes(self, nums, k) -> int:
            nums = [-1, *nums]
            b = [0] * len(nums)

            def check(l, r):
                if b[r] - b[l - 1] <= k:
                    return True
                return False

            def find(R):
                l, r = 1, len(b) - 1
                while l < r:
                    mid = l + r >> 1
                    if check(mid, R):
                        r = mid
                    else:
                        l = mid + 1
                return r

            for i in range(len(nums)):
                if nums[i] == 0:
                    b[i] = 1
                b[i] += b[i - 1]
            res = 0
            for i in range(1, len(nums)):
                L = find(i)
                if i == L and k == 0 and nums[i] == 0:
                    continue
                res = max(res, i - L + 1)
            return res

    ```
  
  * **双指针**
  
    ```python
    class Solution:
        def longestOnes(self, nums, k) -> int:
            n = len(nums)
            res, j, cnt = 0, 0, 0  #j左端点, cnt为0的个数
            for i in range(n):
                if nums[i] == 0:
                    cnt += 1
                while j <= i and cnt > k:
                    if nums[j] == 0:
                        cnt -= 1
                    j += 1
                res = max(res, i - j + 1)
            return res
    ```