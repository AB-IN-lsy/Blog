---
title: C++/Python结构体
tags:
  - C++
  - Python
  - 结构体
categories:
  - [ACM]
  - [C++]
  - [Python]
toc: true
quicklink: true
math: true
sidebar: true
copyright: true
reward: true

date: 2022-01-07 21:21:47
---

{% note info %}
**摘要**
定义C++/Python结构体的注意事项
{% endnote %}
<!-- more -->

<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

@[TOC](文章目录)

# <font color=#6495ED size=6>C++结构体写法</font>

* 结构体的定义
* 结构体内部排序（重写小于号）
  * 两个$const$都不能省，&也不能省

```cpp
struct sa
{
    int w, to;
    bool operator<(const sa &a) const
    {
        return w < a.w; 
        // 升序，但在优先队列里是降序
    }
    sa(int w, int to) : w(w), to(to){}; //构造函数
};
```
# <font color=#6495ED size=6>Python结构体写法</font>

* 结构体的定义
* 结构体内部排序（重写小于号）

```python
class Line(object):
    def __init__(self, k=0, b=0):  #构造函数(通过默认值实现构造函数的多态性)
        self.k = k
        self.b = b

    def __lt__(self, t): #重写小于号
        if self.k != t.k:
            return self.k < t.k
        else:
            return self.b < t.b
```

