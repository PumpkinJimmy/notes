# Numerical Method
## Horner's Method
Horner's Method = Synthetic Division

因为霍纳方法的本质是多项式除法，每次除以x，然后表示为“商*除数+余数”的形式

## Error Analysis

Absolute Error:
$$  
    |p - \hat{p}|
$$

Relative Error:
$$
    \left|\frac{p - \hat{p}}{p}\right|
$$

**Approximate *p* to *d* significant digits**:
$$
    \left|\frac{p - \hat{p}}{p}\right| < \frac{10^{1-d}}{2}
$$

注意：
- 不要漏$1/2$
- 有效数字有可能反直觉
- 一个简答法则：
  - 加法：与较大数与较小数对齐可知
  - 乘法：以有效数字位数较少的为准

## Root of Nonlinear Equation
Three Classic Method:
- Fixed Point Iteration
- Bisection
- Newton

Fixed Point Iteration:

To solve $f(x) = 0$,let $g(x) = f(x) + x$,such that:
$$
 x_{i+1} = g(x_i)
$$

After several iteration, $x_i$ is the root

To solve $f(x) = 0$:
$$
    x_{i+1} = x_i - \frac{f(x_i)}{f'(x_i)}
$$

After several iteration, $x_i$ is what we want.

### Fixed-Point

不动点法是一种框架而非具体的算法。牛顿迭代法就可以算是这一框架下的算法。

核心思想是针对$f(x)$构造$g(x)$然后寻找方程$x=g(x)$的根。在计算中对进行$x_{i+1} = g(x_i)$的迭代直到收敛。

fixed point的系列命题：
1. fixed point exist
   
   $$
    x \in [a,b], g(x) \in C[a,b]\\
    \Rightarrow \exist x_0 = g(x_0)
   $$

   proof: 介值定理

2. fixed point unique
   
   if fixed point exist and $|g'(x)| \le K <1$, fixed point unique

   proof: 中值定理

3. fixed-point iteration convergence

   for $g(x) \in C[a,b]$, if $|g'(x)| \le K < 1$，fixed-point iteration convergence

   proof: 中值定理 + 放缩

   It is proved that:
   $$
     |P - p_n| = |g(P) - g(p_{n-1})|\\
     =|g'(\xi)(P-p_{n-1})| \\
     = |g'(\xi)||P-p_{n-1}| \\
     \le K|P-p_{n-1}| < |P-p_{n-1}|
   $$

   That is, $|P-p_0| \le K^n|P-p_n|\to 0$,if $K <1$

不动点收敛的的条件：

For Fixed-Point Iteration $x_{i+1} = g(x_i)$, if 
$$
    |g'(x)| \le K < 1
$$
then the Fixed-Point Iteration convergence. $K$ is a constant

note: when condition above is satisfied, the computing result is **unique**, and we call this point *attractive fixed point*

if
$$
    |g'(x)| > 1
$$
then disvergence

the fixed-point not convergenced to called *repelling fixed point*

### Bisection Method

考虑方程$f(x) = 0$在$[a,b]$上的根

由介值定理，若$f(a)f(b) < 0$则在存在$x_0 \in (a, b), f(x_0) = 0$

对$[a,b]$做第$n$次二分得到的中点的解误差的上界：
$$
    \delta_n = \frac{b-a}{2^n}
$$

反解上式，可得若要保证解误差不超过$\delta$，最小的迭代次数$N$是：
$$
    N = \left\lfloor \frac{\ln(b-a) - \ln \delta}{\ln 2}\right\rfloor
$$

### 错位法

### Newton Iteration

To solve $f(x) = 0$:
$$
    x_{i+1} = x_i - \frac{f(x_i)}{f'(x_i)}
$$

After several iteration, $x_i$ is what we want.

牛顿迭代法可以看作一种有效的不动点构造方法

牛顿迭代法可以从标量推广到向量以及张量（在张量上需要矩阵分析）

以向量函数牛顿法为例子：
$$
    \mathbf{x} \in  R^{n}, f(\mathbf{x})\in R^{m}\\
    \mathbf{x_{i+1}} = \mathbf{x_{i}} - J(f(\mathbf{x_i}))^{-1}f(\mathbf{x_i})
$$

其中
$$
    J(f(\mathbf{x})) = \frac{\partial f(\mathbf{x})}{\partial \mathbf{x}}
$$
即*雅可比矩阵*

通过求方程“导数=0”的根，牛顿迭代法可以用于解最优化问题

牛顿迭代法求最优化的局限在于它迭代出来的点起始是*驻点*，在高维向量情形下很容易碰到*鞍点*