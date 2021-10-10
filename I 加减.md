Powered by:**NEFU AB_IN**

# <font color=#6495ED size=6>[I 加减](https://ac.nowcoder.com/acm/contest/11214/I)</font>

* ## 题意

  一个长度为 $n$的数组。她每次操作可以让某个数加 $1$ 或者某个数减$1$ ，最多能进行$k$次操作，希望操作结束后，该数组出现次数最多的元素次数尽可能多，求最多的元素次数

* ## 思路

  * 显然优先变化尽量接近的数。所以可以先对数组进行排序

  * 来自官方题解的引理

    > 引理：一个数组每次操作可以让一个数加一或者减一，使所有数都相等的操作最小次数的方式是所有数都变成中位数（或两个中间的数的任意一个，若共有偶数个数）。

  * ![image-20210907211656499](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210907211656499.png)

  * 那么我们就只需要求出一个**最长**为$x$的区间，满足令其区间内的数变成**区间的中位数**的次数不超过$k$即可

  * 可以看出$x$具有二分性，那么就向右查找$x$即可，二分时可以用前缀和进行优化，分别统计中位数左边的和还有右边的和

* ## 代码

  ```cpp
  /*
   * @Author: NEFU AB_IN
   * @Date: 2021-09-07 20:27:13
   * @FilePath: \Vscode\ACM\NiuKe\2021.8.27\I.cpp
   * @LastEditTime: 2021-09-07 21:09:08
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
  
  const int N = 1e5 + 10;
  LL a[N], sum[N];
  LL n, k, m;
  bool check(int x)
  {
      for (int i = 1; i + x - 1 <= n; ++i)
      {
          int j = i + x - 1;
          m = i + j >> 1; //midian
          LL left = sum[m - 1] - sum[i - 1];
          LL right = sum[j] - sum[m];
          if (abs(left - (m - i) * a[m]) + abs(right - (j - m) * a[m]) <= k)
          {
              return true;
          }
      }
      return false;
  }
  signed main()
  {
      IOS;
      cin >> n >> k;
      for (int i = 1; i <= n; ++i)
      {
          cin >> a[i];
      }
      sort(a + 1, a + 1 + n);
      for (int i = 1; i <= n; ++i)
      {
          sum[i] = sum[i - 1] + a[i];
      }
      int l = 1, r = n;
      while (l < r)
      {
          int mid = l + r + 1 >> 1;
          if (check(mid))
              l = mid;
          else
              r = mid - 1;
      }
      cout << l << '\n';
      return 0;
  }
  ```

  

