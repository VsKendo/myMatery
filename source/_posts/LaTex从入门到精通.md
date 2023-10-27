---
title: LaTex从入门到精通
date: 2022-03-19 17:03:16
tags: [LaTex, Math]
top: false
author: VsKendo
mathjax: true
categories: 写作
summary: LaTeX是一种基于TeX的文档排版系统，成为现在最流行的科技写作——尤其是数学写作的工具之一.
description: LaTeX是一种基于TeX的文档排版系统，成为现在最流行的科技写作——尤其是数学写作的工具之一.
---

# 简介

LaTeX是一种基于TeX的文档排版系统,把大片排版的格式细节隐藏在若干样式之后，以内容的逻辑结构统帅纷繁的格式，遂成为现在最流行的科技写作——尤其是数学写作的工具之一. 在Markdown中插入数学公式的语法是 `$数学公式$`和 `$$数学公式$$`.

# 行内公式

行内公式是可以让公式在文中与文字或其他东西混编，而不独占一行。在数学模式下，符号会使用单独的字体，字母通常是倾斜的意大利体，数字和符号则是直立体。而数学符号之间的距离也与一般的水平模式不同:

| 示例           | 显示         |
|--------------|------------|
| `$2x+3y=34$` | $2x+3y=34$ |
| `2x+3y=34`   | 2x+3y=34   |

因此，在排版数学公式时，即使没有特殊符号的算式（如1+1=2），或者简单的一个字母变量，也建议进入数学模式，使用`$1+1=2$`，`$x$`，而不是使用排版普通文字的方式。

## 注意

并非所有的 Markdown解析器/编辑器 都支持数学公式。所以在不支持的情况下显示出来的东西可能不尽人意。hexo要安装mathjax数学公式支持插件才支持，JetBrains家的产品默认不支持。

在本博客中，使用的是2.7.9的mathjax.js解析器，并不能解析指令//和/newline。所以部分显示可能有bug。

# 独立公式

独立公式单独占一行,不和其他文字混编
示例: ` $$c=2πr$$`
显示: $$c=2πr$$

# 断行和分页

使用 `\\`或 `\newline`  来换行。

也可以使用 `\\[5pt]`来指定换行后，与前一行之间间隔5个位置。

示例:

```latex
$$   
2x+3y=34\\
x+4y=25
$$
```

显示
$$
2x+3y=34\\
x+4y=25
$$

此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：

![image-20220504133602115](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/133602-b1.png)

# 常用符号

| 符号  | 示例                                                  | 显示                                                |
|-----|-----------------------------------------------------|---------------------------------------------------|
| 上下标 | `S=a_{1}^2+a_{2}^2+a_{3}^2$`                        | $S=a_{1}^2+a_{2}^2+a_{3}^2$                       |
| 括号  | `$f(x, y) = 100 * \lbrace[(x + y) * 3] - 5\rbrace$` | $f(x, y) = 100 * \lbrace[(x + y) * 3] - 5\rbrace$ |
| 分数  | `$\frac{1}{3} 与 \cfrac{1}{3}$`                      | $\frac{1}{3} 与 \cfrac{1}{3}$                      |
| 开方  | `$\sqrt[3]{X}$`和`$\sqrt{5 - x}$`                    | $\sqrt[3]{X}$`和`$\sqrt{5 - x}$                    |
| 省略号 | `\dots`或`\cdots`                                    | $\dots$                                           |
| 加粗  | `$ f(x) = \textbf{x}^2 $ `                          | $ f(x) = \textbf{x}^2 $                           |
| 纯文本 | `$ f(x) = x^2 \mbox{abcd} $`                        | $ f(x) = x^2 \mbox{abcd} $                        |
| 特殊  | `$\TeX$ `  斜杠加上任意普通字符即可                             | $\TeX$                                            |
| 波浪线 | `$\sim$`                                            | $\sim$                                            |
| 空格  | `$\ $`                                              | $\ $                                              |
| tab | `$\qquad$`                                          | $\qquad$                                          |
| 爱心  | `$\heartsuit$`                                      | $\heartsuit$                                      |



