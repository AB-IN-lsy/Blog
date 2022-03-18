---
title: ABC242_c 1111gal password
tags:
  - AtCoder
  - 每日一题
  - dp
  - 组合数
categories:
  - [ACM]
  - [2022大三下学期]
  - [Python]
toc: true
quicklink: true
math: true
sidebar: true
copyright: true
reward: true
date: 2022-03-06 10:49:37
---


{% note info %}
**摘要**
Title: ABC242_c 1111gal password
Tag: dp、组合数
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://atcoder.jp/contests/abc242/tasks/abc242_c)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>ABC242_c 1111gal password</font>

* ## <font size=4 face=粗体>题意</font>

  >以9位为基数的n位数字的数量，相邻数字相差一个或更少。
  > 比如 n = 2, 则11 12 21 22 23 32 33 34 43 44 45 54 55 56 65 66 67 76 77 78 87 88 89 98 99
  > 共25个

* ## <font size=4 face=粗体>思路</font>

  $dp$即可，$dp[i][j]$表示前$i$位，第$i$位为$j$的情况的数量
  递推式就为
  $$ 
  dp[i][j] = (dp[i - 1][j - 1] + dp[i - 1][j] + dp[i - 1][j + 1])
  $$

  那么$dp[n][1->10]$就是答案

* ## <font size=4 face=粗体>代码</font>

  ps: $python3$ 比 $pypy3$ 要慢很多很多
  ```python
  '''
  Author: NEFU AB-IN
  Date: 2022-03-06 10:46:17
  FilePath: \ACM\AtCoder\abc242\c.py
  LastEditTime: 2022-03-06 10:46:18
  '''

  N = int(1e6 + 10)
  MOD = 998244353
  dp = [[0] * 12 for _ in range(N)]

  for i in range(1, 10):
      dp[1][i] = 1

  n = int(input())
  for i in range(2, n + 1):
      for j in range(1, 10):
          dp[i][j] = (dp[i - 1][j - 1] + dp[i - 1][j] + dp[i - 1][j + 1]) % MOD

  print(sum(dp[n]) % MOD)
  ```