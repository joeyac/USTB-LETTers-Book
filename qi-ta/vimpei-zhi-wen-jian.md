**F2** 开双屏并新建输入文件

**F9** 编译

**F5** 正常运行

**F8** 用F2新建的输入文件作为输入来运行程序

**\[\[** 输入{ }

**F3 **显示自定义模板

```
set nu cindent smartindent autoindent
set mouse=a ts=4 sw=4
syntax on
set noswapfile
nmap<F2> : vs %<.in <CR>
nmap<F8> : !./%< < %<.in <CR>
nmap<F9> : make %< <CR>
nmap<F5> : !./%< <CR>
inoremap [[ {<cr>}<c-o>O
map <F3> :call SetTitle()<CR>
func SetTitle()
let l = 0
let l = l + 1 | call setline(l,'#include <cstdio>')
let l = l + 1 | call setline(l,'#include <cstring>')
let l = l + 1 | call setline(l,'#include <iostream>')
let l = l + 1 | call setline(l,'#include <algorithm>')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'#define fi first')
let l = l + 1 | call setline(l,'#define se second')
let l = l + 1 | call setline(l,'#define sq(x) ((x)*(x))')
let l = l + 1 | call setline(l,'#define mp make_pair')
let l = l + 1 | call setline(l,'#define pb push_back')
let l = l + 1 | call setline(l,'#define mem(x,y) memset(x,y,sizeof(x))')
"let l = l + 1 | call setline(l,'#define _c1(x) cout<<(x)<<endl')
"let l = l + 1 | call setline(l,'#define _c2(x,y) cout<<(x)<<" "<<(y)<<endl')
"let l = l + 1 | call setline(l,'#define _c3(x,y,z) cout<<(x)<<" "<<(y)<<" "<<(z)<<endl')
"let l = l + 1 | call setline(l,'#define _c4(x,n) do{for(int _=0;_<n;cout<<x[_++]<<" ");puts("");}while(0)')
"let l = l + 1 | call setline(l,'#define _c5(x,n,m) do{for(int __=0;__<n;__++)_c4(x[__],m);puts("");}while(0)')
let l = l + 1 | call setline(l,'using namespace std;')
let l = l + 1 | call setline(l,'typedef double DB;')
let l = l + 1 | call setline(l,'typedef long long LL;')
let l = l + 1 | call setline(l,'typedef unsigned int U;')
let l = l + 1 | call setline(l,'typedef pair<int, int> P;')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'int main(){')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'    return 0;')
let l = l + 1 | call setline(l,'}')
endfunc
```



