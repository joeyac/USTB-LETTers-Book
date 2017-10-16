## Fast Fourier transform {#fast-fourier-transform-in-modulo-any-integer}

快速计算两个多项式的卷积，注意精度问题，在计算出来$$C[i]$$之后需要做如下处理： $$ans[i]=(int)(c[i]+0.5);$$

有必要的话，可以将var类型改为long double。

maxn应该设为大于n的最小的2^k次方的两倍。

## 模板2.0（crazyX）

常数巨小的好板子……

题目：[a times b](https://dmoj.ca/problem/atimesb)

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn=1e6+7;
const int maxN=(1 << 21) + 7;
typedef double DB;
const DB pi = acos(-1);
//           FFT           //
struct CP {  
    DB x, y; CP(){} inline CP(DB a, DB b):x(a),y(b){}  
    inline CP operator + (const CP&r) const { return CP(x + r.x, y + r.y); }  
    inline CP operator - (const CP&r) const { return CP(x - r.x, y - r.y); }  
    inline CP operator * (const CP&r) const { return CP(x*r.x-y*r.y, x*r.y+y*r.x); }  
    inline CP conj() { return CP(x, -y); }  
} A[maxN], B[maxN], t;

inline void Swap(CP&a, CP&b) { t = a; a = b; b = t; }

void FFT(CP*a, int n, int f) {  
    int i, j, k;  
    for(i = j = 0; i < n; ++ i) {  
        if(i > j) Swap(a[i], a[j]);  
        for(k = n>>1; (j ^= k) < k; k >>= 1);
    }  
    for(i = 1; i < n; i <<= 1) {  
        CP wn(cos(pi/i), f*sin(pi/i));  
        for(j = 0; j < n; j += i<<1) {  
            CP w(1, 0);  
            for(k = 0; k < i; ++ k, w=w*wn) {  
                CP x = a[j+k], y = w*a[i+j+k];  
                a[j+k] = x+y; a[i+j+k] = x-y;  
            }  
        }  
    }  
    if(-1 == f) for(i = 0; i < n; ++ i) a[i].x /= n;  
}

int polyMul(int a[], int alen, int b[], int blen, int c[]) {// c not in {a, b}
    int len, clen = alen + blen; // optional: parameter
    for(len = 1; len <= alen + blen; len <<= 1);

    for(int i = 0; i < len; i++) {
        A[i].x = i < alen ? a[i] : 0;
        A[i].y = i < blen ? b[i] : 0;
    }

    FFT(A, len, 1); CP Q(0, -0.25);
    for(int i = 0, j; i < len; i++)
        j = (len - i) & (len - 1),
        B[i] = (A[i] * A[i] - (A[j] * A[j]).conj() ) * Q;
    FFT(B, len, -1);

    for(int i = 0; i < clen; i++) c[i] = (int)(B[i].x + 0.5);

    return clen;
}
//           FFT           //

char s1[maxn], s2[maxn];
int ans[maxN], a[maxn], b[maxn], n, m;

//Main Program
int main()
{
    while (scanf("%s%s",s1, s2) != EOF) {
        n=strlen(s1); m=strlen(s2);
        for (int i = 0; i < n; i += 1)a[i] = s1[n-1-i] - '0';
        for (int i = 0; i < m; i += 1)b[i] = s2[m-1-i] - '0';

        int len = polyMul(a, n, b, m, ans);


        for (int i = 0; i < len; i += 1){
            ans[i+1]+=ans[i]/10;
            ans[i]%=10;
        }
        bool flag=false;
        for (int i = len - 1; i >= 0; i += -1){
            if(flag||ans[i]){
                flag=true;
                printf("%d",ans[i]);
            }
        }
        if(!flag)printf("%d",0);
        printf("\n");
    }
    return 0;
}
```

## 模板\(crazyX\)

常数有一些，不过基本够用，不会被卡

```cpp
#include<bits/stdc++.h>
using namespace std;

//   ，        FFT           //
typedef double var;
//typedef long double var;

const int maxn = 1 << 18; // maxn=1;while(maxn<n)maxn<<=1; maxn<<=1;
const var PI = acos(-1);
//复数类
struct Virt
{
    double r,i;
    Virt(var r=0.0,var i=0.0):r(r),i(i){}
    friend Virt operator - (const Virt &a, const Virt &b) { return Virt(a.r - b.r, a.i - b.i); }
    friend Virt operator + (const Virt &a, const Virt &b) { return Virt(a.r + b.r, a.i + b.i); }
    friend Virt operator * (const Virt &a, const Virt &b) { return Virt(a.r * b.r - a.i * b.i, a.r * b.i + a.i * b.r); }
}FA[maxn], FB[maxn];

inline void Rader(Virt F[], int n){
    int j=n>>1;
    for (int i = 1; i < n-1; i += 1){
        if(i<j)swap(F[i],F[j]);
        int k=n>>1;
        while(j>=k)j-=k,k>>=1;
        if(j<k)j+=k;
    }
}

inline void FFT(Virt F[],int n,int flag){
    Rader(F,n);
    for (int h = 2; h <= n; h <<= 1){
        Virt wn(cos(flag * 2 * PI / h), sin(flag * 2 * PI / h));
        for (int i = 0; i < n; i += h){
            Virt w(1,0);
            for (int j = i; j < i+h/2; j += 1){
                Virt u = F[j];
                Virt v = w * F[j + h / 2];
                F[j] = u + v;
                F[j + h / 2 ] = u - v;
                w = w* wn;
            }
        }
    }
    if(flag == -1)  for(int i = 0; i < n; i++)  F[i].r /=(double)n;
}

//  求a,b的卷积
inline int Convolution(var a[], int n, var b[], int m, var c[])
{
    int i, len = 1;
    while(len < 2 * n || len < 2 * m)   len <<= 1;
    for(i = 0; i < n; i++)  FA[i] = Virt(a[i]);
    for(; i < len; i++) FA[i] = Virt();
    for(i = 0; i < m; i++)  FB[i] = Virt(b[i]);
    for(; i < len; i++) FB[i] = Virt();

    FFT(FA, len, 1);    FFT(FB, len, 1);
    for(i = 0; i < len; i++)    FA[i] = FA[i] * FB[i];
    FFT(FA, len, -1);

    for(i = 0; i < len; i++)    c[i] = FA[i].r;
    return len;
}
//           FFT           //

int main() {
    return 0;
}
```

## 