# 其他字符

数学中常用的字符，比如关系运算符、对数运算符、三角运算符等。



## 关系运算符

| 代码         | 符号           |
|------------|--------------|
| \pm        | $\pm$        |
| \times     | $\times$     |
| \div       | $\div$       |
| \mid       | $\mid$       |
| \nmid      | $\nmid$      |
| \cdot      | $\cdot$      |
| \circ      | $\circ$      |
| \ast       | $\ast$       |
| \bigodot   | $\bigodot$   |
| \bigotimes | $\bigotimes$ |
| \bigoplus  | $\bigoplus$  |
| \leq       | $\leq$       |
| \geq       | $\geq$       |
| \neq       | $\neq$       |
| \approx    | $\approx$    |
| \equiv     | $\equiv$     |
| \sum       | $\sum$       |
| \prod      | $\prod$      |



## 对数运算符

| 代码 | 符号   |
| ---- | ------ |
| \log | $\log$ |
| \lg  | $\lg$  |
| \ln  | $\ln$  |



## 三角运算符

| 代码   | 符号     |
| ------ | -------- |
| \bot   | $\bot$   |
| \angle | $\angle$ |

还有\sin、\cos等，所有三角函数的缩写都可以。



## 微积分运算符

| 代码       | 符号         |
| ---------- | ------------ |
| \prime     | $\prime$     |
| \int       | $\int$       |
| \iint      | $\iint$      |
| \iiint     | $\iiint$     |
| \oint      | $\oint$      |
| \infty     | $\infty$     |
| \nabla     | $\nabla$     |
| \mathrm{d} | $\mathrm{d}$ |



## 集合运算符

| 代码      | 符号        |
| --------- | ----------- |
| \emptyset | $\emptyset$ |
| \in       | $\in$       |
| \notin    | $\notin$    |
| \subset   | $\subset$   |
| \subseteq | $\subseteq$ |
| \supseteq | $\supseteq$ |
| \bigcap   | $\bigcap$   |
| \bigcup   | $\bigcup$   |
| \bigvee   | $\bigvee$   |
| \bigwedge | $\bigwedge$ |
| \biguplus | $\biguplus$ |
| \bigsqcup | $\bigsqcup$ |



## 希腊字母

LATEX2$\epsilon$ 没有定义Alpha 的大写形式，因为它和普通的罗马字体A 很像。也许新的数学编码完成后会有所变化。

| 代码     | 大写       | 代码     | 小写       |
| -------- | ---------- | -------- | ---------- |
| A        | $A$        | \alpha   | $\alpha$   |
| B        | $B$        | \beta    | $\beta$    |
| \Gamma   | $\Gamma$   | \gamma   | $\gamma$   |
| \Delta   | $\Delta$   | \delta   | $\delta$   |
| E        | $E$        | \epsilon | $\epsilon$ |
| Z        | $Z$        | \zeta    | $\zeta$    |
| H        | $H$        | \eta     | $\eta$     |
| \Theta   | $\Theta$   | \theta   | $\theta$   |
| I        | $I$        | \iota    | $\iota$    |
| K        | $K$        | \kappa   | $\kappa$   |
| Lambda   | $\Lambda$  | \lambda  | $\lambda$  |
| M        | $M$        | \mu      | $\mu$      |
| N        | $N$        | \nu      | $\nu$      |
| Xi       | $Xi$       | \xi      | $\xi$      |
| O        | $O$        | \omicron | $\omicron$ |
| \Pi      | $\Pi$      | \pi      | $\pi$      |
| P        | $P$        | \rho     | $\rho$     |
| \Sigma   | $\Sigma$   | \sigma   | $\sigma$   |
| T        | $T$        | \tau     | $\tau$     |
| \Upsilon | $\Upsilon$ | \upsilon | $\upsilon$ |
| \Phi     | $\Phi$     | \phi     | $\phi$     |
| X        | $X$        | \chi     | $\chi$     |
| \Psi     | $\Psi$     | \psi     | $\psi$     |
| \Omega   | $\Omega$   | \omega   | $\omega$   |



