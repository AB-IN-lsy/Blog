---
title: Nowcoder 小梁的背包
tags:
  - Nowcoder
  - 每日一题
  - 01背包
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
date: 2022-03-07 22:36:12
---


{% note info %}
**摘要**
Title: Nowcoder 小梁的背包
Tag: 01背包
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://ac.nowcoder.com/acm/contest/6106/J)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>Nowcoder 小梁的背包</font>

* ## <font size=4 face=粗体>题意</font>

  >小梁来到了伽勒尔地区并参加了联盟赛热身赛，比赛小岛上有n个精灵散落在岛上各处，她有一个大小为s的背包，每个精灵的战斗值为vi体积为wi。
  >请问在她临走之前背包内宝可梦的战斗力总和最多为多少，并输出其战斗值总和sum以及背包内的精灵数量ans。

* ## <font size=4 face=粗体>思路</font>

  **01背包记录物品数量**，关键思路就是在于**最后的第i件物品选几个**，在这就是选不选，也就是说
  选的时候加1，不选就不动就好

  其实同理，也就可以求**背包中物品都是什么**
  求背包物品都有什么，详情参见[01背包](https://blog.ab-in.cn/2022/03/05/Acwing2022-3-5-3/)
* ## <font size=4 face=粗体>代码</font>

  ```python
  for _ in range(int(input())):
      N = int(1e4 + 100)
      v, w, dp, cnt = [0] * N, [0] * N, [0] * N, [0] * N
      n, s = map(int, input().split())
      for i in range(1, n + 1):
          w[i], v[i] = map(int, input().split())
      for i in range(1, n + 1):
          for j in range(s, w[i] - 1, -1):
              if dp[j] < dp[j - w[i]] + v[i]:
                  dp[j] = dp[j - w[i]] + v[i]
                  cnt[j] = cnt[j - w[i]] + 1
      print(dp[s], cnt[s])
  ```