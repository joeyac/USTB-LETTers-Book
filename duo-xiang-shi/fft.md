## Fast Fourier transform {#fast-fourier-transform-in-modulo-any-integer}

快速计算两个多项式的卷积，注意精度问题，在计算出来$$C[i]$$之后需要做如下处理： $$ans[i]=(int)(c[i]+0.5);$$

有必要的话，可以将var类型改为long double。

maxn应该设为大于n的最小的2^k次方的两倍。

## 模板\(crazyX\)

```cpp
#include<bits/stdc++.h>
using namespace std;

//           FFT           //
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

## 模板\(Anonymous\)

```cpp
#include <bits/stdc++.h>  
using namespace std;  
const int maxn = 2e6+233;  
#define DB double  
const DB pi = acos(-1);  

struct CP {  
    DB x, y; CP(){} inline CP(DB a, DB b):x(a),y(b){}  
    inline CP operator + (const CP&r) const { return CP(x + r.x, y + r.y); }  
    inline CP operator - (const CP&r) const { return CP(x - r.x, y - r.y); }  
    inline CP operator * (const CP&r) const { return CP(x*r.x-y*r.y, x*r.y+y*r.x); }  
    inline CP conj() { return CP(x, -y); }  
} a[maxn], b[maxn], t;  

int n, m;  
inline void Swap(CP&a, CP&b) { t = a; a = b; b = t; }  
const int BUF=8096000,OUT=8000000;  

char Buf[BUF],*buf=Buf,Out[OUT],*ou=Out;int Outn[64],Outcnt;  
inline int read() {int a;for(a=0;*buf<48;buf++);while(*buf>47)a=a*10+*buf++-48;return a;}  
inline void print(int x){ if(!x) *ou ++ = 48; else { for(Outcnt=0;x;x/=10) Outn[++Outcnt]=x%10+48; while(Outcnt)*ou++=Outn[Outcnt--]; } *ou++=' '; }  

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
int main() {  
    fread(Buf, 1, BUF, stdin);  
    n = read(); m = read();  
    for(int i = 0; i <= n; ++ i) a[i].x = read();  
    for(int i = 0; i <= m; ++ i) a[i].y = read();  
    for(m += n, n = 1; n <= m; n <<= 1);  
    FFT(a, n, 1); CP Q(0, -0.25);  
    for(int i = 0, j; i < n; ++ i) j = (n-i)&(n-1), b[i] = (a[i]*a[i]-(a[j]*a[j]).conj())*Q;  
    FFT(b, n,-1);  
    for(int i = 0; i <= m; ++ i) print(int(b[i].x+0.2));  
    fwrite(Out, 1, ou-Out, stdout);  
    return 0;  
}
```



