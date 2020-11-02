## 数分证明题目

### 1. 实数理论互推
- 有限覆盖=>聚点定理
  
  设$S$是一有界无限集，证明：$S$有聚点
  $$
    用反证法.假设S无聚点.取E为覆盖S的一有界闭集\\
    则\forall P \in E, P的邻域内只有有限个点\\
    （注意到若邻域内有无限个点,则它们都必定是聚点）\\
    取E中所有点的邻域组成一开覆盖U_0\\
    由有限覆盖定理，E存在一组有限开覆盖U_1\\
    设U = \bigcap_{u\in U_1} u,则U只含有限个点\\
    由覆盖定义，只存在有限个点属于S，推出矛盾
  $$
- 致密性定理=>柯西收敛原理
  
  已知有界数列必有收敛子列，证明柯西收敛原理。

  证：

  先证必要性：
  $$
  
  由点列极限的定义，\\
  \lim P_n = P_0 \Rightarrow \\
  r(P_m,P_n) \le r(P_m,P_0) + r(P_n,P_0) < \varepsilon/2 + \varepsilon / 2 = \varepsilon\\
  必要性得证
  $$
  再证充分性：
  $$
    
    由\forall p \in Z^+,r(P_{n+p},O) \le r(P_{n+p},P_n) + r(P_n,O) < \varepsilon + M_1\\
    可知\{P_n\}有界\\
    （其中M_1为P_1到P_n中距离远点最远的点的距离）\\
  $$
  故存在收敛子列$\{P_{n_k}\}$
  $$
    令\lim P_{n_k} = P_0,有：\\
    \exists N_1 > 0, \forall n_k > N_1, r(P_{n_k},P_0)<\varepsilon/2\\
    由柯西条件，有：\\
    \exists N_2 > 0, \forall n,n_k >N_2, r(P_{n_k},P_n) < \varepsilon/2\\
    令N = \max(N_1,N_2)，\forall n > N,有：\\
    r(P_n,P_0) \le r(P_n,P_{n_k}) + r(P_{n_k},P_0)\\
    < \varepsilon/2 + \varepsilon/2 = \varepsilon\\
    也即\lim_{n\to\infty} P_n = P_0，充分性得证
  $$
### 开闭集互补
证明：开集的补集是闭集
$$
    用反证法.设开集E的补集E^c不是闭集\\
    则\exists P \in E为E^c的极限点\\
    则由开集定义，\exists \delta > 0,U(P,\delta) \subset E \\
    也即\exist \delta, U(P,\delta) \cap E^c = \phi\\
    与“P为E^c”聚点矛盾，原命题得证
$$
证明：闭集的补集为开集
$$
    用反证法.设闭集E的补集E^c不是开集\\
    则\exists P \in E^c满足\\
    \forall \delta > 0,U(P,\delta) \cap E \neq \phi \\
    也即P为E的聚点\\
    由闭集定义，P \in E与 P \in E^c矛盾\\
    原命题得证
$$
### 2. 有界闭集连续函数性质2：一致连续
用反证法，设函数$f$在有界闭集D上连续但不一致连续，则：
$$      
\exists \varepsilon >0 满足 \forall \delta >0 \exists P,Q \in D:r(P,Q)<\delta，使得|f(P)-f(Q)| \ge \varepsilon\\
取\delta = \frac{1}{n}则\\
r(P_n,Q_n) < \frac{1}{n}且|f(P_n)-f(Q_n)| \ge \varepsilon\\
P_n \in D，故存在收敛子列\{P_{n_k}\}.设极限为P_0\\

有r(P_0, Q_{n_k}) \le r(P_0,P_{n_k}) + r(P_{n_k}, Q_{n_k})\\
< \varepsilon_1 + \frac{1}{n_k}\\

也即Q_{n_k} \to P_0\\

由极限定义和连续性，有|f(P_{n_k}) - f(Q_{n_k})| \to 0, 与假设矛盾，原命题得证
$$

