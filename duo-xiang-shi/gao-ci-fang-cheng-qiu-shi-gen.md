---
plugins:
  - katex
---

# 高次方程求实根

求出形如 $$a[n] * x^n + a[n-1] * x^(n-1) + ... + a[1] * x + a[0]$$ 的方程的解

### 调用接口：

```cpp
equation(vector<double> coef,int n)
```

其中 $$ coef $$ 为 $$ a[0] \to a[n] , n $$ 最高次幂幂数

### 调用示范：

解方程$$x^2-2x+1=0$$:

```cpp
vector<double> test;
test.pb(1);
test.pb(-2);
test.pb(1);
test =  equation(test, 2);
for (int i = 0; i < SZ(test); i += 1) 
    printf("%.2f\n",test[i]);
```

### 模板：

```cpp
const double eps=1e-12;  
const double inf=1e+12;  
inline int sign(double x)  
{  
    return x<-eps?-1:x>eps;  
}  
inline double get(const vector<double> &coef,double x)  
{  
    double e=1,s=0;  
    for(int i=0;i<(int)coef.size();++i) s+=coef[i]*e,e*=x;  
    return s;  
}  
double find(const vector<double> &coef,int n,double lo,double hi)  
{  
    double sign_lo,sign_hi;  
    if((sign_lo=sign(get(coef,lo)))==0) return lo;  
    if((sign_hi=sign(get(coef,hi)))==0) return hi;  
    if(sign_lo*sign_hi>0) return inf;  
    for(int step=0;step<100&&hi-lo>eps;step++)  
    {  
        double m=(lo+hi)*.5;  
        int sign_mid=sign(get(coef,m));  
        if(sign_mid==0) return m;  
        if(sign_lo*sign_mid<0) hi=m;  
        else lo=m;  
    }  
    return (lo+hi)*.5;  
}  
vector<double> equation(vector<double> coef,int n)  
{  
    vector<double> ret;  
    if(n==1)  
    {  
        if(sign(coef[1])) ret.push_back(-coef[0]/coef[1]);  
        return ret;  
    }  
    vector<double> dcoef(n);  
    for(int i=0;i<n;i++) dcoef[i]=coef[i+1]*(i+1);  
    vector<double> droot=equation(dcoef,n-1);  
    droot.insert(droot.begin(),-inf);  
    droot.push_back(inf);  
    for(int i=0;i+1<(int)droot.size();i++)  
    {  
        double tmp=find(coef,n,droot[i],droot[i+1]);  
        if(tmp<inf) ret.push_back(tmp);  
    }  
    return ret;  
}
```



