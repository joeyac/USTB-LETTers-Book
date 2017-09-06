# 后缀数组

## Description

## Template

```cpp
int t1[N], t2[N], c[N];
bool cmp(int *r, int a, int b, int l) {
    return r[a] == r[b] && r[a + l] == r[b + l];
}

void da(int str[], int sa[], int rank[], int height[], int n, int m) {
    n ++;
    int i, j, p, *x = t1, *y = t2;
    for (i = 0; i < m; i++) c[i] = 0;
    for (i = 0; i < n; i++) c[x[i] = str[i]] ++;
    for (i = 1; i < m; i++) c[i] += c[i - 1];
    for (i = n - 1; i >= 0; i--) sa[--c[x[i]]] = i;
    for (j = 1; j <= n; j <<= 1) {
        p = 0;
        for (i = n - j; i < n; i++) y[p ++] = i;
        for (i = 0; i < n; i++) if (sa[i] >= j) y[p ++] = sa[i] - j;

        for (i = 0; i < m; i++) c[i] = 0;
        for (i = 0; i < n; i++) c[x[y[i]]] ++;
        for (i = 1; i < m; i++) c[i] += c[i - 1];
        for (i = n - 1; i >= 0; i--) sa[--c[x[y[i]]]] = y[i];

        swap(x, y);
        p = 1; x[sa[0]] = 0;
        for (i = 1; i < n; i++) 
            x[sa[i]] = cmp(y, sa[i - 1], sa[i], j) ? p - 1 : p ++;
        if (p >= n) break;
        m = p;
    }

    int k = 0;
    n -- ;
    for (i = 0; i <= n; i++) rank[sa[i]] = i;
    for (i = 0; i < n; i++) {
        if (k) k--;
        j = sa[rank[i] - 1];
        while (str[i + k] == str[j + k]) k ++;
        height[rank[i]] = k;
    }
}
```

## Problem

[http://acm.hdu.edu.cn/showproblem.php?pid=1403](http://acm.hdu.edu.cn/showproblem.php?pid=1403)

**思路** 模板题

```cpp
# include <bits/stdc++.h>
using namespace std;

# define x first
# define y second
# define sq(x) ((x)*(x))
# define cmin(x,y) ((x)=min(x,y))
# define cmax(x,y) ((x)=max(x,y))
# define mem(x,y) memset(x,y,sizeof(x))
# define show(x) cerr << #x << " = " << x << endl
# define time cerr<<"\n\n"<<"Time Spec = "<<clock()/1000.<<"s\n"

typedef double DB;
typedef long long LL;
typedef pair<int, int> Pr;
typedef unsigned long long U;

const LL INF = 0x3f3f3f3f;
const DB EPS = 1e-8;
const DB PI = acos(-1.0);
const int N = 2e5 + 100;
const int M = 26;
const int MOD = 1e9 + 7;

int t1[N], t2[N], c[N];
bool cmp(int *r, int a, int b, int l) {
    return r[a] == r[b] && r[a + l] == r[b + l];
}

void da(int str[], int sa[], int rank[], int height[], int n, int m) {
    n ++;
    int i, j, p, *x = t1, *y = t2;
    for (i = 0; i < m; i++) c[i] = 0;
    for (i = 0; i < n; i++) c[x[i] = str[i]] ++;
    for (i = 1; i < m; i++) c[i] += c[i - 1];
    for (i = n - 1; i >= 0; i--) sa[--c[x[i]]] = i;
    for (j = 1; j <= n; j <<= 1) {
        p = 0;
        for (i = n - j; i < n; i++) y[p ++] = i;
        for (i = 0; i < n; i++) if (sa[i] >= j) y[p ++] = sa[i] - j;

        for (i = 0; i < m; i++) c[i] = 0;
        for (i = 0; i < n; i++) c[x[y[i]]] ++;
        for (i = 1; i < m; i++) c[i] += c[i - 1];
        for (i = n - 1; i >= 0; i--) sa[--c[x[y[i]]]] = y[i];

        swap(x, y);
        p = 1; x[sa[0]] = 0;
        for (i = 1; i < n; i++) 
            x[sa[i]] = cmp(y, sa[i - 1], sa[i], j) ? p - 1 : p ++;
        if (p >= n) break;
        m = p;
    }

    int k = 0;
    n -- ;
    for (i = 0; i <= n; i++) rank[sa[i]] = i;
    for (i = 0; i < n; i++) {
        if (k) k--;
        j = sa[rank[i] - 1];
        while (str[i + k] == str[j + k]) k ++;
        height[rank[i]] = k;
    }
}

int sa[N], ra[N], he[N];
int str[N];

char s1[N], s2[N];
char s[N];

int main() {
# ifdef owly
    freopen("in.txt", "r", stdin);
    // freopen("ou.txt", "w", stdout);
# endif

    while (~scanf("%s%s", s1, s2)) {
        int len1 = strlen(s1);
        strcpy(s, s1);
        strcat(s, "#");
        strcat(s, s2);

        int len = strlen(s);
        for (int i = 0; i <= len; i++) 
            str[i] = s[i];

        da(str, sa, ra, he, len, 128);
        int ans = 0;
        for (int i = 0; i < len; i++) {
            if (min(sa[i], sa[i + 1]) < len1 && len1 < max(sa[i], sa[i + 1])) {
                cmax(ans, he[i + 1]);
            }
        }
        printf("%d\n", ans);
    }

    return 0;
}

```



