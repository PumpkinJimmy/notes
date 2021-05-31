## Matlab Quick Ref
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

% 循环
for i=start:stop
    % body
end

% 分支
if cond
    % body1
elseif cond
    % body2
else
    % body3
end

% 二项分布
binopdf(n,p,k) % 概率密度
binocdf(n,p,k) % 累积密度

% 显示格式设置
format long % 长格式（15位）
format short % 短模式（5位）
grid on % 显示网格

```