### 3. 证明$\cos xy$不一致连续
$$
只要寻找 \varepsilon >0 满足 \forall \delta >0 只要P,Q \in D有r(P,Q)<\delta，则|f(P)-f(Q)| \ge \varepsilon\\
    构造点列：\\
    \{P_n: (\sqrt{n\pi}, \sqrt{n\pi})\} \\
    \\
    \{Q_n: (\sqrt{(n+\frac{1}{2})\pi}, \sqrt{(n+\frac{1}{2})\pi})\}
    \\
    r(P_n,Q_n) \to 0\quad (n \to \infty)\\

    |f(P_n)-f(Q_n)| = |\cos n\pi - \cos(n+\frac{1}{2})\pi| = 1 \ge \varepsilon

    于是找到了\varepsilon=\frac{1}{2}
$$

### 4. 有界闭区域连续函数性质3：介值定理
$$
证明：\forall \mu \in [m,M],\exists P \in D, s.t.f(P) = \mu,\\
其中m,M是函数的最大/最小值。
$$
$$
    证：构造函数F(x) = f(x) - \mu\\
    取P_1,P_2 \in D,f(P_1) < 0, f(P_1) > 0,\\
    由闭区域性质，存在有限条折线连接P_1, P_2\\
    逐段检查，必存在一段，其端点异号。\\然后用参数方程化二元为一元，然后利用一元介值定理得证
$$

### 5. 证明不一致连续
证明：$1/(1-xy)$在$[0,1) \times [0,1)$上连续但不一致连续
证：
$$
  设f(x,y) = \frac{1}{1-xy}\\
  由初等函数性质可知连续\\
  取点列\{(\frac{n-1}{n}, \frac{n-1}{n})\}以及\{(\frac{n}{n+1}, \frac{n}{n+1})\}，记作P_n,Q_n \\
  当n \to \infty 时 两点列都趋于1\\
  且有|f(P_n)-f(Q_n)| \ge \frac{1}{3}\\
  故不一致连续
$$
### 6. 对分量连续与函数连续
1. 证明：若$f(x,y)$分别对$x,y$连续，且对其中一个单调，则$f(x,y)$连续
   
   取$x_0$的$\delta$邻域，有：
   $$
    |x - x_0| < \delta \\
    |y - y_0| < \delta
   $$

   不妨设对$x$单调增，有：
   $$
    f(x_0 - \delta, y) <f(x,y) < f(x_0 + \delta, y)
   $$

   故有：
   $$ 
    f(x,y) < f(x_0 + \delta, y) \\
           < f(x_0 + \delta, y_0) + \varepsilon /2 \quad（y的连续性）\\
           < f(x_0,y_0) + \varepsilon /2 + \varepsilon /2 \quad(x的连续性)\\
           = f(x_0,y_0) + \varepsilon
   $$
   同理有：
   $$ 
    f(x,y) > f(x_0 - \delta, y) \\
           > f(x_0 - \delta, y_0) - \varepsilon /2 \quad（y的连续性）\\
           > f(x_0,y_0) - \varepsilon /2 - \varepsilon /2 \quad(x的连续性)\\
           = f(x_0,y_0) - \varepsilon
   $$
   综上有：
   $$
    |f(x,y) - f(x_0, y_0)| < \varepsilon
   $$
   连续性得证.

   对单调减情形同理可证

