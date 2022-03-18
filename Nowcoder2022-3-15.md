---
title: Nowcoder 筑巢
tags:
  - Nowcoder
  - 每日一题
  - 最大子段和
  - 树形dp
  - dp
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
date: 2022-03-15 20:27:26
---


{% note info %}
**摘要**
Title: Nowcoder 筑巢
Tag: 最大子段和、树形dp、dp
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://ac.nowcoder.com/acm/contest/11222/E)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>Nowcoder 筑巢</font>

* ## <font size=4 face=粗体>题意</font>

  >小沙转生成为了蚂蚁子，现在他攻占了一颗树，树里面还是实心的木头，所以小沙想要将里面连续的一部分掏空让自己居住（因为小沙只想住一个家）。但是并不是每个部分都适合开采，某些地方的开采后可能导致舒适度下降。
  >将上面的问题抽象出来，我们可以理解成，给定你一个n个节点的树，你需要在树上选取一个非空连通块，使其舒适度和最大。选择的边和点的舒适度都是舒适度。

* ## <font size=4 face=粗体>思路</font>

  最大子段和 + 树形dp
  这里的**连通块**可以抽象为**连续的子段**，唯一的不同就是**最大子段和**只能从一个状态转移过来，而在树上可以通过两个子节点转移而来，所以需要累加

  ps: 一般树形dp都是二维的，即$dp[i][0 / 1]$，由于此题没有选不选的条件限制，所以可以直接一维来做

* ## <font size=4 face=粗体>代码</font>

  ```python
  N = int(1e5 + 10)
  INF = int(2e9)
  g = [[] for _ in range(N)]
  dp = [0] * N
  val = [0] * N
  res = -INF

  def dfs(fa, u):
      global res
      dp[u] = val[u]
      for v, w in g[u]:
          if v == fa:
              continue
          dfs(u, v)
          dp[u] += max(0, dp[v] + w)
          res = max(res, dp[u])
          
  n = int(input())
  val[1:] = list(map(int, input().split()))

  for i in range(n - 1):
      u, v, w = map(int, input().split())
      g[u].append([v, w])
      g[v].append([u, w])

  dfs(0, 1)
  print(res)
  ```