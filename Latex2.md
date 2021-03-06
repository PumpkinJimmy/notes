## Latex笔记
### 概念
- Tex
  一个排版系统，相当于内核，只提供原始的命令，但可以通过组装命令来提供拓展功能
- Tex格式
  理解为一种书写文档的语法规则吧，原始Tex配套一个PlainTex的Tex格式
- Latex
  世界上最流行的Tex格式
- Tex套装
  包含核心部分和一些宏包
- TexLive
  流行的跨平台Tex套装
### 安装
直接装TexLive，Unbuntu下直接apt install texlive
中文支持：apt install texlive-lang-chinese
### 基本使用
- latex src.tex 使用latex，输出div
- xelatex src.tex 使用latex，输出pdf
### 注意事项
- latex里面不要随便加空行，因为空行用于分割段落！
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
### 插入代码片段
使用宏包`listings`来完成
（待完善）
### 数学符号
- 在数学环境里面，排版是根据符号之间的逻辑来自动进行的，所以渲染时会**忽略源码的空格，自动在适当位置添加空格**
- 某些数学符号（尤其是一些大符号）需要amsmath宏包，强烈推荐使用
- 常用数学环境：
  - 标准的`$$` `\[\]`
  - 多行公式`equation` `align`以及它们的无编号版本`equation*` `align*`（需要amsmath)
- 方程组`cases`，注：这**不是一个数学环境**，是在数学环境下才能使用的一个环境
- `_`下标；`^`上标
- 矢量：
  - 单符号矢量`\vec a`: $\vec a$
  - 多符号量`\overrightarrow{AB}`: $\overrightarrow{AB}$
- 希腊字母:
  - 小写如`\phi`:$\phi$
  - 大写如`\Phi`:$\Phi$
  - 某些特殊写法如`\varphi`:$\varphi$

- 二元运算符：
  - `\times`:$\times$; `\div`: $\div$; `\cdot`: $\cdot$
  - `\ge`: $\ge$; `\le`: $\le$; `\approx` :$\approx$;`\ne`: $\ne$; `\propto`: $\propto$; `\sim`: $\sim$
  - `\pm`: $\pm$; `\mp`: $\mp$
  - 大的横式除号:`\left. \middle/ \right.` 注意：左右的left/right是必须的，`.`是占位的，不表示具体定界符号
  - `\overline{A}`: $\overline{A}$
- 集合：
  - `\in`: $\in$; `\not\in`: $\not\in$
  - `\exist`: $\exist$; `\forall`: $\forall$
  - `\subset`:$\subset$; `\subseteq` $\subseteq$
  - `\cap`: $\cap$; `\cup`: $\cup$
  - `\empty`:$\empty$; `\varbnothing`:好看的空集号
  - `\bigcap`:$\bigcap$；`\bigcup`:$\bigcup$
- 定界符号
  - `\{[()]\]`: $\{[()]\}$ （注意大括号转义）
  - `\left/\right` + 定界符号：自适应大小的定界符号
- 累计符号:
  - `\sum_{i=1}^{n}`:$\sum_{i=1}^{n}$
  - `\sum_{i=1}^{n}（行间）`:
  $$\sum_{i=1}^{n}$$
  - `\sum\limits_{i=1}^{n}（行内）`:$\sum\limits_{i=1}^{n}$
  - `\prod`同理：$\prod_{i=0}^{n}$
- 微积分
  - 所有大符号都可以通过加`\limits`来把下标转成正确的格式
  - `\lim`: $\lim_{x \to \infty}$：
  - `\int`: $\int_{a}^{b}$;
  - `\oint`: $\oint$
  - `\iint`: $\iint$；通过`\limts`加上积分限
  - 微分符号`\mathrm{d}x`: $\mathrm{d}x$（之所以这么麻烦是因为数学环境默认将字母渲染为斜体，但标准的微分里的"d"是非斜体的）
  - `\partial x`: $\partial x$
  - 大竖线`\big|`: $\big|$
- 分式、根式：
  - `\frac{a}{b}`: $\frac{a}{b}$
  - `\sqrt{a+b}`: $\sqrt{a+b}$
- 空格：`\quad`
- 上方标记: `\overset{ }{AB}`，第一个大括号内填上方的东西，比如`\frown`得到弧AB的符号
- 矩阵，行列式：
  ```latex
  \left| \begin{array}{cccc}
  a & b \\
  c & d 
  \end{array} \right|
  ```
  只要适当更换`\left``\right`后面的符号就可以用于表示矩阵等

- 关于三角函数这类符号
  
  把`sin`写作`\sin`看起来是没有意义的，但观察两种写法：$sin t, \sin t$，其中前者是`sin t`，后者是`\sin t`，可以看到不加反斜杠，`sin`是斜体的，且与后面的`t`没有间隔；加了反斜杠，`sin`变回正体且与后面的内容有合理空格。
  
  本质上，这是因为Latex数学模式将`sin`认定为三个代数字母，故为斜体且与无视空格后面的字母紧密连接（认为是一个因式）；而加了斜杠以后`\sin`是一个控制序列，其有了合理的排版特性，且可以使用花括号记法如`\sin{AC}`：$\sin{AC}$

  因尽量使用`\sin`这样的*控制序列*而不是`sin`这样的*字母*

- `\text`允许在数学环境里面插入普通模式下的字符,使用场景包括:
  - 不需要标注斜体的字母,如dx中的d
  - 物理上的单位
  - 中文

### Latex机制详解
#### class & package
- *文档类(class)*文件以`.cls`结尾，使用`\documentclass`指定文档类
- `.sty`文件为*package*，使用`\usepackage`引入package.注：一条usepackage可以引入多个package，用逗号分隔
- 使用`\documentclass[<option>]{}` `\usepackage[<option>]{}`指定选项；一般来说，文档类的选项会自动应用到每一个package
- 标准文档类（常用）：
  - article
  - book
  - letter
  - report
  - slides
  - minimal
- ctex中文文档类：
  - ctexart
  - ctexrep
  - ctexbook
#### Tex安装文件夹结构
- 通常称Tex的安装根目录为“texmf”；Windows下的MikTex一般就是安装文件的文件夹内的MikTex文件夹
- texmf下的tex目录下会有许多文件夹。通常每个文件夹就是一组宏包，其中可能包含.sty/.cls/.def文件

#### 安装宏包
三种方法：
1. 直接把需要的`.cls/.sty`文件放到被编译的tex文件的同一个目录下。这个方法通常用于应用某些特定期刊的模板。
2. 使用自带的宏包管理器管理（Linxu TexLive下tlmgr命令，Windows MikTex下有Package Manager GUI）
3. 手动安装：根据README的说明调用`latex xxx.ins`编译生成`.sty/.cls`文件，或者有Makefile的直接make，然后把文件夹放到正确的位置。

通常还可以`latex xxx.dtx`生成文档

**注意！** 在Windows MikTex下必须使用`Setting(Admin)`工具更新包索引信息才能识别出