# KMP

## Template

```cpp
int nxt[N];
int g[N][M];
int c[N][M];
void kmp(char *s, char *t) {
    Mem(nxt, 0);
    int len_s = strlen(s);
    int len_t = strlen(t);
    for (int i = 0; i < len_t; i++) {
        int j = i;
        while (j > 0) {
            j = nxt[j];
            if (t[i] == t[j]) {
                nxt[i + 1] = j + 1;
                break;
            }
        }
    }
    for (int i = 0; i < len_t; i++) {
        for (int j = 0; j < M; j++) {
            c[i][j] = 0;
            int k = i;
            while (k > 0 && t[k] != j + 'a') {
                k = nxt[k];
            }
            if (t[k] == j + 'a') 
                k ++;
            if (k == len_t) 
                k = nxt[k], c[i][j] = 1;
            g[i][j] = k;
        }
    }
    for (int i = 0, j = 0; i < len_s; i++) {
        if (c[j][s[i] - 'a']) 
            show(i - j);
        j = g[j][s[i] - 'a'];
    }
}
```

# 扩展KMP

```cpp
#include<iostream>
#include<string>
using namespace std;
# define Min(x,y) ((x)<(y)?(x):(y))
/* 求解extend[] */
void GetExtend(string S, string T, int extend[], int next[])
{
    int a;
    int p;             //记录匹配成功的字符的最远位置p，及起始位置a
    int s_len = S.size();
    int t_len = T.size();
    int m_len = Min(s_len, t_len);
    for (extend[0] = 0; S[extend[0]] == T[extend[0]] && extend[0] < m_len; extend[0] ++);
    for (int i = 1, j = -1; i < s_len; i++, j--)  //j即等于p与i的距离，其作用是判断i是否大于p（如果j<0，则i大于p）
    {
        if (j < 0 || i + next[i - a] >= p)  //i大于p（其实j最小只可以到-1，j<0的写法方便读者理解程序），
        {                                   //或者可以继续比较（之所以使用大于等于而不用等于也是为了方便读者理解程序）
            if (j < 0)
                p = i, j = 0;  //如果i大于p
            while (p < s_len&&j < t_len&&S[p] == T[j])
                p++, j++;
            extend[i] = j;
            a = i;
        }
        else
            extend[i] = next[i - a];
    }
}
int main()
{
    int next[100] = { 0 };
    int extend[100] = { 0 };
    string S = "aaaaabbb";
    string T = "aaaaac";
    GetExtend(T, T, next, next);
    GetExtend(S, T, extend, next);
    //打印next和extend
    cout << "next:    " << endl;
    for (int i = 0; i < T.size(); i++)
        cout << next[i] << " ";
    cout << "\nextend:  " << endl;
    for (int i = 0; i < S.size(); i++)
        cout << extend[i] << " ";
    cout << endl;
    return 0;
}
```



