# 组合数求解

**对于p很小，n,m很大的情况**

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

\*\*对于

