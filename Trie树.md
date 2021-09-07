Powered by:**NEFU AB_IN**

# <font color=#6495ED size=6>Trie树</font>
模板带注释

```cpp
// son[][]存储树中每个节点的子节点
// cnt[]存储以每个节点结尾的单词数量
namespace Trie{
    const int N = 1e5 + 10;
    int son[N][26], cnt[N], idx; // 'son[N][10]'
    void init(){
        memset(son, 0, sizeof son);
        memset(cnt, 0, sizeof cnt);
        idx = 0;
    }
    void insert(char *s){
        int p = 0; // p代表每个节点的下标，现在为根节点
        for(int i = 0; s[i]; ++ i){
            int u = s[i] - 'a'; // '0'代表减数字时
            // u代表p结点的子节点，但不是下标，u和p性质不一样
            if(!son[p][u]) son[p][u] = ++ idx; // 新建节点，每个节点都有自己的下标
            p = son[p][u]; // 往下走
        }
        cnt[p] ++; // 统计以p下标结尾单词的数量
    }
    int query(char *s){
        int p = 0;
        for(int i = 0; s[i]; ++ i){
            int u = s[i] - 'a'; // '0'
            if(!son[p][u]) return 0;
            p = son[p][u];
        }
        return cnt[p];
    }
}
using namespace Trie;
```

无注释

```cpp
namespace Trie{
    const int N = 1e5 + 10;
    int son[N][26], cnt[N], idx;
    void init(){
        memset(son, 0, sizeof son);
        memset(cnt, 0, sizeof cnt);
        idx = 0;
    }
    void insert(char *s){
        int p = 0;
        for(int i = 0; s[i]; ++ i){
            int u = s[i] - 'a';
            if(!son[p][u]) son[p][u] = ++ idx;
            p = son[p][u];
        }
        cnt[p] ++;
    }
    int query(char *s){
        int p = 0;
        for(int i = 0; s[i]; ++ i){
            int u = s[i] - 'a';
            if(!son[p][u]) return 0;
            p = son[p][u];
        }
        return cnt[p];
    }
}
using namespace Trie;
```



# <font color=#6495ED size=6>**[Phone List](http://poj.org/problem?id=3630)**</font>

* ## 题意

  给定$n$个长度不超过$10$的数字串，问其中是否存在两个数字串$S,T$，使得$S$是$T$的前缀，多组数据。

* ## 思路

  $Trie$树模板题

  常规思路就是把每个字符串都进树，然后离线处理，这样时间和空间复杂度都比较大

  所以我们可以做到**动态查询**

  * 比如$91$和$9134$，$91$以$1$结尾，那么当$9134$遍历到$1$时，发现这个下标的$cnt \ne0$，说明前面有单词$91$，那么就不符合
  * 比如$9134$和$91$，$91$遍历到$1$的时候，发现这个下标的$cnt = 0$，但是$91$是$9134$的前缀，也不符合要求。要做到这点，只需要一开始记个$flag = false$，如果插入的过程中开点了，那么$flag = ture$，像刚才就没有动态开点，所以依然是$false$

  ```cpp
  /*
   * @Author: NEFU AB_IN
   * @Date: 2021-08-26 14:00:53
   * @FilePath: \Vscode\ACM\Project\Trie\Poj3630.cpp
   * @LastEditTime: 2021-08-26 15:25:14
   */
  #include<iostream>
  #include<vector>
  #include<cstring>
  #include<string.h>
  #include<cstdio>
  #include<climits>
  #include<cmath>
  #include<algorithm>
  #include<queue>
  #include<deque>
  #include<map>
  #include<set>
  #include<string>
  #include<stack>
  using namespace std;
  #define LL                          long long
  #define MP                          make_pair
  #define SZ(X)                       ((int)(X).size())
  #define IOS                         ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
  #define DEBUG(X)                    cout << #X << ": " << X << endl;
  typedef pair<int , int>             PII;
  
  namespace Trie{
      const int N = 1e5 + 10;
      int son[N][10], cnt[N], idx;
      void init(){
          memset(son, 0, sizeof son);
          memset(cnt, 0, sizeof cnt);
          idx = 0;
      }
      bool insert(char *s){
          int p = 0, flag = 0;
          for(int i = 0; s[i]; ++ i){
              int u = s[i] - '0';
              if(!son[p][u]) son[p][u] = ++ idx, flag = 1;
              p = son[p][u];
              if(cnt[p]) return false;
          }
          cnt[p] ++;
          return flag;
      }
      int query(char *s){
          int p = 0;
          for(int i = 0; s[i]; ++ i){
              int u = s[i] - '0';
              if(!son[p][u]) return 0;
              p = son[p][u];
          }
          return cnt[p];
      }
  }
  using namespace Trie;
  
  int t;
  char s[N];
  
  signed main()
  {
      IOS
      cin >> t;
      while(t --){
          init();
          int n, flag = 0;
          cin >> n;
          for(int i = 1; i <= n; ++ i){
              cin >> s;
              if(!insert(s)) flag = 1;
          }
          if(flag) cout << "NO\n";
          else cout << "YES\n";
      }
      return 0;
  }
  ```