2. 证明：若二元函数对其中一个变量连续，对另一个变量李普希兹连续，则二元函数连续
   
   证明：
   $$
   设f(x,y)对x,y分别连续，由题意有：\\
   \forall y,y_0,\exist L>0, \\
   |f(x,y) - f(x,y_0)| \le L|y-y_0|
   $$
   以及：
   $$
   \forall x,\exist \varepsilon_1 > 0, \\
   |f(x,y_0) - f(x_0,y_0)| < \varepsilon_1
   $$

   只要令：
   $$
    \forall \varepsilon > 0, \exists \delta >0,对满足P(x,y) \in U^0(P_0, \delta)有\\
    |f(x,y)-f(x_0,y_0)| = \\
    |f(x,y) - f(x,y_0)+f(x,y_0)-f(x_0,y_0)| \\
    < L|y-y_0| + \varepsilon_1 < L\delta + \varepsilon_1 < \varepsilon\\
    取\delta = (\varepsilon-\varepsilon_1/L)得证
   $$

   **注：李普希兹连续不可少。** 这是由于在使用三角不等式后第一个差项，若无李普希兹，无法用连续的定义放到下一步。

   总结两道题，都可以发现，**必须把其中一个变元取定，才可以对另一个变元应用性质**

   本质上，之所以不能使用类似：
   $$
    f对y连续\Rightarrow |f(x,y) - f(x,y_0)|<\epsilon
   $$
   的定理，是因为该不等式的前提：
   $$
    |y-y_0| < \delta
   $$
   不一定能不满足。

   注意到$(x,y)$是任意取的点，**y可能受制于x**。

   此外，极限语言中的 **$\delta$是取决于$\varepsilon$的**
   
   具体来说，如果点取在某些路径上，那对于给定的$\varepsilon$，取定某些$x,\delta$使得$|x-x_0| < \delta$，但同时$y$却不一定能满足这个$\delta$使得$|y-y_0|<\delta$

   但我们能使用：
   $$
    f对y连续\Rightarrow |f(x_0,y) - f(x_0,y_0)|<\epsilon
   $$

   这是因为$x_0$已经定死了，**$y$不再受制于$x$**，故不等式成立

   本质上，李普希兹连续承诺了**无论$|y-y_0|$是多少，都可以找到李普希兹常数L来限制函数的差分**，也就是说有：
   $$
    f对y李普希兹连续 \Rightarrow |f(x,y) - f(x,y_0)| \le L|y-y_0|
   $$
   
### 7. 可微的必要条件
证明：可微推出偏导数存在

证：
由$f(x,y)$可微，有
$$
  f(x_0+\Delta ,y_0+\Delta y)- f(x_0,y_0) = A\Delta x + B\Delta y + o(\rho)
$$

下证$f_x$存在

由偏导数定义，令$\Delta y = 0$，有

$$
  f(x_0+\Delta x ,y_0)- f(x_0,y_0) = A\Delta x + o(\rho)
$$

由微分中值定理，有：
$$
  f(x_0+\Delta x ,y_0)- f(x_0,y_0) = f_x(x_0 + \theta \Delta x,y_0)\Delta x = A\Delta x + o(\rho)
  \\
  (\theta \in (0,1))
$$
也即
$$
  \lim_{\Delta x \to 0} f_x(x_0 + \theta \Delta x,y_0) = A
$$
结合导数连续定理，可得：
$$
  f_x(x_0,y_0) = A
$$
同理可得：
$$
  f_y(x_0,y_0) = B
$$
证毕

### 8. 可微充分条件
证明：若二元函数$f(x,y)$的两偏导数连续，则$f$可微
（待完善）

### 9. 混合二阶偏导数相等的条件
证明：若一二元函数在$(x_0,y_0)$处连续可导，则$f_{xy}(x_0,y_0) = f_{yx}(x_0,y_0)$
（待完善）

### 10. 证明函数在点的任一邻域内无界
证明：
$$
  f(x,y) = \frac{x}{x^2+y^2} \cos \frac{1}{x^2+y^2}
$$
在$(0,0)$的任意邻域内无界

证：只要证：
$$
  \forall M > 0, \forall \delta > 0\\
  \exist P \in U^0(O,\delta),\\
  s.t.
  |f(P)| > M
$$
取定路径$y = x,x>0,y>0$上的点, 令：
$$
  x^2 + y^2 = \frac{1}{2k\pi}, k \in Z\\
  \Rightarrow x=y=\sqrt{\frac{1}{4k\pi}}
$$
  下证总$\exist k$，使得上式成立:
