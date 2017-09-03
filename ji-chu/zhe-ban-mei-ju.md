# 折半枚举

## Description

我觉得是对于一些枚举量，它们之间有相互关系，以及于枚举其中一部分之后，对于后面的部分加了限制可以进行快速的操作。

## Problems

[https://www.nowcoder.com/acm/contest/submit/eeeb0f3b7d994c39b7c74fd1f4a05648?ACMContestId=1&tagId=4](https://www.nowcoder.com/acm/contest/submit/eeeb0f3b7d994c39b7c74fd1f4a05648?ACMContestId=1&tagId=4 "选择困难症")

**思路 **给 k\( &lt;= 6\) 类物品，每类物品有最多 100 个，每个物品有一个价值；求有多少种选择方案使得选到的物品价值总和大于 m，其中对于一类物品，要么选一个，要么不选。可以将前 3 类的物品的所有的选取方案用一个有序的数组维护起来，再枚举之后的几类物品，对于一个枚举的结果，会得到一个至少还要获得的目标价值，用二分搜索就可以得到满足要求的价值的预处理好的方案数。

```
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
const int N = 2e6 + 100;
const int M = 110;
const int MOD = 1e9 + 7;

int a[10][M], k, m;
int num[10];

int b[N], cnt;
void init_map(int i, LL val) {
    if (i == k || i == 3)
        b[cnt ++] = val;
    else {
        for (int j = 0; j < num[i]; j++)
            init_map(i + 1, val + a[i][j]);
    }
}

vector<int> v, tim;
LL dfs(int i, LL val) {
    if (i == k) {
        int tmp = upper_bound(v.begin(), v.end(), m - val) - v.begin();
        if (tmp == 0)
            return cnt;
        return cnt - tim[tmp - 1];
    }

    LL ret = 0;
    for (int j = 0; j < num[i]; j++) {
        ret += dfs(i + 1, val + a[i][j]);
    }
    return ret;
}


int main() {
# ifdef owly
    freopen("in.txt", "r", stdin);
    // freopen("ou.txt", "w", stdout);
# endif

    while (~scanf("%d%d", &k, &m)) {
        for (int i = 0; i < k; i++) {
            scanf("%d", &num[i]);
            for (int j = 0; j < num[i]; j++)
                scanf("%d", &a[i][j]);
            a[i][num[i] ++] = 0;
        }

        v.clear();
        tim.clear();
        cnt = 0;
        if (k <= 3) {
            init_map(0, 0);
            LL ans = 0;
            for (int i = 0; i < cnt; i++)
                if (b[i] > m)
                    ans ++;
            printf("%lld\n", ans);
        } else {
            init_map(0, 0);
            sort(b, b + cnt);
            int i, j;
            for (i = 0, j = 1; i < cnt - 1; i++) {
                if (b[i] != b[i + 1]) {
                    v.push_back(b[i]);
                    tim.push_back(j);
                    j = 1;
                } else
                    j ++;
            }
            v.push_back(b[cnt - 1]);
            tim.push_back(j);
            for (int i = 1; i < (int)v.size(); i++)
                tim[i] += tim[i - 1];

            printf("%lld\n", dfs(3, 0));
        }
    }

    return 0;
}
```