# <font color=#6495ED size=6>01Trie树</font>

模板带注释

$01$字典树，每个分支只有$01$两个，主要解决**异或问题**

如果多组数据运用$01trie$树时，需要进行初始化，可以像正常$trie$树一样进行$memset$操作，但有些题目就会$T$，这时我们可以采用更好的方法进行初始化。观察到分支只有两个，那么就可以在**创建新分支**的同时，对新分支的两个$01$子节点进行初始化，注意别忘了一开始也要对根节点的子节点进行初始化！

```cpp
namespace Trie01{
    const int N = 1e5 + 10;
    int son[N * 32][2], val[N * 32], idx; // n个数，每个数最多被拆成不超过32位
    void init(){
        idx = 0;
        son[0][0] = son[0][1] = 0;
        // val没必要进行初始化，因为是可以覆盖的
    }
    void insert(int x){
        int p = 0;
        for(int i = 31; i >= 0; -- i){ 
            // 贪心时构建01trie树的基础，从二进制的高位开始贪心
            int u = (x >> i) & 1;
            if(!son[p][u]){
                son[p][u] = ++ idx;
                p = son[p][u];
                // 构造新节点的同时初始化新节点
                son[p][0] = son[p][1] = 0;
            }
            else p = son[p][u];
        }
        val[p] = x; // 记录终点
    }
    int query(int x){
        int p = 0;
        for(int i = 31; i >= 0; -- i){
            int u = (x >> i) & 1;
            // 贪心的选择优先走和当前位不同的路
            if(son[p][u ^ 1]) p = son[p][u ^ 1];
            else p = son[p][u];
        }
        return val[p];
    }
}
using namespace Trie01;
```

无注释

```cpp
namespace Trie01{
    const int N = 1e5 + 10;
    int son[N * 32][2], val[N * 32], idx;
    void init(){
        idx = 0;
        son[0][0] = son[0][1] = 0;
    }
    void insert(int x){
        int p = 0;
        for(int i = 31; i >= 0; -- i){
            int u = (x >> i) & 1;
            if(!son[p][u]){
                son[p][u] = ++ idx;
                p = son[p][u];
                son[p][0] = son[p][1] = 0;
            }
            else p = son[p][u];
        }
        val[p] = x;
    }
    int query(int x){
        int p = 0;
        for(int i = 31; i >= 0; -- i){
            int u = (x >> i) & 1;
            if(son[p][u ^ 1]) p = son[p][u ^ 1];
            else p = son[p][u];
        }
        return val[p];
    }
}
using namespace Trie01;
```



# <font color=#6495ED size=6>**[Xor Sum(hdu4825)](https://acm.hdu.edu.cn/showproblem.php?pid=4825)**</font>

* ## 题意

  $n$个正整数，$m$次询问，找一个正整数$K$，使得 $K$ 与 $S$ 的异或结果最大

