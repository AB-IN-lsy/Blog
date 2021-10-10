<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link 9.23](https://vjudge.net/contest/458957#overview)</font>

<font color=#FFA500 size=5 face=楷体>[Link 9.24](https://vjudge.net/contest/459307#overview)</font>

@[TOC]

# <font color=#6495ED size=6 >NEFU 9.23</font>

## <font color=#FFA500 size=5>A - Valid BFS? </font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  给出一个树和一个$bfs$序，问是否是它的其中一个

* <font color=#000000 size=4 face=粗体>思路</font>

  每次进行弹出队列时，将它的子节点全部放进一个$set$里，遍历题目给的$bfs$序，如果在$set$里找到了，那么在$set$里删去，并加入队列；如果到最后$set$还有值，说明$bfs$序是错的

* <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-23 20:03:59
   * @FilePath: \ACM\CF\2021.9.23\a.cpp
   * @LastEditTime: 2021-09-23 20:44:24
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
  
  const int N = 1e6 + 10;
  struct Edge
  {
      int v, ne;
  } e[N << 2];
  int h[N], vis[N];
  int cnt;
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
  }
  vector<int> vv;
  int pos;
  void bfs(int x)
  {
      queue<int> q;
      q.push(x);
      vis[x] = 1;
  
      while (q.size())
      {
          int u = q.front(), cnt1 = 0;
          q.pop();
          set<int> s;
          for (int i = h[u]; ~i; i = e[i].ne)
          {
              int v = e[i].v;
              if (vis[v])
                  continue;
              vis[v] = 1;
              s.insert(v);
          }
          for (int i = pos; i < SZ(vv); ++i)
          {
              if (!SZ(s))
                  break;
              if (s.find(vv[i]) != s.end())
              {
                  s.erase(vv[i]);
                  q.push(vv[i]);
                  cnt1++;
              }
          }
          if (SZ(s))
          {
              cout << "No" << '\n';
              return;
          }
          pos += cnt1;
      }
  }
  signed main()
  {
      IOS;
      init();
      int n;
      cin >> n;
      for (int i = 1; i < n; ++i)
      {
          int x, y;
          cin >> x >> y;
          add(x, y);
          add(y, x);
      }
      for (int i = 1; i <= n; ++i)
      {
          int x;
          cin >> x;
          vv.push_back(x);
      }
      if (vv[0] != 1)
      {
          cout << "No" << '\n';
          return 0;
      }
      vv.erase(vv.begin());
      bfs(1);
      if (pos == SZ(vv))
      {
          cout << "Yes" << '\n';
      }
      return 0;
  }
  ```



# <font color=#6495ED size=6 >NEFU 9.24</font>

## <font color=#FFA500 size=5>A - Modulo Equality</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  给出两个数组$a$$，b$，问$a$数组每个数加多少次$1$并取模$m$才能和$b$数组相同

* <font color=#000000 size=4 face=粗体>思路</font>

  用$multiset$模拟即可，看$a$数组的哪个值到达$b$的最大值时，$a$就和$b$数组相等了

* <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
      * @Author: NEFU AB-IN
      * @Date: 2021-09-24 20:21:23
   * @FilePath: \ACM\CF\2021.9.24\a.cpp
   * @LastEditTime: 2021-09-24 21:21:32
      */
  #include <bits/stdc++.h>
  using namespace std;
  #define LL long long
  #define MP make_pair
  #define SZ(X) ((LL)(X).size())
  #define IOS                      \
      ios::sync_with_stdio(false); \
      cin.tie(0);                  \
      cout.tie(0);
  #define DEBUG(X) cout << #X << ": " << X << endl;
  typedef pair<LL, LL> PII;
  const LL N = 2010;
  LL a[N];
  
  multiset<LL> b;
  signed main()
  {
      IOS;
      LL n, m;
      cin >> n >> m;
      for (LL i = 1; i <= n; ++i)
      {
          cin >> a[i];
      }
      LL max = 0;
      for (LL i = 1; i <= n; ++i)
      {
          LL x;
          cin >> x;
          b.insert(x);
      }
      LL t = *(b.rbegin());
      sort(a + 1, a + 1 + n, greater<LL>());
      LL minx = 0x3f3f3f3f;
      for (LL i = 1; i <= n; ++i)
      {
          LL tmp = t - a[i] >= 0 ? t - a[i] : (t - a[i] + m);
          multiset<LL> s;
          for (LL i = 1; i <= n; ++i)
          {
              s.insert((a[i] + tmp) % m);
          }
          if (s == b)
          {
              minx = min(minx, tmp);
          }
      }
      cout << minx << '\n';
      return 0;
  }
  ```

## <font color=#FFA500 size=5>B - Maximum Sum on Even Positions</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  给定一个包含 $n$ 个元素的序列（下标从 $0$ 到 $n-1$），你可以选择一个连续区间进行翻转，使得翻转过后的序列偶数项的总和（即 $a_0,a_2,\ldots,a_{2k}$ 的和，其中 $k=\lfloor \dfrac{n-1}{2} \rfloor$）最大。

