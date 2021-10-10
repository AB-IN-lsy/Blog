Powered by:**NEFU AB-IN**

# <font color=#6495ED size=6>[F - 0-1 MST](https://codeforces.com/gym/345048/problem/F)</font>

- ## 题意

  有一个菊花图，给出$n$个点，$m$条边权为$1$的边，其余的边边权为$0$，问最小生成树的权值

- ## 思路

  最优的情况是尽可能的多连边权为$0$的边，即题目没有给出的边，那么连好了边权为$0$的边，就会生成多个连通块，连通块之间的连边即是答案，所以答案就是**补图的连通块数量-1**

  实现的方法就是进行$dfs$，每次进行$dfs$找出现存顶点$u$中不与该点相连的点，那么这些点就与$u$在同一个连通块内，在总集合里删掉这些点，并且接着$dfs$这些点

- ## 代码

  ```cpp
  /*
   * @Author: NEFU AB-IN
   * @Date: 2021-09-21 18:43:45
   * @FilePath: \ACM\CF\2021.9.17\e.cpp
   * @LastEditTime: 2021-09-21 19:49:56
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
  int h[N];
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
  set<int> s;
  
  void dfs(int u)
  {
      set<int> tmp = s;
      for (int i = h[u]; ~i; i = e[i].ne)
      {
          int to = e[i].v;
          tmp.erase(to);
      }
      for (auto i : tmp)
          s.erase(i);
      for (auto i : tmp)
          dfs(i);
  }
  
  signed main()
  {
      IOS;
      init();
      int n, m;
      cin >> n >> m;
      for (int i = 1; i <= m; ++i)
      {
          int u, v;
          cin >> u >> v;
          add(u, v);
          add(v, u);
      }
  
      int ans = 0;
      for (int i = 1; i <= n; ++i)
          s.insert(i);
      for (int i = 1; i <= n; ++i)
      {
          if (s.find(i) != s.end())
          {
              dfs(i);
              ans++;
          }
      }
      cout << ans - 1 << '\n';
      return 0;
  }
  ```
  
  