$$
  因为\sqrt{x^2+y^2} < \delta\\
  只要取k > \frac{1}{2\pi \delta}即可
$$
此时：
$$
  
  要令|f(x,y)| = \left|\frac{x}{x^2+y^2}\right| = \left|\frac{1}{2x}\right| > M\\
  ，则只要|x|=\sqrt{\frac{1}{4k\pi}} < \frac{1}{2M}\\
  即k > M^2/\pi,取k = M^2
$$
故存在$(x,y) = (\sqrt{\frac{1}{4M^2\pi}}, \sqrt{\frac{1}{4M^2\pi}})$使得$|f(x,y)| > M$

**注1：** 一种等价但逻辑更清晰的表达方式是取点列$P_n = \left(\sqrt{\frac{1}{4n\pi}}, \sqrt{\frac{1}{4n\pi}}\right)$。但这种取法往往不是正面推导想到的，所以可以依照上述的逻辑*分析*出思路，再*整理*出使用点列的表达



**注2：** 通过证明$\left|\frac{x}{x^2+y^2}\right|$趋向于无穷的方法是不成立的。因为这个式子沿不同路径趋近于$(0,0)$的极限不相等（甚至有的是0，有的是无穷），也即不是趋向于无穷。

但我们只需要*找到*一个充分大的点即可，故可以**取定特定的路径**。

### 11. 证明n次齐次函数充要条件
证明：
$$
  f(tx,ty,tz) = t^nf(x,y,z) \Leftrightarrow xf_x +yf_y+zf_z = nf(x,y,z)\quad \forall t \in R
$$
证：
$$
  左到右：对t求导，t取1即可
$$
$$
  右到左：构造G(t) = \frac{f(tx,ty,tz)}{t^n}\\
  只要证G(t) = f(x,y,z)即可\\
  G'(t) = \frac{n t^{- n} f{\left(t x,t y,t z \right)}}{t} +\\ 
  t^{- n} 
  \left(
    x \frac{\partial}{\partial (tx)} 
  f{\left(tx,t y,t z \right)}
  + y \frac{\partial}{\partial (ty)} f{\left(t x,\xi_{2},t z \right)}
  + z \frac{\partial}{\partial (tz)} f{\left(t x,t y,tz \right)}
  \right)\\
  = n t^{- n - 1} f{\left(t x,t y,t z \right)} - \frac{n t^{- n} f{\left(t x,t y,t z \right)}}{t}（根据右边公式）\\
  = 0
$$
$$
  故G(t)为常函数，G(t) = G(1) = f(x,y,z)，证毕
$$

### 12. 证明隐函数存在性定理

### 13. 证明隐函数组存在性定理

### 14. 证明二元微分中值定理

### 15. 证明二元泰勒公式

### 16. 证明二元连续可微函数的极值点充分条件

### 17. 求以下重极限
1. 
  $$
    \lim_{(x,y)\to(0,0)} \frac{\sin(x^3+y^3)}{x^2+y^2}\\
    解：\left|\frac{\sin(x^3+y^3)}{x^2+y^2}\right|\\
    = \left|\frac{x^3+y^3+o(x^3+y^3)}{x^2+y^2}\right|\\
    \le |x| + |y| + \left|\frac{o(x^3+y^3)}{x^2+y^2}\right|\\
    \to 0 + 0 + 0 = 0\\
    故所求为0
  $$
2. 
  $$
    \lim_{(x,y)\to(0,0)}x^2 y^2 \ln(x^2+y^2)\\
    解：|x^2 y^2 \ln(x^2+y^2)|\\
    = |\frac{x^2}{x^2+y^2}(x^2+y^2)\ln(x^2+y^2)|\\
    \le |(x^2+y^2)\ln(x^2+y^2)| \to 0\\
    故所求为0
  $$
