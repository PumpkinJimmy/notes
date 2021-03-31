## ARMA模型
### 白噪声
若随机过程$\varepsilon_t$满足：
$$
    \forall t\\
    E(\varepsilon_t) = 0, Var(\varepsilon_t) = \sigma^2\\
    cov(\varepsilon_t, \varepsilon_{t-s}) = 0
$$

则称其为白噪声。总结而言白噪声的特点是**零均值、同方差、无自相关**

### AR模型
- 即自回归模型
- 符号表达式：
  
  AR(p)模型记作：
  $$
  y_t = \alpha_1 y_{t-1} + \alpha_2 y_{t-2} + \cdots + \alpha_p y_{t-p} + \varepsilon_t
  $$

  表示t时刻的值可以表示为前p个时刻的值的线性组合加上一个白噪声

### MA模型
- 即滑动平均模型
- 符号表达
  
  MA(q)模型记作：
  $$
    y_t = \varepsilon_t + \beta_1 \varepsilon_{t-1} + \cdots + \beta_q \varepsilon_{t-q}
  $$

  表示t时刻的值可以表示为前q个时刻的白噪声的线性组合

### ARMA模型
- 将AR模型和MA模型组合起来
- 符号表达：
  
  ARMA(p,q)记作：

  $$
    y_t = \alpha_1 y_{t-1} + \alpha_2 y_{t-2} + \cdots + \alpha_p y_{t-p} + \\
    \varepsilon_t + \varepsilon_t + \beta_1 \varepsilon_{t-1} + \cdots + \beta_q \varepsilon_{t-q}
  $$