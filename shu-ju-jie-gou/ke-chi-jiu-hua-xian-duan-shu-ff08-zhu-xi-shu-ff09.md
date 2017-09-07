# 可持久化线段树

## Template

```cpp
namespace Snb_tree {
    const int N = 2e6 + 100;
    struct node {
        int l, r;
        int s;
    };
    struct Snb_tree {
        int rt[N];
        int sz;
        node ts[N];
        int left, right;
        void init(int a, int b) {
            MEM(rt, 0);
            MEM(ts, 0);
            sz = 0;
            left = a, right = b;
        }
        void _add(int &root, int old_rt, int l, int r, int x) {
            root = ++ sz;
            ts[root].l = ts[root].r = 0;
            ts[root].s = ts[old_rt].s + 1;
            if (l == r) return;
            int mid = (l + r) / 2;
            if (x > mid) _add(ts[root].r, ts[old_rt].r, mid + 1, r, x), ts[root].l = ts[old_rt].l;
            else _add(ts[root].l, ts[old_rt].l, l, mid, x), ts[root].r = ts[old_rt].r;
        }
        void add(int i, int x) {
            _add(rt[i], rt[i - 1], left, right, x);
        }
        int _query(int rtl, int rtr, int l, int r, int k) {
            if (l == r) return l;
            int t = ts[ts[rtr].l].s - ts[ts[rtl].l].s;
            int mid = (l + r) / 2;
            if (k <= t) return _query(ts[rtl].l, ts[rtr].l, l, mid, k);
            else return _query(ts[rtl].r, ts[rtr].r, mid + 1, r, k - t);
        }
        int query(int l, int r, int k) {
            return _query(rt[l - 1], rt[r], left, right, k);
        }
    };
}
Snb_tree::Snb_tree g;
```