* <font color=#000000 size=4 face=粗体>思路</font>

  首先翻转奇数元素个数的区间时没有意义的，那么就翻转**偶数**元素个数的区间，翻转之后的结果是奇数的位置和偶数的位置互换了，那么就找某个**长度为偶数，并且奇数位和$-$偶数位的和最大的区间**即可，最后的答案就是$max(原先偶数位的和, 原先偶数位的和+(奇数位和-偶数位的和最大值))$

  如何实现呢？

  * 首先分两种情况的**奇数位-偶数位**

    * $a_1-a_0, a_3-a_2,\dots$
    * $a_1-a_2,a_3-a_4,\dots$

    ​	画一画图就明白了

  * **最大子段和**，来求连续区间的最大和是多少，用到**dp**

    ```cpp
    dp[i] = max(dp[i - 1] + a[i], a[i]);
    ```

    最后遍历数组求出最大值，注意，**`dp[n]`不是最大值**

* <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-24 21:29:09
   * @FilePath: \ACM\CF\2021.9.24\b.cpp
   * @LastEditTime: 2021-09-29 23:02:03
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
  LL a[N], b[N], c[N], dp[N];
  
  void solve()
  {
      // construct the minus array
      int n;
      cin >> n;
      for (int i = 0; i < n; ++i)
      {
          cin >> a[i];
      }
      int cnt1 = 0, cnt2 = 0;
      for (int i = 0; i < n; i += 2)
      {
          if (i + 1 >= n)
              break;
          b[++cnt1] = a[i + 1] - a[i];
      }
      for (int i = 1; i < n; i += 2)
      {
          if (i + 1 >= n)
              break;
          c[++cnt2] = a[i] - a[i + 1];
      }
      // start dp function 1
      dp[0] = 0;
      for (int i = 1; i <= cnt1; ++i)
      {
          dp[i] = max(dp[i - 1] + b[i], b[i]);
      }
      LL mx = dp[1];
      for (int i = 1; i <= cnt1; ++i)
      {
          mx = max(mx, dp[i]);
      }
      // start dp function 2
      dp[0] = 0;
      for (int i = 1; i <= cnt2; ++i)
      {
          dp[i] = max(dp[i - 1] + c[i], c[i]);
      }
      for (int i = 1; i <= cnt2; ++i)
      {
          mx = max(mx, dp[i]);
      }
      LL ans = 0;
      for (int i = 0; i < n; ++i)
      {
          if (i % 2 == 0)
              ans += a[i];
      }
      cout << max(ans, ans + mx) << '\n';
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

## <font color=#FFA500 size=5>C - Sasha and a Bit of Relax</font>

* ### <font color=#000000 size=4 face=粗体>题意</font>

  长度为$n$的数列，求有多少区间满足

  * 长度为偶数

  * 前一半异或值等于后一半异或值

* <font color=#000000 size=4 face=粗体>思路</font>

  如果前一半异或值等于后一半异或值，说明两者异或为$0$，说明这个区间异或值为$0$

  根据异或前缀和的性质，$[l,r]$的区间异或值为$sum[r] \ xor \ sum[l - 1]$，那么就是找$sum[r] \ xor \ sum[l - 1] = 0$的，也就是$sum[r] = sum[l - 1]$

  所以就是找$sum[r] = sum[l - 1]$且$(r-l+1)\%2==0$的区间有多少个

  **代码十分精妙**

  做到这点，可以运用$dp$，即开一个数组$dp[N][2]$

  * 初始值$dp[0][0] = 1$，代表异或前缀和为$0$，且下标为偶数的有一个，就是在$0$点

  * $dp[i][0]$表示异或前缀和为$i$，且下标为偶数
  * $dp[i][1]$表示异或前缀和为$i$，且下标为奇数

  ```cpp
  dp[sum[i]][i & 1] ++;
  ```

  这样就确保了区间长度为偶数

  题目问有多少个，如果有$3$个相同的异或前缀和，那么就是$3$对，即$1 + 2$，所以在递增的时候就加上即可

* <font color=#000000 size=4 face=粗体>代码</font>

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-29 23:25:32
   * @FilePath: \ACM\CF\2021.9.24\c.cpp
   * @LastEditTime: 2021-09-29 23:52:57
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
  const int INF = 0x3f3f3f3f;
  
  int n;
  const int N = 1e6 + 10;
  LL a[N], sum[N], dp[N][2];
  
  signed main()
  {
      IOS;
      cin >> n;
      for (int i = 1; i <= n; ++i)
      {
          cin >> a[i];
          sum[i] = a[i] ^ sum[i - 1];
      }
      LL ans = 0;
      dp[0][0] = 1;
      for (int i = 1; i <= n; ++i)
      {
          ans += dp[sum[i]][i & 1];
          dp[sum[i]][i & 1]++;
      }
      cout << ans << '\n';
      return 0;
  }
  ```

  

