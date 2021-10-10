<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://codeforces.com/contest/1579)</font>

@[TOC]

# <font color=#6495ED size=6 >Codeforces Round #744 (Div. 3)</font>

## <font color=#FFA500 size=5>A. Casimir's String Solitaire</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  有两种操作

  * 同时删$A$和$B$
  * 同时删$B$和$C$

  问一个字符串是否能删完

* ### <font color=#000000 size=4 face=粗体>思路</font>

  就是看$B$的个数和$A$与$C$的个数

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-28 22:30:17
   * @FilePath: \ACM\CF\2021.9.28.div3\a.cpp
   * @LastEditTime: 2021-09-28 22:38:06
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
  #define DEBUG(X) cout << #X << ": " << X << endl;
  typedef pair<int, int> PII;
  
  const int N = 1e4 + 10;
  
  signed main()
  {
      IOS;
      int t;
      cin >> t;
      while (t--)
      {
          string s;
          cin >> s;
          int a = 0, b = 0, c = 0;
          for (auto i : s)
          {
              if (i == 'A')
                  a++;
              if (i == 'B')
                  b++;
              if (i == 'C')
                  c++;
          }
          if (b == a + c)
              cout << "YES\n";
          else
              cout << "NO\n";
      }
      return 0;
  }
  ```

## <font color=#FFA500 size=5>B. Shifting Sort</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  有一个长度为$n$的数组$a$，进行不超过$n$次的操作，将数组转变为升序，问几次操作和每次操作的$l,r,d$

  操作是选取一段区间$[l,r]$，进行左移$d$个单位的操作，溢出的元素补到后面

* ### <font color=#000000 size=4 face=粗体>思路</font>

  类似于冒泡排序，一定正确的策略就是每次就把最小的移到最前面，然后对数组进行拼接的操作，**即将最小的元素前面的元素拼接到数组后面**，之后剔除第一个元素

  对于数组进行操作的，比较方便用$Python$进行操作

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```python
  '''
  Author: NEFU AB-IN
  Date: 2021-09-28 23:22:27
  FilePath: \ACM\CF\2021.9.28.div3\b.py
  LastEditTime: 2021-09-28 23:35:54
  '''
  import math
  
  def solve():
      n = int(input())
      l = list(map(int, input().split()))
      ll = []
      for i in range(n):
          id = l.index(min(l))
          if id != 0:
              ll.append([i + 1, n, id])
          l = (l[id:] + l[:id])[1:]
      print(len(ll))
      for i in ll:
          print(f"{i[0]} {i[1]} {i[2]}")
  
  
  for _ in range(int(input())):
      solve()
  ```



## <font color=#FFA500 size=5>C. Ticks</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  在包含$*$和$.$的图中，有多个（包含$0$）由$*$组成的$V$型图案，问$*$点是否都用来组成$V$，$V$由$2d+1$个点组成，要求$d \ge k$

* ### <font color=#000000 size=4 face=粗体>思路</font>

  大模拟即可，枚举每个$*$做为$V$最底下那个点，并枚举$d$，判断是否可以组成$V$，如果可以，就给每个组成$V$的$*$打上标记即可，复杂度$O(n^3m)$

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-29 00:20:19
   * @FilePath: \ACM\CF\2021.9.28.div3\c.cpp
   * @LastEditTime: 2021-09-29 00:46:32
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
  typedef pair<int, int> pII;
  const int INF = 0x3f3f3f3f;
  
  const int N = 100;
  char a[N][N];
  bool f[N][N];
  
  void solve()
  {
      memset(f, 0, sizeof f);
      int n, m, k;
      cin >> n >> m >> k;
      for (int i = 1; i <= n; ++i)
      {
          for (int j = 1; j <= m; ++j)
          {
              cin >> a[i][j];
          }
      }
      for (int i = 1; i <= n; ++i)
      {
          for (int j = 1; j <= m; ++j)
          {
              if (a[i][j] == '*')
              {
                  for (int d = k; d <= n; ++d)
                  {
                      int flag = 0;
                      if (i - d < 1 || j - d < 1 || j + d > m)
                      {
                          break;
                      }
                      for (int kk = 1; kk <= d; ++kk)
                      {
                          if (a[i - kk][j - kk] != '*')
                          {
                              flag = 1;
                              break;
                          }
                      }
                      if (flag)
                          continue;
                      for (int kk = 1; kk <= d; ++kk)
                      {
                          if (a[i - kk][j + kk] != '*')
                          {
                              flag = 1;
                              break;
                          }
                      }
                      if (!flag)
                      {
                          f[i][j] = 1;
                          for (int kk = 1; kk <= d; ++kk)
                          {
                              f[i - kk][j - kk] = 1;
                          }
                          for (int kk = 1; kk <= d; ++kk)
                          {
                              f[i - kk][j + kk] = 1;
                          }
                      }
                  }
              }
          }
      }
      int flag = 0;
      for (int i = 1; i <= n; ++i)
      {
          for (int j = 1; j <= m; ++j)
          {
              if (a[i][j] == '*' && f[i][j] == 0)
              {
                  flag = 1;
                  cout << "NO\n";
                  break;
              }
          }
          if (flag)
              break;
      }
      if (!flag)
          cout << "YES\n";
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

  

## <font color=#FFA500 size=5>D. Productive Meeting</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  有$n$个人，每个人有$a_i$个谈话次数，每次可以两个人单独出来谈话一次，问最多可以谈话几次，并输出每次的人的编号

* ### <font color=#000000 size=4 face=粗体>思路</font>

  很明显的大根堆的题，每次出来两个次数最多的谈话，谈话时间减完塞回到大根堆里

  **注意**

  * 大根堆元素只有$1$个时就退出
  * 每次出来的两个元素，有$0$了就退出
  * 如果减完时变成$0$，不加进大根堆
  * 存操作采用的双端队列$deque$

  在这里记一下$deque$

  * `emplace_front()` 代表从前面插入元素
  * `emplace_back()`代表从后面插入元素
  * `pop_front()`从前面弹出，从前出类似于**栈**
  * `pop_back()`从后面弹出，从后出就类似于**队列**
  * 都是$O(1)$的操作
  * 而且$deque$具有可迭代性，即有`cbegin() rbegin()`，是队列和优先队列所没有的

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-28 23:44:08
   * @FilePath: \ACM\CF\2021.9.28.div3\d.cpp
   * @LastEditTime: 2021-09-29 15:34:54
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
  
  void solve()
  {
      int n;
      cin >> n;
      priority_queue<PII, vector<PII>> q;
      for (int i = 1; i <= n; ++i)
      {
          int x;
          cin >> x;
          q.push(MP(x, i));
      }
      deque<PII> Q;
      while (q.size() > 1)
      {
          PII x1 = q.top();
          q.pop();
          PII x2 = q.top();
          q.pop();
          if (x1.first == 0 || x2.first == 0)
          {
              break;
          }
          x1.first--;
          x2.first--;
          Q.emplace_front(MP(x1.second, x2.second));
          if (x1.first)
              q.push(x1);
          if (x2.first)
              q.push(x2);
      }
      cout << Q.size() << '\n';
      for (auto i : Q)
      {
          cout << i.first << " " << i.second << '\n';
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

## <font color=#FFA500 size=5>E1. Permutation Minimization by Deque</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  给出一个排列，利用双端队列构造出**字典序**最小的序列

* ### <font color=#000000 size=4 face=粗体>思路</font>

  按题意模拟即可，小的就放在前面即可，用`vector`会被卡，因为$insert$复杂度为$O(n)$，而`deque`操作为$O(1)$

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-28 23:39:42
   * @FilePath: \ACM\CF\2021.9.28.div3\e1.cpp
   * @LastEditTime: 2021-09-29 16:36:31
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
  #define DEBUG(X) cout << #X << ": " << X << endl;
  typedef pair<int, int> PII;
  
  void solve()
  {
      int n;
      cin >> n;
      deque<int> q;
      for (int i = 1; i <= n; i++)
      {
          int x;
          cin >> x;
          if (i == 0)
          {
              q.emplace_back(x);
              continue;
          }
          if (*q.begin() >= x)
              q.emplace_front(x);
          else
              q.emplace_back(x);
      }
      for (auto i : q)
          cout << i << " ";
      cout << '\n';
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

## <font color=#FFA500 size=5>E2. Array Optimization by Deque</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  给出一个排列，利用双端队列构造出**逆序对**最小的序列

* ### <font color=#000000 size=4 face=粗体>思路</font>

  元素插入只有两种情况

  * **最前面**，对后面的元素产生逆序对
  * **最后面**，对前面的元素产生逆序对

  那么就每次插入的时候比较一下，两种情况的逆序对大小，每次加上小的就可以，$tree[i]$还是正常加，相当于塞进去

  顺便复习了一遍**树状数组求逆序对**，**一定要离散化！！！**

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-29 18:19:55
   * @FilePath: \ACM\CF\2021.9.28.div3\e2.cpp
   * @LastEditTime: 2021-09-29 20:37:33
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
  #define lowbit(x) ((x) & -(x))
  typedef pair<int, int> PII;
  const int INF = 0x3f3f3f3f;
  
  const int N = 1e6 + 10;
  LL tree[N], n, t[N], a[N];
  void add(LL x, LL d)
  {
      while (x <= n)
      {
          tree[x] += d;
          x += lowbit(x);
      }
  }
  LL sum(LL x)
  {
      LL sum = 0;
      while (x > 0)
      {
          sum += tree[x];
          x -= lowbit(x);
      }
      return sum;
  }
  
  void solve()
  {
      memset(tree, 0, sizeof tree);
      cin >> n;
      for (int i = 1; i <= n; ++i)
      {
          cin >> a[i];
          t[i] = a[i];
      }
      sort(t + 1, t + 1 + n);
      int m = unique(t + 1, t + 1 + n) - t - 1;
      for (int i = 1; i <= n; ++i)
      {
          a[i] = lower_bound(t + 1, t + 1 + m, a[i]) - t;
      }
      LL ans = 0;
      for (int i = 1; i <= n; ++i)
      {
          add(a[i], 1);
          ans += min(i - sum(a[i]), sum(a[i] - 1));
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

  



完结。