* ## 思路

  $01Trie$板子题

  ```cpp
  /*
   * @Author: NEFU AB_IN
   * @Date: 2021-08-26 16:40:07
   * @FilePath: \Vscode\ACM\Project\Trie\hdu4825.cpp
   * @LastEditTime: 2021-08-26 22:54:39
   */
  #include<bits/stdc++.h>
  using namespace std;
  #define LL                          long long
  #define MP                          make_pair
  #define SZ(X)                       ((int)(X).size())
  #define IOS                         ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
  #define DEBUG(X)                    cout << #X << ": " << X << endl;
  typedef pair<int , int>             PII;
  
  namespace Trie01{
      const int N = 1e5 + 10;
      int son[N * 32][2], val[N * 32], idx;
      void init(){
          idx = 0;
          son[0][0] = son[0][1] = 0;
      }
      void insert(int x){
          int p = 0;
          for(int i = 31; i >= 0; -- i){
              int u = (x >> i) & 1;
              if(!son[p][u]){
                  son[p][u] = ++ idx;
                  p = son[p][u];
                  son[p][0] = son[p][1] = 0;
              }
              else p = son[p][u];
          }
          val[p] = x;
      }
      int query(int x){
          int p = 0;
          for(int i = 31; i >= 0; -- i){
              int u = (x >> i) & 1;
              if(son[p][u ^ 1]) p = son[p][u ^ 1];
              else p = son[p][u];
          }
          return val[p];
      }
  }
  using namespace Trie01;
  
  int t, n, m, x;
  signed main()
  {
      IOS
      while(~scanf("%d", &t)){
          for(int j = 1; j <= t; ++ j){
              init();
              scanf("%d%d", &n, &m);
              for(int i = 1; i <= n; ++ i){
                  scanf("%d", &x);
                  insert(x);
              }
              printf("Case #%d:\n", j);
              for(int i = 1; i <= m; ++ i){
                  scanf("%d", &x);
                  printf("%d\n", query(x));
              }
          }
      }
      return 0;
  }
  ```

# [<font color=#6495ED size=6>**A - L 语言**</font>](https://vjudge.net/contest/449699#problem/A)

* ## 题意

  给出$n$个字符串构成一个字典$D$，$m$次询问，每次询问一个字符串，输出这段字符串在字典$D$可以被理解的最长前缀的位置

* ## 思路

  **dp + trie**

  思路都在注释中

  这里的查询函数改了，改成了从字符串的某处开始查询

  主要思路

  * 先，从头开始进行$dp$，遇到了前缀就标记$dp[] = 1$记作下次$dp$的起点
  * 后，从后往前遍历，最先$dp[] = 1$的下标就是最佳答案

  ```cpp
  /*
   * @Author: NEFU AB_IN
   * @Date: 2021-08-27 01:01:30
   * @FilePath: \Vscode\ACM\Project\Trie\L.cpp
   * @LastEditTime: 2021-08-27 01:34:09
   */
  #include<bits/stdc++.h>
  using namespace std;
  #define LL                          long long
  #define MP                          make_pair
  #define SZ(X)                       ((int)(X).size())
  #define IOS                         ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
  #define DEBUG(X)                    cout << #X << ": " << X << endl;
  typedef pair<int , int>             PII;
  
  
  namespace Trie{
      const int N = 2e6 + 20;
      int n, m, dp[N];
      int son[N][26], cnt[N], idx;
      char s[N];
      void init(){
          memset(son, 0, sizeof son);
          memset(cnt, 0, sizeof cnt);
          idx = 0;
      }
      void insert(char *s){
          int p = 0;
          for(int i = 0; s[i]; ++ i){
              int u = s[i] - 'a';
              if(!son[p][u]) son[p][u] = ++ idx;
              p = son[p][u];
          }
          cnt[p] ++;
      }
      void query(int x){ // the searching start index of string s
          int p = 0;
          for(int i = x; s[i]; ++ i){
              int u = s[i] - 'a';
              if(!son[p][u]) break;
              p = son[p][u];
              if(cnt[p]) dp[i] = 1; // explain there exists a word's end, so next time
                                    // dp will call this function, the param is id plus one
          }
      }
  }
  using namespace Trie;
  
  signed main()
  {
      IOS;
      cin >> n >> m;
      for(int i = 1; i <= n; ++ i){
          cin >> s;
          insert(s);        
      }
      for(int j = 1; j <= m; ++ j){
          memset(dp, 0, sizeof dp);
          cin >> s + 1;
          dp[0] = 1; // init
          for(int i = 0; s[i]; ++ i){
              if(dp[i]) query(i + 1);
          }
          for(int i = strlen(s); i >= 0; -- i){
              // searching from end to start, in order to gain the max number depend 
              // on greedy thought, and has made sure if there r not exist a solution
              // then output 0 because of dp[0] = 1
              if(dp[i]){
                  cout << i << '\n';
                  break;
              }
          }
      }
      return 0;
  }
  ```



# [<font color=#6495ED size=6>**B - Secret Message 秘密信息**</font>](https://vjudge.net/contest/449699#problem/B)

