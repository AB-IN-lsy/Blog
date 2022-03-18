---
title: Codeforces Round 748 (Div. 3)
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
A,B,C,D1,D2（差值+gcd）,E（类拓扑）
{% endnote %}
<!-- more -->


<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://codeforces.com/contest/1593)</font>

@[TOC](文章目录)	


# <font color=#6495ED size=6 >Codeforces Round #748 (Div. 3)</font>

## <font color=#FFA500 size=5>A. Elections</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  三个数，每个数最少加多少成最大的数

* ### <font color=#000000 size=4 face=粗体>思路</font>

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  // Problem: A. Elections
  // Contest: Codeforces - Codeforces Round #748 (Div. 3)
  // Author: NEFU AB-IN
  // Edit Time:2021-10-13 22:37:34
  // URL: https://codeforces.com/contest/1593/problem/A
  // Memory Limit: 256 MB
  // Time Limit: 1000 ms
  
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((int)(X).size())
  #define IOS                                                                                                            \
      ios::sync_with_stdio(false);                                                                                       \
      cin.tie(0);                                                                                                        \
      cout.tie(0);
  typedef pair<int, int> PII;
  const int INF = 0x3f3f3f3f;
  
  void solve()
  {
      LL a, b, c;
      cin >> a >> b >> c;
      LL mx = max(max(a, b), c);
      int cnt = 0;
      if (a == mx)
          cnt++;
      if (b == mx)
          cnt++;
      if (c == mx)
          cnt++;
      if (cnt == 3)
      {
          cout << "1 1 1\n";
          return;
      }
      if (cnt == 2)
      {
          if (a == mx)
          {
              cout << "1 ";
          }
          else
          {
              cout << mx - a + 1 << ' ';
          }
          if (b == mx)
          {
              cout << "1 ";
          }
          else
          {
              cout << mx - b + 1 << ' ';
          }
          if (c == mx)
          {
              cout << "1\n";
          }
          else
          {
              cout << mx - c + 1 << '\n';
          }
      }
      else
      {
          if (a == mx)
          {
              cout << "0 ";
          }
          else
          {
              cout << mx - a + 1 << ' ';
          }
          if (b == mx)
          {
              cout << "0 ";
          }
          else
          {
              cout << mx - b + 1 << ' ';
          }
          if (c == mx)
          {
              cout << "0\n";
          }
          else
          {
              cout << mx - c + 1 << '\n';
          }
      }
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

## <font color=#FFA500 size=5>B. Make it Divisible by 25</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  字符串$s$，问最少删多少个数，使得能被$25$整除，且是正数

* ### <font color=#000000 size=4 face=粗体>思路</font>

  找后缀为$00,25,50,75$即可

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  // Problem: B. Make it Divisible by 25
  // Contest: Codeforces - Codeforces Round #748 (Div. 3)
  // Author: NEFU AB-IN
  // Edit Time:2021-10-13 22:37:38
  // URL: https://codeforces.com/contest/1593/problem/B
  // Memory Limit: 256 MB
  // Time Limit: 1000 ms
  
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((int)(X).size())
  #define IOS                                                                                                            \
      ios::sync_with_stdio(false);                                                                                       \
      cin.tie(0);                                                                                                        \
      cout.tie(0);
  typedef pair<int, int> PII;
  const int INF = 0x3f3f3f3f;
  
  const int N = 1e6 + 10;
  int a[N];
  
  void solve()
  {
      string s;
      cin >> s;
      int ans = INF;
      for (int i = SZ(s) - 1; i >= 0; --i)
      {
          if (s[i] == '5')
          {
              for (int j = i - 1; j >= 0; --j)
              {
                  if (s[j] == '2')
                  {
                      ans = min(ans, SZ(s) - j - 2);
                  }
                  if (s[j] == '7')
                  {
                      ans = min(ans, SZ(s) - j - 2);
                  }
              }
          }
          if (s[i] == '0')
          {
              for (int j = i - 1; j >= 0; --j)
              {
                  if (s[j] == '5')
                  {
                      ans = min(ans, SZ(s) - j - 2);
                  }
                  if (s[j] == '0')
                  {
                      ans = min(ans, SZ(s) - j - 2);
                  }
              }
          }
      }
      cout << ans << '\n';
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

## <font color=#FFA500 size=5>C. Save More Mice</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  猫在$0$，洞在$n$，$k$个老鼠在$(1,n)$，每次只能有一个老鼠往右走一步，猫也走一步，猫如果到老鼠的位置，老鼠被吃，问最多多少到洞

* ### <font color=#000000 size=4 face=粗体>思路</font>

  贪心模拟

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-10-13 23:48:34
   * @FilePath: \ACM\CF\2021.10.13\c.cpp
   * @LastEditTime: 2021-10-13 23:54:01
   */
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((LL)(X).size())
  #define IOS                                                                                                            \
      ios::sync_with_stdio(false);                                                                                       \
      cin.tie(0);                                                                                                        \
      cout.tie(0);
  typedef pair<LL, LL> PII;
  const LL INF = 0x3f3f3f3f;
  
  const LL N = 1e6 + 10;
  LL a[N];
  
  void solve()
  {
      LL n, k;
      cin >> n >> k;
      for (LL i = 1; i <= k; ++i)
      {
          cin >> a[i];
      }
      sort(a + 1, a + 1 + k, greater<LL>());
      LL m = 0, ans = 0;
      for (LL i = 1; i <= k; ++i)
      {
          LL xx = n - a[i];
          if (m + xx >= n)
          {
              cout << ans << '\n';
              return;
          }
          m += xx;
          ans += 1;
      }
      cout << ans << '\n';
  }
  
  signed main()
  {
      IOS;
      LL t;
      cin >> t;
      while (t--)
      {
          solve();
      }
      return 0;
  }
  ```


## <font color=#FFA500 size=5>D1. All are Same</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  有$n$个数，找一个大于$1$的数$k$，通过让数减$k$，问能否让**整个**数组的数相同，求最大的$k$

* ### <font color=#000000 size=4 face=粗体>思路</font>

  考虑先进行$set$去重，如果只有一种元素，那么$k=0$，则输出$-1$

  其他情况，就在$set$排完序后，进行对**差**的$gcd$操作，因为如果要全部一样，那么就要差全为$0$

  让$x$个差只减一个数全变成$0$，就是求这$x$数的$gcd$

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-10-13 23:56:44
   * @FilePath: \ACM\CF\2021.10.13\d.cpp
   * @LastEditTime: 2021-10-13 23:58:12
   */
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((int)(X).size())
  #define IOS                                                                                                            \
      ios::sync_with_stdio(false);                                                                                       \
      cin.tie(0);                                                                                                        \
      cout.tie(0);
  typedef pair<int, int> PII;
  const int INF = 0x3f3f3f3f;
  
  const int N = 400010;
  int a[N], b[N];
  
  void solve()
  {
      int n, cnt = 0;
      set<int> S;
      cin >> n;
      for (int i = 1; i <= n; i++)
      {
          int x;
          cin >> x;
          if (S.count(x) != 0)
              continue;
          S.insert(x);
          a[++cnt] = x;
      }
      if (cnt == 1)
      {
          cout << "-1\n";
          return;
      }
      int xx;
      sort(a + 1, a + cnt + 1);
      xx = a[2] - a[1];
      for (int i = 3; i <= cnt; i++)
      {
          xx = __gcd(xx, a[i] - a[i - 1]);
      }
      cout << xx << '\n';
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



## <font color=#FFA500 size=5>D2. Half of Same</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  有$n$个数，找一个大于$1$的数$k$，通过让数减$k$，问能否让**大于一半**数组的数相同，求最大的$k$

* ### <font color=#000000 size=4 face=粗体>思路</font>

  同样如果有一半的数相同，也是$-1$

  否则，就暴力搜一下，枚举数组内两两元素差值，对这些差值求两两之间的$gcd$，枚举$gcd$，看看有没有$n/2$元素的差值是该$gcd$的倍数

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  // Problem: D2. Half of Same
  // Contest: Codeforces - Codeforces Round #748 (Div. 3)
  // Author: NEFU AB-IN
  // Edit Time:2021-10-13 22:37:48
  // URL: https://codeforces.com/contest/1593/problem/D2
  // Memory Limit: 256 MB
  // Time Limit: 1000 ms
  
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((int)(X).size())
  #define IOS                                                                                                            \
      ios::sync_with_stdio(false);                                                                                       \
      cin.tie(0);                                                                                                        \
      cout.tie(0);
  typedef pair<int, int> PII;
  const int INF = 0x3f3f3f3f;
  
  const int N = 1e6 + 10;
  int a[N];
  
  void solve()
  {
      int n;
      cin >> n;
      unordered_map<int, int> mp;
      for (int i = 1; i <= n; ++i)
      {
          cin >> a[i];
          mp[a[i]]++;
      }
      for (auto [x, y] : mp)
      {
          if (y >= n / 2)
          {
              cout << "-1\n";
              return;
          }
      }
      set<int> cha1;
      set<int> cha2;
      for (int i = 1; i <= n; ++i)
      {
          for (int j = i + 1; j <= n; ++j)
          {
              cha1.insert(abs(a[i] - a[j]));
          }
      }
      for (auto i : cha1)
      {
          for (auto j : cha1)
          {
              cha2.insert(__gcd(i, j));
          }
      }
      int ans = 0;
      for (auto ii : cha2)
      {
          if (!ii)
              continue;
          for (int i = 1; i <= n; i++)
          {
              int tmp = 0;
              for (int j = 1; j <= n; j++)
              {
                  int now = abs(a[i] - a[j]);
                  if (now % ii == 0)
                      tmp++;
              }
              if (tmp >= n / 2)
                  ans = max(ans, ii);
          }
      }
      cout << ans << '\n';
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

  

## <font color=#FFA500 size=5>E. Gardener and Tree</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  给一棵树$n$个节点，每次删掉所有的叶子，操作$k$次，求剩下的节点数

* ### <font color=#000000 size=4 face=粗体>思路</font>

  **类拓扑排序**

  对于无向无环图，先统计入度为$\le1$的点进队列作为源点（其实此时的**入度**就相当于**连有多少边**，当$\le1$时其实就相当于只有出去的那一条边了），进行队列操作遍历相邻节点

  * 若本身这个节点的度$\le1$，说明只有进来的那一条边了，说明后面无节点，故不加队列
  * 否则就记录深度为父节点$+1$，并减度，若此时$\le1$，那么说明后面还有节点，加入队列

  最后统计有多少节点$\le k$，最后用$n$一减即可

  另外$n$有$4e5$会卡$memset$，所以建图采用$vector$，并用循环清空

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-10-14 00:12:16
   * @FilePath: \ACM\CF\2021.10.13\e.cpp
   * @LastEditTime: 2021-10-14 00:39:34
   */
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((int)(X).size())
  #define IOS                                                                                                            \
      ios::sync_with_stdio(false);                                                                                       \
      cin.tie(0);                                                                                                        \
      cout.tie(0);
  typedef pair<int, int> PII;
  const int INF = 0x3f3f3f3f;
  const int N = 1e6 + 10;
  vector<int> vv[N];
  int deg[N], tag[N], dis[N];
  void solve()
  
  {
      int n, k;
      cin >> n >> k;
      for (int i = 1; i <= n; ++i)
      {
          deg[i] = 0;
          tag[i] = 0;
          dis[i] = 0;
          vv[i].clear();
      }
      for (int i = 1; i < n; ++i)
      {
          int u, v;
          cin >> u >> v;
          vv[u].push_back(v);
          vv[v].push_back(u);
          deg[u]++;
          deg[v]++;
      }
      if (n == 1)
      {
          cout << "0\n";
          return;
      }
      queue<int> q;
      for (int i = 1; i <= n; ++i)
      {
          if (deg[i] <= 1)
          {
              tag[i] = 1;
              q.push(i);
              dis[i] = 1;
          }
      }
      while (q.size())
      {
          int u = q.front();
          q.pop();
          for (auto v : vv[u])
          {
              if (deg[v] <= 1)
                  continue;
              deg[v]--;
              dis[v] = dis[u] + 1;
              if (deg[v] <= 1)
                  q.push(v);
          }
      }
      int ans = 0;
      for (int i = 1; i <= n; ++i)
      {
          if (dis[i] <= k)
              ans++;
      }
      cout << n - ans << '\n';
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

  
