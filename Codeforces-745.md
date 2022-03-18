---
title: Codeforces Round 745 (Div. 2)
tags:
  - Codeforces
categories:
  - [ACM]
  - [Match Solution]
toc: true
quicklink: true
math: true
sidebar: true
copyright: true
reward: true
date: 2022-01-06 19:37:21
---
{% note info %}
**摘要**
A,B,C（二维前缀和）
{% endnote %}
<!-- more -->


<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://codeforces.com/contest/1581)</font>

@[TOC](文章目录)	


# <font color=#6495ED size=6 >Codeforces Round #745 (Div. 2)</font>

## <font color=#FFA500 size=5>A. CQXYM Count Permutations</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  对于给定的一个$n$,长度为$2n$的序列，值包含$[1,2n]$中的每一个，$a[i+1]>a[i]$为符合条件，问形成不少于$n$个这种情况的序列有多少种

* <font color=#000000 size=4 face=粗体>思路</font>

  猜的结论 $\frac{1}{2}(2n)!$

  其实意思就是只有一半的排列是满足的

  为了乘的方便，直接乘到了$(2n-1)$，最后再乘个$n$，这样就不用逆元了

* <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-30 17:58:25
   * @FilePath: \ACM\CF\2021.9.30.div2\a.cpp
   * @LastEditTime: 2021-09-30 18:49:15
   */
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((int)(X).size())
  #define IOS                      \
      ios::sync_with_stdio(false); \
      cin.tie(0);                  \
      cout.tie(0);
  typedef pair<int, int> PII;
  const int INF = 0x3f3f3f3f;
  
  const LL mod = 1e9 + 7;
  LL mul(LL x, LL y){return 1LL * x * y % mod;}
  LL dec(LL x, LL y){return x >= y ? x - y : x + mod - y;}
  LL add(LL x, LL y){return x + y >= mod ? x + y - mod : x + y;}
  LL pmod(LL x) {return (x + mod) % mod;}
  
  void solve()
  {
      LL n;
      cin >> n;
      LL ans = 1;
      for (int i = 1; i <= (2 * n - 1); ++i)
      {
          ans = mul(ans, i);
      }
      cout << mul(ans, n) << '\n';
  }
  
  signed main()
  {
      IOS;
      int t;
      cin >> t;
      while (t--)
      {
          solve();
      }
      return 0;
  }
  ```

## <font color=#FFA500 size=5>B. Diameter of Graph</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  一个$n$点$m$边的无向连通图（无自环与重边），问这个图的直径是否可以小于$k-1$

* <font color=#000000 size=4 face=粗体>思路</font>

  分类讨论

  * $n=1$时，边只有$0$条，$k$只有$0$时符合条件的
  * 当是菊花图时，直径为$1$
  * 其余就考虑
    * 能否构成图 $m<n-1$
    * 边是否多余 $m>n * (n - 1) / 2$

* <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-30 17:58:50
   * @FilePath: \ACM\CF\2021.9.30.div2\b.cpp
   * @LastEditTime: 2021-09-30 20:12:46
   */
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  
  void solve()
  {
      LL n, m, k;
      cin >> n >> m >> k;
      LL mxx = n * (n - 1) / 2;
      if (n == 1)
      {
          if (k <= 1 || m > 0)
              cout << "NO\n";
          else
              cout << "YES\n";
          return;
      }
      k -= 2;
      if (m < n - 1 || k <= 0 || m > mxx)
          cout << "NO\n";
      else if (k == 1 && m != mxx)
          cout << "NO\n";
      else
          cout << "YES\n";
  }
  
  signed main()
  {
      int t;
      cin >> t;
      while (t--)
      {
          solve();
      }
      return 0;
  }
  ```

## <font color=#FFA500 size=5>C</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  存在一个$n×m$的$01$矩阵，现在要做出一个传送门（类似于$MC$里的地狱门），要求是

  * 行$\ge5$，列$\ge4$，
  * “黑曜石”的地方为$1$
  * 中间区域为$0$
  * 拐角处不做要求

  矩阵中的$01$均可翻转，问造出传送门的最小翻转数

* <font color=#000000 size=4 face=粗体>思路</font>

  **二维前缀和 + 扫描**

  复杂度大约是$O(n^3)$

  * 先对矩阵进行二维前缀和，枚举矩阵上下两条边，顶边从$i=1$开始，底边从$ii=i+4$开始，右列从$jj=4$，这样就知道了左上角的点和右下角的点，就可以计算这个矩阵的翻转次数

  * **翻转次数 = 中间区域$1$数量 + 四边中$0$的数量 - 四个角如果有$0$就减相应个数**

  * 算法：先算出**两倍的**中间区域，再减去总体的，这样就得到了**中间区域$1$的数量 - 四边$1$的数量**，之后加上四边的长度，由于四边只有$01$，那么就得到了**中间区域$1$的数量 + 四边$0$的数量**，剩下拐角挨个判断即可
  * 剩下就是对左列$j$的处理了，左列实时记录最优，跟着$jj$往右即可，别忘了判断$jj-3$处的左列即可，类似于双指针的思想

* <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-10-01 15:57:43
   * @FilePath: \ACM\CF\2021.9.30.div2\c1.cpp
   * @LastEditTime: 2021-10-01 17:00:34
   */
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((int)(X).size())
  #define IOS                      \
      ios::sync_with_stdio(false); \
      cin.tie(0);                  \
      cout.tie(0);
  typedef pair<int, int> PII;
  const int INF = 0x3f3f3f3f;
  
  const int N = 550;
  char a[N][N];
  int sum[N][N];
  
  int getsum(int x1, int y1, int x2, int y2)
  {
      return sum[x2][y2] - sum[x1 - 1][y2] - sum[x2][y1 - 1] + sum[x1 - 1][y1 - 1];
  }
  
  int f(int x1, int y1, int x2, int y2)
  {
      int ans = 0;
      ans += 2 * getsum(x1 + 1, y1 + 1, x2 - 1, y2 - 1);
      ans -= getsum(x1, y1, x2, y2);
      ans += 2 * (x2 - x1 + y2 - y1);
      ans -= (a[x1][y1] == '0');
      ans -= (a[x1][y2] == '0');
      ans -= (a[x2][y1] == '0');
      ans -= (a[x2][y2] == '0');
      return ans;
  }
  
  void solve()
  {
      int n, m;
      scanf("%d%d", &n, &m);
      for (int i = 1; i <= n; ++i)
      {
          scanf("%s", a[i] + 1);
          for (int j = 1; j <= m; ++j)
          {
              sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + (a[i][j] == '1');
          }
      }
      int ans = f(1, 1, 5, 4);
  
      for (int i = 1; i + 4 <= n; ++i)
      {
          for (int ii = i + 4; ii <= n; ++ii)
          {
              int j = 1;
              for (int jj = 4; jj <= m; ++jj)
              {
                  int tmp;
                  if ((tmp = f(i, j, ii, jj)) < ans) // 这样赋值，一定要将括号括号
                  {
                      ans = tmp;
                  }
                  while (jj - j > 3 && (tmp = f(i, j + 1, ii, jj)) < ans)
                  {
                      j++;
                      ans = tmp;
                  }
                  if ((tmp = f(i, jj - 3, ii, jj)) < ans)
                  {
                      j = jj - 3;
                      ans = tmp;
                  }
              }
          }
      }
      printf("%d\n", ans);
  }
  
  signed main()
  {
      int t;
      scanf("%d", &t);
      while (t--)
      {
          solve();
      }
      return 0;
  }
  ```

  