* ## 题意

  有多少信息和这条暗号有着相同的前缀，这些信息和暗号都是不完整的

* ## 思路

  既然是不完整的，那么暗号里只要找到在$trie$可以匹配的即可

  比如$11$ 配对 $1,110$，就分成了两种情况

  * 一种就是像$1$这种在匹配路上
  * 一种就是像$110$正常结束的

  所以答案就是（途径-结尾）

  ```cpp
  /*
   * @Author: NEFU AB_IN
   * @Date: 2021-08-27 02:41:04
   * @FilePath: \Vscode\ACM\Project\Trie\SecretMessage.cpp
   * @LastEditTime: 2021-08-27 09:57:06
   */
  #include<bits/stdc++.h>
  using namespace std;
  #define LL                          long long
  #define MP                          make_pair
  #define SZ(X)                       ((int)(X).size())
  #define IOS                         ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
  #define DEBUG(X)                    cout << #X << ": " << X << endl;
  typedef pair<int , int>             PII;
  
  namespace Trie{
      const int N = 2e6 + 10;
      int son[N][2], cnt[N], idx, ans[N];
      char s[N];
      void init(){
          memset(son, 0, sizeof son);
          memset(cnt, 0, sizeof cnt);
          idx = 0;
      }
      void insert(char *s){
          int p = 0;
          for(int i = 0; s[i]; ++ i){
              int u = s[i] - '0';
              if(!son[p][u]) son[p][u] = ++ idx;
              p = son[p][u];
              ans[p] ++;
          }
          cnt[p] ++;
      }
      int query(char *s){
          int p = 0, tot = 0, flag = 0;
          for(int i = 0; s[i]; ++ i){
              int u = s[i] - '0';
              if(!son[p][u]) {
                  flag = 1;
                  break;
              }
              p = son[p][u];
              tot += cnt[p];
          }
          if(!flag) tot += ans[p] - cnt[p];
          return tot;
      }
  }
  using namespace Trie;
  
  int n, m;
  signed main()
  {
      IOS;
      cin >> n >> m;
      for(int i = 1; i <= n; ++ i){
          int k, idx = 0;
          cin >> k;
          for(int j = 1; j <= k; ++ j){
              char x;
              cin >> x;
              s[idx ++] = x;
          }
          s[idx] = '\0';
          insert(s);
      }
      for(int i = 1; i <= m; ++ i){
          int k, idx = 0;
          cin >> k;
          for(int j = 1; j <= k; ++ j){
              char x;
              cin >> x;
              s[idx ++] = x;
          }
          s[idx] = '\0';
          cout << query(s) << '\n';
      }
      return 0;
  }
  ```



# [<font color=#6495ED size=6>**C - The XOR-longest Path**</font>](https://vjudge.net/contest/449699#problem/C)

* ## 题意

  给定一棵 $n$ 个点的带权树，求树上最长的异或和路径

