---
title: 2182. 构造限制重复的字符串
tags:
  - Leetcode
  - 2022寒假
  - 每日一题
  - 优先队列
  - 贪心
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
date: 2022-02-22 10:02:54
---


{% note info %}
**摘要**
Title: 2182. 构造限制重复的字符串
Tag: 优先队列、贪心
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://leetcode-cn.com/problems/construct-string-with-repeat-limit/)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>2182. 构造限制重复的字符串</font>

* ## <font size=4 face=粗体>题意</font>

  >给你一个字符串 s 和一个整数 repeatLimit ，用 s 中的字符构造一个新字符串 repeatLimitedString ，使任何字母 连续 出现的次数都不超过 repeatLimit 次。你不必使用 s 中的全部字符。
  >返回 字典序最大的 repeatLimitedString 。
  >如果在字符串 a 和 b 不同的第一个位置，字符串 a 中的字母在字母表中出现时间比字符串 b 对应的字母晚，则认为字符串 a 比字符串 b 字典序更大 。如果字符串中前 min(a.length, b.length) 个字符都相同，那么较长的字符串字典序更大。

* ## <font size=4 face=粗体>思路</font>

  采用的贪心策略是**尽可能的放字典序大的字母，若有剩余，则用一个次大的字母隔开**
  那么我们就可以用优先队列（大根堆）来实现

  注意
    * `Counter(s)` 可以自动统计s中的字符数量
    * 为了形成大根堆，将字典序的负数加入到优先队列中
    * 只有当存在次大的且大的还有值时候，才将大的放回去
* ## <font size=4 face=粗体>代码</font>

  ```python
  class Solution:
      def repeatLimitedString(self, s: str, repeatLimit: int) -> str:
          d = Counter(s)
          h = []
          res = ""
          for i in d.keys():
              heapq.heappush(h, [-ord(i), d[i]])

          while len(h):
              t = heapq.heappop(h)
              if t[1] <= repeatLimit:
                  res += chr(-t[0]) * t[1]
              else:
                  res += chr(-t[0]) * repeatLimit
                  t[1] -= repeatLimit

                  if len(h):
                      t1 = heapq.heappop(h)
                      res += chr(-t1[0])
                      t1[1] -= 1
                      if t1[1] > 0:
                          heapq.heappush(h, t1)
                  heapq.heappush(h, t)
          return res
  ```