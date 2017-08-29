# 大数操作模板

```java
import java.io.*;
import java.math.*;
import java.util.*;


public class Main {
	
	static PrintWriter cout = new PrintWriter(new OutputStreamWriter(System.out),true);
	
	static final int maxn=200003;
	static final int INF=1000000007;
    static BigInteger[] h = new BigInteger[maxn];
    static BigInteger[] b = new BigInteger[maxn];
	static BigInteger[] a = new BigInteger[maxn];
	static BigInteger[] c = new BigInteger[maxn], d = new BigInteger[maxn];
	
	static BigDecimal ffk = new BigDecimal("6.430703308172535824813264335274");
	static BigInteger g(int x){
		return BigInteger.valueOf(x);
	}
	static BigInteger g(int x,BigInteger y){
		return y.multiply(g(x));
	}
	static BigInteger g(int x,BigInteger y,BigInteger z){
		return z.multiply(y.multiply(g(x)));
	}
	public static void main(String[] args) {
		h[0] = g(2);
		h[1] = g(3);
		h[2] = g(6);
		c[1] = g(9);
		c[2] = g(15);
		int n = 20;
		for (int i = 3; i <= n; i += 1){
			h[i]=g(4,h[i-1]).add(g(17,h[i-2])).subtract(g(12,h[i-3]));
			h[i]=h[i].subtract(g(16));
//			c[i]=g(7,c[i-1]).add(g(-4,c[i-2]));
//			d[i]=myBigNumSqrt( g(3,c[i]).multiply(c[i-1]) );
//			cout.print(i+" "+d[i]+" ");
//			if(i>=6){
//				BigInteger x = g(4,d[i-1]).add(g(17,d[i-2])).add(g(-12,d[i-3]));
//				cout.println(x.compareTo(d[i]));
//			}
//			else cout.println("");
		}
		for (int i = 1; i < n; i += 1){
			b[i]=g(3,h[i+1],h[i]).add(g(9,h[i+1],h[i-1])).add(g(9,h[i],h[i])).add(g(27,h[i],h[i-1])).subtract(g(18,h[i+1])).subtract(g(126,h[i])).subtract(g(81,h[i-1])).add(g(192));
		}
		for (int i = 2; i < n; i += 1){
			a[i]=b[i].add(g(4).pow(i));
			a[i]=myBigNumSqrt(a[i]);
			if(i>=10){
				BigInteger x = g(4,a[i-1]).add(g(17,a[i-2])).add(g(-12,a[i-3]));
				cout.println(i+ " "+ a[i] + " " + x);
			}else cout.println(i+ " "+ a[i]);
			
//			BigDecimal b1 = new BigDecimal(d[i+1].toString());
//			BigDecimal b2 = new BigDecimal(d[i].toString());
//			cout.println(i+ " "+ b1.divide(b2, 30, RoundingMode.HALF_UP));
			//cout.print(a[i]+" ");
			//cout.print(myBigNumSqrt(a[i])+" "+myBigNumSqrt(b[i]));
//			BigDecimal b1 = new BigDecimal(myBigNumSqrt(a[i]).toString());
//			BigDecimal b2 = new BigDecimal(g(1,h[i-2]).add(h[i+1]).toString());
//			cout.println(i+ " "+ b1.divide(b2, 30, BigDecimal.ROUND_HALF_UP));
			
			
//			BigDecimal b1 = new BigDecimal(myBigNumSqrt(a[i]).toString());
//			BigInteger x = h[i];
//			BigDecimal b2 = new BigDecimal(x.toString());
//			cout.println(i+ " "+ b1.divide(b2, 30, RoundingMode.HALF_UP));
			//cout.println(" " + myBigNumSqrt(b[i]).divide(h[i]));
			cout.println(a[i].mod(g(INF)));
		}
		cout.println("");
		//h[i]=h[i-1].mutiply()+17*h[i-2]-12*h[i-3]-16;
    }
    
    
	public static BigInteger myBigNumSqrt(BigInteger xx){
		BigDecimal x=new BigDecimal(xx);
		BigDecimal n1=BigDecimal.ONE;
		BigDecimal ans=BigDecimal.ZERO;
		//int i=1;
		while((n1.multiply(n1).subtract(x)).abs().compareTo(BigDecimal.valueOf(0.001))==1)
		{
		//System.out.println(i+"..."+n1);
		//i++;
		//BigDecimal s1=x.divide(n1,2000,BigDecimal.ROUND_HALF_UP);
		BigDecimal s1=x.divide(n1,2000,RoundingMode.HALF_UP);
		BigDecimal s2=n1.add(s1);
		n1=s2.divide(BigDecimal.valueOf(2),2000,RoundingMode.HALF_UP);

		}
		ans=n1;
		//System.out.println(ans);
		BigInteger rt =new BigInteger(ans.toString().split("\\.")[0]);
		return rt;
	}
}
```