* ## 思路

  根据**异或前缀和**的性质，$l,r$区间异或和为$sum[r] \ xor \  sum[l - 1]$

  那么既然有$n$个点，$n-1$条边，那么每个点之间都有一条双向边，且无环

  问题就转换成了，先进行$dfs$将从源点到别的点的异或和插入到$01Trie$，再遍历整个$01Trie$找到最大的异或和即可，即$D$题

  ```cpp
  /*
   * @Author: NEFU AB_IN
   * @Date: 2021-08-27 10:11:04
   * @FilePath: \Vscode\ACM\Project\Trie\poj3764.cpp
   * @LastEditTime: 2021-08-27 11:10:44
   */
  #include<iostream>
  #include<vector>
  #include<cstring>
  #include<string.h>
  #include<cstdio>
  #include<climits>
  #include<cmath>
  #include<algorithm>
  #include<queue>
  #include<deque>
  #include<map>
  #include<set>
  #include<string>
  #include<stack>
  using namespace std;
  #define LL                          long long
  #define MP                          make_pair
  #define SZ(X)                       ((int)(X).size())
  #define IOS                         ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
  #define DEBUG(X)                    cout << #X << ": " << X << endl;
  typedef pair<int , int>             PII;
  
  const int N = 1e6 + 10;
  struct Edge
  {
      int u, v, w, ne;
  }e[N << 2];
  int h[N];
  int cnt;
  void add(int u, int v, int w)
  {
      e[cnt].u = u;
      e[cnt].v = v;
      e[cnt].w = w;
      e[cnt].ne = h[u];
      h[u] = cnt++;
  }
  void init(){
      memset(h, -1, sizeof(h));
      cnt = 0;
  }
  namespace Trie01{
      int son[N * 32][2], val[N * 32], idx;
      void init(){
          idx = 0;
          son[0][0] = son[0][1] = 0;
      }
      void insert(int x){
          int p = 0;
          for(int i = 31; i >= 0; -- i){
              int u = (x >> i) & 1;
              if(!son[p][u]){
                  son[p][u] = ++ idx;
                  p = son[p][u];
                  son[p][0] = son[p][1] = 0;
              }
              else p = son[p][u];
          }
          val[p] = x;
      }
      int query(int x){
          int p = 0;
          for(int i = 31; i >= 0; -- i){
              int u = (x >> i) & 1;
              if(son[p][u ^ 1]) p = son[p][u ^ 1];
              else p = son[p][u];
          }
          return val[p] ^ x;
      }
  }
  using namespace Trie01;
  int n, a[N], u, v, w, deg[N];
  
  void dfs(int u, int fa, int dis){
      a[u] = dis;
      for(int i = h[u]; ~i; i = e[i].ne){
          int vv = e[i].v;
          int ww = e[i].w;
          if(vv == fa) continue;
          dfs(vv, u, dis ^ ww);
      }
  }
  
  signed main()
  {
      ::init();
      cin >> n;
      for(int i = 1; i <= n - 1; ++ i){
          cin >> u >> v >> w;
          add(u, v, w);
          add(v, u, w);
      }
      dfs(1, 0, 0);
      for(int i = 1; i <= n; ++ i){
          insert(a[i]);
      }
      int ans = a[1] ^ a[2];
      for(int i = 1; i <= n; ++ i){
          ans = max(ans, query(a[i]));
      }
      cout << ans << '\n';
      return 0;
  }
  ```

  

# [<font color=#6495ED size=6>**D - The XOR Largest Pair**</font>](https://vjudge.net/contest/449699#problem/D)

* ## 题意

  在给定的 $N$ 个整数 $A1,A2,…,AN$ 中选出两个进行异或运算，得到的结果最大是多少？

* ## 思路

  $O(n)$遍历的$01Trie$

  ```cpp
  /*
   * @Author: NEFU AB_IN
   * @Date: 2021-08-27 10:11:04
   * @FilePath: \Vscode\ACM\Project\Trie\poj3764.cpp
   * @LastEditTime: 2021-08-27 10:32:58
   */
  #include<iostream>
  #include<vector>
  #include<cstring>
  #include<string.h>
  #include<cstdio>
  #include<climits>
  #include<cmath>
  #include<algorithm>
  #include<queue>
  #include<deque>
  #include<map>
  #include<set>
  #include<string>
  #include<stack>
  using namespace std;
  #define LL                          long long
  #define MP                          make_pair
  #define SZ(X)                       ((int)(X).size())
  #define IOS                         ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
  #define DEBUG(X)                    cout << #X << ": " << X << endl;
  typedef pair<int , int>             PII;
  
  namespace Trie01{
      const int N = 1e5 + 10;
      int son[N * 32][2], val[N * 32], idx;
      void init(){
          idx = 0;
          son[0][0] = son[0][1] = 0;
      }
      void insert(int x){
          int p = 0;
          for(int i = 31; i >= 0; -- i){
              int u = (x >> i) & 1;
              if(!son[p][u]){
                  son[p][u] = ++ idx;
                  p = son[p][u];
                  son[p][0] = son[p][1] = 0;
              }
              else p = son[p][u];
          }
          val[p] = x;
      }
      int query(int x){
          int p = 0;
          for(int i = 31; i >= 0; -- i){
              int u = (x >> i) & 1;
              if(son[p][u ^ 1]) p = son[p][u ^ 1];
              else p = son[p][u];
          }
          return val[p] ^ x;
      }
  }
  using namespace Trie01;
  int n, a[N];
  signed main()
  {
      cin >> n;
      for(int i = 1; i <= n; ++ i){
          cin >> a[i];
          insert(a[i]);
      }
      int ans = a[1] ^ a[2];
      for(int i = 1; i <= n; ++ i){
          ans = max(ans, query(a[i]));
      }
      cout << ans << '\n';
      return 0;
  }
  ```

  

