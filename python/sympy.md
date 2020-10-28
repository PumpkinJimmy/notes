## Sympy
数学符号运算
### Quick Start
```python
from sympy import Symbol
x = Symbol('x')
y = Symbol('y')
f = (x+1)**2
f # 显示公式
f.subs(x,y**2) # 代入式子
float(f.subs(x,2)) # 代入求值
sympy.diff(f,x) # 求导
sympy.integrate(f,x) # 不定积分
f.simplify # 化简
g = f.expand() # 拆开多项式
g.factor() # 因式分解
g.as_ordered_factors() # 取因子
```
### 基本操作
```python
f.subs # 代入，可以是式子，或者数值
f.evalf # 计算为浮点数
lambdify(x,f,'numpy') # 把符号表达式转换成方便数值计算的函数（如用于Numpy的ufunc）
oo # 无穷
Rational # 有理数
sympy.simplify(f1-f2) # 比较两个表达式是否恒等（不能用==）
f1.equal(f2) # 通过随机取点测试两个表达式是否恒等
expr.doit() # 把一个保持形式计算的式子展开计算
sympy.latex(expr) # 输出latex

# 抽象函数，可以求导
f = sympy.Function('f')
f = f(x,y,z) 

Sum(i,(i,1,n)) # 求和/级数
```
### 特殊函数
```python
factorial # 阶乘
binomial # 组合数
gamma # Gamma函数
```
### 表达式化简
```python
f.simplify # 化简，但“简”是不易定义的，故只能稍作简化
f.expand # 把括号和分式全部拆开
f.factor # 因式分解，expand的逆运算
f.as_ordered_factors # 拆成一个因子列表
f.cancel # 繁分式化简
f.collect # 以某一变量为主元整理式子（注意要先拆开所有括号）
f.apart # 裂项
f.trigsimp # 三角化简
f.expand_trig # 三角拆角（三角化简得逆运算）
f.combsimp # 组合化简
f + O(x**a) # O记号化简
```
### 微积分
```python
limit # 求极限
integrate # 求定积分/不定积分
diff # 求任意阶导
series # 泰勒级数
fourier_series # 傅里叶级数
```

### 方程求解
```python
eq = Eq(expr_left, expr_right)
solve(eq)
```