# 其他技巧

例子一：行距技巧

```latex
$$
\TeX\ is\ pronounced\ as\ 
\tau\epsilon\chi.\\[6pt]
100-m^{3}\ of\ water\\[6pt]
This\ comes\ from\ my\ \heartsuit
$$
```

$$
\TeX\ is\ pronounced\ as\ 
\tau\epsilon\chi.\
100-m^{3}\ of\ water\
This\ comes\ from\ my\ \heartsuit
$$

此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：

![image-20220504133719255](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/133719-6d.png)

例子二：极限、求和、分数使用技巧

```latex
$$
\lim_{n \to \infty}
\sum_{k=1}^n \frac{1}{k^2}
= \frac{\pi^2}{6}
$$
```


$$
\lim_{n \to \infty}
\sum_{k=1}^n \frac{1}{k^2}
= \frac{\pi^2}{6}
$$

例子三：集合例子

数学家们通常对使用什么样的符号非常挑剔：习惯上使用“空心粗体”（blackboard bold）来表示实数集合。这种字体可用amsfonts 或amssymb 宏包中的命令\mathbb 来得到(如代码中的倒数第二行所示)。

```latex
$$ 
\forall x \in \mathbf{R}:
\qquad x^{2} \geq 0\
x^{2} \geq 0\qquad
\textrm{for all }x\in\mathbf{R}\
x^{2} \geq 0\qquad
\textrm{for all }x\in\mathbb{R}
$$
```


$$
\forall x \in \mathbf{R}:
\qquad x^{2} \geq 0\
x^{2} \geq 0\qquad
\textrm{for all }x\in\mathbf{R}\
x^{2} \geq 0\qquad
\textrm{for all }x\in\mathbb{R}
$$

例子四：简单等式

```latex
$$
a^x+y \neq a^{x+y}
$$
```


$$
a^x+y \neq a^{x+y}
$$

例子五：平方根。方根符号的大小由LATEX自动加以调整。也可用\surd 仅给出符号。

```latex
$$
\sqrt{x} \qquad
\sqrt{ x^{2}+\sqrt{y} }
\qquad \sqrt[3]{2}\
\surd[x^2 + y^2]
$$
```


$$
\sqrt{x} \qquad
\sqrt{ x^{2}+\sqrt{y} }
\qquad \sqrt[3]{2}\
\surd[x^2 + y^2]
$$

例子六：命令\overline 和\underline 在表达式的上、下方画出水平线。

```latex
$$
\overline{m+n} \qquad
\underline{m+n}
$$
```


$$
\overline{m+n} \qquad
\underline{m+n}
$$

例子七：命令\overbrace 和\underbrace 在表达式的上、下方给出一水平的大括号。

```latex
$$
\underbrace{ a+b+\cdots+z }_{26}
$$
```


$$
\underbrace{ a+b+\cdots+z }_{26}
$$

例子八：数学重音符号如小箭头和˜（tilde）。可覆盖多个字符的宽重音符号可由\widetilde 和\widehat 等得到。

```latex
$$
y=x^{2}\qquad y'=2x\qquad y''=2
$$
```


$$
y=x^{2}\qquad y'=2x\qquad y''=2
$$

例子九：向量（Vectors）通常用上方有小箭头（arrow symbols）的变量表示。这可由\vec 得到。

另两个命令\overrightarrow 和\overleftarrow在定义从A 到B 的向量时非常有用。

```latex
$$
\vec a\quad\overrightarrow{AB}
$$
```


$$
\vec a\quad\overrightarrow{AB}
$$

例子十：一般情况下，乘法算式中的圆点符可以省略。然而有时为了帮助读者解读复杂的公式，也有必要用命令\cdot 将圆点符表示出来。

