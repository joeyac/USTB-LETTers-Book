# AC 自动机

```cpp
struct Trie {
    int ch[N][2];
    int end[N];
    int fail[N];
    int sz;
    int root;
    int mark[M];
    int new_node() {
        ch[sz][0] = ch[sz][1] = -1;
        end[sz] = 0;
        return sz ++;
    }
    void init() {
        sz = 0;
        root = new_node();
    }
    void insert(char *s, int f) {
        int len = strlen(s);
        int now = root;
        for (int i = 0; i < len; i++) {
            int etr = s[i] - '0';
            if (ch[now][etr] == -1)
                ch[now][etr] = new_node();
            now = ch[now][etr];
        }
        end[now] = f;
        if (f != -1) 
            mark[f] = now;
    }
    queue<int> que;
    void build() {
        while (!que.empty()) 
            que.pop();
        fail[root] = root;
        for (int k = 0; k < 2; k++) 
            if (ch[root][k] == -1) 
                ch[root][k] = root;
            else {
                fail[ch[root][k]] = root;
                que.push(ch[root][k]);
            }
        while (!que.empty()) {
            int v = que.front(); que.pop();
            for (int k = 0; k < 2; k++) {
                if (ch[v][k] == -1) 
                    ch[v][k] = ch[fail[v]][k];
                else {
                    fail[ch[v][k]] = ch[fail[v]][k];
                    que.push(ch[v][k]);
                }
            }
        }
    }
    void debug() {
        for (int i = 0; i < sz; i++) {
            printf("id = %3d, fail = %3d, end = %3d, chi = [", i, fail[i], end[i]);
            for (int j = 0; j < 2; j++) 
                printf("%4d", ch[i][j]);
            printf("]\n");
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                printf("%d ", path[i][j]);
            }
            puts("");
        }
    }
} t;
```

## Problem

[http://acm.hdu.edu.cn/showproblem.php?pid=3247](http://acm.hdu.edu.cn/showproblem.php?pid=3247)

```cpp
# include <bits/stdc++.h>
using namespace std;
# define X first
# define Y second
# define sq(x) ((x)*(x))
# define Mem(x,y) memset(x,y,sizeof(x))
# define cMin(x,y) ((x)=((x)<(y)?(x):(y)))
# define cMax(x,y) ((x)=((x)>(y)?(x):(y)))
# define In(x1,y1,x2,y2) ((x2)<=(x1)&&(y1)<=(y2))
# define Dv(x1,y1,x2,y2) ((y1)<(x2)||(y2)<(x1))
# define show(x) cerr << #x << " = " << x << endl
# define time cerr<<"\n\n"<<"Time Spec = "<<clock()/1000.<<"s\n"
typedef double DB;
typedef long long LL;
typedef pair<int, int> Pr;
typedef unsigned long long U;
const LL INF = 1e9 + 100;
const DB EPS = 1e-8;
const int N = 6e4 + 100;
const int M = 12;
const int L = 1e5 + 100;
const int MOD = 998244353;
int n, m;
struct Trie {
    int ch[N][2];
    int end[N];
    int fail[N];
    int sz;
    int root;
    int mark[M];
    int new_node() {
        ch[sz][0] = ch[sz][1] = -1;
        end[sz] = 0;
        return sz ++;
    }
    void init() {
        sz = 0;
        root = new_node();
    }
    void insert(char *s, int f) {
        int len = strlen(s);
        int now = root;
        for (int i = 0; i < len; i++) {
            int etr = s[i] - '0';
            if (ch[now][etr] == -1)
                ch[now][etr] = new_node();
            now = ch[now][etr];
        }
        end[now] = f;
        if (f != -1) 
            mark[f] = now;
    }
    queue<int> que;
    void build() {
        while (!que.empty()) 
            que.pop();
        fail[root] = root;
        for (int k = 0; k < 2; k++) 
            if (ch[root][k] == -1) 
                ch[root][k] = root;
            else {
                fail[ch[root][k]] = root;
                que.push(ch[root][k]);
            }
        while (!que.empty()) {
            int v = que.front(); que.pop();
            for (int k = 0; k < 2; k++) {
                if (ch[v][k] == -1) 
                    ch[v][k] = ch[fail[v]][k];
                else {
                    fail[ch[v][k]] = ch[fail[v]][k];
                    que.push(ch[v][k]);
                }
            }
        }
        n = n + 1;
        mark[0] = 0;
        for (int i = 0; i <= n; i++) 
            bfs(i);
    }
    int path[M][M];
    int d[N];
    bool inque[N];
    void bfs(int s) {
        while (!que.empty()) 
            que.pop();
        for (int i = 0; i < sz; i++) 
            d[i] = INF, inque[i] = false;
        d[mark[s]] = 0;
        que.push(mark[s]);
        inque[mark[s]] = true;
        while (!que.empty()) {
            int v = que.front(); que.pop();
            for (int k = 0; k < 2; k++) {
                int u = ch[v][k];
                if (end[u] == -1) continue;
                if (d[u] > d[v] + 1) {
                    d[u] = d[v] + 1;
                    if (!inque[u]) 
                        que.push(u), inque[u] = true;
                }
            }
            // if (v != root) {
            //     if (d[fail[v]] > d[v]) {
            //         d[fail[v]] = d[v];
            //         if (!inque[fail[v]]) 
            //             que.push(fail[v]), inque[fail[v]] = true;
            //     }
            // }
            inque[v] = false;
        }
        for (int i = 0; i < n; i++) 
            path[s][i] = d[mark[i]];
    }
    int dp[1<<M][M];
    int get_ans() {
        for (int s = 0; s < (1 << n); s++) 
            for (int j = 0; j < n; j++) 
                dp[s][j] = INF;
        dp[1][0] = 0;
        for (int s = 0; s < (1 << n); s++) {
            for (int j = 0; j < n; j++) {
                for (int i = 0; i < n; i++) {
                    if ((s >> i) & 1) 
                        continue;
                    cMin(dp[s | (1 << i)][i], dp[s][j] + path[j][i]);
                }
            }
        }
        int ret = INF;
        int s = (1 << n) - 1;
        for (int i = 0; i < n; i++)
            cMin(ret, dp[s][i]);
        return ret;
    }
    void debug() {
        for (int i = 0; i < sz; i++) {
            printf("id = %3d, fail = %3d, end = %3d, chi = [", i, fail[i], end[i]);
            for (int j = 0; j < 2; j++) 
                printf("%4d", ch[i][j]);
            printf("]\n");
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                printf("%d ", path[i][j]);
            }
            puts("");
        }
    }
} t;
char s[L];
int main() {
# ifdef owly
    freopen("in.txt", "r", stdin);
    // freopen("ou.txt", "w", stdout);
# endif
    while (scanf("%d%d", &n, &m), n || m) {
        t.init();
        for (int i = 1; i <= n; i++) {
            scanf("%s", s);
            t.insert(s, i);
        }
        for (int i = 0; i < m; i++) {
            scanf("%s", s);
            t.insert(s, -1);
        }
        t.build();
        printf("%d\n", t.get_ans());
        // t.debug();
    }
    return 0;
}
```



