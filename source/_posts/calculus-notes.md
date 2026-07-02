---
title: 听着，所谓微积分
slug: calculus-notes
date: 2022-06-09 20:00:00
categories:
  - 2022
tags:
  - 知识
  - OI
mathjax: true
---
$\fcolorbox{red}{aqua}{How to Ace Calculus}$

# 一、引入

- $\csc\theta=\frac{1}{\sin\theta}\quad\sec\theta=\frac{1}{\cos\theta}$
- 以$r$为半径，以点$(a,b)$为圆心的圆的方程式：

  $\sqrt{(x-a)^2+(y-b)^2}=r$

  即：
  $\boxed{(x-a)^2+(y-b)^2=r^2}$
  
- 椭圆：
  $\frac{x^2}{a^2}+\frac{y^2}{b^2}=1$
  
  要想移动中心点至$(x_0,y_0)$，则变成：
  
  $\boxed{\frac{(x-x_0)^2}{a^2}+\frac{(y-y_0)^2}{b^2}=1}$
  
- $y^2-x^2=9$的图象：“**双曲线**”

# 二、极限

- $\forall\varepsilon>0,\exists\delta>0,$若$0<|x-x_0|<\delta,$则$|f(x)-A|<\varepsilon$,有：$\lim\limits_{x\to{x_0}}f(x)=A$

  特别地，$\forall\varepsilon>0,\exists M >0,$若$x>M,$则$|f(x)-A|<\varepsilon$,有：$\lim\limits_{x\to{\infty}}f(x)=A$

- 当极限存在时，是指左右两边的极限都存在，**而且相等**。

- 下面给出一些**不存在**的极限：

  $\lim\limits_{x\to2}\frac{1}{(x-2)^3}\quad\lim\limits_{x\to0}\sin\frac{1}{x}\quad\lim\limits_{x\to0}\frac{1+x^2}{x^2}$
  
  $\lim\limits_{x\to{2^+}}\frac{1}{x-2}=+\infty\quad\lim\limits_{x\to{2^-}}\frac{1}{x-2}=-\infty$
  
  注意，$\lim\limits_{x\to1}\frac{x-1}{x^2-1}=\lim\limits_{x\to1}\frac{1}{x+1}=\frac{1}{2}$存在其极限。
  
- $\boxed{\lim\limits_{x\to0}\frac{\sin x}{x}=1}$（当$x$逐渐靠近$0$时，两函数愈来愈相似）

- $\boxed{e^x=\lim\limits_{n\to\infty}(1+\frac x n)^n}$

- “$x$趋于无穷大”，即$x$的值**要多大有多大**。

- “这个极限没有定义”，即**不等于任何一个数**。

- （乱入）$PV=nRT$中，$R=8.314\ \boxed{J*mol^{-1}*K^{-1}}$

# 三、导数

- $f(x)$在$x=a$是连续的，条件有三：

  1.$\textcolor{red}{f(a)\text{是有定义的}}$
  
  2.$\textcolor{red}{\lim\limits_{x\to a}f(x)\text{存在}}$
  
  3.$\textcolor{red}{\lim\limits_{x\to a}f(x)=f(a)}$
  
