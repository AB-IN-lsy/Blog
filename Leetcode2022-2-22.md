---
title: 2181. 合并零之间的节点
tags:
  - Leetcode
  - 2022寒假
  - 每日一题
  - 链表
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
date: 2022-02-21 20:37:30
---


{% note info %}
**摘要**
Title: 2181. 合并零之间的节点
Tag: 链表
Memory Limit: 64 MB
Time Limit: 1000 ms
{% endnote %}
<!-- more -->

<font size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://leetcode-cn.com/problems/merge-nodes-in-between-zeros/)</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>2181. 合并零之间的节点</font>

* ## <font size=4 face=粗体>题意</font>

  >给你一个链表的头节点 head ，该链表包含由 0 分隔开的一连串整数。链表的 开端 和 末尾 的节点都满足 Node.val == 0 。
  >对于每两个相邻的 0 ，请你将它们之间的所有节点合并成一个节点，其值是所有已合并节点的值之和。然后将所有 0 移除，修改后的链表不应该含有任何 0 。
  >返回修改后链表的头节点 head 。

* ## <font size=4 face=粗体>思路</font>

  思路:
    * 求[0,0]之间节点的和
    * 修改节点的前后指向关系即可
    * 最后的next特殊判断

* ## <font size=4 face=粗体>代码</font>

  ```python
  class Solution:
      def mergeNodes(self, head: ListNode) -> ListNode:
          pre, cur = head, head.next
          cnt = 0
          while cur:
              cnt += cur.val
              if cur.val == 0:
                  pre.val = cnt
                  pre.next = None if cur.next == None else cur
                  pre = pre.next
                  cnt = 0
              cur = cur.next
          return head
  ```