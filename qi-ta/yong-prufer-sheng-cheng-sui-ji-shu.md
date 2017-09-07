```cpp
# include <bits/stdc++.h>
using namespace std;
# define Mem(x,y) memset(x,y,sizeof(x))
typedef pair<int, int> P;
const int N = 1e6 + 100;
int prufer[N], size;
void random_prufer(int size) {
    srand(time(NULL));
    for (int i = 0; i < size - 2; i++) {
        prufer[i] = rand() % size;
    }
}
vector<P> edges;
int prufer_num[N];
set<int> ac;
void gen_edges(int size) {
    Mem(prufer_num, 0);
    ac.clear();
    edges.clear();
    for (int i = 0; i < size - 2; i++) {
        prufer_num[prufer[i]] ++;
    }
    for (int i = 0; i < size; i++) {
        if (prufer_num[i] == 0) ac.insert(i);
    }
    for (int i = 0; i < size - 2; i++) {
        int u = prufer[i];
        int v = *ac.begin();
        ac.erase(ac.begin());
        prufer_num[u] --;
        if (prufer_num[u] == 0) {
            ac.insert(u);
        }
        edges.push_back(P(u, v));
    }
    int u = *ac.begin();
    int v = *ac.rbegin();
    edges.push_back(P(u, v));
}
vector<int> g[N];
int fa[N];
void dfs(int v, int f) {
    fa[v] = f;
    for (int i = 0; i < g[v].size(); i++) {
        if (g[v][i] != f) {
            dfs(g[v][i], v);
        }
    }
}
void build_tree(int size) {
    for (int i = 0; i < size; i++) {
        g[i].clear();
    }
    for (int i = 0; i < size - 1; i++) {
        g[edges[i].first].push_back(edges[i].second);
        g[edges[i].second].push_back(edges[i].first);
    }
    int root = 0;                   // change root at it.
    dfs(root, 0);
}
void solve(int n) {
    random_prufer(n);
    gen_edges(n);
    build_tree(n);
    /*
    fa[]: 0...n-1 's father, root's father is 0, 0 is the root initially.
    edges[]: the n-1 edges
    */
    for (int i = 1; i < n; i++) {
        printf("%d\n", fa[i]);
    }
    for (int i = 0; i < n - 1; i++) {
        printf("%d %d\n", edges[i].first, edges[i].second);
    }
}
int main(void) {
    freopen("ou.txt", "w", stdout);
    int n = 11;
    solve(n);         // n is the size of the tree. 
    return 0;
}
```



