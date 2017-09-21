# 强连通分量

在有向图G中，如果两个顶点间至少存在一条路径，称两个顶点强连通\(strongly connected\)。如果有向图G的每两个顶点都强连通，称G是一个强连通图。非强连通图有向图的极大强连通子图，称为强连通分量\(strongly connected component\)。



## Tarjan算法

### 一、介绍

直接根据定义，用双向遍历取交集的方法求强连通分量，时间复杂度为O\(N^2+M\)。更好的方法是Kosaraju算法或Tarjan算法，两者的时间复杂度都是O\(N+M\)。

Tarjan算法是基于对图深度优先搜索的算法，每个强连通分量为搜索树中的一棵子树。搜索时，把当前搜索树中未处理的节点加入一个堆栈，回溯时可以判断栈顶到栈中的节点是否为一个强连通分量。

定义DFN\(u\)为节点u搜索被搜索到时的次序编号\(时间戳\)，Low\(u\)为u或u的子树能够追溯到的最早的栈中节点的次序号。由定义可以得出，

```cpp
Low(u)=Min
{
   DFN(u),（第一次被搜索到）
   Low(v),(u,v)为树枝边，u为v的父节点
   DFN(v),(u,v)为指向栈中节点的后向边(非横叉边)（即沿着u搜到v时，发现v已经被搜过了，也就是说形成了一个环）
}
```

当DFN\(u\)=Low\(u\)时，以u为根的搜索子树上所有节点是一个强连通分量。\(实质上就是这个点的整个子树回溯完了，并且下面都是死路\)

可以发现，运行Tarjan算法的过程中，每个顶点都被访问了一次，且只进出了一次堆栈，每条边也只被访问了一次，所以该算法的时间复杂度为O\(N+M\)。

### 二、代码模板

```cpp
const int maxn = 1e3 + 7;

int n, tp, tim, num, sta[maxn]; //模拟栈
int scc[maxn], low[maxn], dfn[maxn];
vector<int> g[maxn]; //邻接表

void dfs(int x) {
	dfn[x] = low[x] = ++tim;
	sta[++tp] = x;
	for (auto y : g[x])
		if (!dfn[y])
			dfs(y), low[x] = min(low[x], low[y]);
		else if (!scc[y])
			low[x] = min(low[x], dfn[y]);
	if (dfn[x] == low[x]) {
		int cur = -1; num++;
		while (cur != x)
			scc[cur = sta[tp --]] = num;
	}
}

void tarjan(int N) {
	for (int i = 0; i < N; i += 1)
		dfn[i] = low[i] = scc[i] = 0;
	tp = tim = num = 0;

	for (int i = 0; i < N; i += 1) if (!dfn[i]) dfs(i);
}
```



## 应用\(TODO\)

### 一、2-SAT

在2-SAT求解问题中，可以通过dfs染色的方式来确定是否有解（即《挑战程序设计竞赛》中所给出的代码），实际上这样的效率相对较低，注意到2-SAT问题中，存在对称性，也就是说如果A\(true\)-&gt;A\(false\)那么也会有A\(false\)-&gt;A\(true\)，因此我们只需要把原图进行强连通分量缩点，判断若干个变量对应的两个点是否在同一个强联通分量内即可判断是否有解。

### 二、\(TODO\)







