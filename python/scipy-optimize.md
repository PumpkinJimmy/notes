## SciPy.Optimize
数值优化算法库

### 无约束优化
- 梯度优化算法
- （待完善）

### 约束优化
- `minimize`：多变量连续函数的约束优化
  - 三种可选的内置算法：`trust-constr,SLSQP,COBYLA`
  - 依赖Jacobian和Hessian（可以作为参数传入，也可以直接让`minimize`用有限差分法和拟牛顿法去估计）
  - `trust-constr`
    - “信赖域算法”的一种
    - 可以传入bounds参数来提供自变量范围
    - 使用constraints参数来设置约束：使用类LinearConstraint/NonLinearConstraint来指定
  - 如果约束对应的可行域为空，优化会失败且**会给出意义不明的失败信息**

### 最小二乘法
- 最小二乘法更适合*拟合*而非*规划*（也即将规划问题用最小二乘法来做不一定合适）
- `least_squares`
  - 参数`func`用于指定**残差函数**，给出的是每个待拟合的点与拟合函数的残差（**含符号**）
  - 传入bounds参数来指定每个分量的取值范围
  - 无法指定约束（请使用`minimize`）
  - 可以指定`loss`参数来指定loss函数，从而降低对离群值的敏感度，提高拟合的鲁棒性
