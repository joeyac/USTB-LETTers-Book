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





