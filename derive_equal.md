## 微分方程
### 通解与特解
含任意常数个数等于微分方程阶数的解就是通解。若通过个别取值确定了任意常数的值的解就是特解
### 可分离变量的微分方程
形如$f(x)dx = g(y)dy$的微分方程可以直接通过等式两端积分得到结果（虽然结果可能是隐函数）
这是所有微分方程解法的根源，也是最为灵活的解法，理论上可以处理各种乱七八糟的形式
但如何进行变量代换和等式变形才能分离才是最大的难点
可以说，除了特定简单形式，微分方程没有系统解法，只能想办法化成可分离变量的形式
### 线性
形如$y'+ P(x)y=Q(x)$的叫一阶线性微分方程；形如$y'' + P(x)y' + Q(x)y = R(x)$的叫做二阶线性微分方程，以此类推
之所以叫线性，是由于对y以及y的导数都是线性的（也就是没有y的高阶、yy'这样搅在一起的，等等）
若等式右端的不含y以及y的导数的项为0，则是齐次，否则是非齐次。非齐次通解可以表示为齐次通解加一个特解（类似线性方程组的解）
究其根本，只要将微分方程中的函数及其导函数看作是向量空间中的向量，线性微分方程就可以直接套用线性代数关于解的所有结论
若y及y的导数的系数为常数，则称为常系数微分方程，否则是变系数微分方程
一般高阶情形下只有常系数线性微分方程以及一些特殊形式的变系数微分方程（如欧拉方程）能解出来
### 齐次方程
可以化为$y'=f(y/x)$的称为齐次方程，可以通过整体换元求解（这是少有的能处理非线性微分方程的系统方法）
### 降阶
某些特殊形式的二阶甚至高阶微分方程可以通过降阶的方式来求解如
$$
y^{(n)} = f(x) \\
y'' = f(x, y') \\
y'' = f(y, y')
$$
### 常系数线性齐次微分方程
结合线性代数的向量空间理论可以证明此类方程的通解就是线性无关的特解的线性组合。
同时，由构造法和欧拉公式得出此类方程可以通过解特征方程根据解直接写出通解（由代数方程在复数域内有解，解必然只能是单实根、重实根、单复根、重复根）
