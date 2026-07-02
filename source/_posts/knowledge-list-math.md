---
title: 知识清单——数学
slug: knowledge-list-math
date: 2022-07-17 20:00:00
categories:
  - 2022
tags:
  - 知识
  - OI
mathjax: true
---
勿念，想更会更。

------------

# 第二类斯特林数

$\begin{Bmatrix} n \\\\ m \end{Bmatrix}$ 表示将 $n$ 个两两不同的元素，划分为 $m$ 个互不区分的非空子集的方案数。

$\begin{Bmatrix} n \\\\ m \end{Bmatrix}=\sum\limits_{i=0}^m\dfrac{(-1)^{m-i}i^n}{i!(m-i)!}$

------------

# $lucas$定理

$\dbinom{n}{m}\equiv \dbinom{\lfloor n/p \rfloor}{\lfloor m/p \rfloor}\dbinom{n\bmod p}{m\bmod p}\pmod p$

------------

# 二项式定理

$(a+b)^n=\displaystyle\sum_{i=0}^{n}\dbinom{n}{i}a^{n-i} b^i$

------------

# 容斥原理

设 $U$ 中元素有 $n$ 种不同的属性，而第 $i$ 种属性称为 $P_i$ ，拥有属性 $p_i$ 的元素构成集合 $S_i$ ，那么：

$\Bigg|\displaystyle\bigcup_{i=1}^{n}S_i\Bigg|=\displaystyle\sum_{i}|S_i|-\displaystyle\sum_{i < j} |S_i\cap S_j|+\displaystyle\sum_{i < j < k}|S_i\cap S_j\cap S_k|-\cdots +(-1)^{m-1}\displaystyle\sum_{a_i < a_{i+1}}|\bigcap_{i=1}^{m}S_{a_i}|+\cdots+(-1)^{n-1}|S_1\cap\cdots\cap S_n|$

即： $\Bigg|\displaystyle\bigcup_{i=1}^{n}S_i\Bigg|=\displaystyle\sum_{m=1}^n(-1)^{m-1}\sum_{a_i<a_{i+1}}\Bigg|\bigcap_{i=1}^mS_{a_i}\Bigg|$

-----------

# 卡特兰数

$H_n = \dfrac{\dbinom{2n}{n}}{n+1}\quad (n \geq 2,\ n \in \mathbf{N_{+}})$

<div>$$H_n=\begin{cases}
\displaystyle\sum_{i=1}^{n} H_{i-1}H_{n-i}, & n \ge 2,\ n \in \mathbf{N_{+}} \\\\
1, & n = 0,1
\end{cases}$$</div>

$H_n = \dfrac{H_{n-1} (4n-2)}{n+1}$

$H_n = \dbinom{2n}{n} - \dbinom{2n}{n-1}$

------------

# 欧拉公式

$e^{i\theta}=\cos \theta + i \sin \theta$

------------

# 组合计数

从 $n$ 个元素中取出 $m$ 个可重元素，方案数为： $C_{n+m-1}^{m}$

证明自己意淫。

------------


# 快速幂

### link：

