# LCA模板

$$ O(nlogn) $$ 预处理之后$$ O (1) $$ 求树上两个点的祖先节点

### 模板一（不带距离查询只查询LCA）

```cpp
typedef pair<int,int> P;
vector<int> g[maxn], sp;
int dep[maxn], dfn[maxn], lg[2*maxn];
P dp[21][2*maxn];

void dfs(int u,int fa){
	dep[u]=dep[fa]+1;
	dfn[u]=sp.size();
	sp.pb(u);
	for(auto v: g[u]){
		if(v==fa)continue;
		dfs(v,u);
		sp.pb(u);
	}
}
void lca_init(){
	int n=SZ(sp);
	for (int i = 1, j = 0; i <= n; i += 1){
		if(i == (1 << (j+1))) j++;
		lg[i] = j;
	}
	for (int i = 0; i < n; i += 1)
		dp[0][i] = {dfn[sp[i]], sp[i]};
	
	for (int j = 1; (1 << j) <= n; j += 1)
		for (int i = 0; i + (1 << j) - 1 < n; i += 1)
			dp[j][i] = min(dp[j-1][i], dp[j-1][i+(1<<(j-1))]);
}
int lca(int u,int v){
	int l = dfn[u], r = dfn[v];
	if(l > r)swap(l , r);
	int j = lg[r - l + 1];
	return min(dp[j][l], dp[j][r - (1<<j) + 1]).se;
}

// 调用示范：
dfs(1, 0);
lca_init();
printf("%d", lca(1, 2));

//注意： 多组输入记得清空图的vector
```

### 模板二（预处理查询树上两点间距离）

```cpp
int dep[maxn], dist[maxn], fa[maxn][20];
void DFS(int u, int _dist, int _dep, int _fa) {
    dist[u] = _dist; dep[u] = _dep; fa[u][0] = _fa;
    for(int i = 0; i < SZ(g[u]); i++) {
        int to = g[u][i].fi, w = g[u][i].se;
        if(to == u || to == _fa) continue;
        DFS(to, _dist + w, _dep + 1, u);
    }
}
void presolve() {
    clr(fa, -1);
    for (int i = 1; i <= n; i++) {
        if(fa[i][0] == -1) DFS(i, 0, 0, i);
    }

    for(int i = 1; i < 20; i++) {
        for(int j = 1; j <= n; j++) {
            fa[j][i] = fa[fa[j][i - 1]][i - 1];
        }
    }
}
int LCA(int u, int v) {
    while(dep[u] != dep[v]) {
        if(dep[u] < dep[v]) swap(u, v);
        int d = dep[u] - dep[v];
        for(int i = 0; i < 20; i++) {
            if(d >> i & 1) u = fa[u][i];
        }
    }
    if(u == v) return u;
    for(int i = 20 - 1; i >= 0; i--) {
        if(fa[u][i] != fa[v][i]) {
            u = fa[u][i];
            v = fa[v][i];
        }
    }
    return fa[u][0];
}
int query(int u, int v) {
    return dist[u] + dist[v] - dist[LCA(u, v)] * 2;
}

//调用：
presolve();
//注意： 这个模板可以处理多棵不连通树的情况

```



