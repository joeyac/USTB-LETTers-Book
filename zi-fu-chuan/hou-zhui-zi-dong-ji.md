# 后缀自动机

## Description

后缀自动机是一个可以识别一个字符串所有的后缀的自动机，同时这个自动机满足最简的性质，即结点数最少。

可以参考 陈立杰的ppt [https://wenku.baidu.com/view/fa02d3fff111f18582d05a81.html](https://wenku.baidu.com/view/fa02d3fff111f18582d05a81.html) 以及 [http://blog.csdn.net/qq\_35649707/article/details/66473069](http://blog.csdn.net/qq_35649707/article/details/66473069) 学习

设有一个字符串ababcc，现求出它的所以子串和其在原字符串中出现的位置：a{0,2}、b{1,3}、c{4,5}、ab{1,3}、ba{2}、bc{4}、cc{5}、aba{2}、bab{3}、abc{4}、bcc{5}、abab{3}、babc{4}、abcc{5}、ababc{4}、babcc{5}、ababcc{5}

将位置相同的子串分为同一组：

{0,2} - {a}

{1,3} - {b、ab}

{4,5} - {c}

{2} - {ba、aba}

{3} - {bab、abab}

{4} - {bc、abc、babc、ababc}

{5} - {cc、bcc、abcc、babcc、ababcc}

将其作为自动机的状态，再加一个初始状态，画出这个自动机，就是这样

```
                              a    +----------+     b     +-------------+
                 +----------------->{2}|ba aba+----------->{3}|bab abab |
                 |                 +----------+           +----+--------+
         b  +----+-----+              c                        |
  +--------->{1,3}|b ab+------------------------+              |
  |         +---^------+                        |             c|
  |             |                               |              |
  |            b|                               |              |
  |             |                               |              |
+-+--+   a  +---+----+                        +-v--------------v----+
|root+------>{0,2}|a |                        |{4}|bc abc babc ababc|
+-+--+      +--------+                        +----------+----------+
  |                                                      |
  |                                                     c|
  |                                                      |
  |      c  +-------+          c        +----------------v-----------+
  +--------->{4,5}|c+------------------->{5}|cc bcc abcc babcc ababcc|
            +-------+                   +----------------------------+
```

还有与之相关的一个parent树

```
                  +------------------+
                  |{0,1,2,3,4,5}|root|
                  +-------+----------+
                          |
      +---------------------------------------------------+
      |                   |                               |
      |                   |                               |
  +---v----+         +----v-----+                     +---v---+
  |{0,2}|a |         |{1,3}|b ab|                     |{4,5}|c|
  +---+----+         +---+------+                     +--+----+
      |                  |                               |
      |                  |                               |
      |                  |                     +---------+------------------+
      |                  |                     |                            |
      |                  |                     |                            |
+-----v----+      +------v------+   +----------v----------+    +------------v---------------+
|{2}|ba aba|      |{3}|bab abab |   |{4}|bc abc babc ababc|    |{5}|cc bcc abcc babcc ababcc|
+----------+      +-------------+   +---------------------+    +----------------------------+
```

## Template

```cpp
const int N = 2e5 + 100;
const int M = 26;
struct SAM {
    int ch[N][M];
    int par[N];
    int max_len[N];
    int root, last, sz;
    void init() {
        sz = 0;
        root = sz ++;
        mem(ch[root], -1);
        par[root] = -1;
        max_len[root] = 0;
        last = root;
    }
    void extend(int x) {
        int p = last, np = sz ++;
        mem(ch[np], -1);
        max_len[np] = max_len[p] + 1;
        while (p != -1 && ch[p][x] == -1) 
            ch[p][x] = np, p = par[p];
        if (p == -1) 
            par[np] = root;
        else {
            int q = ch[p][x];
            if (max_len[q] == max_len[p] + 1) 
                par[np] = q;
            else {
                int nq = sz ++;
                memcpy(ch[nq], ch[q], sizeof(ch[nq]));
                max_len[nq] = max_len[p] + 1;
                par[nq] = par[q];
                par[q] = nq;
                par[np] = nq;
                while (p != -1 && ch[p][x] == q) 
                    ch[p][x] = nq, p = par[p];
            }
        }
        last = np;
    }
    int l[N];
    int ra[N];
    void count() {
        for (int i = 0; i <= max_len[last]; i++) 
            l[i] = 0;
        for (int i = 0; i < sz; i++) 
            l[max_len[i]] ++;
        for (int i = 1; i <= max_len[last]; i++) 
            l[i] += l[i - 1];
        for (int i = sz - 1; i >= 0; i--) 
            ra[--l[max_len[i]]] = i;
        for (int i = 0; i < sz; i++) 
            printf("%d%c", ra[i], " \n"[i==sz-1]);
    }
    void debug() {
        for (int i = 0; i < sz; i++) 
            for (int j = 0; j < M; j++) if (ch[i][j] != -1) 
                printf("%d %c %d\n", i, j + 'a', ch[i][j]);
    }
} t;
```

## Problems

[http://acm.hdu.edu.cn/showproblem.php?pid=1403](http://acm.hdu.edu.cn/showproblem.php?pid=1403 "Longest Common Substring")

**思路 **对于串s和t，先对s建立后缀自动机，用后缀自动机读入串t，同时维护一下当前匹配的长度，如果失配就顺着parent树走，直到root或有相应的后继，同时用当前结点 的max\_len来更新当前匹配的长度。

**代码**

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
const int N = 2e5 + 100;
const int M = 26;
const int MOD = 1e9 + 7;

struct SAM {
    int ch[N][M];
    int par[N];
    int max_len[N];
    int root, last, sz;

    void init() {
        sz = 0;
        root = sz ++;
        mem(ch[root], -1);
        par[root] = -1;
        max_len[root] = 0;
        last = root;
    }

    void extend(int x) {
        int p = last, np = sz ++;
        mem(ch[np], -1);
        max_len[np] = max_len[p] + 1;
        while (p != -1 && ch[p][x] == -1) 
            ch[p][x] = np, p = par[p];
        if (p == -1) 
            par[np] = root;
        else {
            int q = ch[p][x];
            if (max_len[q] == max_len[p] + 1) 
                par[np] = q;
            else {
                int nq = sz ++;
                memcpy(ch[nq], ch[q], sizeof(ch[nq]));
                max_len[nq] = max_len[p] + 1;
                par[nq] = par[q];
                par[q] = nq;
                par[np] = nq;
                while (p != -1 && ch[p][x] == q) 
                    ch[p][x] = nq, p = par[p];
            }
        }
        last = np;
    }

    int l[N];
    int ra[N];
    void count() {
        for (int i = 0; i <= max_len[last]; i++) 
            l[i] = 0;
        for (int i = 0; i < sz; i++) 
            l[max_len[i]] ++;
        for (int i = 1; i <= max_len[last]; i++) 
            l[i] += l[i - 1];
        for (int i = sz - 1; i >= 0; i--) 
            ra[--l[max_len[i]]] = i;

        for (int i = 0; i < sz; i++) 
            printf("%d%c", ra[i], " \n"[i==sz-1]);
    }

    void debug() {
        for (int i = 0; i < sz; i++) 
            for (int j = 0; j < M; j++) if (ch[i][j] != -1) 
                printf("%d %c %d\n", i, j + 'a', ch[i][j]);
    }

    int lcs(char *s) {
        int len = 0, ret = 0;;
        int cur = root;
        for (int i = 0; s[i]; i++) {
            int x = s[i] - 'a';
            while (cur != root && ch[cur][x] == -1) 
                cur = par[cur], cmin(len, max_len[cur]);
            if (ch[cur][x] != -1) 
                cur = ch[cur][x], len ++;

            cmax(ret, len);
        }
        return ret;
    }
} t;

char str1[N], str2[N];

int main() {
# ifdef owly
    freopen("in.txt", "r", stdin);
    // freopen("ou.txt", "w", stdout);
# endif

    while (~scanf("%s%s", str1, str2)) {
        t.init();
        for (int i = 0; str1[i]; i++) 
            t.extend(str1[i] - 'a');

        printf("%d\n", t.lcs(str2));
    }

    return 0;
}
```



