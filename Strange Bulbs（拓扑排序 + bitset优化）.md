Powered by:**NEFU AB_IN**

# <font color=#6495ED size=6>Strange Bulbs（拓扑排序 + bitset优化）</font>

* ## 题意

  有一个由小灯泡和电线连成的$DAG$，其中一号小灯泡的灯是亮着的，并且不会有边指向一号灯，每个灯泡上有一个开关，拨动开关会沿着电线一路改变灯泡的状态，即亮着的灯会熄灭，熄灭的灯则会亮起。状态改变只能顺着有向弧的方向传播，具体来说，如果 $u,v$ 之间有一个有向弧 $(u,v)$，拨动 $u$ 的开关时，$u$ 和 $v$ 的状态都会改变，但拨动 $v$ 的开关时，$u$ 的状态不会改变。

  如果要使所有灯熄灭，至少要拨动多少次开关。

* ## 思路

  * 首先，按照拓扑序的顺序进行波动开关，后面的开关不会影响前面的灯泡，那么拨到最后一定有解

  * 思路很简单，按照拓扑序依次进队列，首先此点应继承父节点的状态，并判断此点需不需要波动开关

    * 若此点被波及了**偶数**次，那么不需要波动，因为操作偶数次还是黑的
    * 若此点被波及了**奇数**次，需要波动

  * 由此可见这是一种$dp$的思想，但普通的动态规划无法合并得到影响这个灯泡的数量或集合，所以选择$bitset$是最佳的，**每个点维护一个**$bitset$，使用**位运算**来优化$dp$

  * [$bitset$学习博客]([c++ bitset类用法_Liam Q的专栏-CSDN博客](https://blog.csdn.net/qll125596718/article/details/6901935))

  * 具体怎么维护呢？

  * 举个例子

  * *6 7*

    *1 2*

    *1 3*

    *2 4*

    *3 6*

    *2 5*

    *5 6*

    *4 6*

  * ![Screenshot_20210820_152712](D:\1719827581\MobileFile\Screenshot_20210820_152712.jpg)

  * ![Screenshot_20210820_154129](D:\1719827581\MobileFile\Screenshot_20210820_154129.jpg)

  * ![Screenshot_20210820_153219](D:\1719827581\MobileFile\Screenshot_20210820_153219.jpg)

  * ![Screenshot_20210820_153606](D:\1719827581\MobileFile\Screenshot_20210820_153606.jpg)

  * ![Screenshot_20210820_153829](D:\1719827581\MobileFile\Screenshot_20210820_153829.jpg)

  * ![Screenshot_20210820_153907](D:\1719827581\MobileFile\Screenshot_20210820_153907.jpg)

  * ![Screenshot_20210820_153931](D:\1719827581\MobileFile\Screenshot_20210820_153931.jpg)

```cpp
/*
 * @Author: NEFU AB_IN
 * @Date: 2021-08-20 16:00:13
 * @FilePath: \Vscode\ACM\Project\bitset\StrangeBulbs.cpp
 * @LastEditTime: 2021-08-20 16:00:13
 */
#include<bits/stdc++.h>
using namespace std;
#define LL                          long long
#define MP                          make_pair
#define SZ(X)                       ((int)(X).size())
#define IOS                         ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define DEBUG(X)                    cout << #X << ": " << X << endl;
typedef pair<int , int>             PII;

const int N = 4e4 + 10;
struct Edge
{
    int u, v, ne;
}e[N << 2];
int h[N];
int cnt;
void add(int u, int v)
{
    e[cnt].u = u;
    e[cnt].v = v;
    e[cnt].ne = h[u];
    h[u] = cnt++;
}
void init(){
    memset(h, -1, sizeof(h));
    cnt = 0;
}
int n, m, u, v, ans;
int deg[N];
bitset <N> dp[N];

signed main()
{
    IOS;
    init();
    cin >> n >> m;
    for(int i = 1; i <= m; ++ i){
        cin >> u >> v;
        add(u, v);
        deg[v] ++;
    }
    queue <int> q;
    for(int i = 1; i <= n; ++ i){
        if(!deg[i]) {
            q.push(i);
            dp[i][i] = 1; //init
            ans ++;
        }
    }
    while(q.size()){
        int tp = q.front();
        q.pop();
        for(int i = h[tp]; ~i; i = e[i].ne){
            v = e[i].v;
            dp[v] |= dp[tp];
            if(!--deg[v]){
                q.push(v);
                if(dp[v].count() & 1){
                    dp[v][v] = 1;
                    ans ++;
                }
            }
        }
    }
    cout << ans << '\n';
    return 0;
}
```

完结。