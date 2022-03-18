---
title: ABC228_e Integer Sequence Fair 
tags:
  - Acwing
  - 每日一题
  - 欧拉定理
  - 快速幂
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
date: 2022-03-08 11:23:44
---


{% note info %}
**摘要**
Title: ABC228_e Integer Sequence Fair 
Tag: 欧拉定理、快速幂
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://atcoder.jp/contests/abc228/tasks/abc228_e)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>ABC228_e Integer Sequence Fair </font>

* ## <font size=4 face=粗体>题意</font>

  > 求 $m^k^n$

* ## <font size=4 face=粗体>思路</font>

  ![oula](https://oss.ab-in.cn/Pictures/ouladingli.png)

  首先要保证 gcd(m, MOD) = 1, 才能运用欧拉定理
  因为MOD为质数，所以 **m和MOD互质 -> m不是MOD的倍数**
* ## <font size=4 face=粗体>代码</font>

  ```python
  '''
  Author: NEFU AB-IN
  Date: 2022-03-08 13:39:02
  FilePath: \ACM\AtCoder\abc228\e.py
  LastEditTime: 2022-03-08 15:43:01
  '''
  MOD = 998244353


  def qpow(a, b, p):
      res = 1
      while b:
          if b & 1:
              res = res * a % p
          b >>= 1
          a = a * a % p
      return res


  n, k, m = map(int, input().split())

  # m ^ k ^ n
  if m % MOD == 0:
      print(0)
      exit(0)

  print(qpow(m, qpow(k, n, MOD - 1), MOD) % MOD)
```