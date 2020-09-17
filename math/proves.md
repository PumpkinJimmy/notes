## 数分证明题目
### 实数理论互推
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
### 开闭集互补
证明：开集的补集是闭集
$$
    用反证法.设开集E的补集E^c不是闭集\\
    则\exists P \in E为E^c的极限点\\
    则由开集定义，\forall \delta > 0,U(P,\delta) \subset E \\
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
### 有界闭集连续函数性质2：一致连续
用反证法，设函数$f$在有界闭集D上连续但不一致连续，则：
$$      
\exists \varepsilon >0 满足 \forall \delta >0 只要P,Q \in D有r(P,Q)<\delta，则|f(P)-f(Q)| \ge \varepsilon\\
取\delta = \frac{1}{n}则\\
|f(P_n)-f(Q_n)| \ge \frac{1}{n}\\
P_n \in D，故存在收敛子列\{P_{n_k}\}.设极限为P_0\\

又|f(P_{n_k}) - f(Q_{n_k})| \ge \frac{1}{n_k}\\
且r(P_0, Q_{n_k}) \le r(P_0,P_{n_k}) + r(P_{n_k}, Q_{n_k})\\
\le \varepsilon_1 + \frac{1}{n_k}\\

也即Q_{n_k} \to P_0\\

故|f(P_{n_k}) - f(Q_{n_k})| \to 0, 与假设矛盾，原命题得证
$$

### 证明$\cos xy$不一致连续
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

### 有界闭区域连续函数性质3：介值定理
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

### 证明不一致有界
证明：$ 1/(1-xy)$在$[0,1) \times [0,1)$上连续但不一致连续
证：
$$
  设f(x,y) = \frac{1}{1-xy}\\
  由初等函数性质可知连续\\
  取点列\{(\frac{n-1}{n}, \frac{n-1}{n})\}以及\{(\frac{n}{n+1}, \frac{n}{n+1})\}，记作P_n,Q_n \\
  当n \to \infty 时 两点列都趋于1\\
  且有|f(P_n)-f(Q_n)| \ge \frac{1}{3}\\
  故不一致连续
$$