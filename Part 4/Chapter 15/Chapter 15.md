# Chapter 15 Dynamic Programming

分治法将问题划分为互不相交的子问题，但动态规划的子问题互相重叠。故如果递归求解时，子问题的重叠部分会多次计算，故需要表格存储已经计算的子问题。

动态规划通常用来求解最优化问题。找到**一个**最优解。

设计算法：

1. 刻画一个最优解的结构特征。
2. 递归定义最优解的值。
3. 计算，通常自底向上。
4. 利用值构造一个最优解。

## 15.2

### Catalan numbers

- 卡特兰数的定义：在一个凸多边形中，通过若干条互不相交的对角线，把这个多边形划分成了若干个三角形。$C_n$ 为 $n+1$ 边形不同划分的方案数。很明显，$C_2=1,C_3=2$。定义 $C_1=1$。

- 在一个 $n+1$ 边形内先划出一个最小的顶点三角形，三角形左右形成两个多边形，假设一个是 $m+1$ 边型，另一个就是 $n-m+1$ 边形。
  $$
  C_n=C_1C_{n-1}+C_2C_{n-2}\dots+C_{n-1}C_1=\sum_{k=1}^{n-1}C_kC_{n-k}
  $$
  用生成函数解这个递归式：
  $$
  \begin{align*}
  G(x)&=\sum_{k=1}^{\infty}C_kx^k\\
  G^2(x)&=\sum_{k=1}^{\infty}C_kx^k\sum_{k=1}^{\infty}C_kx^k\\
  &= C_1C_1x^2+(C_1C_2+C_2C_1)x^3+\dots\\
  &= C_2x^2+C_3x^3+\dots\\
  &= G(x)-x\\
  G(x)&=\frac{1-\sqrt{1-4x}}{2}
  \end{align*}
  $$

  $$
  \begin{align*}
  (1-4x)^{\frac{1}{2}}&=\sum_{k=0}^{\infty}\binom{k}{\frac{1}{2}}(-4)^kx^k\\
  &=\sum_{k=0}^{\infty}\frac{\frac{1}{2}(-\frac{1}{2})\dots(\frac{3-2k}{2})}{k!}(-4)^kx^k\\
  &=\sum_{k=0}^{\infty}\frac{1}{k!}\frac{(-1)^{k-1}}{2^k}\frac{(2k-2)!}{(2k-2)!!}(-4)^kx^k\\
  &=\sum_{k=1}^{\infty}\frac{-2}{k!}\frac{(-1)^{k-1}}{2^k}\frac{(2k-2)!}{(k-1)!}x^k+1\\
  \end{align*}
  $$

  $$
  G(x)=\sum_{k=1}^{\infty}\frac{1}{k}\binom{2k-2}{k-1}\\
  C_n=\frac{1}{n}\binom{2n-2}{n-1}
  $$

  完全括号化问题

  出栈顺序问题 ……

## 15.3 动态规划原理

### 15.3.1 最优子结构

如果一个问题的最优解包含子问题的最优解，那么我们称这个问题具有最优子结构性质。

1. 做出选择。
2. 假定已经知道最优的选择。
3. 解决因为选择产生的子问题。

最短路径具有最优子结构，最长路径没有。

## 15.4 最长子序列 LCS

令 $X=<x_1,\dots,x_m>$ 和 $Y=<y_1,\dots,y_n>$ 为两个序列，$Z=<z_1,\dots,z_k>$ 为 $X$ 和 $Y$ 的一个 LCS 。

1. 如果 $x_m=y_n$，则 $z_k=x_m=y_n$，且 $Z_{k-1}$ 是 $X_{m-1}$ 和 $Y_{n-1}$ 的一个LCS。
2. 如果 $x_m\ne y_n$，$z_k\ne x_m$，则 $Z$ 是 $X_{m-1}$ 和 $Y$ 的一个LCS。
3. 如果 $x_m\ne y_n$，$z_k\ne y_n$，则 $Z$ 是 $X$ 和 $Y_{n-1}$ 的一个LCS。