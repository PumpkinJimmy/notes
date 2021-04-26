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

**不动点迭代收敛的的条件：**

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
### Bracketing Method

The bracketing methods depend on finding an interval $[a, b]$ so that $f(a)f(b)<0$ (for the imtermediate value theorem)

#### Bisection Method

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

#### False Position Method（错位法）
$$
    c = b - \frac{f(b)(b-a)}{f(b)-f(a)}
$$

可以看作二分法的改进

注意区别secant method

### Newton-Raphson Iteration（牛顿迭代法）

To solve $f(x) = 0$:
$$
    x_{i+1} = x_i - \frac{f(x_i)}{f'(x_i)}
$$

After several iteration, $x_i$ is what we want.

Newton Iteration 也被称为*切线法*，因为其不动点迭代公式是通过作切线与横轴的交点得到的

#### Convergence 
牛顿迭代法可以看作一种有效的不动点构造方法，也可以用不动点迭代的条件来证明。
 
It is proved that, for a simple root,**the convergence of Newton Iteration is quadratic**.

For multiple root, Newton Iteration is not quadratic.

If $M$ of the root is known, simple use the iteration:
$$
    p_{n+1} = p_n - \frac{Mf(p_n)}{f'(p_n)}
$$

instead of the original one, we can still get the quadratic convergence (however mostly $M$ is unknown)

#### Pitfall

If the convergence condition of the Newton Iteration is not satisfied, the Newton Iteration can produce *divergent sequence* （发散序列）or *cyclic sequence*（循环序列）

#### For multivariable
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

#### Newton Iteration Finding Sqrt Root
一个有用的推论：求平方根：
$$
    要求\sqrt{A}，\\
    令f(x) = x^2 - A，则有迭代公式：\\
    p_{n+1} = p_n - \frac{p_n^2-A}{2p_n}\\
        = \frac{p_n + \frac{A}{p_n}}{2}
$$

从牛顿迭代还可以给出开k次方根的迭代算法。

### Secant Method （割线法）
若导数不易计算，可以使用割线法替换牛顿迭代法：
$$
    p_{n+1} = p_n - \frac{f(p_n)(p_n - p_{n-1})}{f(p_n)-f(p_{n-1})}
$$

这个方法在形式上是使用差分估计微分

### order of convergence （收敛及收敛阶）

order of convergence（收敛阶）：

Assume that ${p_n}$ convergence to $p$,let $E_n = p - p_n$, if
$$
    \lim_{n\to \infty} \frac{|E_{n+1}|}{|E_n|^R} = A
$$

then the **order of convergence** is $R$,and $A$ represant the *asymptotic error rate constant*

If $R$ is 1, the convergence is *linear*
If $R$ is 2, the convergence is *quadratic*

根的重数(multiplicity)：
If $p$ satisfy：
$$
    f(p) = f'(p) = f''(p) = \cdots = f^{(M)}(p) = 0
$$

then $p$ is a *simple root*（单重根） when $M = 0$,

and $p$ is a *multiple root* （多重根） when $M > 0$

### Initial Approximation & Convergence Criteria

Error of solution（解误差）: $|p_n - p|$

残差：$|f(p_n)|$

解之间的误差：$|p_n - p_{n-1}|$

（待完善）

## Linear Equation
解线性方程组通常有两条路：
1. 解析法（高斯消元，$O(n^3)$）
2. 迭代法（通常更快，但具有收敛条件的限制）
### Gauss Elimination（高斯消元法）

Steps:
- Forward Elimination
- Back Substitution

Complexity: $O(n^3)$,where $n$ is the number of pivot

### LU Factorization（LU分解）

考虑对满秩矩阵$A$做高斯消元，其实相当于求满秩矩阵$P$，使得：
$$
    U = PA
$$

其中，$U$是上三角矩阵

由于$P$是一连串初等行变换连乘得到的，因此是可逆的。

也即，有如下分解：
$$
    A = P^{-1}U
$$

若不涉及行交换操作，可以证明，$P^{-1}$是下三角矩阵，故写作$L$

进一步的，可以令$L$的对角线元素全部为1，则如下分解：

$$
    A = LU
$$

称为**LU分解**，也或者称为**Triangular Factorization**，
其中$A$是满秩矩阵，$L$是**对角线全为1的下三角矩阵(lower triangular matrix)**，$U$是**上三角矩阵(upper triangular matrix)**


- **求LU分解**
    方法：
    在$A$左侧附加一个等大的单位阵，然后带着这个增广矩阵对原矩阵做高斯消元，消元成功之后增广部分就是$L$，消元结果部分是$U$

complexity：同高斯消元，$O(n^3)$

- **LU分解求线性方程组的解**
    有了LU分解，求解$Ax=b$等价于求解：
    $$
        Ly = b\\
        Ux = y
    $$

    其中$L,U$都是三角矩阵，因此可以直接回代求出解

    complexity: $O(n^2)$

- 使用LU分解的动机

    可以发现，使用LU分解求解线性方程组的复杂度并没有改进。

    而且，矩阵求逆同样可以用来求解线程方程组，且复杂度也是$O(n^3)$

    使用LU分解（而不是简单做高斯消元、矩阵求逆）的主要动机：
    1. 可以发现，对于同一个$A$，只需做一次LU分解，就可以对任意$b$在$O(n^2)$的时间内求解。
    2. LU分解的数值稳定性由于矩阵求逆

- LUP分解
  
  在高斯消元的任意一个步骤中，由于要做除法，若当前对角线元素为0（或接近0），则**必须要行交换来选择主元（Partial Pivoting）**

  可以证明，对非奇异矩阵$A$，如下分解总是成立：
  
  $$
    PA = LU
  $$

  where $P$ is a permutation matrix, $L$ is a lowr triangular matrix, $U$ is a upper triangular matrix

  这一分解称为*LUP分解*

  注意到，可逆矩阵$A$的LU分解不总是存在，因为它要去不涉及行交换

  但是，可逆矩阵的$A$的LUP分解总是存在的

### Jacobi Method

A *iterative method* to solve a linear system.

Jacobi method:

Assume that $x^{n} \in R^{m}$

$$
    x^n_1 = \left(
        \frac{b_1 - (a_2 x^{n-1}_2+a_3 x^{n-1}_3 \cdots a_m x^{n-1}_m)}{a_1^{n-1}}
        \right)\\
    x^n_2 = \left(
        \frac{b_1 - (a_1 x^{n-1}_1+a_3 x^{n-1}_3 \cdots a_m x^{n-1}_m)}{a_2^{n-1}}
        \right)\\
        \vdots\\
    x^n_m = \left(
        \frac{b_1 - (a_1 x^{n-1}_1+a_3 x^{n-1}_3 \cdots a_{m-1} x^{n-1}_{m-1})}{a_m^{n-1}}
        \right)\\
$$

依次类推

### Gauss-Seidel Method

### Gauss Elimination vs. Iterative methods
- Gauss Elimination:
  - Slow on large datasets ($O(n^3)$)
  - **Subject to roundoff error & ill condition**
- Iterative methods are good alternatives to elimination method.
  - Work well for large datasets
  - **Not depends on the initial guess**

### Iterative Method: Convergence
It is proof that Jacobi method & Gauss-Seidel will converge when the matrix $A$ is **strictly diagonally dominant（严格对角占优）**

A matrix $A \in R^{N\times N}$ is said to be *strictly diagonally dominant* provided that:
$$
    |a_{kk}| > \sum_{j=1,j\ne k}^{N} |a_{kj}|\quad \mathrm{for}\ k = 1, 2, \cdots N
$$

Besides, it is proved that assuming that the matrix $A \in R^{N\times N}$ is strictly diagonally dominant, $\forall P_0 \in R^{N\times N}$, the iterative method is convergent. That is, Jaccobi method is *globally convergent*

## Errors and Stopping Criteria
Two major sources of error in numerical methods:
1. **Roundoff Error（舍入误差）**
   
   Computer represent quantities with finite number of digits. （计算机使用有限位表示实数）

   舍入误差主要由硬件决定（比如双精度浮点数决定了能保证的有效数字至多15位）

2. **Truncation Error（截断误差）**

   比如泰勒展开，截断误差来自主动丢弃的余项部分。

   （待完善）

### Error Measure（待完善）
Let $x$ represents the true value of the problem; $\tilde{x}$ represents the numerical approximation of the problem.

the absolute error:
$$
    E = 
$$

significant digits

注意从有效数字的定义导出的结论的某些情形下是反直觉的。比如，考虑如下问题：

考虑$\pi$的一个近似：$3.1416$，若我们再取一个更好的近似$3.1415927$作为$\pi$的真实值，则$3.1416$的有效数字位数是？

根据公式：
$$
    \frac{|\tilde{x} - x|}{|x|} < \frac{1}{2}10^{1-d}
$$

有
$$
    d < 1 - \ln(2\frac{|\tilde{x} - x|}{|x|}) / \ln 10
$$

即取$d=6$。 但是乍一看$3.1416$只有5位数字。这其实是因为6后面的0也是有效的

### Error of the Linear System Solution

Let $X$ be the numerical solution of the linear system, $X^*$ be the true solution of the linear system, $|| X ||$ be the norm of the vector/matrix

Three major types of error:

1. **residual error（残差）**
    $$
        r = || AX-b ||
    $$

2. **solution error（解误差）**
    $$
        e = || X^* - X ||
    $$

3. 解间误差（迭代法）

   $$
     || P_{n+1} - P_n ||
   $$

   使用这种误差的原因是的动机是不知道真正的解，相对误差不好求。

**The relationship between residual error and solution error**

Caculating the solution error is impractical (for the $X^*$ is unknown). However, residual error is easy to calcuate and the relationship between residual error and solution error is as follow:

$$
    \begin{cases}AX^* = b\\
    e = X^* - X\\
    r = AX - b\end{cases}\\
$$ 
so that
$$
    \frac{||e||}{||X^*||} = \frac{|| A^{-1} r ||}{||x^*||}\\
    \le \frac{||A^{-1}||||r||}{||X^*||}\\
    \le ||A||\ ||A^{-1}||\frac{||r||}{||b||} 
$$

Let $\frac{||A||\ ||A^{-1}||}{||b||}$ be the **condition number** of the matrix $A$

当条件数很大的时候，就算残差很小，也不意味着解误差很小。

### Loss of Significance

计算机的有限位实数表示的问题除了舍入误差，还有**运算中的有效数字精度丢失 (loss of significance)**：
- 两个很相近的数相减
- 除以一个很接近0的数

## Matlab Note
```matlab
% 百分号表示注释

% 矩阵书写
% 写法1：行内
E = [1 0 0; 0 1 0; 0 0 1]
% 写法2：跨行
E2 = [
    1 0 0
    0 1 0
    0 0 1
]

% 矩阵分片索引：
% 注意：矩阵索引从1开始！
A(:,1,3)

% 定义内置函数：
f = inline('1/(1-exp(-x))', 'x')

% linspace
xx =  -1:0.01:1

% 画图
plot()

% 矩阵乘法
A*B

% 数组逐元素运算
A.*B % 逐元素乘
xx.^2 % 逐元素平方

% 常用矩阵
zeros(n) % 全0方阵
ones(n) % 全1方阵
randn(n,m) % 高斯分布随机矩阵
eye(n) % 单位阵

% 常用矩阵操作
eig(A) % 特征值
inv (A) % 逆
pinv(A) % 伪逆
[L,U] = lu(A) % LU分解
[L,U,P] = lu(A) % LUP分解
det(A) % 行列式

% 矩阵范数
norm(A)

% 二项分布
binopdf(n,p,k) % 概率密度
binocdf(n,p,k) % 累积密度

% 显示格式设置
format long % 长格式（15位）
format short % 短模式（5位）
grid on % 显示网格
```

## 卡西欧计算器使用
- 设置角度制、弧度制：Shift-Mode(Set Up)
- 设置显示定点数、浮点数、有效数字、小数点后：Shift-Mode(Set Up)
- 存储变量：计算结果+Shift-RCL(STO)+变量字母
- 历史记录：上下键
- 迭代：使用Ans连点等号，或者使用STO
