# 模线性方程组求解

求形如$$x \mod M[i] = r[i] $$的解

先说一个故事

说秦末，刘邦的将军韩信带领1500名士兵经历了一场战斗，战死四百余人。韩信为了清点人数让士兵站成三人一排，多出来两人；站成五人一排，多出来四人；站成七人一排，多出来六人。韩信立刻就知道了剩余人数为1049人。

这就是著名的韩信点兵的故事，化成数学模型就是：

韩信是为了计算的是士兵的人数，那么我们设这个人数为x。三人成排，五人成排，七人成排，即x mod 3, x mod 5, x mod 7。也就是说我们可以列出一组方程：

x mod 3 = 2

x mod 5 = 4

x mod 7 = 6

将这个方程组推广到一般形式：给定了n组除数m\[i\]和余数r\[i\]，通过这n组\(m\[i\],r\[i\]\)求解一个x，使得x mod m\[i\] = r\[i\]

这就是模线性方程组

一开始就直接求解多个方程不是太容易，我们从n=2开始递推：

已知：

$$x \mod m[1] = r[1]$$

$$ x \mod m[2] = r[2]$$

根据这两个式子，我们存在两个整数k\[1\],k\[2\]：

$$x = m[1] * k[1] + r[1]$$

$$ x = m[2] * k[2] + r[2] $$

由于两个值相等，因此我们有：

$$ m[1] * k[1] + r[1] = m[2] * k[2] + r[2] 
 \\ \Rightarrow  m[1] * k[1] - m[2] * k[2] = r[2] - r[1]$$

由于$$m[1],m[2],r[1],r[2]$$都是常数，若令$$A=m[1],B=m[2],C=r[2]-r[1],x=k[1],y=k[2]$$，则上式变为：$$Ax + By = C$$。

这就转换成扩展的欧几里得算法

可以先通过$$gcd(m[1], m[2])能否整除r[2]-r[1]$$来判定是否存在解。

假设存在解，则我们通过扩展欧几里德求解出$$k[1],k[2]$$。

$$再把k[1]代入x = m[1] * k[1] + r[1]，就可以求解出x。$$

同时我们将这个x作为特解，可以扩展出一个解系：

$$X = x + k*lcm(m[1], m[2]) k为整数$$

将其改变形式为：

$$X \mod lcm(m[1], m[2]) = x$$

$$令M = lcm(m[1], m[2]), R = x，则有新的模方程X mod M = R。$$

$$此时，可以发现我们将x \mod m[1] = r[1]，x \mod m[2] = r[2]合并为了一个式子$$:

$$X \mod lcm(m[1], m[2]) = x。满足后者的X一定满足前两个式子。个式子。$$

每两个式子都可以通过该方法化简为一个式子。那么我们只要重复进行这个操作，就可以将n个方程组化简为一个方程，并且求出一个最后的解了。

### 代码：

```cpp
int n;

long long m[maxn], r[maxn];

long long gcd(long long a, long long b){
    if (!b) return a;
    return gcd(b, a%b);
}
pair<long long, long long > extend_gcd(long long a, long long b){
    pair<long long, long long> tmp;
    if (a%b == 0){
        return pair<long long , long long>(0, 1);
    }
    tmp = extend_gcd(b, a%b);
    long long tmp_x = tmp.first, tmp_y = tmp.second;
    int x = tmp_y;
    int y = tmp_x-(a/b)*tmp_y;
    return pair<long long , long long>(x, y);
}
long long solve(){
    long long M = m[0], R = r[0];
    for (int i = 1; i < n; i++){
        long long d = gcd(M, m[i]);
        long long c = r[i] - R;
        if (c%d != 0){
            return -1;  // 无解的情况
        }
        pair<long long, long long> t = extend_gcd(M/d, m[i]/d); // 扩展欧几里德计算
        t.first = (c/d*t.first)%(m[i]/d);   // 扩展解系
        R = R+t.first*M;        
        M = M/d*m[i];   // 求解lcm(M, m[i])
        R %= M;     // 求解合并后的新R，同时让R最小
    }
    if (R < 0){
         R += M;
    }
    return R;
}
```