[P1226 【模板】快速幂||取余运算](https://www.luogu.com.cn/problem/P1226)

### 思路：

无。

### 功能：

解决一些 指数/数值 很大 且 答案取模 的问题。

### 复杂度：

$O(\log_2 n)$

### 代码：

```cpp
inline LL ksm(LL a,int k) {
	LL res = 1ll;
	while(k) {
		if(k&1) res = res*a%mod;
		a = a*a%mod;
		k >>= 1;
	}
	return res;
}
```

# 欧拉筛

### link: 

[P3383 【模板】线性筛素数](https://www.luogu.com.cn/problem/P3383)

### 思路：

从2开始，将每个质数的倍数都标记成合数，以达到筛选素数的目的。在此基础上，让每个合数只被它的**最小质因子**筛选一次，以达到**不重复**的目的。

### 复杂度： 

$O(n)$

### 代码：

```cpp
int n,v[N];
vector <int> prime;
inline void oulashai() {
	for(register int i = 2; i <= n; ++i) {
		if(!v[i]) {
			prime.push_back(i);
			v[i] = i;
		}
		for(register int j = 0; j < prime.size(); ++j) {
			if(prime[j]>v[i] || i*prime[j]>n) break;
			v[i*prime[j]] = prime[j];
		}
	}
}
```

# gcd & lcm

### [link](https://www.bilibili.com/video/BV1GJ411x7h7)

### 思路：

$\gcd(a,b)=\gcd(b,a \mod b)$，即欧几里得公式。

### 复杂度：

$O(\log_2(\max(a,b)))=O(1)?$

### 代码：

```cpp
int gcd(int x,int y) {
	return (y == 0) ? x : gcd(y,x%y);
}
int lcm(int x,int y) {
	return x/gcd(x,y)*y;
}
```

# ~~龟速乘~~ 快速乘

[link](https://cdn.luogu.com.cn/upload/image_hosting/3qm76thf.png)

### 思路：

快速乘法是求两个数相乘，即求解$a*b\mod c$。但是如果当 $a$ 和 $b$ 特别大的时候，两个数相乘可能超过 long long 范围，所以要使用快速乘法，因为在加法运算的时候不会超，而且可以直接取模，这样就会保证数据超不了了。

加法稍微慢了些，考虑用 long double 来进行优化取模运算。运用自动溢出这个操作，虽然代码中的 $c$ 的前后两个部分都是会溢出的，但 unsigned 保证了它们溢出后的差值基本不变，所以即使它会溢出也不会影响最终结果。

### 复杂度：

$O(1)$

### 代码：

```cpp
inline int ksc(int a,int b,int mod) {
    int c = a*b-(int)((long double)a*b/mod+0.5)*mod;
    return (c < 0) ? c+mod : c;
}
```

# 扩展欧几里得算法(exgcd)

### link：

[P1082 [NOIP2012 提高组] 同余方程](https://www.luogu.com.cn/problem/P1082)

[P5656 【模板】二元一次不定方程 (exgcd)](https://www.luogu.com.cn/problem/P5656)

[P2613 【模板】有理数取余](https://www.luogu.com.cn/problem/P2613)

### 思路：

[link](https://leohh.moe/?p=130)

### 复杂度：

$O(\log \max(a,b))$

### 代码：

```cpp
void exgcd(int a,int b,int &g,int &x,int &y) {
	if(!b) {
		g = a;
		x = 1; y = 0;
	} else {
		exgcd(b,a%b,g,x,y);
		int tmp = x;
		x = y;
		y = tmp - (a/b)*y;
	}
}
```

# 乘法逆元

### link：

[P3811 【模板】乘法逆元](https://www.luogu.com.cn/problem/P3811)

[P5431 【模板】乘法逆元2](https://www.luogu.com.cn/problem/P5431)

### 思路：

[link](https://www.cnblogs.com/zjp-shadow/p/7773566.html)

### 复杂度：

一个：$O(\log n)$

$n$ 个：$O(n)$

### 代码：

```cpp
//P3811
a[1] = 1; puts("1");
for(int i = 2; i <= n; i++) {
	a[i] = (long long)(p-p/i)*a[p%i]%p;
	printf("%d\n",a[i]);
}
```

```cpp
//P5431
long long n,p,k,ans,tmp,a[N],b[N],s[N];

n = read(); p = read(); k = read();
s[0] = 1;
for(register int i = 1; i <= n; ++i) {
	a[i] = read();
	s[i] = (s[i-1]*a[i])%p;
}
b[n] = ksm(s[n],p-2);
for(register int i = n; i > 1; --i) {
	b[i-1] = (b[i]*a[i])%p;
}
for(register int i = 1, ki = 1; i <= n; ++i) {
	ki = (ki*k)%p;
	ans = (ans+((b[i]*s[i-1])%p)*ki)%p;
}
printf("%lld\n",ans);
```

# 中国剩余定理(CRT)

### link：

[P1495 【模板】中国剩余定理(CRT)/曹冲养猪](https://www.luogu.com.cn/problem/P1495)

### 思路：

中国剩余定理是一种用于求解诸如：

<div>$$\begin{cases}
x \equiv a_1 \pmod{n_1} \\\\
x \equiv a_2 \pmod{n_2} \\\\
\vdots \\\\
x \equiv a_k \pmod{n_k}
\end{cases}$$</div>

形式的同余方程组的定理，其中， $n_1,n_2,...,n_k$ 为两两互质的整数，我们的目的，是找出 $x$ 的最小非负整数解。

令 $M=\displaystyle\prod_{i=1}^kn_i$ ， $M_i=\dfrac{M}{n_i}$

求解出所有 $M_i^{-1}$ ，使得 $M_iM_i^{-1}≡1\;\;(mod\;\;n_i)$ 

记 $c_i=M_iM_i^{-1}$ 

则原方程组存在唯一解 $x=\displaystyle\sum_{i=1}^na_ic_i$

证明：

对于任意的 $a_j$ 和 $n_j$ ，因为 $c_i=M_iM_i^{-1}$ 且 $n_j \mid M_i$ ，所以 $a_ic_i≡0\;\;(mod\;\;n_j)$ 

又 $c_j≡1\;\;(mod\;\;n_j)$ ，所以 $x≡a_jc_j≡a_j\;\;(mod\;\;n_j)$

### 复杂度：

$O(n \log n)$

### 代码：

```cpp
//P1495
#include <bits/stdc++.h>
#define int long long
namespace CRT {
	const int N = 15;
	int n,a[N],b[N],c[N];
	int M = 1,m[N],nim[N];
	void exgcd(int a,int b,int &x,int &y) {
		if(!b) {
			x = 1; y = 0;
		} else {
			exgcd(b,a%b,x,y);
			int tmp = x;
			x = y;
			y = tmp - (a/b)*y;
		}
	}
	void input() {
		scanf("%lld",&n);
		for(int i = 1; i <= n; i++) {
			scanf("%lld%lld",&a[i],&b[i]);
			M *= a[i];
		}
	}
	void work() {
		for(int i = 1, x, y; i <= n; i++) {
			m[i] = M/a[i];
			exgcd(m[i],a[i],x,y);
			nim[i] = (x%a[i]+a[i])%a[i];
			c[i] = m[i]*nim[i];
		}
	}
	void output() {
		int ans = 0;
		for(int i = 1; i <= n; i++) {
			ans += b[i]*c[i];
		}
		printf("%lld\n",ans%M);
	}
}
signed main() {
	CRT::input();
	CRT::work();
	CRT::output();
	return 0;
}
```

# 扩展中国剩余定理(EXCRT)

### link：

[P4777 【模板】扩展中国剩余定理（EXCRT）](https://www.luogu.com.cn/problem/P4777)

### 思路：

先来解决一个问题：[为什么和 CRT 不一样？](https://www.luogu.com.cn/blog/blue/kuo-zhan-zhong-guo-sheng-yu-ding-li)

其实就是解一般同余方程组，可能无解。

<div>$$\begin{cases}
x \equiv a_1 \pmod{n_1} \\\\
x \equiv a_2 \pmod{n_2} \\\\
\vdots \\\\
x \equiv a_k \pmod{n_k}
\end{cases}$$</div>

- 第一个方程有特解 $x = a_1$ 

- ·如果以及得到了一个满足前 $i-1$ 个方程的解 $x$ ，令 $n = lcm(n_1,n_2,...,n_{i-1})$，则通解为 $x=x_0 + nt$

- 考虑第 $i$ 个方程， $x_0+nt=a_i(mod \; n_i)$，等价于解同余方程 $nt=a_i-x_0(mod\; n_i)$

- 求出方程的特解 $t_0$ ，则可以得到前 $i$ 个方程的一个特解$x'=x_0+ nt_0$

### 复杂度：

$O(n \log n)$

### 代码：

```cpp
//P4777
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e5+10;
int n,m[N],a[N];
inline int ksc(int a,int b,int mod) {
    int c = a*b-(int)((long double)a*b/mod+0.5)*mod;
    return (c < 0) ? c+mod : c;
}
int gcd(int x,int y) {
	return (y == 0) ? x : gcd(y,x%y);
}
int lcm(int x,int y) {
	return x/gcd(x,y)*y;
}
void exgcd(int a,int b,int &g,int &x,int &y) {
	if(!b) {
		g = a;
		x = 1; y = 0;
	} else {
		exgcd(b,a%b,g,x,y);
		int tmp = x;
		x = y;
		y = tmp - (a/b)*y;
	}
}
signed main() {
	while(~scanf("%lld",&n)) {
		for(int i = 1; i <= n; i++) {
			scanf("%lld%lld",&m[i],&a[i]);
		}
		bool ok = 0;
		int x, y, g, lcm_ = 1, tmp = 0, ans = 0;
		for(int i = 1; i <= n; i++) {
			exgcd(lcm_,m[i],g,x,y);
			tmp = ((a[i]-ans)%m[i]+m[i])%m[i];
			if(tmp%g) ok = 1;
			x = ksc(x,tmp/g,m[i]/g);
			ans += x*lcm_;
			lcm_ *= m[i]/g;
			ans = (ans%lcm_+lcm_)%lcm_;
		}
		if(ok) puts("-1");
		else printf("%lld\n",ans);
	}
	return 0;
}
```

