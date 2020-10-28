## Scipy Linear Algebra
线性代数支持
- `qr` QR分解。等你需要处理线性问题的时候，QR分解将任意矩阵分解为一个正规正交矩阵$Q$和上三角矩阵$R$
  
  实际上考虑到$R$为上三角矩阵，这个分解实际上可以看作是类似消出行阶梯行的一个过程

- `svd`
  > 解决线性问题时考虑SVD可以降低难度

  > 这是完美的分解

  SVD的形式：
  $$
    A = U\Sigma V^T 
  $$
  说明：
  $$
    其中，A \in R^{m\times n}\\
    U \in R^{m \times m}, V \in R^{n\times n}是正交矩阵\\
    \Sigma \in R^{m\times n}是对角矩阵 \\
    \Sigma的所有对角元素都是A的奇异值 \\
    V的前r列是A的列空间基（r为A的秩），后面n-r列为A的零空间基（可用于解欠定方程组）\\
    U的前r行是A的行空间基（r为A的秩），后面m-r行为A的左零空间基
  $$