```latex
$$
v = {\sigma}_1 \cdot {\sigma}_2
{\tau}_1 \cdot {\tau}_2
$$
```


$$
v = {\sigma}_1 \cdot {\sigma}_2
{\tau}_1 \cdot {\tau}_2
$$

例子十一：分数（fraction）使用\frac{...}{...} 排版。一般来说，1/2 这种形式更受欢迎，因为对于少量的分式，它看起来更好些。

```latex
$$
1\frac{1}{2}~hours\\
\frac{ x^{2} }{ k+1 }\qquad
x^{ \frac{2}{k+1} }\qquad
x^{ 1/2 }
$$
```


$$
1\frac{1}{2}~hours\\
\frac{ x^{2} }{ k+1 }\qquad
x^{ \frac{2}{k+1} }\qquad
x^{ 1/2 }
$$

此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：

![image-20220504140314315](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/140314-6e.png)

例子十二：排版二项系数或类似的结构可以使用命令{... \choose ...} 或{... \atop ...}。

第二个命令与第一个命令的输出相同，只是没有括号。（注意这些旧命令在amsmath 宏集中禁止使用， 而是用\binom和\genfrac来代替。后者是所有相关结构的超集， 例如可以通过、newcommand{\newatop}[2]% 来得到\atop 的一个类似结构）

```latex
$$
{n \choose k}\qquad {x \atop y+2}
$$
```


$$
{n \choose k}\qquad {x \atop y+2}
$$

例子十三：对于二元关系，将符号堆在一起可能更有用。\stackrel 将第一项中的符号以上标大小放在处于正常位置的第二项上。

```latex
$$
\int f_N(x) \stackrel{!}{=} 1
$$
```


$$
\int f_N(x) \stackrel{!}{=} 1
$$

例子十四：积分运算符（integral operator）。

求和运算符（sum operator）。

乘积运算符（product operator）

```latex
$$
\sum_{i=1}^{n} \qquad
\int_{0}^{\frac{\pi}{2}} \qquad
\prod_\epsilon
$$
```


$$
\sum_{i=1}^{n} \qquad
\int_{0}^{\frac{\pi}{2}} \qquad
\prod_\epsilon
$$

例子十五：某些情况下有必要手工指出数学分隔符的正确大小。

```latex
$$
\Big( (x+1) (x-1) \Big) ^{2}\\
\big(\Big(\bigg(\Bigg(\quad
\big\}\Big\}\bigg\}\Bigg\}\quad
\big\|\Big\|\bigg\|\Bigg\|
$$
```


$$
\Big( (x+1) (x-1) \Big) ^{2}\\
\big(\Big(\bigg(\Bigg(\quad
\big\}\Big\}\bigg\}\Bigg\}\quad
\big\|\Big\|\bigg\|\Bigg\|
$$

此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：

![image-20220504140243018](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/140243-08.png)

例子十六：如果公式中由TEX选择的的空格不令人满意，可以通过插入特殊的空格命令来进行调节。

```latex
$$
\int\!\!\!\int_{D} g(x,y)
\,  x\, y\\
instead of\\
\int\int_{D} g(x,y)x y
$$
```


$$
\int\!\!\!\int_{D} g(x,y)
\,  x\, y\\
instead of\\
\int\int_{D} g(x,y)x y
$$

此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：

![image-20220504140225122](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/140225-36.png)

例子十七：排版arrays 使用array 环境来排版数组（arrays）。

```latex
$$
\mathbf{X} =
\left( \begin{array}{ccc}
x_{11} & x_{12} & \ldots \\
x_{21} & x_{22} & \ldots \\
\vdots & \vdots & \ddots
\end{array} \right)
$$
```


$$
\mathbf{X} =
\left( \begin{array}{ccc}
x_{11} & x_{12} & \ldots \\
x_{21} & x_{22} & \ldots \\
\vdots & \vdots & \ddots
\end{array} \right)
$$

此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：

![image-20220504135850072](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/135850-24.png)