- 函数$g(x)$在$x$处的导数，写成$g'(x)$，且定义为：

  $\boxed{g'(x)=\lim\limits_{h\to 0}\frac{f(x+h)-f(x)}{h}}$
  
  有的时候，导数$g'(x)$**无定义**。
  
- $f(x)$导数写法：$f'(x)/\frac{df}{dx}$等等。

  令$y=f(x)$，又有：$y'/\frac{dy}{dx}$等等。
  
- 幂法则：

  $\boxed{\frac{d}{dx}(x^n)=nx^{n-1}(n\in R)}$
  
  特别地，$\frac{d}{dx}(x)=1$
  
- 积法则：

  $\boxed{\frac{d}{dx}(fg)=f'g+fg'}$
  
- 商法则：

  $\boxed{\frac{d}{dx}(\frac f g)=\frac{f'g-fg'}{g^2}}$
  
- 三角函数的导数：

  $\boxed{\frac{d}{dx}(\sin x)=\cos x}$
  
  $\boxed{\frac{d}{dx}(\cos x)=\fcolorbox{red}{aqua}{-}\sin x}$
  
  $\boxed{\frac{d}{dx}(\tan x)=\sec^2 x}$
  
- $\textcolor{red}{f^{(n)}(x)}$指$f(x)$的$\textcolor{red}{n\text{阶}}$导数。

- 试证明$\frac{d}{dx}(\sin x)=\cos x$

  $\begin{aligned}
  \frac{d}{dx}(\sin x)
  & =\lim\limits_{h\to 0}\frac{f(x+h)-f(x)}{h} \\
  & =\lim\limits_{h\to 0}\frac{2\sin \frac h 2 \cos (x+\frac h 2)}{h} \\
  & =\lim\limits_{h\to 0}\frac{\frac{\sin \frac h 2}{\frac h 2}  \cos (x+\frac h 2)}{h} \\
  & =(\lim\limits_{h\to 0}\frac{\sin \frac h 2}{\frac h 2})(\lim\limits_{h\to 0}\cos (x+\frac h 2))\\
  & =1*\cos (x+0)\\
  & =\cos x
  \end{aligned}
  $
  
  其中，用到了[和差化积公式](https://baike.baidu.com/item/%E5%92%8C%E5%B7%AE%E5%8C%96%E7%A7%AF#1)，以及$\boxed{\lim\limits_{x\to0}\frac{\sin x}{x}=1}$的结论。
  
- 链式法则：

  $\boxed{\frac{d}{dx}f(g(x))=f'(g(x))g'(x)}$
  
  亦：$\boxed{\frac{df}{dx}=\frac{df}{du}\frac{du}{dx}}$
  
- “广义幂法则”：

  $\boxed{\frac{d}{dx}(g(x))^n=n(g(x))^{n-1}g'(x)}$
  
- 例题(复杂的)令$k(x)=[\cos (\sqrt x - x)]^3$,求$k'(x)$.

  这个题目相当刁钻，但是我们不难看出，题目所给的函数是由几个小零件凑起来的:先是$\sqrt x - x$取余弦，然后整团东西再取三次方所以我们可以令:
  
  $f(x)=\sqrt x-x$
  
  $g(x)=\cos x$

  $h(x)=x^3$
  
  于是$h(x)= h(g(f(x)))$
  
  那么$k'(x)$等于什么呢?若把前面积累起来的知识重复运用，我们就可以得到:
  
  $
  \begin{aligned}
  k'(x)
  & =(h(g(f(x))))'\\
  & =h'(g(f(x)))(g(f(x)))'\\
  & =h'(g(f(x))g'(f(x))f'(x).
  \end{aligned}
  $
  
  在上面的算式里，我们其实只需把链式法则应用到第二行的“尾巴”$g(f(x))$上.这时你可能会问:“什么时候我才知道演算已经完成了呢?”答案很简单，如果你看到还有$\ \ '\ \ $这个微分符号紧跟在括号后面，像第一行跟第二行那样，就表示还有待努力;一旦$\ \ '\ \ $全都紧跟在英文字母后头，就表示你做完了.现在，再把式子中各个项目分别计算出来:
  
  $f'(x)=\frac 1 2(x)^{-\frac 1 2}-1\colorbox{red}{}$
  
  $g'(x)=\fcolorbox{red}{aqua}{-}\sin x$
  
  $h'(x)=3x^2$
  
  $g'(f(x))=-\sin(\sqrt x-x)\colorbox{red}{}$

  $g(f(x))=\cos(\sqrt x-x)$

  $h'(g(f(x)))=3(\cos(\sqrt x-x))^2\colorbox{red}{}$
  
  最后，$k'(x)$就是.上面标红的三行式子的乘积:
  
  $k'(x)=3(\cos(\sqrt x-x))^2[-\sin(\sqrt x-x)][\frac 1 2(x)^{-\frac 1 2}-1]$
  
  在你想知道是否还有时间退选微积分课之前，你应该先有个心理准备:理论上，教授有可能叫你去微分一个包括了 7个函数的合成函数，因此你需要连续应用6次链式法则,浪费一大堆纸，才能找出答案.不过话说回来,教授出此考题，其实是非常不切实际的，因为总得有个可怜人去批改考卷，他自己当然可以避免这项苦差事，但是就连他的硕士生和博士生，也不会高高兴兴地替他仔细追踪检查你那密密麻麻的6大张算式.

  所以，如果你的教授不是不近情理的神经病，考题最多只会要求你使用两次链式法则;即使这样，你班上至少有一半的同学，还压根儿不会应用两次链式法则呢.因此你最好多看看最后那个例题，做熟练一些，你的成绩就会在全班的前50%中啦.
  
# 四、极值

- 局部极大值与局部极小值所在点，导致可能**不存在**，也可能**等于0**。

- 若$x=a$是个临界点，且$f'(a)=0$，则：

  1.若$f''(a)>0$，则函数在$x=a$有局部极**小**值
  
  2.若$f''(a)<0$，则函数在$x=a$有局部极**大**值
  
- $f''(x)=(f'(x))'$，即一阶导数的变化率，亦为切线斜率的变化率。

  若$f''(x)>0$，则切线斜率递增，也就表示函数图形的凹口向上；若$f''(x)<0$，则斜率正在递减，则凹口向下。
  
- 当函数有一个局部极大值或局部极小值，而且若它的导数存在，则导数为0。所以, 一个函数的绝对极大值或极小值，就在导数为0或导数不存在的点上(这两种点都是临界点),否则就是在定义域的头尾两端点上。

- $\boxed{f(x+\varDelta x)\thickapprox f(x)+f'(x)\varDelta x}$

# 五、微分定理

- **介值定理**：如果在$[a, b]$区间上有一个连续函数$f$,且$p$是介于$f(a)$跟$f(b)$之间的任何一个函数值，则在$[a, b]$区间上必须存在一个数$c$，使得$f(c)=p$

- 罗尔定理：若函数$f(x)$在$[a, b]$区间上连续而且可微，且若$f(a)=0$，$f(b)=0$, 则在区间$[a, b]$内必须存在一点$c$,使得$f(c)=0$

  亦：在函数$f(x)$的曲线上**必有一条切线，与该曲线两端点的连线平行**
  
- **中值定理**：若函数$f(x)$在$[a,b]$区间上连续且可微，则在该区间内必存在一点$c$,使$\boxed{f'(c)=\frac{f(b)-f(a)}{b-a}}$

# 六、积分

- $\int x^n\mathrm{d}x=\frac{x^{n+1}}{n+1}+C$

  当$n=-1$时，$\int \frac 1 x\mathrm{d}x=\ln |x|+C$
  
- $\int \cos x \mathrm{d}x=\sin x+C$

  $\int \sin x \mathrm{d}x=\fcolorbox{red}{aqua}{-}\cos x+C$
  
  $\int \sec^2 x\mathrm{d}x=\tan x+C$
  
- $\int f'(u(x))\mathrm{d}x=f(u(x))+C$

- [积分表](https://baike.baidu.com/item/%E7%A7%AF%E5%88%86%E8%A1%A8/6384165)

- 微积分基本定理：

  1.$\boxed{\int_a^b f(x)\mathrm{d}x=F(b)-F(a)}$
  
  （式子中的$F(x)$为函数$f(x)$的**不定积分**或**反导函数**）
  
  2.$\boxed{\frac d {dx} \int_a^x f(t)\mathrm{d}t=f(x)}$
  
- 定积分的一些基本法则：

  1.$\boxed{\int_a^b f(x)\mathrm{d}x=-\int_b^a f(x)\mathrm{d}x}$
  
  2.$\boxed{\int_a^b cf(x)\mathrm{d}x=c\int_a^b f(x)\mathrm{d}x}$
  
  3.$\boxed{\int_a^c f(x)\mathrm{d}x=\int_a^b f(x)\mathrm{d}x+\int_b^c f(x)\mathrm{d}x}$
  
  证明方法：将积分想象成曲线下的**面积**
  
- 矩形法（**黎曼和**）

  $\int_a^b f(x)\mathrm{d}x\thickapprox \frac {b-a} n[\displaystyle\sum_{i=0}^{n-1}f(x_i)]$
  
  $\int_a^b f(x)\mathrm{d}x\thickapprox \frac {b-a} n[\displaystyle\sum_{i=1}^{n}f(x_i)]$
  
- 中点法

  $\int_a^b f(x)\mathrm{d}x\thickapprox \frac {b-a} n[\displaystyle\sum_{i=1}^{n}f(\frac {x_{i-1}+x_i}{2})]$
  
- 梯形法

  $\int_a^b f(x)\mathrm{d}x\thickapprox \frac {b-a} n[\frac{f(x_0)}{2}+\displaystyle\sum_{i=1}^{n-1}f(x_i)+\frac {f(x_n)}{2}]$
  
- 辛普森法

  $\int_a^b f(x)\mathrm{d}x\thickapprox \frac {b-a} {3n}[f(x_0)+4f(x_1)+2f(x_2)+...+2f(x_{n-2})+4f(x_{n-1})+f(x_n)]$
  
  括号内每个$f(x_i)$的系数有一个固定的模式，即为：
  
  $1,4,2,4,2,4,...,2,4,2,4,1$
  
- 定积分的**技术定义**

  $\boxed{\int_a^b f(x)\mathrm{d}x=\lim\limits_{n\to \infty}\frac {b-a} n[\displaystyle\sum_{i=1}^{n}f(x_i)]}$
  
  令$\varDelta x=\frac {b-a}n$，则可化为：
  
  $\boxed{\int_a^b f(x)\mathrm{d}x=\lim\limits_{n\to \infty}\displaystyle\sum_{i=1}^{n}f(x_i) \varDelta x}$
  
  最复杂的版本：
  
  $\boxed{\int_a^b f(x)\mathrm{d}x=\lim\limits_{n\to \infty}\displaystyle\sum_{i=1}^{n}f(c_i)(x_i-x_{i-1})}$
  
  式子里的$c_i$，为子区间$[x_{i-1},x_i]$上的任何一点。
  
- $F(b)-F(a)=\boxed{F(x)|_a^b}$
  
- $\because \boxed {\frac d {dx}(e^x)=e^x}$

  $\therefore \boxed{\int e^x\mathrm{d}x=e^x+C}$
  
  $\therefore \boxed{\int e^{kx}\mathrm{d}x=\frac{e^{kx}}{k}+C(k\not = 0)}$
  
- $\boxed{\frac d {dx}(\ln x)=\frac 1 x}$

- 关于$dx$

  $d(f(x))=f'(x)dx$
  
  $d(x)=1*\varDelta x$
  
  $d(x)=\varDelta x$
  
  （不绝对正确，但可以这么理解）
  
- $e^{ix}=\cos x + i \sin x$

- $\varGamma (x)=\int_0^{+\infty} t^{x-1}e^{-t}\mathrm{d}t$（$x$是**不为非正整数**的复数）

  其实，$\varGamma (x)=(x-1)!(x\in N^*)$
  
- $\frac d {dx}[\ln g(x)]=\frac{g'(x)}{g(x)}$

- $\frac d {dx}(\log_bx)=\frac 1 {x\ln b}$

- $\boxed{\int \frac 1 x \mathrm{d}x=\ln x +C}$

- 例：求$f(x)=(x^2-3)(x^3-4)(x^7-5)(x^2-6)$的导数

  $
  \begin{aligned}
  ln f(x)
  & =ln[(x^2-3)(x^3-4)(x^7-5)(x^2-6)]\\
  & =\ln (x^2-3)+\ln(x^3-4)+\ln(x^7-5)+\ln(x^2-6)
  \end{aligned}
  $
  
  $\frac{f'(x)}{f(x)}=\frac{2x}{x^2-3}+\frac{3x^2}{x^3-4}+\frac{7x^6}{x^7-5}+\frac{2x}{x^2-6}$
  
  $
  \begin{aligned}
  f'(x)
  & =f(x)\bigg (\frac{2x}{x^2-3}+\frac{3x^2}{x^3-4}+\frac{7x^6}{x^7-5}+\frac{2x}{x^2-6}\bigg )\\
  & =(x^2-3)(x^3-4)(x^7-5)(x^2-6)(\frac{2x}{x^2-3}+\frac{3x^2}{x^3-4}+\frac{7x^6}{x^7-5}+\frac{2x}{x^2-6})
  \end{aligned}
  $
  
- “指数增长”/“指数衰退”方程式：

  $\because \frac{dN}{dt}=kN$
  
  $\therefore \frac{dN}{N}=kdt$
  
  $\therefore \int\frac{dN}{N}=\int kdt$
  
  $\therefore \ln |N|=kt+C$
  
  $\therefore |N|=e^{kt+C}=e^{kt}e^C=Ae^{kt}$
  
  $\because t=0$时，$N(0)=Ae^{0k}=A$
  
  $\therefore\frac{dN}{dt}=kN$这个微分方程的解的一般式为：
  
  $N(t)=N(0)e^{kt}$
  
- 若投资$P$元，为期$t$年，年利率为$r$，每年计息$n$次，则等到$t$年期满，钱数为$P(1+\frac r n)^{nt}$

  若投资$P$元，年利率为$r$，为期$t$年，且以**连续复利计息**，则到期时的本利和为$Pe^{rt}$
  
- 分部积分法：

  把积法则反过来：
  
  $\int u(x)v'(x)\mathrm{d}x=u(x)v(x)-\int u'(x)v(x)\mathrm{d}x$
  
  或写得更简单些：
  
  $\int uv'\mathrm{d}x=uv-\int u'v\mathrm{d}x$
  
  将下面两式代入：
  
  $du=\frac{du}{dx}dx=u'dx$
  
  $dv=\frac{dv}{dx}dx=v'dx$
  
  则有了分部积分法最常见的形式：
  
  $\boxed{\int u\mathrm{d}v=uv=\int v\mathrm{d}u}$
  
  $\Delta$如何确定$u$和$dv$
  
  $\quad$1.$dv$是你**能积分**的项
  
  $\quad$2.$\int v\mathrm{d}u$理应比原来的积分**更好解**
  
- 三角代换法

  $\because \sin^2\theta+\cos^2\theta=1$
  
  
  $
  \begin{aligned}
  \therefore
  & \frac { \sin^2\theta}{ \sin^2\theta}+\frac{\cos^2\theta}{ \sin^2\theta}=\frac 1{ \sin^2\theta}\\
  & \frac { \sin^2\theta}{ \cos^2\theta}+\frac{\cos^2\theta}{ \cos^2\theta}=\frac 1{ \cos^2\theta}
  \end{aligned}
  $
  
  即有：
  
  $
  \begin{aligned}
  & \boxed{1+\cot^2\theta=\csc^2\theta}\\
  & \boxed{\tan^2\theta+1=\sec^2\theta}
  \end{aligned}
  $
  
- 部分分式积分法：比较基础，不多阐述
  
# 七、常犯的几个错误

- $\frac d{dx}2^x\not =x^12^{x-1}$

  正确的是：$\frac d{dx}2^x=2x(\ln 2)$
  
  幂法则只能应用在**底数为变数**，**指数为常数**的函数。
  
- $\frac d{dx}\cos x\not = \sin x$

  $\frac d{dx}\cos x = -\sin x$
  
- $\frac d{dx}(\frac f g)\not=\frac{fg'-gf'}{g^2}$

  $\frac d{dx}(\frac f g)=\frac{gf'-fg'}{g^2}$
  
- $\frac d{dx}(\ln 3)=\frac d{dx}(e)=\frac d{dx}(\sin \frac \pi 2)=0$，因为它们都是常数。

- $\int x\mathrm{d}x\not=\frac {x^2} 2$

  $\int x\mathrm{d}x=\frac {x^2} 2+C$
  
- $\int x\sqrt x\mathrm{d}x$

  $\because x\sqrt x=x^{\frac 3 2}$，$\therefore$使用幂法则，不用分部积分法或代换法。
  
- 求$\lim\limits_{x\to2}\frac{x-2}{\sqrt{x+2}-2}$

  $
  \begin{aligned}
  \frac{x-2}{\sqrt{x+2}-2}
  &=\frac{x-2}{\sqrt{x+2}-2}*\frac{\sqrt{x+2}+2}{\sqrt{x+2}+2}\\
  &=\frac{(x-2)(\sqrt{x+2}+2)}{x+2-4}\\
  &=\frac{(x-2)(\sqrt{x+2}+2)}{x-2}\\
  &=\sqrt{x+2}+2
  \end{aligned}
  $
  
  $\therefore \lim\limits_{x\to2}\sqrt{x+2}+2=\sqrt{2+2}+2=4$
  
- 隐微分题特点：

  1.题目方程式无法写成$y=f(x)$的形式
  
  2.计算$(x_0,y_0)$上的$\frac{dy}{dx}$，且$x_0,y_0$已知
  
# 八、一些求积分的问题

- 代换法： $\int x^2e^{x^3}\mathrm{d}x\quad\int x\sqrt{x^2+1}\mathrm{d}x\quad\int \sin^3 x\cos x\mathrm{d}x$

- 分部积分： $\int \ln x\mathrm{d}x\quad\int x\ln x\mathrm{d}x\quad\int xe^x\mathrm{d}x\quad\int x\sin x\mathrm{d}x\quad\int e^x\sin x\mathrm{d}x$

- 部分分式：
  $\int \frac 1 {x^2-1}\mathrm{d}x\quad\int \frac{2x-3}{x^2-5x+6}\mathrm{d}x\quad$
  
- 三角代换： $\int \frac 1 {x^2+1}\mathrm{d}x\quad\int \frac {1}{\sqrt{1-x^2}}\mathrm{d}x\quad$

# 九、一些有趣的词

- 笛卡尔平面$(Cartesian\;plane)$ :亦称$Cartesia$小国的空军势力范围。

- 芝诺$(Zeno\;of\;Elea)$：任何一本微积分英文词典里的最后一个词条，他因[芝诺悖论](https://baike.baidu.com/item/%E8%8A%9D%E8%AF%BA%E6%82%96%E8%AE%BA)$(Zeno's\;paradox)$而出名。

- [碳定年法](https://baike.baidu.com/item/%E7%A2%B3%E5%AE%9A%E5%B9%B4%E6%B3%95)：考题素材之宠。

- 双曲函数：

  双曲正弦函数：$\boxed{\sinh x=\frac{e^x-e^{-x}}2}$
  
  双曲余弦函数：$\boxed{\cosh x=\frac{e^x+e^{-x}}2}$
  
  它们俩互为对方的**导数**。
  
- $calculus$

  当你不刷牙的时候，牙齿上堆积起来的一层坚硬沉淀物？（牙垢石的英文为$calculus$，和“微积分”共用同一个词）
  
  应是一种由牛顿和莱布尼茨发明的数学计算系统，最早是用在求斜率和面积上。
  
------------
# The end!

参考文献：《微积分之屠龙宝刀》，百度百科。

[$Tommy$_$Keen$](https://www.luogu.com.cn/user/393407)$\text{敬上}$

$2021.5.15$
