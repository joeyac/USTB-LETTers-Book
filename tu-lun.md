```cpp
/*
HDU 4694
*/
# include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const int N = 200010;
struct EG { int to,before; } es[N];
int n,m,tot,ecnt;
int last[N],pos[N],idx[N],fa[N],sdom[N],idom[N];
int father[N],val[N];
vector<int> pre[N],bkt[N];

int findf(int p) {
    if(father[p]==p) return p;
    int r=findf(father[p]);
    if(sdom[val[father[p]]]<sdom[val[p]]) val[p] = val[father[p]];
    return father[p] = r;
}

inline int eval(int p) {
    findf(p);
    return val[p];
}

void dfs(int p) {
    idx[pos[p]=++tot]=p,sdom[p]=pos[p];
    for(int pt=last[p];pt;pt=es[pt].before) if(!pos[es[pt].to])
    dfs(es[pt].to),fa[es[pt].to]=p;
}

void work() {
    int p;
    dfs(n);                                                     // DFS 根节点，这里 n 是根节点
    for(int i=tot;i>=2;i--) {
        p=idx[i];
        for(int k:pre[p])
            if(pos[k])sdom[p]=min(sdom[p],sdom[eval(k)]);
        bkt[idx[sdom[p]]].push_back(p);
        int fp=fa[p];father[p]=fa[p];
        for(int v:bkt[fp]) {
            int u = eval(v);
            idom[v] = sdom[u]==sdom[v]?fp:u;
        }
        bkt[fp].clear();
    }
    for(int i = 2; i <= tot; i ++)
        p=idx[i],idom[p]=(idom[p]==idx[sdom[p]])?idom[p]:idom[idom[p]];
    for(int i = 2;i <= tot;i ++)
        p=idx[i],sdom[p]=idx[sdom[p]];
}

inline void link(int a,int b) {
    es[++ecnt].to=b,es[ecnt].before=last[a],last[a]=ecnt;
    pre[b].push_back(a);
}

void init(int n, int m) {
    tot=ecnt=0;
    for(int i=1;i<=n;i++)
        last[i]=pos[i]=0,father[i]=val[i]=i,pre[i].clear(),bkt[i].clear();
}

LL ans[N];
LL get_ans(int v) {
    if (ans[v]) return ans[v];
    if (v == n) return ans[v] = n;
    else return ans[v] = v + get_ans(idom[v]);
}

int main() {
# ifdef owly
    freopen("in.txt", "r", stdin);
# endif
    while (~scanf("%d%d", &n, &m)) {
        init(n, m);                                                  // n 是点数， m 是边数

        int a, b;
        for (int i = 0; i < m; i++) {
            scanf("%d%d", &a, &b);
            link(a, b);
        }
        work();
        for (int i = 1; i <= n; i++) ans[i] = 0;
        for (int i = 1; i <= n; i++) {
            printf("%lld%c", pos[i] ? get_ans(i) : 0, " \n"[i==n]);
        }
    }
    return 0;
}
```



