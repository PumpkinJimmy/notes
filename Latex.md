## Latex笔记
### Quick Start
```latex
% 百分号注释
\documentclass[UTF8]{ctexart} % 中文文章的文档类
\usepackage{amsmath}
\title{Latex Hello World}
\author{bbpumpkin}
\date{\today}
\begin{document}
    \maketitle % 显示元信息
    \section{Hello World}
    \paragraph{My First Par}
    Hello, World!

    \section{Math}
    行内公式: $ E= mc^2 $

    单行公式:
    \[
        E = mc^2
    \]

    :
    % "&" 用于对齐, "\\" 表示换行
    \begin{align*}
        F &= \frac{mv^2}{r} \\
          &= m\omega^2 r \\
    \end{align*}
\end{document}
```

### 文档结构基本
（待完成）
### 数学符号
- 在数学环境里面，排版是根据符号之间的逻辑来自动进行的，所以渲染时会**忽略源码的空格，自动在适当位置添加空格**
- 某些数学符号（尤其是一些大符号）需要amsmath宏包，强烈推荐使用
- 常用数学环境：
  - 标准的`$$` `\[\]`
  - 多行公式`equation` `align`以及它们的无编号版本`equation*` `align*`（需要amsmath)
  - 方程组`cases`
- `_`下标；`^`上标
- 矢量：
  - 单符号矢量`\vec a`: $\vec a$
  - 多符号量`\overrightarrow{AB}`: $\overrightarrow{AB}$
- 希腊字母:
  - 小写如`\phi`:$\phi$
  - 大写如`\Phi`:$\Phi$
  - 某些特殊写法如`\varphi`:$\varphi$

- 二元运算符：
  - `\times`:$\times$; `\div`: $\div$
  - `\ge`: $\ge$; `\le`: $\le$; `\approx` :$\approx$;`\ne`: $\ne$; `\propto`: $\propto$; `\sim`: $\sim$
  - `\pm`: $\pm$; `\mp`: $\mp$
  - 
  - `\overline{A}`: $\overline{A}$
- 集合：
  - `\in`: $\in$; `\not\in`: $\not\in$
  - `\exist`: $\exist$; `\forall`: $\forall$
  - `\subset`:$\subset$; `\subseteq`
  - `\cap`: $\cap$; `\cup`: $\cup$
  - `\empty`:$\empty$; `\varbnothing`:好看的空集号
  - `\bigcap`:$\bigcap$；`\bigcup`:$\bigcup$
- 累计符号:
  - `\sum_{i=1}^{n}`:$\sum_{i=1}^{n}$
  - `\sum\limits_{i=1}^{n}`:$\sum\limits_{i=1}^{n}$
  - `\prod`同理：$\prod_{i=0}^{n}$
- 微积分
  - 所有大符号都可以通过加`\limits`来把下标转成正确的格式
  - `\lim`: $\lim_{x \to \infty}$：
  - `\int`: $\int_{a}^{b}$;
  - `\iint`: $\iint$；通过`\limts`加上积分限
  - 微分符号`\mathrm{d}x`: $\mathrm{d}x$（之所以这么麻烦是因为数学环境默认将字母渲染为斜体，但标准的微分里的"d"是非斜体的）
  - `\partial x`: $\partial x$
  - 大竖线`\big|`: $\big|$
- 分式、根式：
  - `\frac{a}{b}`: $\frac{a}{b}$
  - `\sqrt{a+b}`: $\sqrt{a+b}$
- 空格：`\quad`
- 矩阵，行列式：
  ```latex
  \left| \begin{array}{cccc}
  a & b \\
  c & d 
  \end{array} \right|
  ```
  只要适当更换`\left``\right`后面的符号就可以用于表示矩阵等