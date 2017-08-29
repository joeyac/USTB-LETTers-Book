# 矩阵类 && 常系数线性齐次递推

说明：

模板：

```cpp
struct matrix{
	#define var long long
	#define _ %INF
	#define __ %=INF
	vector< vector<var> > a;
	int n;
	matrix(){}
	void init(){a.resize(n, vector<var>(n) );}
	void clear(){
		for (int i = 0; i < n; i += 1)
			for (int j = 0; j < n; j += 1)
				(*this)[i][j] = 0;
	}
	void unit(){
		this->clear();
		for (int i = 0; i < n; i += 1)
				(*this)[i][i] = 1;
	}
	matrix(int _n){n = _n;this->init();}
	vector<var> operator [] (int i){return a[i];}
	
	matrix operator * (matrix y){
		assert(n==y.n);
		matrix ret=matrix(n);ret.clear();
		
		for (int i = 0; i < n; i += 1)
			for (int j = 0; j < n; j += 1)
				for (int k = 0; k < n; k += 1)
					(ret[i][j] += (a[i][k] * y[k][j]) _) __;
		return ret;
	}
	
	matrix operator ^ (ll m){
		assert(m>=0);
		matrix ret=matrix(n);ret.unit();
		matrix base=*(this);
		while(m){
			if(m&1)ret=ret*base;
			base=base*base;
			m>>=1;
		}
		return ret;
	}
	void out(){
		for (int i = 0; i < n; i += 1)
			for (int j = 0; j < n; j += 1)
			printf("%lld%c",a[i][j]," \n"[j==n-1]);
	}
	#undef var
	#undef _
	#undef __
};

//常系数线性齐次递推
//f[0]=a[0],f[1]=a[1],...,f[x-1]=a[x-1]
//f[n]=b[0]*f[n-x]+b[1]*f[n-x+1]+...+b[x-1]*f[n-1]
//给定t 求f[t]
ll solve(int a[], int b[], int x, ll t){
	if(t<=x)return a[t-1];
	
	matrix M(x);M.clear();
	
	for (int i = 0; i < x; i += 1)
		M[i][x-1] = b[i];
	for (int i = 0; i < x-1; i += 1)
		M[i+1][i] = 1;
//	M.out();
	M = M ^ (t-x);
	ll res = 0;
	for (int i = 0; i < x; i += 1)
		(res += M[i][x-1] * a[i] % INF) %=INF;
	return (res+INF) % INF;
}
```



