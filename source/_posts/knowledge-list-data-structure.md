---
title: 知识清单——数据结构
slug: knowledge-list-data-structure
date: 2022-08-21 20:00:00
categories:
  - 2022
tags:
  - 知识
  - OI
mathjax: true
---
$update:\text{2021/6/29行数破千纪念！}$

为什么没有普及的知清？因为被我搞掉了。。。

需要练习？可以加入我的[团队](https://www.luogu.com.cn/team/33750)。

# ST表
[P3865 【模板】ST 表](https://www.luogu.com.cn/problem/P3865)
```cpp
int m,n,a[N],st[N][logN],lg[N],ans[N];
inline void prest() {
	for(int i=1; i<=m; i++) st[i][0]=a[i];
	for(int j=1; (1<<j)<=m; j++) {
		for(int i=1; i+(1<<j)-1<=m; i++) {
			st[i][j]=max(st[i][j-1],st[i+(1<<(j-1))][j-1]);
		}
	}
}
inline void prelg() {
	for(int i=2; i<=m; i++) {
		lg[i]=lg[i>>1]+1;
	}
}
int main() {
	prest();
	prelg();
	for(int i=1; i<=n; i++) {
		int l,r,k;
		l=read();
		r=read();
		k=lg[r-l+1];
		printf("%d\n",max(st[l][k],st[r-(1<<k)+1][k]));
	}
}
```

# lca
1. lca中dfs建图，递推f时应该写else break;
   ```cpp
   for(int i = 1; i <= 20; i++) {
		if(f[u][i-1]) f[u][i] = f[f[u][i-1]][i-1];
		else break;
	}
   ```
2. 开数组时：```int f[N][25];```

   不应是：```int f[N][20];```
   
3. code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 500010;
vector <int> g[N];
int n,m,s,f[N][25],dep[N];
void dfs(int u) 
{
	for(int i = 1; i <= 20; i++) 
	{
		if(f[u][i-1]) 
		{
			f[u][i] = f[f[u][i-1]][i-1];
		}
		else break;
	}
	for(int i = 0; i < g[u].size(); i++)
	{
		int v = g[u][i];
		if(v != f[u][0])
		{
			f[v][0] = u;
			dep[v] = dep[u]+1;
			dfs(v);
		}
	}
}
int lca(int a,int b)
{
	if(dep[a] < dep[b]) swap(a,b);
	for(int i = 20; i >= 0; i--)
	{
		if(dep[f[a][i]] >= dep[b])
		{
			a = f[a][i];
		}
	}
	if(a == b) return a;
	for(int i = 20; i >= 0; i--)
	{
		if(f[a][i] != f[b][i])
		{
			a = f[a][i];
			b = f[b][i];
		}
	}
	return f[a][0];
}
int main() 
{
	scanf("%d%d%d",&n,&m,&s);
	for(int i = 1, x, y; i < n; i++) 
	{
		scanf("%d%d",&x,&y);
		g[x].push_back(y);
		g[y].push_back(x);
	}
	dep[s] = 1;
	dfs(s);
	for(int i = 1, a, b; i <= m; i++) {
		scanf("%d%d",&a,&b);
		printf("%d\n",lca(a,b));
	}
	return 0;
}
```

# 树上差分
1. 如果需要把x,y之间的路径上的点的权值都增加k：

   ```cpp
   diff[x] += k,diff[y] += k;
   diff[lca(x,y)] -= k,diff[f[lca(x,y)][0]] -= k;
   ```

2. 如果权值不在点上，而在**边**上，也可以类似的处理。记节点i为根的子树中节点的diff之和，为节点i与其父亲之间的边的权值变化值。如果需要把x,y之间的路径上的边的权值都增加k：

   ```cpp
   diff[x] += k,diff[y] += k;
   diff[lca(x,y)] -= 2*k;
   ```

# 分块
1. 块的大小： 
   ```cpp
   s = sqrt(n+0.5);
   ```
2. 块的数目：
   ```cpp
   m = (n-1)/s+1;
   ```
3. 分块:区间修改与和维护

   题意
   
   长度为 n (n<=50000)的数列 a，有 n 次操作，操作分两种：

   把下标在 [l,r] 之间的数全部增加 c
   
   求下标在 [l,r] 之间的所有数之和

   输入格式

   第一行输入一个数字 n 。

   第二行输入 n 个数字，第 i 个数字为 ai，以空格隔开。

   接下来输入 n 行询问，每行输入四个数字 opt,l,r,c，以空格隔开。

   若 opt=0，表示将位于[l,r] 的之间的数字都加 c 。

   若 opt=1，表示询问位于[l,r] 的所有数字的和模 (c+1)。
   
   输出格式

   对于每次询问，输出一行一个数字表示答案
   
   输入输出样例
   
   输入 #1
   
   4
   
   1 2 2 3
   
   0 1 3 1
   
   1 1 4 4
   
   0 1 2 2
   
   1 1 2 4
   
   输出 #1
   
   1
   
   4

   code
   
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 50010;
int n,opt,l,r,c,size,m;
int a[N],b[N],sum[N],add[N];
void pre() {
	size = sqrt(n+0.5);
	m = (n-1)/size+1;
	for(int i = 1; i <= m; i++) {
		for(int j = (i-1)*size+1; j <= min(i*size,n); j++) {
			sum[i] += a[j];
			b[j] = i;
		}
	}
}
void plus_(int l,int r,int c) {
	for(int i = l; i <= min(r,b[l]*size); i++) {
		a[i] += c;
	}
	sum[b[l]] += c*(min(r,b[l]*size)-l+1);
	
	if(b[r] > b[l]) {
		for(int i = (b[r]-1)*size+1; i <= r; i++) {
			a[i] += c;
		}
		sum[b[r]] += c*(r-(b[r]-1)*size);
		
		for(int i = b[l]+1; i <= b[r]-1; i++) {
			add[i] += c;
		}
	}
}
int ask(int l,int r) {
	int ans = 0;
	for(int i = l; i <= min(r,b[l]*size); i++) {
		ans += (a[i]+add[b[l]])%(c+1);
	}
	
	if(b[r] > b[l]) {
		for(int i = (b[r]-1)*size+1; i <= r; i++) {
			ans += (a[i]+add[b[r]])%(c+1);
		}
		
		for(int i = b[l]+1; i <= b[r]-1; i++) {
			ans += (sum[i]+add[i]*size)%(c+1);
		}
	}
	return ans%(c+1);
}
int main() {
	scanf("%d",&n);
	for(int i = 1; i <= n; i++) {
		scanf("%d",&a[i]);
	}
	pre();
	for(int i = 1; i <= n; i++) {
		scanf("%d%d%d%d",&opt,&l,&r,&c);
		if(opt==0) plus_(l,r,c);
		if(opt==1) printf("%d\n",ask(l,r));
	}
	return 0;
}
```

# 树状数组
1. ```c[x] = sum[x] - sum[x-lowbit(x)]```

2. 常用的一些函数：

   ```cpp
   inline int lowbit(int x) {
	   return x & (-x);
   }
   inline int query(int x,int* arr) {
	   int res = 0;
	   for(; x; x -= lowbit(x)) res += arr[x];
	   return res;
   }
   inline void update(int x,int y,int* arr) {
	   for(; x <= n; x += lowbit(x)) arr[x] += y;
   }
   ```
   
3. 数列区间修改与查询

   题意

   给一个长度为 n 的数列 a，共有 m次操作，操作分两种：

   “1 l r c” : 求 a 下标在 [l,r] 之间的所有元素加上 c

   “2 l r” : 求 a 下标在 [l,r] 之间的所有元素之和
   
   输入格式

   第一行两个整数 n,m

   第二行 n 个整数表示数列 a

   接下来 m 行，每行描述一个操作
   
   输出格式

   对于每一个操作2，输出答案，每个答案占一行
   
   输入输出样例

   输入 #1

   5 10
   
   2 6 6 1 1
   
   2 1 4
   
   1 2 5 10
   
   2 1 3
   
   2 2 3
   
   1 2 2 8
   
   1 2 3 7
   
   1 4 4 10
   
   2 1 2
   
   1 4 5 6
   
   2 3 4

   输出 #1
   
   15
   
   34
   
   32
   
   33
   
   50

   说明/提示

   $n,m<=10^6$

   $|ai|,|c|<=10^6$

   $1<=l,r<=n$
   
   思路
   
   $a[x]=\displaystyle\sum_{i=1}^{x}diff[i]$
   
   <div>$$\begin{aligned}
   sum[x]
   & = \displaystyle\sum_{i=1}^{x} a[i] \\\\
   & = \displaystyle\sum_{i=1}^{x}\displaystyle\sum_{j=1}^{i} diff[j] \\\\
   & = \displaystyle\sum_{i=1}^{x}(x-i+1)\cdot diff[i] \\\\
   & = (x+1)\displaystyle\sum_{i=1}^{x} diff[i]-\displaystyle\sum_{i=1}^{x} i\cdot diff[i] \\\\
   & = (x+1)a[x]-\displaystyle\sum_{i=1}^{x} i\cdot diff[i]
   \end{aligned}$$</div>
   
   其中，i*diff[i]可用另一个树状数组维护。

   code
   
```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e6+10;
int n,m,c[N],a[N],d[N],sum[N];
inline int lowbit(int x) {
	return x & (-x);
}
inline int query(int x,int* arr) {
	int res = 0;
	for(; x; x -= lowbit(x)) res += arr[x];
	return res;
}
inline void update(int x,int y,int* arr) {
	for(; x <= n; x += lowbit(x)) arr[x] += y;
}
signed main() {
	scanf("%lld%lld",&n,&m);
	for(int i = 1; i <= n; i++) {
		scanf("%lld",&a[i]);
		sum[i] = sum[i-1] + i*(a[i]-a[i-1]);
	}
	for(int i = 1; i <= n; i++) {
		c[i] = a[i] - a[i-lowbit(i)];
		d[i] = sum[i] - sum[i-lowbit(i)];
	}
	for(int i = 1; i <= m; i++) {
		int opt,x,y,k;
		scanf("%lld",&opt);
		if(opt == 1) {
			scanf("%lld%lld%lld",&x,&y,&k);
			update(x,k,c);
			update(y+1,-k,c);
			update(x,k*x,d);
			update(y+1,-k*(y+1),d);
		}
		if(opt == 2) {
			scanf("%lld%lld",&x,&y);
			int sum1 = (x)*query(x-1,c)-query(x-1,d);
			int sum2 = (y+1)*query(y,c)-query(y,d);
			printf("%lld\n",sum2-sum1);
		}
	}
	return 0;
}
```
4. 多维树状数组

```cpp
inline int query(int a,int b,int arr[N][N]) {
	int res = 0;
	for(int x = a; x; x -= lowbit(x)) {
		for(int y = b; y; y -= lowbit(y)) {
			res += arr[x][y];
		}
	}
	return res;
}
inline void update(int a,int b,int k,int arr[N][N]) {
	for(int x = a; x <= n; x += lowbit(x)) {
		for(int y = b; y <= m; y += lowbit(y)) {
			arr[x][y] += k;
		}
	}
}
```

   或者：

```cpp
inline int query(int a,int b,int arr[N][N]) {
	int res = 0, i = a;
	while(i) {
		int j = b;
		while(j) {
			res += arr[i][j];
			j -= lowbit(j);
		}
		i -= lowbit(i);
	}
	return res;
}
inline void update(int a,int b,int k,int arr[N][N]) {
	int i = a;
	while(i <= n) {
		int j = b;
		while(j <= m) {
			arr[i][j] += k;
			j += lowbit(j);
		}
		i += lowbit(i);
	}
}
```

# 线段树
1. 建立：

```cpp
int n,m,a[N],sum[4*N];
inline int lc(int x) {
	return x<<1;
}
inline int rc(int x) {
	return x<<1|1;
}
void build(int u,int l,int r) {
	if(l == r) {
		???
	} else {
		int mid = (l+r)>>1;
		build(lc(u),l,mid);
		build(rc(u),mid+1,r);
		pushup(u);
	}
}
```

2. 修改：

```cpp
inline void pushup(int u) {
	???
}
void modify(int u,int l,int r,int x,int y) {
	if(l == r && l == x) {
		???
    } else {
    	int mid = (l+r)>>1;
    	if(x <= mid) modify(lc(u),l,mid,x,y);
    	else modify(rc(u),mid+1,r,x,y);
    	pushup(u);
	}
}
```

3. 查询：

```cpp
??? query(int u,int l,int r,int x,int y) {
	if(x <= l && y >= r) {
		???
	} else {
		int mid = (l+r)>>1;
		??? res = 0;
		if(x <= mid) ???query(lc(u),l,mid,x,y);
		if(y > mid) ???query(rc(u),mid+1,r,x,y);
		return ???;
	}
}
```

4. 延迟标记(参考[P3372](https://www.luogu.com.cn/problem/P3372)和[P3373](https://www.luogu.com.cn/problem/P3373))：

```cpp
//P3372
int n,m,a[N],sum[4*N],add[4*N];
inline void pushdown(int u,int l,int r) {
	add[lc(u)] += add[u];
	add[rc(u)] += add[u];
	int mid = (l+r)>>1;
	sum[lc(u)] += add[u]*(mid-l+1);
	sum[rc(u)] += add[u]*(r-mid);
	add[u] = 0;
}
void modify(int u,int l,int r,int x,int y,int z) {
	if(x <= l && y >= r) {
		sum[u] += z*(r-l+1);
		add[u] += z;
    } else {
    	int mid = (l+r)>>1;
		!!!pushdown(u,l,r);!!!
    	if(x <= mid) modify(lc(u),l,mid,x,y,z);
    	if(y > mid) modify(rc(u),mid+1,r,x,y,z);
    	pushup(u);
	}
}
int query(int u,int l,int r,int x,int y) {
	if(x <= l && y >= r) {
		return sum[u];
	} else {
		int mid = (l+r)>>1;
		!!!pushdown(u,l,r);!!!
		int res = 0;
		if(x <= mid) res = query(lc(u),l,mid,x,y);
		if(y > mid) res += query(rc(u),mid+1,r,x,y);
		return res;
	}
}
```

# 最短路
一、 单源最短路

   1. SPFA 算法$--$用 bfs 求最短路
   
      - 定义 $d_i$ 表示从起点到第 $i$ 个节点的最短路长度。
      
      - 把起点 $s$ 加入 BFS 队列， ```d[s]=0```。
      
      - 依次处理队内元素，对于当前队首元素 $k$ ，尝试**用所有 $k$ 出发的边 $(k,p)$ 去更新 $d_p$ 的最小值**。如果更新成功，且 **$p$ 不在队列中**，把 $p$ 入队。
      
      - 对边的权值没有任何限制，即可以处理负权边。
      
      - 负环处理方法：如果不存在负环，则一个点 $i$ 的最短路权值和 $d_i$ 最多被修改 $n-1$ 次。所以，我们可以用一个数组 $c_i$ 记录 $d_i$ 的修改次数，如果 $c_i>=n$ 说明存在负环。
      
      - 例：[P3371 【模板】单源最短路径（弱化版）](https://www.luogu.com.cn/problem/P3371)
      
      - 参考：
      
```cpp
void bfs() {
	q.push(s);
	d[s] = 0;
	vis[s] = 1;
	while(!q.empty()) {
		int k = q.front();
		q.pop();
		vis[k] = 0;
		for(int i = 0; i < g[k].size(); i++) {
			int v = g[k][i].to;
			int now = g[k][i].val;
			if(d[v] > d[k]+now) {
				d[v] = d[k]+now;
				if(!vis[v]) {
					q.push(v);
					vis[v] = 1;
				}
			}
		}
	}
}
```

   2. Dijkstra 算法$--$贪心处理正值边权
   
      - 把节点分为 $A$ 和 $B$ 两个集合，集合 $A$ 中的点已经找到了最短路，而集合 $B$ 没有。
      
      - 用一个 $d$ 数组记录每个节点的最短路，初始化起点 $s$ ，```d[s] = 0```。
      
      - 从集合 $B$ 中找一个 $k$ ，使得 $d_k$ 最小，放入集合 $A$ 。遍历 $k$ 出发的所有边，用```d[k]+g[k][i]```，更新 $B$ 中节点 $i$ 的 $d_i$ 。(```g[k][i]```是边 $k$ -> $i$ 的权值)。重复共 $n$ 次。
      
      - 当图中的边较多时，我们可以利用二叉堆对 Dijkstra 算法进行优化。把 $d$ 数组中的值放入堆中维护，不用遍历 $d$ 数组来求最小的 $d$ 。
      
      - 例：[P4779 【模板】单源最短路径（标准版）](https://www.luogu.com.cn/problem/P4779)
      
      - 参考：
      
```cpp
struct node {
	int id, val;
	node(int a,int b): id(a),val(b){}
	bool operator < (const node &k) const{
		return val > k.val;
	}
};
priority_queue <node> pq;

void Dijkstra() {
	memset(d,0x3f,sizeof(d));
	d[s] = 0;
	pq.push(node(s,d[s]));
	for(int t = 1, k; t <= n; t++) {
		while(vis[pq.top().id]) pq.pop();
		k = pq.top().id;
		vis[k] = 1;
		for(int i = 0; i < g[k].size(); i++) {
			int v = g[k][i].to;
			int now = g[k][i].w;
			if(d[v] > d[k]+now) {
				d[v] = d[k]+now;
				pq.push(node(v,d[v]));
			}
		}
	}
}
```

   - 练：[单源次短路及解析](https://www.luogu.com.cn/blog/Tommy-Keen/p2865-usaco06novroadblocks-g-ti-xie#)

二、多源最短路

   - Floyd 算法$--$动态规划的思想迁移
   
     - 一个最短路径（起点 $i$ ,终点 $j$ )上的子路径(起点 $x$ ,终点 $y$ )也是 $x,y$ 之间的最短路径？
     
       可以用反证法证明：如果这条 $x$ 到 $y$ 的子路径不是 $x,y$ 之间的最短路径，那可以把这个子路径替换成 $x,y$ 之间的最短路径，使得 $i$ 到 $j$ 的最短路径缩短，这与假设矛盾。
     
       **可以枚举中间节点来把短的最短路径延长为更长的最短路径。**
     
     - 令 ```d[i][j][k]``` 表示从 $i$ 到 $j$ ,路上只经过编号 $1$-$k$ 的节点的最短路径值。
     
     - 状态转移方程：  ```d[i][j][k] = min(d[i][k][k-1]+d[k][j][k-1]，d[i][j][k-1])``` 。前一个值对应 $i$ 和 $j$ 以 $k$ 为中间节点连接新最短路，后一个值对应保持 $i$ 和 $j$ 的最短路值不变。
     
     - 边界条件：
     
       ```d[i][i][0] = d[i][i][1] =...d[i][i][n] = 0```
     
       ```d[i][j][0] = g[i][j]```, 如果 $i,j$ 之间有边
     
       ```d[i][j][0] = 0x3f3f3f3f```，如果 $i,j$ 之间没有边
     
     - Floyd 算法能处理负权边。
     
     - 怎么发现负环？
     
       如果存在负环，则对于环上的点 $k$ ，```d[k][k]```会被更新；而不存在负环的图， ```d[i][i]``` 不可能被更新。因此一旦发现 ```d[i][i]``` 被更新说明负环出现，多源最短路不存在。
     
   - 对于一个有向图，求任意两点间的连通性，被称为有向图的传递闭包问题。可以由 floyd 算法求解。
   
     - 把floyd算法中的 ```f[i][j]``` 定义改为 ```f[i][j] = 1``` 表示可以从 $i$ 出发到达 $j$ ， ```f[i][j] = 0``` 表示无法到达。
     
     - 转移方程为：```f[i][j] = f[i][j] | (f[i][k] & f[k][j])```。
     
     - 传递闭包优化写法：
     
```cpp
bitset<N> f[N];
for (int k=1;k<=n;++k)
	for (int i=1;i<=n;++i)
		if (f[i][k]) f[i] |= f[k]
```

# 负环和差分约束算法

一、SPFA算法的优化

   - 对于 BFS 实现的 SPFA 算法，可以使用双端队列进行优化，一般称为 SLF(Small Label First) 优化。
   
   - 新入队的元素，如果其最短路值小于队首元素，从队头入队；否则，从队尾入队。
   
   - 减少次优解的扩展几率。
   
   - 没有参考，我不想打。

二、负环判定的优化

   - 当SPFA算法中一个点的d值被更新达到n次，说明图中存在负环。通常情况下，单纯利用这个方法求负环效率较低，我们应该考虑一下剪枝方法。
   
   - 如果存在负环，$\text{一个点的最短路值} < n*minedge$ ，其中 $minedge$ 是图中最小的边权，且 $minedge<0$ 。

   - 剪枝：如果当前节点的最短路值小于 $n*minedge$ ，说明存在负环。
   
   - 例：[P3385 【模板】负环](https://www.luogu.com.cn/problem/P3385)

三、差分约束算法
   - 题意：
   
     给出一组包含 $m$ 个不等式，有 $n$ 个未知数的形如：
   
      <div>$$\begin{cases}
      x_{c_1}-x_{c_1^\prime}\le y_1 \\\\
      x_{c_2}-x_{c_2^\prime}\le y_2 \\\\
      \vdots \\\\
      x_{c_m}-x_{c_m^\prime}\le y_m
      \end{cases}$$</div>
     
     的不等式组，求任意一组满足这个不等式组的解。
     
     $1≤n,m≤5×10^3$，$-10^4≤y≤10^4$，$1≤c,c'≤n$，$c≠c'$
     
   - 思路：
   
     1. 如果把 $xi$ 看作最短路值数组 $d$ 中的 $d_i$ 。移项后有 $ d_i <= d_j + c$ ，这个式子是我们在一个图的最短路**计算完毕**后，图上**两点及它们之间的边权**所满足的一个三角不等式。
     
     2. 因此，我们**把未知数看作图上的节点**，对形如 $d_x <= d_y + c$ 的不等式，在图上**连一条 $y$->$x$ 的权为 $c$ 的边**，然后对图做一次**单源最短路**，即可求的未知数的**一组合法取值**。
     
     3. 不难发现，按照以上方法建立的有向图并不能确保是连通的，为了求出每个点的最短路，我们还需要另外建立一个虚拟节点 $0$ （也可以叫超级源点），我们 $0$ 号节点作为最短路的起点。 $0$ 号节点有到所有 $1$-$n$ 号节点的有向边，边权一般设置成 $0$ ，不会影响解的正确性。
     
     4. 如果建立的图上存在负环，最短路不存在，说明什么？此时原不等式组存在矛盾，无解。当最长路而图上存在正环时，同理。

     5. 建立差分约束图之后，可以选择两种方式求解：
        - 用BFS实现的SPFA算法，一边判负环，一边求单源最短路。
        - 先用DFS判负环，然后用BFS实现的SPFA算法求单源最短路。
     
        第二种做法虽然更麻烦，但可以避免BFS判环超时，应该根据情况选择对策。
     
   - 例：[P5960 【模板】差分约束算法](https://www.luogu.com.cn/problem/P5960)

# 最小生成树
一、 定义：

在一个有 $V$ 个顶点的无向连通图中，选取 $V-1$ 条边，如果这些边能使所有点连通，所得到的子图称为原图的一棵生成树。在一个带权无向连通图中，边权和最小的生成树被称为最小生成树( MST )。

二、 判断：

如果图上存在唯一的权值最小边，它一定属于 MST ？

是的。因为：假设MST不包含最小权值边 $x$-$y$ ，用 $x$-$y$ 替换 MST 上 $x$ 到 $y$ 路径上的任何一边都可以得到更小的生成树，产生矛盾。

三、 kruskal 算法

   - 贪心选择——对于 $n$ 个点的带权无向连通图，依次选择 $n-1$ 条边组成 MST ，选边遵循两个原则：
   
      1. 这条边不能在树上产生环。
      
      2. 这条边的权值应当尽可能小。（即先把边按照权值顺序排序，依次选择）
      
   - 算法步骤：
   
      1. 把图上的所有边（假设有 $m$ 条）按照权值从小到大排序。
      
      2. 初始化 MST 的边数 $cnt = 0$。
      
      3. 按照下标顺序从小到大依次判断每一条边：
         
         - 如果边在 MST 上产生环，跳过；
         
         - 如果边不在 MST 上产生环，把这条边加入 MST ，$cnt$++。
      
      4. 如果 $cnt = n-1$ ，结束算法。
      
   - 如何判断一条边是否让 MST 产生环？
   
     维护一个关于点的并查集。
     
     - 当把一条边 $x$-$y$ 加入 MST 时，合并 $x,y$ 所在的集合。
     
     - 对于一条边 $x$-$y$ 如果 $x,y$ 原本就属于同一集合，说明边 $x$-$y$ 在 MST 上产生环。
       
     
     并查集的合并和查询可以视为 $O(1)$。
     
   - 因此，Kruskal算法的时间复杂度为 $O(m \log m+n)= O(m \log m)$。
   
   - 参考：

```cpp
//O(mlogm+n)
#include <bits/stdc++.h>
using namespace std;
const int N = 5e3+10;
const int M = 2e5+10;

int n,m,h[M],f[M];
struct Edge {
	int x, y, z;
	bool operator < (const Edge &k) const{
		return z < k.z;
	}
} g[M];

int find(int x) {
	if(f[x] != x) f[x] = find(f[x]);
	return f[x];
}

inline void gather(int x,int y) {
	int fx = find(x),fy = find(y);
	if(h[fx] > h[fy]) swap(fx,fy);
	f[fx] = fy;
	h[fy] = max(h[fy],h[fx]+1);
}

inline void input() {
	scanf("%d%d",&n,&m);
	for(int i = 1; i <= m; i++) {
		f[i] = i; h[i] = 1;
	}
	for(int i = 1; i <= m; i++) {
		scanf("%d%d%d",&g[i].x,&g[i].y,&g[i].z);
	}
}

inline void kruskal() {
	sort(g+1,g+m+1);
	int cnt = 0, sum = 0;
	for(int i = 1; i <= m; i++) {
		if(find(g[i].x) == find(g[i].y)) continue;
		gather(g[i].x,g[i].y);
		sum += g[i].z;
		if(++cnt == n-1) break;
	}
	if(cnt < n-1) puts("orz");
	else printf("%d\n",sum);
}

int main() {
	input();
	kruskal();
	return 0;
}
```

四、 Prim 算法

   - 在学习 Prim 算法前，请先回顾一下 Dijkstra 算法。

   - 贪心选择——对于 $n$ 个点的带权无向连通图，尝试从任一点出发，逐步构造出一棵 n 个节点的最小生成树，逐步为树添加新边，并遵循两个原则：
   
     - 新的边必须把一个新的点连入树。（即边的两个端点一个在树上，一个不在）
     
     - 边的权值尽可能小。
     
   - 算法步骤：
     
       1. 维护一个数组 $d$ ， $d_i$ 表示能把节点 $i$ 连入树的最小边权，初始 $d_i = ∞$。
       
          维护一个数组 $vis$ ， $vis_i=1$ 表示节点 $i$ 在树上，初始 $vis_i = 0$。

       2. 选择一个构造树的起点 $s$ ，令 $d_s = 0$ 。

       3. 求出一个 $k$ ，满足 $vis_k = 0$ 且 $d_k$ 最小，令 $vis_k = 1$ 。

       4. 用 $k$ 更新所有与 $k$ 相连的节点的 $d$ 。

       5. 重复 $3$ ， $4$ 直到对于任意 $i∈[1,n]$，$vis_i=1$ 。
       
   - 朴素 Prim 算法的复杂度为 $O(n^2)$ 。
   
   - 参考：

```cpp
//O(n^2)
#include <bits/stdc++.h>
using namespace std;
const int N = 5e3+10;

int n,m,d[N],g[N][N];
bool vis[N];

inline void input() {
	memset(g,0x3f,sizeof(g));
	scanf("%d%d",&n,&m);
	for(int i = 1, x, y, z; i <= m; i++) {
		scanf("%d%d%d",&x,&y,&z);
		g[x][y] = min(g[x][y],z);
		g[y][x] = min(g[y][x],z);
	}
}

inline void Prim() {
	int sum = 0;
	memset(d,0x3f,sizeof(d));
	int s = 1;
	d[s] = 0;
	bool ok = 1;
	for(int i = 1; i <= n; i++) {
		int k = -1;
		for(int j = 1; j <= n; j++) {
			if(!vis[j] && (k==-1||d[k]>d[j])) {
				k = j;
			}
		}
		if(d[k] == 0x3f3f3f3f) {
			ok = 0;
			break;
		}
		sum += d[k];
		vis[k] = 1;
		for(int j = 1; j <= n; j++) {
			if(g[k][j] < d[j]) d[j] = g[k][j];
		}
	}
	if(!ok) puts("orz");
	else printf("%d\n",sum);
}

int main() {
	input();
	Prim();
	return 0;
}
```

   - 例：
   
     [P3366 【模板】最小生成树](https://www.luogu.com.cn/problem/P3366)
   
   - 练：
   
     [两道最小生成树的妙题](https://www.luogu.com.cn/blog/Tommy-Keen/guan-yu-liang-dao-zui-xiao-sheng-cheng-shu-di-miao-ti)
   
     [严格次小生成树及解析](https://www.luogu.com.cn/blog/Tommy-Keen/p4180-bjwc2010-yan-ge-ci-xiao-sheng-cheng-shu-ti-xie)
     
     [最小生成树计数及解析](https://www.luogu.com.cn/blog/Tommy-Keen/p4208-jsoi2008-zui-xiao-sheng-cheng-shu-ji-shuo-ti-xie)

# 欧拉道路
一、 定义：

从某个点出发，不重复的遍历连通图中的所有边的路径。为了纪念欧拉，后人把满足以上条件的路径称为欧拉道路。假设欧拉道路存在，显然，除了起点和终点外，路径上所有的点都会被“到达”的次数和“离开”的次数应当相等。

二、 判断**连通**图存在欧拉道路的充要条件：

- 对于无向图：
  1. 图中所有点的度均为偶数（起终点相同）。
  2. 图中有两个点的度为奇数，其余为偶数（起终点不同）。

- 对于有向图：
  1. 图中所有点的入度 $=$ 出度（起终点相同）。
  2. 图中有一个点的入度 $=$ 出度 $+$ $1$ ，一个点的入度 $=$ 出度 $-$ $1$ ，其余点的入度 $=$ 出度（起终点不同）。
  
- 起终点相同的欧拉道路，习惯上称为**欧拉回路**。

  存在欧拉回路的图，也被叫做**欧拉图**。
  

三、 求法：

1. 判断图的连通性 $O(n)$ 。
2. 判断存在欧拉道路的充要条件 $O(n)$ 。
3. 遍历图并记录欧拉道路 $O(n^2)$ 。

- 对于第 $3$ 步，要注意正确选取遍历的出发点：
  
  如果图存在欧拉回路，可以从任意点出发；
  
  如果图只有欧拉道路，无向图必须从奇数度点出发，有向图必须从出度大于入度的唯一点出发。
  
# 强连通分量
一、 定义：

一个**有向图**中，任意两点都可以互相到达的子图称为原图的**强连通（ Strongly Connected , SC ）子图**，图的**极大**的强连通子图是图的**强连通分量**。

（极大：不存在包含当前子图的全部节点且总节点数更多的强连通子图）

二、 求法$--$ Tarjan 算法：

1. 性质：一个强连通分量由图上多个相互存在公共点的环组成。

   可以通过合并一个节点所在的环（标记一个节点所在的所有环，属于同一个强连通分量），来求出强连通分量。
   
2. Tarjan 算法中为了找环，定义了 $dfn$ 和 $low$ 两个数组,以及栈 $s$ ：
   
   - 访问到节点 $i$ ，就把 $i$ 入栈。
   
   - 访问次序 $dfn_i$ 表示节点 $i$ 的入栈次序（第几个被访问到）。
   
   - 追溯值 $low_i$ 表示从节点 $i$ 的在搜索树上的后代出发或 $i$ 经过非搜索树上的边能到达在栈 $s$ 中的点的最小 $dfn$ 。
   
   -  $low$ 可以确保找到最大的环，而不是大环上的小环。
   
   - 如果回溯时当前点的 $low$ 和 $dfn$ 相等，说明找到了一个强连通分量，弹出栈中的所有节点，并标记这些节点属于同一强连通分量。
   
   - low 可以按如下规则求出：

     - 初始访问到一个节点 $i$ 时，令 ```low[i] = dfn[i]```

     - 对于当前访问的节点 $i$ ，当存在边 $i$-$j$ ，且 $j$ 没有被访问过时，递归访问 $j$ ，并且在回溯时令 ```low[i] = min(low[i],low[j])``` ，因为 $i$ 后代能到达的点， $i$ 一定也可以到达。

     - 对于当前访问的节点 $i$ ，当存在边 $i$-$j$ ，且 $j$ 已经被访问过时，根据 $low$ 数组的定义令```low[i] = min(low[i],dfn[j])```。
   
3. 总结实现步骤如下：

   - 如果图中还存在未被访问过的节点，从任意未被访问过的节点出发做 DFS 。
   
   - 当前节点 $u$ 被访问时，把 $u$ 压入栈 $s$ ，记录 $dfn_u$ ，并初始化 ```low[u] = dfn[u]``` 。
   
   - 如果当前节点 $u$ 有一条连向一个未访问节点 $v$ 的边，则 ```low(u) = min(low(u),low(v))``` 。
   
   - 如果当前节点 $u$ 有一条连向一个还在栈 $s$ 中的节点 $v$ 的边，则 ```low(u) = min(low(u), dfn(v))``` 。
   
   - 遍历 u 出发的所有边后，如果 ```low[u] = dfn[u]``` ，把栈 $s$ 中 $u$ 及以上的元素出栈，并标记它们属于同一个强连通分量。
   
   - $O(n+m)$
   

三、 缩点：

1. 题意：

   给定一个 $n(n<=10^4)$ 个点 $m(m<=10^5)$ 条边有向图，每个点有一个权值（大于 $0$ ），求一条路径，使路径经过的点权值之和最大。你只需要求出这个权值和。允许多次经过一条边或者一个点，但是重复经过的点，权值只计算一次。
   
   [P3387 【模板】缩点](https://www.luogu.com.cn/problem/P3387)

2. 思路：

   - 求出有向图的强连通分量后，我们可以把同一个强连通分量中的点合并为一个节点（缩点），得到的新图是一个 DAG （有向无环图）。缩点后，我们要把强连通分量有关的非内部边加入到新图。用邻接表存边时，会不可避免的产生重边问题，必要时可以通过排序去除这些重边。
   
   - 先对原图进行一次缩点(新点的权值 $=$ 分量内所有节点权值之和），然后在新图上令 $dp_i$ 表示到达节点 $i$ 的最大权值和，做一次 DP 即可。
   
     - 为什么一定要缩点后才能做DP？
     
     - 如果不缩点，由于图上可能有**环**存在，状态之间没有拓扑顺序，无法执行 DP 。
   
3. 参考：

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e4+10;
int tot,cnt,dfn[N],low[N],sc[N];
int n,m,a[N],f[N],ru[N],w[N];
vector <int> g[N],t[N];
stack <int> s;
queue <int> q;

void tarjan(int u) {//O(n+m)
	s.push(u);
	low[u] = dfn[u] = ++tot;
	for(int i = 0; i < g[u].size(); i++) {
		int v = g[u][i];
		if(!dfn[v]) {
			tarjan(v);
			low[u] = min(low[u],low[v]);
		} else if(!sc[v]){
			low[u] = min(low[u],dfn[v]);
		}
	}
	if(low[u] == dfn[u]) {
		sc[u] = ++cnt;
		while(s.top() != u) {
			sc[s.top()] = cnt;
			s.pop();
		}
		s.pop();
	}
}

inline void input() {
	scanf("%d%d",&n,&m);
	for(int i = 1; i <= n; i++) {
		scanf("%d",&a[i]);
	}
	for(int i = 1, u, v; i <= m; i++) {
		scanf("%d%d",&u,&v);
		g[u].push_back(v);
	}
}

inline void build() {
	for(int i = 1; i <= n; i++) {
		if(!dfn[i]) tarjan(i);
	}
	for(int i = 1; i <= n; i++) {
		for(int j = 0; j < g[i].size(); j++) {
			if(sc[g[i][j]] != sc[i]) {
				t[sc[i]].push_back(sc[g[i][j]]);
				ru[sc[g[i][j]]]++;
			}
		}
	}
	for(int i = 1; i <= n; i++) {
		w[sc[i]] += a[i];
	}
}

inline void solve() {
	for(int i = 1; i <= cnt; i++) {
		if(!ru[i]) {
			f[i] = w[i];
			q.push(i);
		}
	}
	while(!q.empty()) {
		int k = q.front();
		q.pop();
		for(int i = 0; i < t[k].size(); i++) {
			ru[t[k][i]]--;
			f[t[k][i]] = max(f[t[k][i]],f[k]+w[t[k][i]]);	
			if(!ru[t[k][i]]) {
				q.push(t[k][i]);
			}
		}
	}
	printf("%d\n",*max_element(f+1,f+cnt+1));
}

int main() {
	input();
	build();
	solve();
	return 0;
}
```

4. 拓展：

   - 例：[P4782 【模板】2-SAT 问题](https://www.luogu.com.cn/problem/P4782)
   
   - 题意：
   
     有 $n(n≤10^6)$ 个布尔变量，另有 $m(m≤10^6)$ 个需要满足的条件，每个条件的形式都是 「 $x_i$ 为 $true / false$ 或 $x_j$ 为 $true / false$ 」。比如 「 $x_1$ 为真或 $x_3$ 为假」、「 $x_7$ 为假或 $x_2$ 为假」。
     
     2-SAT 问题的目标是给每个变量赋值使得所有条件得到满足。
   
   - 思路：
   
     - 对于每个变量 $x$ ，我们建立两个点：$x$ ， $\neg x$ ， 分别表示变量 $x$ 取 $true$ 和取 $false$ 。所以，图的节点个数是两倍的变量个数。在存储方式上，可以给第 $i$ 个变量标号为 $i$ ，其对应的反值标号为 $i+n$ 。对于每个同学的要求，可以转换：
     
       $(a \vee b) = \neg a \rightarrow b \wedge \neg b→a$
     
       $(\neg a \vee b) = a \rightarrow b \wedge \neg b→ \neg a$
     
       $(\neg a \vee \neg b) = a \rightarrow \neg b \wedge b→ \neg a$
     
       变量前加 $\neg$  表示「不」，即「假」， $\vee$ 表示或， $\wedge$ 表示与。之后按照箭头的方向建有向边就好了。
     
     - 如果 $x$ 与 $\neg x$ 在同一强连通分量内部，一定无解。反之，就一定有解了。
   
     - 对于一组布尔方程，可能会有多组解同时成立。要怎样判断给每个布尔变量赋的值是否恰好构成一组解呢？
   
     - 当 $x$ 所在的强连通分量的拓扑序在 $\neg x$ 所在的强连通分量的拓扑序之后取 $x$ 为真 就可以了。在使用  Tarjan 算法缩点找强连通分量的过程中，已经为每组强连通分量标记好顺序了——不过是反着的拓扑序。
   
   - 参考：
   
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 4e6+10;
int n,m,tot,cnt,dfn[N],low[N],sc[N];
vector <int> g[N];
stack <int> s;

void tarjan(int u) {
	s.push(u);
	low[u] = dfn[u] = ++tot;
	for(int i = 0; i < g[u].size(); i++) {
		int v = g[u][i];
		if(!dfn[v]) {
			tarjan(v);
			low[u] = min(low[u],low[v]);
		} else if(!sc[v]){
			low[u] = min(low[u],dfn[v]);
		}
	}
	if(low[u] == dfn[u]) {
		sc[u] = ++cnt;
		while(s.top() != u) {
			sc[s.top()] = cnt;
			s.pop();
		}
		s.pop();
	}
}

inline void input() {
	scanf("%d%d",&n,&m);
	for(int i = 1, xi, yi, a, b; i <= m; i++) {
		scanf("%d%d%d%d",&xi,&a,&yi,&b);
		if(!a && !b) {
			g[xi].push_back(yi+n);
			g[yi].push_back(xi+n);
		}
		if(!a && b) {
			g[xi].push_back(yi);
			g[yi+n].push_back(xi+n);
		}
		if(a && !b) {
			g[xi+n].push_back(yi+n);
			g[yi].push_back(xi);
		}
		if(a && b) {
			g[xi+n].push_back(yi);
			g[yi+n].push_back(xi);
		}
	}
}

inline void build() {
	for(int i = 1; i <= 2*n; i++) {
		if(!dfn[i]) tarjan(i);
	}
}

inline void solve() {
	puts("POSSIBLE");
	for(int i = 1; i <= n; i++) {
		if(sc[i] < sc[i+n]) printf("1 ");
		else printf("0 ");
	}
}

int main() {
	input();
	build();
	for(int i = 1; i <= n; i++) {
		if(sc[i] == sc[i+n]) {
			puts("IMPOSSIBLE");
			return 0;
		}
	}
	solve();
	return 0;
}
```

# 双连通

看 PPT 吧，我nm实在不想写了。

# 二分图

同上。

# Trie树

[去食用这个。](https://www.cnblogs.com/Silymtics/p/14255102.html)

下面给个模板：

```cpp
struct Trie{
	int tr[MAXN][26], node_cnt = 0;//字典树以及结点个数
	bool cnt[MAXN];//标记是否是某个字符串的结尾
	
	void insert(char *s){//插入操作
		int now = 0, len = strlen(s + 1);//now表示当前所在的结点，len表示字符串长度
		for(int i = 1; i <= len; ++i){
			int ch = s[i] - 'a';//取出要插入的字符
			if(! tr[now][ch]) tr[now][ch] = ++node_cnt;
			//如果这个字符未被插入，新建一个结点将其插入
			now = tr[now][ch];//now指针跳向tr[now][ch]指向的位置
		}
		cnt[now] = true;//在字符串完成时所在的结点处打上标记
	}
	
	int find(char *s){//查询操作
		int now = 0, len = strlen(s + 1);//意义同上
		for(int i = 1; i <= len; ++i){
			int ch = s[i] - 'a';
			if(!tr[now][ch]) return false;//如果没有遍历到的字符，直接返回false
			now = tr[now][ch];//now指针跳向tr[now][ch]指向的位置
		}
		return cnt[now];
		//注意这里不能直接返回true，有可能查询的只是某个串的前缀，比如在样例中查询her
	}
} trie;
```

------------

也许到这里就完结了，因为lg博客行数多了之后写着太卡。
