Powered by:**NEFU AB_IN**

# <font color=#6495ED size=6>[B 擅长解密的小红同学](https://ac.nowcoder.com/acm/contest/11214/B)</font>
* ## 题意

  ![image-20210907170217981](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210907170217981.png)

* ## 思路

  求期望也就是求尝试多少次能成功，**等价于**求这些数字有多少种排列组合能够成功

  求含有重复数字的总共的排列数，就是**多重集排列数**

  ![image-20210907170453300](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210907170453300.png)

* ## 代码

  ```cpp
  /*
   * @Author: NEFU AB_IN
   * @Date: 2021-08-27 21:52:36
   * @FilePath: \Vscode\ACM\NiuKe\2021.8.27\b.cpp
   * @LastEditTime: 2021-09-07 16:45:20
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
  
  LL qm (LL a, LL b, LL c){
      LL ret = 1 % c;
      while(b){
          if(b & 1)
              ret = ret * a % c;
          a = a * a % c;
          b = b >> 1;
      }
      return ret;
  }
  const LL mod = 1e9 + 7;
  LL mul(LL x, LL y) { return 1LL * x * y % mod; }
  LL dec(LL x, LL y) { return x >= y ? x - y : x + mod - y; }
  LL add(LL x, LL y) { return x + y >= mod ? x + y - mod : x + y; }
  LL pmod(LL x) { return (x + mod) % mod; }
  
  LL inv_fm(LL n, LL p) { return qm(n, p - 2, p); }
  
  const int N = 1e7 + 10;
  LL fac[N];
  signed main()
  {
      IOS;
      fac[0] = 1;
      for (int i = 1; i <= N; ++i)
      {
          fac[i] = mul(fac[i - 1], i);
      }
      LL fm = 1, fz = 0;
      for (int i = 1; i <= 10; ++i)
      {
          int x;
          cin >> x;
          fm = mul(fm, fac[x]);
          fz += x;
      }
      cout << mul(inv_fm(fm, mod), fac[fz]) << '\n';
      return 0;
  }
  ```

  

完结。