3. 
  $$
    \lim_{(x,y)\to(0,0)}\frac{x^2 y^{3/2}}{x^4+y^2}\\
    解：取路径y=kx^3,\\
    \frac{x^2 y^{3/2}}{x^4+y^2}=\frac{k^{3/2}}{x^3+k^2x^5} \to \infty\\
    又取y=kx时极限为0，故重极限不存在
  $$

4. 
   $$
   \lim_{(x,y)\to(0,0)}\frac{e^x - e^y}{\sin(xy)}\\
   解：取y=x得路径极限为0\\
   再取y=\ln (x+1)，则\\
   \frac{e^x - e^y}{\sin(xy)} \\
   = \frac{e^x - x - 1}{\sin(x\ln(1+x))}\\
   = \left.1\middle/\left(\frac{x\ln(x+1)}{e^x-x-1} + o(x\ln(x+1))/(e^x-x-1)\right)\right.
   $$
   又
   $$
   \frac{x\ln(x+1)}{e^x-x-1}\\
   = \frac{x(x+o(x))}{x^2/2+o(x^2)} = 2
   $$
   可知原极限为$1/2$，不等于$y=x$路径上的极限，故重极限不存在


### 18. 推广闭集上的连续函数性质至无穷区间
已知$f(x,y)在R^2$上连续，$\lim_{x \to \infty, y \to \infty}f(x,y) = A$，求证：

1. $f(x,y)$在$\mathrm{R}^2$上有界
2. $f(x,y)$在$\mathrm{R}^2$上一致连续

证：

1. 
  $$
    下证有上界，下界同理。\\
    取\varepsilon = 1\\
    由极限性质，有：\\
    \exists M_1 > 0, \forall P:r(P) > M_1,|f(P)-A|<1\\
    也即f(P)<A+1\\
    取D=\{P|r(P) \le M_1\}，D为有界闭集\\
    \forall P \in D，f(P) \le M_2，其中M_2为f在D上的最大值\\
    令M=\max(A+1,M_2),由上可知，\forall P \in \mathrm{R}^2,|f(P)|\le M\\
    也即M为f(P)上界；同理可证有下界\\
    有界性得证
  $$

2. 
   $$
    由极限性质，有：\\
    \exists M_1 > 0, \forall P_1,P_2:r(P_1),r(P_2) > M_1,|f(P_1)-f(P_2)|<\varepsilon/2\\
    （这是柯西收敛准则）\\
    取D_1=\{P|r(P) \le M_1 < M\},\\
    此外，令D = \{P|r(P) \le M\},D为有界闭集\\
    则f在D上一致连续，也即：\\
    \exist \delta >0, \forall P_1,P_2 \in D且r(P_1,P_2)<\delta/2, |f(P_1)-f(P_2)| < \varepsilon /2\\
    由上可知，f在D，\bar{D_1}上分别一致连续\\
  $$

  再证$f在R^2上一致连续$：

  $$
    \forall P_1 \in D_1,  \forall P_3 \in \bar{D},\exist P_2 \in D\cap\bar{D_1}，使得r(P_1,P_2)<\delta/2且r(P_2,P_3)<\delta/2，\\
    也即r(P_1,P_3)<\delta\\
    |f(P_1)-f(P_3)| \le |f(P_1)-f(P_2)|+|f(P_2)-f(P_3)| < \varepsilon / 2 + \varepsilon / 2 = \varepsilon\\
    一致连续性得证
  $$

### 19. 含参数重极限的分类讨论
讨论
$$
  f(x,y) =  \frac{x}{(x^2+y^2)^p},p>0
$$
在$(0,0)$处的重极限

解：
当$p\ge\frac{1}{2}$时，取$y=x$：
$$
\frac{1}{2} x^{1-2p} \to 0
$$
故极限不存在

当$p<\frac{1}{2}$时：
$$
  \left|\frac{x}{(x^2+y^2)^p}\right|\\
  = |x^{1-2p}/(\frac{x^2+y^2}{x^2})|\\
  \le |x^{1-2p}| \to 0
$$
故极限为0