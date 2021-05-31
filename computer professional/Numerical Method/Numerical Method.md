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
  - 理由是隐藏在书写的数字后面的0也有可能是有效的

### 深入理解有效数字

为什么有效数字的标准是：
$$
    \left|\frac{p - \hat{p}}{p}\right| < \frac{10^{1-d}}{2}
$$

右边项分母的2从何而来？

答案：
**某估测值有$d$位有效数字保证了对估测值四舍五入到$d$位数字得到的每一个数字与真值的误差有一个明确的上界**

**除以2**，以及**严格小于号**都是因为这个：四舍五入引入的最大误差来自于$.50$的情况，其余情况$\epsilon<0.5$，当出现$.50$的情况时，严格小于号保证“五入”法则得到了正确的数字。

因此，$d$位有效数字的真正含义不是：$d$个数字都是对的，而是*四舍五入到d个数字的时候，d个数字的误差足够小*

实际上，由于我们采用的是*相对误差*，真值我们无法知道，**我们无法承诺d个有效数字都与真值相同**

一个简答法则：
  - 加法：与较大数与较小数对齐可知
  - 乘法：以有效数字位数较少的为准

### Asymptomatic Analysis & Order
渐进分析（Asymptomatic Analysis）关注函数的渐进性质。现在只介绍最常用的大O记号：
$$
    f(x) = O(g(x))\\
    \Leftrightarrow \exist C>0,\exist x_0,\forall x<x_0,|f(x)|\le C|g(x)|
$$

$$
    x_n = O(y_n) \\
    \Leftrightarrow \exist C>0,\exist N>0,\forall n>N,|x_n| < C|y_n|
$$

注意到不等式中的两个绝对值意味着渐进性质实际上的是关注非负函数。
但是，在函数的情形下，$\forall x < x_0$意味着渐进记号关注函数趋于0的变化率。而数列的情形下$\forall n > N$意味着渐进记号关注数列趋于无穷的增长率。

实际上，不同语境下大O关注的趋向可以不一样。但无论哪种语境，大O都是关注函数变化率之间的比较。

该记号可以很好的用于表征*Order of Approximation（近似的阶数）*和*Order of Convergence（收敛的阶数）*

**Order of Approximation**
Assume that $f(h)$ is approximated by $p(h)$. Then we say **$p(h)$ approximates $f(h)$ with the order of $O(h^n)$** if 

$\exist M > 0, \exist n > 0$

$$
    \frac{|f(h) - p(h)|}{|h^n|} \le M,\quad \text{for sufficently small $n$}
$$

也即：
$$
    f(h) - p(h) = O(h^n)
$$

改写之后有我们最常用的近似阶数的表达形式：
$$
    f(h) = p(h) + O(h^n)
$$

注意在这一语境下大O关心的是$h\to 0$的行为而非无穷的行为。因此$O(h^n)$可以称为*同阶无穷小*

实际上这种近似阶数表达非常常见。最常见的莫过于*泰勒公式*：
$$
    f(x) = \sum_{k=0}^{n} \frac{f^{(k)}(x_0)}{k!}h^k + \frac{f^{(n+1)}(\xi)}{(n+1)!}\\
    = \sum_{k=0}^{n} \frac{f^{(k)}(x_0)}{k!}h^k + O(h^{n+1})
$$

其中$O(h^{n+1})$实质上与皮亚诺余项$o(h^n)$是同一个意思（在泰勒多项式展开这种情况下）

其中$h=x-x_0$

实际上，大O记号可以用于表达收敛的阶：

if $x_n \to x$, and
$$
   \frac{|x_n - x|}{|r_n|} \le M \quad \text{for suffciently large $n$}
$$

then we say that $\{x_n\}$ converges to x with order of convegence $O(r_n)$

note: with $x_n \to x$, it is obvious that $r_n \to 0$

区别后面提到的*平方收敛*和*线性收敛*概念，这里使用的是绝对误差，且需要知道真实值才能计算；而平方收敛和线性收敛使用的是两次迭代之间解误差的比。


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

**不动点迭代收敛的充分条件：**

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

#### Theorm
let $g(x) = x - f(x)/f'(x)$

If $\exist p \in [a,b]$ where $f(p)=0$, and $f(x) \in C^2[a,b]$, $f'(p) \ne 0$

then there **exist** a $\delta$ such that sequence $\{p_n\}$ convergent to $p$ **for any initial approximation $p_0 \in [p-\delta, p+\delta]$**.