例子十八：array 环境也可以使用“.” 作为隐藏右分隔符来排版只有一个大分隔符的表达式。

```latex
$$
y = \left\{ \begin{array}{ll}
a & \textrm{if $d>c$}\\
b+x & \textrm{in the morning}\\
l & \textrm{all day long}
\end{array} \right.
$$
```


$$
y = \left\{ \begin{array}{ll}
a & \textrm{if $d>c$}\\
b+x & \textrm{in the morning}\\
l & \textrm{all day long}
\end{array} \right.
$$

此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：

![image-20220504135831808](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/135832-6f.png)

例子十九：可以在array 环境中画线。例如分隔矩阵中的元素。

```latex
$$
\left(\begin{array}{c|c}
1 & 2 \\
\hline
3 & 4
\end{array}\right)
$$
```


$$
\left(\begin{array}{c|c}
1 & 2 \\
\hline
3 & 4
\end{array}\right)
$$

此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：

![image-20220504135815862](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/135816-99.png)

例子二十：长方程不会自动地分割成小的。作者必须指定在哪里分割以及缩进多少。以下是最常使用的两种方法。

其中，\nonumber 命令将阻止LATEX为此方程生成一个编号。

```latex
$$
{\begin{eqnarray}
\sin x & = & x -\frac{x^{3}}{3!}
+\frac{x^{5}}{5!}-{}
\nonumber\\
& & {}-\frac{x^{7}}{7!}+{}\cdots
\end{eqnarray}}
$$
```


$$
{\begin{eqnarray}
\sin x & = & x -\frac{x^{3}}{3!}
+\frac{x^{5}}{5!}-{}
\nonumber\\
& & {}-\frac{x^{7}}{7!}+{}\cdots
\end{eqnarray}}
$$



```latex
$$
\begin{eqnarray}
{ \cos x = 1
-\frac{x^{2}}{2!} +{} }
\nonumber\\
& & {}+\frac{x^{4}}{4!}
-\frac{x^{6}}{6!}+{}\cdots
\end{eqnarray}
$$
```


$$
\begin{eqnarray}
{ \cos x = 1
-\frac{x^{2}}{2!} +{} }
\nonumber\\
& & {}+\frac{x^{4}}{4!}
-\frac{x^{6}}{6!}+{}\cdots
\end{eqnarray}
$$

例子二十一：改变式样。改变式样也会影响上下界显示的方式。

在数学模式中，字体大小用四个命令来设定：

\displaystyle (123), \textstyle (123), \scriptstyle (123) and\scriptscriptstyle (123).

```latex
$$
\mathop{\mathrm{corr}}(X,Y)=
\frac{\displaystyle
\sum_{i=1}^n(x_i-\overline x)
(y_i-\overline y)}
{\displaystyle\biggl[
\sum_{i=1}^n(x_i-\overline x)^2
\sum_{i=1}^n(y_i-\overline y)^2
\biggr]^{1/2}}
$$
```


$$
\mathop{\mathrm{corr}}(X,Y)=
\frac{\displaystyle
\sum_{i=1}^n(x_i-\overline x)
(y_i-\overline y)}
{\displaystyle\biggl[
\sum_{i=1}^n(x_i-\overline x)^2
\sum_{i=1}^n(y_i-\overline y)^2
\biggr]^{1/2}}
$$

差距还是很明显的。行高大小明显不一样。显然是上面的舒服。

```latex
$$
\mathop{\mathrm{corr}}(X,Y)=
\frac{
\sum_{i=1}^n(x_i-\overline x)
(y_i-\overline y)}
{\biggl[
\sum_{i=1}^n(x_i-\overline x)^2
\sum_{i=1}^n(y_i-\overline y)^2
\biggr]^{1/2}}
$$
```


$$
\mathop{\mathrm{corr}}(X,Y)=
\frac{
\sum_{i=1}^n(x_i-\overline x)
(y_i-\overline y)}
{\biggl[
\sum_{i=1}^n(x_i-\overline x)^2
\sum_{i=1}^n(y_i-\overline y)^2
\biggr]^{1/2}}
$$

例子二十二：在LATEX中很难得到粗体符号。这也许是故意的，因为业余排版者总是过份使用粗体。字体改变命令\mathbf 给出粗体字母，但是这些是罗马字体（竖直的），而数学符号通常是斜体。

```latex
$$
\mu, M \qquad \mathbf{M} \qquad
$$
```


$$
\mu, M \qquad \mathbf{M} \qquad
$$

例子二十三：推导过程

```latex
$$
\begin{align}
	\frac{\partial J(\theta)}{\partial\theta_j}
	& = -\frac1m\sum_{i=0}^m(y^i - h_\theta(x^i)) \frac{\partial}{\partial\theta_j}(y^i-h_\theta(x^i))\\
	& = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i)) \frac{\partial}{\partial\theta_j}(\sum_{j=0}^n\theta_j x^i_j-y^i)\\
	&=-\frac1m\sum_{i=0}^m(y^i -h_\theta(x^i)) x^i_j
\end{align}
$$
```

$$
\begin{align}
\frac{\partial J(\theta)}{\partial\theta_j}
& = -\frac1m\sum_{i=0}^m(y^i - h_\theta(x^i)) \frac{\partial}{\partial\theta_j}(y^i-h_\theta(x^i))\\
& = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i)) \frac{\partial}{\partial\theta_j}(\sum_{j=0}^n\theta_j x^i_j-y^i)\\
&=-\frac1m\sum_{i=0}^m(y^i -h_\theta(x^i)) x^i_j
\end{align}
$$



