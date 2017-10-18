# MTT

在[icpc-camp](https://post.icpc-camp.org/d/408-fft)中看到的黑科技，发文者也提供了[github链接](https://github.com/fotile96/Z_m-PolyMult)，里面说的挺详细的，测试了一下，正确性是有保证的，但是速度还是稍微有点慢，然后在北航TJZ队伍的[wiki](https://wiki-three-investigators.icpc-camp.org/code library#fast-fourier-transform-in-modulo-any-integer)中找到了一份Fast Fourier transform in modulo any integer板子……

模板对应的题目：[a times b](https://dmoj.ca/problem/atimesb)

本来hdu上也有一道大数的a\*b，但是数据太小，甚至说java直接bigInteger可以直接过，参考价值不大，这个OJ上的这道题a\*b给了2s时限，长度&lt;=10^6，经过测试，直接使用c++ complex的板子会T在最后几组，使用我自己（crazyX）的FFT板子可以很快通过，使用github上的MTT板子会卡在最后一组，使用TJZ的板子最后一组大概跑1.6s\(自己的FFT是0.8s\)，非常惊人，因为这是模任意整数的板子……

update: 主要是这个博客中的算法实现：[FFT黑科技](http://blog.csdn.net/samjia2000/article/details/65661468)

这个优化也可以用到多项式求逆等过程中

## 模板（skywalkert）

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef double DB;

const int maxn = 1e6 + 7;
const int maxLen = 21, maxm = 1 << maxLen | 1;
const ll maxv = 469762049 - 1; // 1e14, 1e15
const DB pi = acos(-1.0); // double is enough
struct cp {
    DB r, i;
    cp() {}
    cp(DB r, DB i) : r(r), i(i) {}
    cp operator + (cp const &t) const { return cp(r + t.r, i + t.i); }
    cp operator - (cp const &t) const { return cp(r - t.r, i - t.i); }
    cp operator * (cp const &t) const { return cp(r * t.r - i * t.i, r * t.i + i * t.r); }
    cp conj() { return cp(r, -i); }
} w[maxm];
ll mod = 469762049, nlim, sp, msk;
void FFT_init() {
    for(int i = 0, ilim = 1 << maxLen; i < ilim; ++i) {
        int j = i, k = ilim >> 1; // 2 pi / ilim
        for( ; !(j & 1) && !(k & 1); j >>= 1, k >>= 1);
        w[i] = cp(cos(pi / k * j), sin(pi / k * j));
    }
    nlim = std::min(maxv / (mod - 1) / (mod - 1), maxn - 1LL);
    for(sp = 1; 1 << (sp << 1) < mod; ++sp);
    msk = (1 << sp) - 1;
}
void FFT(int n, cp a[], int flag) {
    static int bitLen = 0, bitRev[maxm] = {};
    if(n != (1 << bitLen)) {
        for(bitLen = 0; 1 << bitLen < n; ++bitLen);
        for(int i = 1; i < n; ++i)
            bitRev[i] = (bitRev[i >> 1] >> 1) | ((i & 1) << (bitLen - 1));
    }
    for(int i = 0; i < n; ++i)
        if(i < bitRev[i])
            std::swap(a[i], a[bitRev[i]]);
    for(int i = 1, d = 1; d < n; ++i, d <<= 1)
        for(int j = 0; j < n; j += d << 1)
            for(int k = 0; k < d; ++k) {
                cp &AL = a[j + k], &AH = a[j + k + d];
                cp TP = w[k << (maxLen - i)] * AH;
                AH = AL - TP, AL = AL + TP;
            }
    if(flag != -1)
        return;
    std::reverse(a + 1, a + n);
    for(int i = 0; i < n; ++i) {
        a[i].r /= n;
        a[i].i /= n;
    }
}
void polyMul(int aLen, int a[], int bLen, int b[], int c[]) { // c not in {a, b}
    static cp A[maxm], B[maxm], C[maxm], D[maxm];
    int len, cLen = aLen + bLen - 1; // optional: parameter
    for(len = 1; len < aLen + bLen - 1; len <<= 1);
    if(std::min(aLen, bLen) <= nlim) {
        for(int i = 0; i < len; ++i)
            A[i] = cp(i < aLen ? a[i] : 0, i < bLen ? b[i] : 0);
        FFT(len, A, 1);
        cp tr(0, -0.25);
        for(int i = 0, j; i < len; ++i)
            j = (len - i) & (len - 1), B[i] = (A[i] * A[i] - (A[j] * A[j]).conj()) * tr;
        FFT(len, B, -1);
        for(int i = 0; i < cLen; ++i) c[i] = (ll)(B[i].r + 0.5) % mod;
        return;
    } // if min(aLen, bLen) * mod <= maxv
    for(int i = 0; i < len; ++i) {
        A[i] = i < aLen ? cp(a[i] & msk, a[i] >> sp) : cp(0, 0);
        B[i] = i < bLen ? cp(b[i] & msk, b[i] >> sp) : cp(0, 0);
    }
    FFT(len, A, 1), FFT(len, B, 1);
    cp trL(0.5, 0), trH(0, -0.5), tr(0, 1);
    for(int i = 0, j; i < len; ++i) {
        j = (len - i) & (len - 1);
        cp AL = (A[i] + A[j].conj()) * trL;
        cp AH = (A[i] - A[j].conj()) * trH;
        cp BL = (B[i] + B[j].conj()) * trL;
        cp BH = (B[i] - B[j].conj()) * trH;
        C[i] = AL * (BL + BH * tr);
        D[i] = AH * (BL + BH * tr);
    }
    FFT(len, C, -1), FFT(len, D, -1);
    for(int i = 0; i < cLen; ++i) {
        int v11 = (ll)(C[i].r + 0.5) % mod, v12 = (ll)(C[i].i + 0.5) % mod;
        int v21 = (ll)(D[i].r + 0.5) % mod, v22 = (ll)(D[i].i + 0.5) % mod;
        c[i] = (((((ll)v22 << sp) + v12 + v21) << sp) + v11) % mod;
    }
}

char numA[maxn], numB[maxn];
int a[maxm], b[maxm], ans[maxm];
int main() {
    FFT_init();

    while (scanf("%s%s",numA,numB)!=EOF)
    {
        int lenA=strlen(numA),lenB=strlen(numB);
        for (int i = 0; i < lenA; i += 1)a[i]=numA[lenA-1-i]-'0';
        for (int i = 0; i < lenB; i += 1)b[i]=numB[lenB-1-i]-'0';
        int len = 1;
        while (len < lenA || len < lenB) len <<= 1;
        len = lenA + lenB - 1;

//        for (int i = 0; i < lenA; i += 1) printf("%d ", a[i]);
//        puts("");
//        for (int i = 0; i < lenB; i += 1) printf("%d ", b[i]);
//        puts("");

        polyMul(lenA, a, lenB, b, ans);
//        for (int i = 0; i < len; i++) cout << ans[i] << endl;
//        continue;

        for (int i = 0; i < len-1; i += 1){
            ans[i+1]+=ans[i]/10;
            ans[i]%=10;
        }
        bool flag=false;
        for (int i = len-1; i >= 0; i += -1){
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



