## 数理统计

### 样本与统计量
- 总体：一个随机变量，可以表示研究对象的全体
- 样本与样本值：
  
  （简单随机）样本是一组关于总体的独立同分布的随机变量

  样本值是样本的一组观察值
- 统计量与统计值：一个随机变量，用一个不包含任何未知参数的关于样本的函数表示；统计值是给的样本值对应的统计量的观察值
- （待完善）

### 参数估计：点估计
- 估计量：表示对未知参数的估测的统计量
- 估计值：表示给定样本值的情况下估计的具体值
- 点估计量的优良性评判标准
  - 无偏性：估计量满足$E\hat{\theta} = \theta$
  - 均方误差：定义估计量的均方误差：
    $$ MSE(\hat{\theta}) = E(\hat{\theta} - \theta)^2$$

    均方误差越小，点估计量越优良。当不存在比某估计量MSE更小的估计量时称该估计量为*最小方差估计量*
  - 相合性：估计量依概率收敛于未知参数，也即满足：
    $$ \lim_{n \to \infty}P(|\hat{\theta} - \theta| \ge \epsilon) = 0
- 矩估计：利用样本矩来构造估计量的估计方法
  常见矩估计：
  - 样本均值估计总体期望 $EX = \bar{X}$
  - 样本二阶中心矩估计总体方差
    $$DX = B_2 = \frac{1}{n}\sum (X_i - \bar{X})^2$$


### 参数估计：区间估计
- 置信度：
- 置信水平：
- 枢轴量

  枢轴量是指随机变量$U(X_1, X_2, ..., X_n; \theta)$

  *U的特点*：与样本以及**未知参数有关**，且U**分布已知**
  
  U的作用：利用不等式$P(-F_{\alpha/2} < U < F_{\alpha/2}) = 1 - \alpha$（其中$\alpha$表示置信度，$F_{\alpha/2}$表示U的分布的上$\alpha/2$分位数）解出未知参数的估计区间

  构造U往往从某些$\theta$的估计量出发来构造，比如正态分布$\mu$的区间估计的枢轴量就是从样本均值出发构造的：
  $$ U = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \sim N(0,1)$$