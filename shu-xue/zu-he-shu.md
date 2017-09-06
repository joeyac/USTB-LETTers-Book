---
plugins:
  - mathjax
---

# 组合数求解

**p很小，n,m很大的情况**

```cpp
#include <iostream>  
#include <string.h>  
#include <stdio.h>  
using namespace std;  
typedef long long LL;
const int NICO = 100000+10;
const int MOD  = 99991;
LL f[NICO];  
LL pow(LL a, LL n)
{
    LL ret = 1;
    while(n)
    {
        if(n & 1)
        {
            ret = (ret * a);
            ret %= MOD;
        }
        a = a * a % MOD;
        n >>= 1;
    }
    return ret;
}

LL Lucas(LL a,LL k) 
{  
    LL res = 1;  
    while(a && k)  
    {  
        LL a1 = a % MOD;
        LL b1 = k % MOD;  
        if(a1 < b1) return 0;
        res = res*f[a1]*pow(f[b1]*f[a1-b1]%MOD,MOD-2)%MOD;
        a /= MOD;  
        k /= MOD;  
    }  
    return res;  
}  

void init()  
{  
    f[0] = 1;  
    for(int i=1;i<=MOD;i++) 
    { 
        f[i] = f[i-1]*i%MOD;    
    }
}  

LL T, n;
int main()  
{  
    init();
    cout << Lucas(5,2) << endl;
}
```

**p很大，n和m都较小的情况**

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>
#include <cmath>
#include <iostream>
#include <algorithm>
using namespace std;
typedef long long LL;
const int P = 1000000007;
LL f[1000001], v[1000001];
LL rp(LL now, int k) 
{
    LL will = 1;
    for (; k; k >>= 1, now *= now, now %= P) 
    {
        if (k & 1) will *= now, will %= P;
    }
    return will;
}
LL C(int n, int m) 
{
    if(n < m) return 0;
    return f[n] * rp(f[m], P - 2) % P * rp(f[n - m], P - 2) % P;
}
void init()
{
    f[0] = 1; v[0] = 1;
    for (int i = 1; i <= 1000000; i++) //1e6以内的组合数
    {
        f[i] = f[i - 1] * i % P;
    }
}
int main() 
{
    init();
    int n, m;
    scanf("%d %d", &n, &m);
    printf("%lld\n", C(m, n));
}
```

**n和m都是1e9的情况**

找题目包含的其它性质

比如[http://acm.hdu.edu.cn/showproblem.php?pid=6084](http://acm.hdu.edu.cn/showproblem.php?pid=6084) 就是要求 $$C_{n-|S|+1}^{\frac{n-|S|}{2} + 1}$$

令$$x = \frac{n-|S|}{2} + 1$$$$，所以题目求 $$$$C_{2x-1}^x = \frac {C_{2x}^x} {2} = F(x) \frac {x + 1} { 2}$$，而 $$F(n) = \frac {2(2n-1)F(n-1)} {n+1}$$，就可以打一个大小为1e3的表出来，

然后直接递推算就可以了。