此处放上显示的效果的图片，因为本博客使用的mathjax无法解析。

真实的显示效果应该如下：![image-20220504135644915](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/04/135645-95.png)

例子二十四：批量梯度下降。

```latex
$$
\frac{\partial J(\theta)}{\partial\theta_j} = -\frac1m\sum_{i=0}^m(y^i - 	h_\theta(x^i))x^i_j
$$
```


$$
\frac{\partial J(\theta)}{\partial\theta_j} = -\frac1m\sum_{i=0}^m(y^i - 	h_\theta(x^i))x^i_j
$$

例子二十五：字符下标。

```latex
$$
\max \limits_{a<x<b}\{f(x)\}	
$$
```


$$
\max \limits_{a<x<b}\{f(x)\}
$$


# 一些说明

1. 函数名通常用罗马字体正体排版，而不是像变量名一样用意大利体排版。因此，LATEX提供下述命令来排版最重要的一些函数名。

```latex
\arccos \cos \csc \exp \ker \limsup \min

\arcsin \cosh \deg \gcd \lg \ln \Pr

\arctan \cot \det \hom \lim \log \sec

\arg \coth \dim \inf \liminf \max \sin

\sinh \sup \tan \tanh
```



2. 如果将命令\left 放在开分隔符前，TEX会自动决定分隔符的正确大

   小。注意必须用对应的右分隔符\right 来关闭每一个左分隔符\left，并

   且只有当这两个分隔符排在同一行时大小才会被正确确定。如果不想在右

   边放任何东西，使用隐藏的‘\right.’ !

# 参考

1. LaTeX写数学公式：https://www.jianshu.com/p/e6d2368e451a
2. LaTeX 中插入数学公式：https://blog.csdn.net/weixin_42373330/article/details/89785443
3. Typora中利用LaTeX 插入数学公式: https://blog.csdn.net/happyday_d/article/details/83715440
4. 试试LaTeX插入数学公式：https://blog.csdn.net/baidu_38060633/article/details/79183905
5. LaTeX官方文档：https://www.latex-project.org/help/
6. Tobias Oetiker,Hubert Partl, Irene Hyna and Elisabeth Schlegl. 中国CTEX 用户小组[译]. 一份不太简短的LATEX2 介绍。Version 3.20, 09 August, 2001

