## SciPy.stats
统计学模块。包含各种分布的计算机实现
1. 常用分布：
- `binom` 二项分布
- `norm` 正态分布
- `expon` 指数分布
- `possion` 泊松分布

2. 分布对象的方法：
- `rvz()` 返回一个服从分布的样本值
- `pdf()` 概率密度
- `pmf()` 概率质量函数
- `cdf()` 累计分布函数
- `isf()` 上$\alpha$分位数
- `mean()`/`var()`/`std()` 均值/方差/标准差