注意：
1. 当满足二阶导数连续且一阶导数不为0时，**存在**一个关于$f(x)$的零点$p$的邻域使得Newton-Raphson迭代收敛。也就是说，**初始点必须靠近真实的零点**
2. 牛顿法使用不动点迭代证明了其收敛性，但前提条件里没有直接出现不动点迭代收敛的条件（$|g'(x)|\le K <0$）。其实该条件隐含了：$f(x)$一阶导数是连续的，根据条件可以看出此时$g'(p) = 0$，且$g'(x)$连续。因此总是存在一个足够小的邻域满足不动点迭代的收敛条件。
3. 综合上述条件可以发现，**定理只能保证Newton-Raphson迭代在关于零点的某个足够小的邻域内收敛**，因此**选择足够接近零点的初始点非常关键**。
4. 作为上述讨论的结论，Newton-Raphson迭代是**局部收敛**的。
   - Bracketing Method是**全局收敛**。确定了零点所在的区间之后，Bracketing Method选择区间内任何点作为起始进行迭代都能保证收敛于零点
   - Newton-Raphson是**局部收敛**的，因为就算确定了包含零点的一个区间，也不能保证从该区间的任何一个点开始迭代能收敛于零点。
5. 尽管定理只能保证在一个小邻域内收敛，但是运气好的话在一些超出定理保证的区间上初始点也有机会收敛

#### Convergence 
牛顿迭代法可以看作一种有效的不动点构造方法，也可以用不动点迭代的条件来证明。

也即：
$$
    |g'(x)| \le K < 1 \Rightarrow \text{convergence}
$$

Be carefule, **Newton-Raphson is locally convergent**.


 
It is proved that, for a simple root,**the convergence of Newton Iteration is quadratic**.

simple proof:

let $g(x) = x - f(x) / f'(x)$, 

then
$$
    E_n = p_n - p\\
    E_{n+1} = p_{n+1} - p = g(p_n) - g(p)
$$

apply Talyor polynomial to $g(p_n)$, we got:
$$
    E_{n+1} = g(p_n) - g(p)\\
    = g(p) + g'(p)(p_n - p) + \frac{1}{2}g''(p)(p_n-p)^2 + O(h^3) - g(p)\\
$$

given $f(p) = 0, g(p) = p$, we got $g'(p) = 0, g''(p) = f''(p)/f'(p)$

