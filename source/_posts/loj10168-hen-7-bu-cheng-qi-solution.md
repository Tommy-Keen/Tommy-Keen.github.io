---
title: loj10168 恨 7 不成妻 题解
slug: loj10168-hen-7-bu-cheng-qi-solution
date: 2022-02-12 20:00:00
categories:
  - 2022
tags:
  - 题解
  - OI
mathjax: true
---
[link](https://loj.ac/p/10168)

# 题意：

给定一个区间 $[a,b]$ $(1<=a,b<=10^{18})$ ，求区间内满足以下条件的数的平方和模 $10^9+7$ 的余数：

1. 数字中不含 $7$ 

2. 数位上的数字之和不能被 $7$ 整除

3. 数不能被 $7$ 整除

# 思路：

**求整数区间 $[L,R]$ 上满足某种条件的数的个数**，一般称这样的问题为数位 DP 问题。

## 设计状态：

- 条件一：状态中需要记录小于等于 $x$ 的**个数**。

- 条件二：状态中需要记录**数字之和**模 $7$ 的**余数**。

- 条件三：状态中需要记录**数本身**模 $7$ 的**余数**。

即有：

#### 设状态为 $[i,j,k,c]$ ，表示第 $i$ 及更高位已经确定， $i+1$ 位及更高位数位上数字之和模 $7$ 余 $j$ ， $i+1$ 及更高位组成的数模 $7$ 余 $k$ 的满足条件的数共有 $c$ 个，记为 $dp_{i,j,k} = c$ 。

## 边界条件：

如果 $j>0$ 且 $k>0$ ，$dp_{0,j,k} = 1$ 。

## 转移方程：

$dp_{i,j,k} = \displaystyle\sum dp_{i-1,(j+p)\mod7,(k*10+p)\mod7}$ 

其中 $0<=p<=9$ 且 $p \not = 7$ ，是枚举确定的第 $i$ 位的数字。

### 但是题目要我们求出满足条件的数的平方和啊！还没有做完。

## 高妙之处：

- 考虑一个数 $x$ 增加 $b$ ， $b$ 对整个平方和的贡献新的数 $c = x + b$

  $c^2=x^2+2xb+b^2$

  $c^2-x^2=2xb\text{（一次项贡献）}+b^2\text{（平方项贡献）}$

  由于数位 DP 是一位位进行的，转移时不能直接计算整个 $b$ 产生的影响，而要单独计算 $b$ 的最高位（设为 $y$ ）的影响，如果 $y$ 是第 $i$ 位，则影响为：

  平方项贡献 -> $(10^{i-1}\cdot y)^2\cdot \text{（b 除 y 以外的其他位的可能取值数目）}$

  一次项贡献 -> $2\cdot y\cdot 10^{i-1}\cdot \text{（b 除 y 以外的其他位的可能取值之和）}$

  例如，$y = 1$，是第 $3$ 位，最低两位的取值有 $(01,02,11,12)$，则 $y$ 的贡献有 $100^2\cdot y^2 \cdot 4$ 以及 $200\cdot y\cdot (1+2+11+12)$。

- 可见还需要引入一个 DP 数组， $sum_{i,j,k}$ 表示满足条件的数之和，然后可以写出 sq (平方和 DP 数组) 的转移方程：

  $sq_{i,j,k}=dp_{i-1,(j+p)\bmod 7,(k+p)\bmod 7}\cdot (p\cdot 10^{i-1})^2 + 2\cdot sum_{i-1,(j+p)\bmod 7,(k+p)\bmod 7}\cdot (p\cdot 10^{i-1}) + sq_{i-1,(j+p)\bmod 7,(k+p)\bmod 7}$

  $p$ 的含义与前面相同。
  
- 则：$sum$ 的转移方程？

  $sum$ 中新增位 $y$ 只有一次项贡献：
  
  一次项贡献 -> $y\cdot 10^{i-1}\cdot \text{（b 除 y 以外的其他位的可能取值数目）}$
  
  $sum$ 的转移方程：
  
  $sum_{i,j,k}=dp_{i-1,(j+p)\bmod 7,(k+p)\bmod 7}\cdot (p\cdot 10^{i-1}) + sum_{i-1,(j+p)\bmod 7,(k+p)\bmod 7}$
  
### 综上,在数位 DP 时同时维护三个 DP 数组 dp 、 sum 、 sq 就可以求出答案了。

### 单次询问的时间复杂度 $O(18\cdot 7\cdot 7\cdot 10)$。

# 代码：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
const LL mod = 1e9+7;
struct node {
	LL dp,sum,sq;//分别对应条件一、条件二、条件三 
};
node a[25][20][20];
LL d[25],poww[25];
node dfs(int pos,int num,int sum,bool flag) {
	node res;
	if(!pos) {
		res.dp = (num>0&&sum>0);//因为其一为0的话不合法 
		res.sum = 0;
		res.sq = 0;
		return res;
	}
	if(!flag && a[pos][num][sum].dp != -1) {
		return a[pos][num][sum];
	}
	LL top = flag ? d[pos] : 9;
	res.dp = res.sum = res.sq = 0;//我tm经常忘 
	for(LL i = 0; i <= top; i++) {
		if(i == 7) continue;
		node k = dfs(pos-1,(num+i)%7,(sum*10+i)%7,flag&&i==top);
		res.dp = (res.dp+k.dp)%mod;
		res.sum = (res.sum+((k.dp*(i*poww[pos-1]%mod))%mod+k.sum)%mod)%mod;
		res.sq = (res.sq+(k.dp*(i*poww[pos-1]%mod*i*poww[pos-1]%mod))%mod+(2*i*poww[pos-1]%mod*k.sum)%mod+k.sq)%mod;
		//三个式子，注意取模运算，式子挺难推的 
	}
	if(!flag) a[pos][num][sum] = res;
	return res;
}
LL solve(LL x) {
	memset(a,-1,sizeof(a));//直接搞成不合法，因为dfs里面到边界的时候可以规定 
	int w = 0;
	while(x) {
		d[++w] = x%10;
		x /= 10;
	}
	return dfs(w,0,0,1).sq;//最高位的时候有限制 
}
int main() {
	int t;
	LL l,r;
	poww[0] = 1; 
	for(int i = 1; i <= 20; i++) {
		poww[i] = (1ll*poww[i-1]*10)%mod;
	}//预处理10^i次方 
	scanf("%d",&t);
	for(int i = 1; i <= t; i++) {
		scanf("%lld%lld",&l,&r);
		printf("%lld\n",((solve(r)-solve(l-1))%mod+mod)%mod);//注意一下取模运算？ 
	}
	return 0;
}
```

### 总之就是非常难推，试着独立再推一遍？
