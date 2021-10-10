<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://codeforces.com/contest/1592)</font>

@[TOC]

# <font color=#6495ED size=6 >Codeforces Round #746 (Div. 2)</font>

## <font color=#FFA500 size=5>A.Gamer Hemose</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  问几轮打完怪

* ### <font color=#000000 size=4 face=粗体>思路</font>

  取最大的和第二大的即可

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-10-03 22:34:40
   * @FilePath: \ACM\CF\2021.10.3.div2\a.cpp
   * @LastEditTime: 2021-10-04 19:00:30
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
  LL a[N];
  
  void solve()
  {
      LL n, H;
      cin >> n >> H;
      for (int i = 1; i <= n; ++i)
      {
          cin >> a[i];
      }
      sort(a + 1, a + 1 + n, greater<LL>());
      LL ans = 0;
      LL tmp = a[1] + a[2];
      if (H <= a[1])
      {
          cout << "1\n";
      }
      else if (H <= tmp)
      {
          cout << "2\n";
      }
      else
      {
          int tt = H / tmp;
          if (H % tmp == 0)
              cout << tt * 2 << '\n';
          else if (H - tt * tmp > a[1])
              cout << tt * 2 + 2 << '\n';
          else if (H - tt * tmp <= a[1])
              cout << tt * 2 + 1 << '\n';
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

## <font color=#FFA500 size=5>B. Hemose Shopping</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  数组经过特定的调换（即下标之差大于$x$）后，能否升序?

* ### <font color=#000000 size=4 face=粗体>思路</font>

  判断中间不能动的与升序的位置是否对应即可

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-10-03 22:34:47
   * @FilePath: \ACM\CF\2021.10.3.div2\b.cpp
   * @LastEditTime: 2021-10-03 23:38:24
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
  
  const int N = 1e6 + 10;
  int a[N], b[N];
  
  void solve()
  {
      int n, x, flag = 0;
      cin >> n >> x;
      for (int i = 1; i <= n; ++i)
      {
          cin >> a[i];
          if (a[i] < a[i - 1])
              flag = 1;
          b[i] = a[i];
      }
      sort(b + 1, b + 1 + n);
      if (!flag)
      {
          cout << "YES\n";
          return;
      }
      for (int i = 1; i <= n; ++i)
      {
          if (i - 1 < x && n - i < x && a[i] != b[i])
          {
              cout << "NO\n";
              return;
          }
      }
      cout << "YES\n";
      return;
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

## <font color=#FFA500 size=5>C.Bakry and Partitioning</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  存在一个节点数为$n$的最小生成树，每个点都有点值，问最少剪一条边，最多剪$k-1$条边，问是否剩下的连通块的异或和均相等

* ### <font color=#000000 size=4 face=粗体>思路</font>

  分类讨论

  * 如果总异或为$0$，说明一定可以剪成两个连通块进行异或结果为$0$，也就是结果相等
  * 如果总异或不为$0$
    * 既然要各连通块异或和相等，而且如果把边都连起来，异或还等于总异或和，那么最优的情况就是，**让每个连通块的异或和都等于总异或和，并且连通块的数量为大于1的奇数**，这样符合情况并最优
    * 如果连通块的个数是偶数的话，那么合起来的异或和不可能等于总异或和，是不成立的

* ### <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-10-03 23:55:53
   * @FilePath: \ACM\CF\2021.10.3.div2\c.cpp
   * @LastEditTime: 2021-10-07 19:36:13
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
  int a[N];
  struct Edge
  {
      int v, ne;
  } e[N << 2];
  int h[N];
  int cnt, ans, sum;
  void add(int u, int v)
  {
      e[cnt].v = v;
      e[cnt].ne = h[u];
      h[u] = cnt++;
  }
  void init()
  {
      memset(h, -1, sizeof(h));
      cnt = 0;
      ans = 0;
      sum = 0;
  }
  
  int dfs(int u, int fa)
  {
      int tmp = a[u];
      for (int i = h[u]; ~i; i = e[i].ne)
      {
          int v = e[i].v;
          if (fa == v)
              continue;
          tmp ^= dfs(v, u);
      }
      if (tmp == sum)
      {
          ans++;
          return 0;
      }
      return tmp;
  }
  
  void solve()
  {
      init();
      int n, k;
      scanf("%d%d", &n, &k);
      for (int i = 1; i <= n; ++i)
      {
          scanf("%d", &a[i]);
          sum ^= a[i];
      }
      int u, v;
      for (int i = 1; i < n; ++i)
      {
  
          scanf("%d%d", &u, &v);
          add(u, v);
          add(v, u);
      }
      if (!sum)
      {
          printf("YES\n");
          return;
      }
      dfs(1, 0);
      if (k >= 3 && ans >= 3)
      {
          printf("YES\n");
      }
      else
      {
          printf("NO\n");
      }
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

  