then:
$$
    |E_{n+1}| \approx \left|\frac{f''(p)}{2f'(p)}\right||p_n-p|^2
$$

By definition, the Newton-Raphson Iteration at simple root is quadratically convergence.

For multiple root, Newton Iteration is not quadratic.

If $M$ of the root is known, simple use the iteration:
$$
    p_{n+1} = p_n - \frac{Mf(p_n)}{f'(p_n)}
$$

instead of the original one, we can still get the quadratic convergence (however mostly $M$ is unknown)

#### Pitfall

If the convergence condition of the Newton Iteration is not satisfied, the Newton Iteration can produce *divergent sequence* （发散序列）or *cyclic sequence*（循环序列）

**Secant Method is neither linear convergent nor quadratic convergent at simple root**, for it is proved that for simple root
$$
    |E_{n+1}|\approx A|E_n|^{0.618}
$$

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
    f(p) = f'(p) = f''(p) = \cdots = 0\\
    f^{(M)}(p) \ne 0
$$

then $p$ is a *simple root*（单重根） when $M = 1$,

and $p$ is a *multiple root* （多重根） when $M > 1$

### Initial Approximation & Convergence Criteria

Error of solution（解误差）: $|p_n - p|$

残差：$|f(p_n)|$

解之间的误差：$|p_n - p_{n-1}|$

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

    对待分解的$A$做高斯消元。使用一个与$A$等大的单位阵**记录高斯消元的过程，每次将行变换的比例系数放置在对应位置，注意不要取相反数**。最终消元得到的是$U$，记录的矩阵是$L$

    该方法的合理性在于每次在记录矩阵填数字都*等效于左乘行变换矩阵的逆矩阵*

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

### Error Measure（待完善）
Let $x$ represents the true value of the problem; $\tilde{x}$ represents the numerical approximation of the problem.

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

## Interpolation & Polynomial Approximation

插值（interpolation）一般特指“内插值”，而外插值有专门的词：extrapolation

### Polynomial Approximation

多项式近似的最常用的工具是泰勒展开：

Assume that $f \in C^{N+1}[a,b]$, if $x \in [a,b]$, then
$$
    f(x) = \sum_{k=0}^{N} \frac{f^{(k)}(x_0)}{k!}(x-x_0)^k + \frac{f^{(N+1)}(\xi)}{(N+1)!}(x-x_0)^{(N+1)}
$$

上式最后一项称为拉格朗日余项，常用于估计误差。

使用多项式（特别是泰勒多项式）近似的目的是多项式性质非常好，便于求值、微分、积分

### Interpolation

对于多项式插值，最简单的方法是待定系数法：
- 对于$N$个点插值，构造$N-1$次多项式，共$N$个待定系数
- 代入每一个点，得到$N$个方程组成的线性方程组，然后求解线性方程组即可

#### Lagrange interpolating polynomial（拉格朗日插值多项式）

这种方法不解线性方程组，而是直接**构造过所有点的多项式**。该多项式称为*拉格朗日插值多项式*

The general form of Lagrange interpolating polynomial:
Given point $(x_0, y_0), (x_1, y_1), \cdots, (x_N, y_N)$

$$
    P_N(x) = \sum_{k=0}^{N} y_k L_{N,k}(x)
$$

where $L_{N,k}(x)$ is:
$$
    L_{N,k}(x) = \frac{\prod_{<i\ne k>}(x-x_i)}{\prod_{<i\ne k>}(x_k - x_i)}
$$

由线性方程组理论，对$N$构造$N-1$阶待定系数多项式然后解方程组，若解唯一，则其得到多项式一定与拉格朗日插值多项式相同

**Lagrange Error Term**:

Assuming that $f(x) \in C^{N+1}[a,b]$, $f(x) = P_N(x) + E_N(x)$
$$
    E_N(x) = \frac{f^{(N+1)}(\xi)}{(N+1)!}\prod_i (x-x_i)
$$

注意到该误差项形式与泰勒公式的Lagrange余项相似

**Lagrange Error Bound**:

考虑对等距采样的点做Lagrange插值，且间距为$h$，再令
$$
    |f^{(N+1)}(x)| \le M_{N+1}
$$

则有如下几个常见的关于$h$的误差界：
$$
    |E_1(x)| \le \frac{h^2 M_2}{8}\\
    |E_2(x)| \le \frac{h^3 M_3}{9\sqrt{3}}\\
    |E_3(x)| \le \frac{h^4 M_4}{24}\\

$$

#### FFT插值
实际上更高效的一种插值方法是FFT。

通过将多项式的$(次数,系数)$点集和多项式曲线上的$n$个点组成的点集视作离散傅里叶变换种对应的点，我们可以通过对值表示的多项式做FFT得到多项式的系数表示，相当于完成了多项式插值。这一过程的复杂度$O(n\log n)$远胜解线性方程组求多项式插值的方法（$O(n^3)$）

## Fitting（拟合）
### Least-Square

对于一维特征的数据点，我们构造直线$y = Ax + B$来拟合。方法是**令得到的直线的平方误差最小**，称为**最小二乘法**。

构造平方误差函数：
$$
    L(A,B) = \sum_{k=1}^{n} (Ax_k + B - y_k)^2
$$

令两个一阶偏导为0：
$$
    \frac{\partial L}{\partial A} = \sum_{k=1}^{n} 2(Ax_k + B - y_k)x_k = 0
$$

$$
    \frac{\partial L}{\partial B} = \sum_{k=1}^{n} 2(Ax_k + B - y_k) = 0
$$

得到一个线性方程组：
$$
    \left(\sum_{k=1}^{n} x_k^2 \right) A + \left(\sum_{k=1}^{n} x_k \right) B = \left(\sum_{k=1}^{n} x_ky_k \right)\\
    \left(\sum_{k=1}^{n} x_k \right) A + n B = \left(\sum_{k=1}^{n} y_k \right)
$$

上述方程称为*normal equation（正规方程）*

解上述线性方程组可以得到最小二乘直线

同理构造并求解任意多项式的最小二乘曲线（仍然是解正规方程）

考虑对高维特征的数据点构造线性模型$y = Xw$，其中$X \in \mathbb{R}^{m\times n}, y \in \mathbb{R}^{m}, w \in \mathbb{R}^{n}$，$X$表示$m$个$n$维特征的数据点，$w$是系数
### 非线性最小二乘法

对非线性模型使用最小二乘法拟合时，令梯度为0得到的正规方程不再是线性方程（组）。可以做变量的变换做*线性化*，再采用最小二乘法解出系数完成拟合。

注意，**数据线性化最小二乘法解得的模型系数不是原模型的最小二乘解**，通常比原模型要差一点。可能的原因是做线性化之后度量空间也被变换了。



### 高维最小二乘解
由矩阵代数有如下正规方程解：

$$
    \hat{w} = (X^TX)^{-1}X^Ty
$$

## Spline（样条）

*Polynomial Wiggle（多项式震荡）*现象出现在高次多项式的插值。对于传统拉格朗日插值多项式，$N$个会插值出$N-1$次的多项式。直观来说，高次多项式具有许多个极值点，为了穿过所有的点并保证足够多的极值点，高次的插值多项式当然会反复震荡。

最糟糕的情况，在极高次多项式中，由于舍入误差，计算机绘制的插值曲线甚至不通过插值点

Polynomial Wiggle现象促使人们考虑能否采用光滑的低次多项式分段插值，得到光滑的总体插值曲线？

*Spline（样条）*就是一个“分段函数”，每两个给定数据点之间的闭区间分为一段，每一段都是低阶的插值多项式，且该曲线连续而又光滑，不震荡。最常用的是*Piecewise cubic spline（三次样条）*。实践表明，三次样条表现最好（足够光滑又不会震荡）

### 三次样条
三次样条是样条的一种。**插值函数的每段都是最高不超过三阶的多项式，且整个插值曲线二阶导连续。**

**由于三阶多项式是保证了二阶导连续，故要使整个插值曲线二阶导连续，只需要讨论分段点处的状态。由此导出三次样条的约束：**

Assuming that there is $N+1$ point, then we are going to find $N$ polynoimals $S_k$,
for $x \in [x_k, x_{k+1}]$, $S(x) = S_k(x)$, where $S_k(x)$ is a cubic polynomial, satisfying that:
$$
    S_k(x_k) = y_k\\
    S_k(x_{k+1}) = S_{k+1}(x_{k+1})\\
    S_k'(x_{k+1}) = S_{k+1}'(x_{k+1})\\
    S_k''(x_{k+1}) = S_{k+1}''(x_{k+1})\\
$$

导函数在端点处的值相等保证了$S(x)$是二阶导连续的。

上述表示中，$N+1$个数据点的三次样条一共有$4N$个未知量（$N$个三次多项式的待定系数），但从约束只能导出$4N-2$个方程（$N$个三次多项式，每个多项式对应4个方程，但最后一个多项式只对应2个方程）。因而**对于基本的三次样条问题，需要解欠定方程组**。

经典的解三次样条问题，采用利用先验知识补充约束的方法使得求三次样条问题变为解线性方程组。这种补充约束称为*Endpoint Constraint（端点约束）*，不同的约束策略可以导出不同类型的三次样条。

三种常见的样条类型：
1. Natural（自然）
2. Parabolic Runout（抛物线逃出）
3. Cubic Runout（三次逃出）（最准）

这三种策略影响的主要都是在样条的两个端点处外延的行为。Natural导致在外延的时候近似直线。

### 三次样条核心推导
（待完善）

### 关于样条的其他问题
- **样条只能用于内插值，不能用于回归**。样条非常好地穿过所有的数据点，它会将所有的噪声考虑在内，**但在数据点所在区间外推时，涉及的只有边界点处的低阶多项式，几乎没有捕捉到数据整体的特征。**
- 解基本三次样条问题的线性方程组的时候，其实不一定需要补充条件，可以直接使用伪逆解欠定方程组得到三次样条
- 实际上样条问题的线性方程组之所以是欠定的，是因为，在两个端点的外延区间上，没有足够约束计算出一条唯一的多项式曲线。**换句话说，两个端点的外延区间上的曲线其实是任意指定的**。**所谓的不同类型的样条，“逃出”，本质上是指使用哪种外延的曲线**。三次逃出最准的根本原因是这段曲线来自于边缘的4个点的插值。如果使用更多的点构造“逃出”曲线，其实无非是对更多的点做拉格朗日插值然后做extrapolation罢了，又回到Polynomial Wiggle的老问题，意义不大。

## 数值微分

采用有限差分估计微分：
前向公式（$O(h)$近似）：
$$
    f'(x_n) \approx \frac{y_{n+1}-y_n}{x_{n+1}-x_n}
$$

后向公式（$O(h)$近似）：
$$
    f'(x_n) \approx \frac{y_{n-1}-y_n}{x_{n-1}-x_n}
$$

以及中间差分公式（$O(h^2)$近似）：
$$
    f'(x_n) \approx \frac{y_{n+1}-y_{n-1}}{x_{n+1}-x_{n-1}}
$$

前向/后向公式虽然不如中间差分公式近似阶高，但胜在能用在边界，以及用于解常微分方程（中间差分公式解常微分方程是会震荡的）。

### 误差估计
（待完善）

### 数值微分中h的权衡：舍入误差vs.截断误差

可以证明，随着$h$减小，截断误差逐渐减小，但舍入误差逐渐增大。由于截断误差和舍入误差是累加的，最终理论上总的误差界是关于$h$的双钩函数，**存在误差最小的h的取值，在双精度浮点数下通常是$10^{-9}$左右**。

**截断误差来源于舍去泰勒展开的高阶无穷小**，因此**截断误差与采用的算法有关**。（比如存在$O(h)$,$O(h^2)$和$O(h^4)$的一阶数值微分公式）

**舍入误差来源于机器使用有限精度表示实数，与机器实现有关**。在数值微分的情景下，舍入误差对计算的影响主要体现为**两个很接近的数相减会造成有效数字损失**，这也是为什么舍入误差与$h$负相关。

### 不等间隔数值求导：拉格朗日插值

**这一方法不但可以用于求不等间隔数值微分公式，还可以导出任意多个点的前向/后向/中间差分的任意阶数值求导公式**

（待完善）

### 高阶数值求导
（待完善）





