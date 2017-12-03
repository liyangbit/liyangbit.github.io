---
layout: posts
title: "Markdown中输入数学公式及LaTex常用数学符号整理"
teaser:
date: 2017-11-13
header:
   image_fullwidth: "image-head.jpg"
categories:
   - Math
tags:
   - Jupyter
   - Math
   - Markdown
   - LaTex
   - recommend
comments: true
show_meta: true
sidebar: right
authors: ["Lemon"]
---




**Markdown中输入数学公式及LaTex常用数学符号整理**


<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>

{% include alert info='请注意：矩阵的公式在正文里没有正常显示，可以在文末的jupyter notebook 中查看效果' %}

众所周知，数据挖掘、机器学习以及深度学习等，在学习与运用过程中，会涉及到大量的数学公式，而公式的编辑往往比较繁琐。

本文将对数学公式的编辑做一个大概的介绍，以加深大家对相关内容的理解。

## 1 Markdown 中使用LaTex基础语法

本文是在 Jupyter Notebook 中用 markdown 格式来编辑公式的。

LaTeX 公式有两种，一种是用在正文中的，一种是单独显示的。

正文中的公式如下:

```
$ ... $
```

单独一行显示的时候使用如下命令：

```
$$...$$
```

其中， \$符号中间包含的三个点表格的是LaTex的公式命令。

**举个例子如下：**

行内公式  $ f(x) = \sum_{i=0}^{N} \int_{a}^{b} g(t,i) \text{d}t $  行内公式

下面是单独一行显示：

 $$ f(x) = \sum_{i=0}^{N} \int_{a}^{b} g(t,i) \text{d}t \tag{a}$$

编辑上述公式的代码如下：

```
行内公式  $ f(x) = \sum_{i=0}^{N} \int_{a}^{b} g(t,i) \text{d}t $  行内公式

下面是单独一行显示：

 $$ f(x) = \sum_{i=0}^{N} \int_{a}^{b} g(t,i) \text{d}t \tag{a}$$
```

## 2 常用数学表达命令

### 2.1 指数和下标

指数和下标可以用 ^ 和 _ 后加相应字符来实现。比如：

LaTex命令| 显示结果 | LaTex命令| 显示结果 | LaTex命令| 显示结果
- |- | - | - | - | -
a_{1} | $a_{1}$ | x^{2} | $x^{2}$| \sum_{i=1}^{N} | $\sum_{i=1}^{N} $

### 2.2 平方根和n次方根

平方根的输入命令为 \sqrt ， n次方根的命令为 \sqrt[n]

LaTex命令| 显示结果 | LaTex命令| 显示结果 | LaTex命令| 显示结果
- |- | - | - | - | -
\sqrt{x} | $\sqrt{x}$ | \sqrt[3]{4} | $\sqrt[3]{4}$| \sqrt{x^2+y^2} | $ \sqrt{x^2+y^2} $

```
$$\sqrt{x^{2}+\sqrt{y}} \tag{2-1}$$
```

$$\sqrt{x^{2}+\sqrt{y}} \tag{2-1}$$

### 2.3 上下水平线

命令 \overline 和 \underline 在表达式的上、下方画出水平线。比如：

LaTex命令| 显示结果 | LaTex命令| 显示结果
- |- | - | - | - | -
\overline{x+y} | $ \overline{x+y} $ | \underline{x+y} | $ \underline{x+y} $

### 2.4 上下大括号

命令 \overbrace 和 \underbrace 在表达式上、下方画出一个水平的大括号。比如：

```
$$
\underbrace{1+2+3+\cdots+100}_{100}
\tag{2-2}
$$
```

$$
\underbrace{1+2+3+\cdots+100}_{100}
\tag{2-2}
$$

### 2.5 向量

向量通常是上方有小箭头的变量表示。这可以由 \vec 得到。 在定义从 A 到 B 的向量时，命令 \overrightarrow  和 \overleftarrow 非常有用。比如：

LaTex命令| 显示结果 | LaTex命令| 显示结果
- |- | - | - | - | -
\vec a | $ \vec a $ | \overrightarrow{AB} | $ \overrightarrow{AB} $

### 2.6 其他

其他，比如 分数可以用 \frac 命令， 求和可以用 \sum 命令， 乘积运算可以用 \prod 生成， 积分可以用 \int 命令。部分示例如下：





LaTex命令| 显示结果 | LaTex命令| 显示结果 | LaTex命令| 显示结果
- |- | - | - | - | -
\sum | $\sum$ | \int | $\int$| \sum_{i=1}^{N} | $\sum_{i=1}^{N} $
\int_{a}^{b} | $\int_{a}^{b}$ | \prod | $\prod$| \iint | $\iint $
\prod_{i=1}^{N} | $\prod_{i=1}^{N}$ | \iint_{a}^{b} | $\iint_{a}^{b}$| \bigcup | $\bigcup $
\bigcap | $\bigcap$ | \bigcup_{i=1}^{N} | $\bigcup_{i=1}^{N}$| \bigcap_{i=1}^{N} | $\bigcap_{i=1}^{N} $
\sqrt[3]{2} | $\sqrt[3]{2}$ | \sqrt{3} | $\sqrt{3}$| x_{3} | $ x_{3} $
\lim_{x \to 0} | $\lim_{x \to 0}$ | \frac{1}{2} | $\frac{1}{2}$

## 3 矩阵编辑

生成矩阵使用的命令如下：

```
$$\begin{matrix}
...
\end{matrix}$$
```

其中 … 表示的是 LaTeX 的矩阵命令，矩阵命令中每一行以 \ 结束，矩阵的元素之间用 & 来分隔开。举例如下：

```
$$
\begin{matrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{matrix} \tag{3-1}
$$
```
结果如下：

$$
\begin{matrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{matrix} \tag{3-1}
$$

上述矩阵显示的不是很美观，可以给矩阵加上括号，加括号的方式有多种。

### 3.1 带括号的矩阵 \left \right

```
$$
\left \{
\begin{matrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{matrix}
\right \} \tag{3-2}
$$
```

$$
\left \{
\begin{matrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{matrix}
\right \} \tag{3-2}
$$

或者：
```
$$
\left [
\begin{matrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{matrix}
\right ] \tag{3-3}
$$
```

$$
\left [
\begin{matrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{matrix}
\right ] \tag{3-3}
$$

### 3.2 带括号的矩阵 \bmatrix \Bmatrix

```
$$
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
 \tag{3-4}
$$
```


$$
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
 \tag{3-4}
$$

或者：

```
$$
\begin{Bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{Bmatrix}
 \tag{3-5}
$$
```

$$
\begin{Bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{Bmatrix}
 \tag{3-5}
$$

### 3.3 带括号的矩阵 \vmatrix \Vmatrix

```
$$
\begin{vmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{vmatrix}
 \tag{3-6}
$$
```

$$
\begin{vmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{vmatrix}
 \tag{3-6}
$$

或者：

```
$$
\begin{Vmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{Vmatrix}
 \tag{3-7}
$$
```

$$
\begin{Vmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{Vmatrix}
 \tag{3-7}
$$

### 3.4 带省略号的矩阵

如果矩阵元素太多，可以用 \cdots $\cdots$  \ddots $\ddots$ \vdots $\vdots$ 等省略符号来定义矩阵。

```
$$
\begin{bmatrix}
1 & 2 & \cdots & 4 \\
7 & 6 & \cdots & 5 \\
\vdots & \vdots & \ddots & \vdots \\
8 & 9 & \cdots & 10
\end{bmatrix} \tag{3-8}
$$
```

$$
\begin{bmatrix}
1 & 2 & \cdots & 4 \\
7 & 6 & \cdots & 5 \\
\vdots & \vdots & \ddots & \vdots \\
8 & 9 & \cdots & 10
\end{bmatrix} \tag{3-8}
$$

### 3.5 带参数的矩阵

写增广矩阵，可能需要最右边一列单独考虑。可以用array命令来处理：

```
$$
\left [
\begin{array}{cc|c}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{array}
\right ]
 \tag{3-9}
$$
```

$$
\left [
\begin{array}{cc|c}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{array}
\right ]
 \tag{3-9}
$$

### 3.6 行内矩阵

可以使用 \big( 和 \big) 来定义行间矩阵。Demo如下：

```
这是行内矩阵的Demo：
$\big(
    \begin{smallmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
    \end{smallmatrix}
 \big)$  后面还有……
```

这是行内矩阵的Demo：
$\big(
    \begin{smallmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
    \end{smallmatrix}
 \big)$  后面还有……

或者：

```
这是行内矩阵的Demo：
$
    \begin{bmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
    \end{bmatrix}
$  后面还有……
```

这是行内矩阵的Demo：
$
    \begin{bmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
    \end{bmatrix}
$  后面还有……

## 4 LaTex 常用数学符号整理

**表1 数学模式重音符号**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ \hat{a} $| \hat{a} | $ \check{a} $ | \check{a} | $ \tilde{a} $ | \tilde{a}
$ \grave{a} $ |\grave{a} | $\dot{a}$| \dot{a} | $\ddot{a}$ | \ddot{a}
$ \bar{a} $ |\bar{a} | $\vec{a}$| \vec{a} | $\widehat{A}$ | \widehat{A}
$ \acute{a} $ |\acute{a} | $\breve{a}$| \breve{a} | $\widetilde{A}$ | \widetilde{A}

<br>

**表2 希腊字母**

符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | - | - | -
$ \alpha $| \alpha | $ \theta $ | \theta | $ o $ | o | $ \upsilon $ | \upsilon
$ \beta $| \beta | $ \vartheta $ | \vartheta | $ \pi $ | \pi | $ \phi $ | \phi
$ \gamma $| \gamma | $ \iota $ | \iota | $ \varpi $ | \varpi | $ \varphi $ | \varphi
$ \delta $| \delta | $ \kappa $ | \kappa | $ \rho $ | \rho | $ \chi $ | \chi
$ \epsilon $| \epsilon | $ \lambda $ | \lambda | $ \varrho $ | \varrho | $ \psi $ | \psi
$ \varepsilon $| \varepsilon | $ \mu $ | \mu | $ \sigma $ | \sigma | $ \omega $ | \omega
$ \zeta $| \zeta | $ \nu $ | \nu | $ \varsigma $ | \varsigma |  $ \mathcal{O} $ | \mathcal{O}
$ \eta $| \eta | $ \xi $ | \xi | $ \tau $ | \tau
$ \Gamma $| \Gamma | $ \Lambda $ | \Lambda | $ \Sigma $ | \sigma |  $ \Psi $ | \Psi
$ \Delta $| \Delta | $ \Xi $ | \Xi | $ \Upsilon $ | \Upsilon |  $ \Omega $ | \Omega
$ \Theta $| \Theta | $ \Pi $ | \Pi | $ \Phi $ | \Phi


**Tips:**
<br>
如果使用大写的希腊字母，把命令的首字母变成大写即可，例如 \Gamma 输出的是  $\Gamma$ 。
<br>
如果使用斜体大写希腊字母，再在大写希腊字母的LaTeX命令前加上var，例如 \varGamma 生成 $\varGamma$。


<br>

**表3 二元关系**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ < $| < | $ > $ | > | $ = $ | =
$ \le $ |\leq 或 \le | $\ge $| \geq 或 \ge | $\equiv$ | \equiv
$ \ll $ |\ll | $\gg $| \gg | $\doteq$ | \doteq
$ \prec $ |\prec | $\succ $| \succ | $\sim$ | \sim
$ \preceq $ |\preceq | $\succeq $| \succeq | $\simeq$ | \simeq
$ \subset $ |\subset | $\supset $| \supset | $\approx$ | \approx
$ \subseteq $ |\subseteq | $\supseteq $| \supseteq | $\cong$ | \cong
$ \sqsubset $ |\sqsubset | $\sqsupset $| \sqsupset | $\Join$ | \Join
$ \sqsubseteq $ |\sqsubseteq | $\sqsupseteq $| \sqsupseteq | $\bowtie$ | \bowtie
$ \in $ |\in | $\ni $| \ni 或 \owns | $\propto$ | \propto
$ \vdash $ |\vdash | $\dashv $| \dashv | $\models$ | \models
$ \mid $ |\mid | $\parallel $| \parallel | $\perp$ | \perp
$ \smile $ |\smile | $\frown $| \frown | $\asymp$ | \asymp
$ : $ | : | $\notin $| \notin | $\neq$ | \neq  或 \ne




** Tips: **

<br>

可以在上述符号的相应命令前加上 \not 命令，得到其否定形式。
<br>

例如： \not\subset 生成 $ \not\subset$

<br>

**表4 二元运算符**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ + $| + | $ - $ | -
$ \pm $ |\pm | $\mp $| \mp | $\triangleleft$ | \triangleleft
$ \cdot $ |\cdot | $\div $| \div | $\triangleright$ | \triangleright
$ \times $ |\times | $\setminus $| \setminus | $\star$ | \star
$ \cup $ |\cup | $\cap $| \cap | $\ast$ | \ast
$ \sqcup $ |\sqcup | $\sqcap $| \sqcap | $\circ$ | \circ
$ \vee $ |\vee 或 \lor | $\wedge $| \wedge 或 \land | $\bullet$ | \bullet
$ \oplus $ |\oplus | $\ominus $| \ominus | $\diamond$ | \diamond
$ \odot $ |\odot | $\oslash $| \oslash | $\uplus$ | \uplus
$ \otimes $ |\otimes | $\bigcirc $| \bigcirc | $\amalg$ | \amalg
$ \bigtriangleup $ |\bigtriangleup | $\bigtriangledown $| \bigtriangledown | $\dagger$ | \dagger
$ \lhd $ |\ldh | $\rhd $| \rhd | $\ddagger$ | \ddagger
$ \unlhd $ |\unldh | $\unrhd $| \unrhd | $\wr$ | \wr

<br>

**表5 "大"运算符**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ \sum $ |\sum | $\bigcup $| \bigcup | $\bigvee$ | \bigvee
$ \prod $ |\prod | $\bigcap $| \bigcap | $\bigwedge$ | \bigwedge
$ \coprod $ |\coprod | $\bigsqcup $| \bigsqcup | $\biguplus$ | \biguplus
$ \int $ |\int | $\oint $| \oint | $\bigodot$ | \bigodot
$ \bigoplus $ |\bigoplus | $\bigotimes $| \bigotimes


<br>

**表6 箭头**



符号|LaTex命令 | 符号 | LaTex命令
- |- | - | -
$ \leftarrow $ |\leftarrow 或 \gets | $\longleftarrow $| \longleftarrow
$ \rightarrow $ |\rightarrow 或 \to | $\longrightarrow $| \longrightarrow
$ \leftrightarrow $ |\leftrightarrow | $\longleftrightarrow $| \longleftrightarrow
$ \Leftarrow $ |\Leftarrow | $\Longleftarrow $| \Longleftarrow
$ \Rightarrow $ |\Rightarrow | $\Longrightarrow $| \Longrightarrow
$ \Leftrightarrow $ |\Leftrightarrow | $\Longleftrightarrow $| \Longleftrightarrow
$ \mapsto $ |\mapsto | $\longmapsto $| \longmapsto
$ \hookleftarrow $ |\hookleftarrow | $\hookrightarrow $| \hookrightarrow
$ \leftharpoonup $ |\leftharpoonup | $\rightharpoonup $| \rightharpoonup
$ \leftharpoondown $ |\leftharpoondown | $\rightharpoondown $| \rightharpoondown
$ \rightleftharpoons $ |\rightleftharpoons | $\iff $| \iff
$ \uparrow $ |\uparrow | $\downarrow $| \downarrow
$ \updownarrow $ |\updownarrow | $\Uparrow $| \Uparrow
$ \Downarrow $ |\Downarrow | $\Updownarrow $| \Updownarrow
$ \nearrow $ |\nearrow | $\searrow $| \searrow
$ \swarrow $ |\swarrow | $\nwarrow $| \nwarrow
$ \leadsto $ |\leadsto


<br>

**表7 定界符**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ ( $ | ( | $ ) $| ) | $\uparrow$ | \uparrow
$ [ $ | [ 或 \lbrack | $ ] $| ] 或 \rbrack | $\downarrow$ | \downarrow
$ \{ $ | \{ 或 \lbrace | $ \} $| \} 或 \rbrace | $\updownarrow$ | \updownarrow
$ \langle $ | \langle | $ \rangle $| \rangle | $ \vert $ | \vert
$ \lfloor $ | \lfloor | $ \rfloor $| \rfloor | $ \lceil $ | \lceil
$ / $ | / | $ \backslash $| \backslash | $ \Updownarrow $ | \Updownarrow
$ \Uparrow $ | \Uparrow | $ \Downarrow $| \Downarrow | $ \Vert $ | \Vert
$ \rceil $ | \rceil


<br>

**表8 大定界符**


符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ \lgroup $ | \lgroup | $ \rgroup $| \rgroup | $\lmoustache$ | \lmoustache
$ \arrowvert $ | \arrowvert | $ \Arrowvert $| \Arrowvert | $\bracevert$ | \bracevert
$ \rmoustache $ | \rmoustache


<br>

**表9 其他字符**

符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | - | - | -
$ \dots $| \dots | $ \cdots $ | \cdots | $ \vdots $ | \vdots | $ \ddots $ | \ddtos
$ \hbar $| \hbar | $ \imath $ | \imath | $ \jmath $ | \jmath | $ \ell $ | \ell
$ \Re $| \Re | $ \Im $ | \Im | $ \aleph $ | \aleph | $ \wp $ | \wp
$ \forall $| \forall | $ \exists $ | \exists | $ \mho $ | \mho | $ \partial $ | \partial
$ ' $| ' | $ \prime $ | \prime | $ \emptyset $ | \emptyset | $ \infty $ | \infty
$ \nabla $| \nabla | $ \triangle $ | \triangle | $ \Box $ | \Box | $ \Diamond $ | \Diamond
$ \bot $| \bot | $ \top $ | \top | $ \angle $ | \angle | $ \surd $ | \surd
$ \diamondsuit $| \diamondsuit | $ \heartsuit $ | \heartsuit | $ \clubsuit $ | \clubsuit | $ \spadesuit $ | \spadesuit
$ \neg $| \neg 或 \lnot | $ \flat $ | \flat | $ \natural $ | \natural | $ \sharp $ | \sharp

<br>

**表10 非数学符号**

符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | - | - | -
$ \dagger $| \dagger | $ \S $ | \S | $\copyright$ | \copyright | $ \textregistered $ | \textregistered
$ \ddagger $| \ddagger | $ \P $ | \P | $ \pounds $ | \pounds | $ \% $ | \%

<br>

**表11 AMS 定界符**

符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | - | - | -
$ \ulcorner $| \ulcorner | $ \urcorner $ | \urcorner | $\llcorner$ | \llcorner | $ \lrcorner $ | \llcorner
$ \lvert $| \lvert | $ \rvert $ | \rvert | $ \lVert $ | \lVert | $ \rVert $ | \rVert


<br>

**表12 AMS 希腊和希伯来字母**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ \digamma $ | \digamma | $ \varkappa $| \varkappa | $\beth$ | \beth
$ \gimel $ | \gimel | $ \daleth $ | \daleth

<br>

**表13 AMS 二元关系**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ \lessdot $ | \lessdot | $ \gtrdot $| \gtrdot | $\doteqdot$ | \doteqdot
$ \leqslant $ | \leqslant | $ \geqslant $| \geqslant | $\risingdotseq$ | \risingdotseq
$ \eqslantless $ | \eqslantless | $ \eqslantgtr $| \eqslantgtr | $\fallingdotseq$ | \fallingdotseq
$ \leqq $ | \leqq | $ \geqq $| \geqq | $\eqcirc$ | \eqcirc
$ \lll $ | \lll 或 \llless | $ \ggg $| \ggg | $\circeq$ | \circeq
$ \lesssim $ | \lesssim | $ \gtrsim $| \gtrsim | $\triangleq$ | \triangleq
$ \lessapprox $ | \lessapprox | $ \gtrapprox $| \gtrapprox | $\bumpeq$ | \bumpeq
$ \lessgtr $ | \lessgtr | $ \gtrless $| \gtrless | $\Bumpeq$ | \Bumpeq
$ \lesseqgtr $ | \lesseqgtr | $ \gtreqless $| \gtreqless | $\thicksim$ | \thicksim
$ \lesseqqgtr $ | \lesseqqgtr | $ \gtreqqless $| \gtreqqless | $\thickapprox$ | \thickapprox
$ \preccurlyeq $ | \preccurlyeq | $ \succcurlyeq $| \succcurlyeq | $\approxeq$ | \approxeq
$ \curlyeqprec $ | \curlyeqprec | $ \curlyeqsucc $| \curlyeqsucc | $\backsim$ | \backsim
$ \precsim $ | \precsim | $ \succsim $| \succsim | $\backsimeq$ | \backsimeq
$ \precapprox $ | \precapprox | $ \succapprox $| \succapprox | $\vDash$ | \vDash
$ \subseteqq $ | \subseteqq | $ \supseteqq $| \supseteqq | $\Vdash$ | \Vdash
$ \shortparallel $ | \shortparallel | $ \Supset $| \Supset | $\Vvdash$ | \Vvdash
$ \blacktriangleleft $ | \blacktriangleleft | $ \sqsupset $| \sqsupset | $\backepsilon$ | \backepsilon
$ \vartriangleright $ | \vartriangleright | $ \because $| \because | $\varpropto$ | \varpropto
$ \blacktriangleright $ | \blacktriangleright | $ \Subset $| \Subset | $\between$ | \between
$ \trianglerighteq $ | \trianglerighteq | $ \smallfrown $| \smallfrown | $\pitchfork$ | \pitchfork
$ \vartriangleleft $ | \vartriangleleft | $ \shortmid $| \shortmid | $\smallsmile$ | \smallsmile
$ \trianglelefteq $ | \trianglelefteq | $ \therefore $| \therefore | $\sqsubset$ | \sqsubset


<br>

**表14 AMS 箭头**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ \dashleftarrow $ | \dashleftarrow | $ \leftleftarrows $| \leftleftarrows | $\leftrightarrows$ | \leftrightarrows
$ \dashrightarrow $ | \dashrightarrow | $ \rightrightarrows $| \rightrightarrows | $\rightleftarrows$ | \rightleftarrows
$ \Lleftarrow $ | \Lleftarrow | $ \twoheadleftarrow $| \twoheadleftarrow | $\leftarrowtail$ | \leftarrowtail
$ \Rrightarrow $ | \Rrightarrow | $ \twoheadrightarrow $| \twoheadrightarrow | $\rightarrowtail$ | \rightarrowtail
$ \leftrightharpoons $ | \leftrightharpoons | $ \Lsh $| \Lsh | $\looparrowleft$ | \looparrowleft
$ \rightleftharpoons $ | \rightleftharpoons | $ \Rsh $| \Rsh | $\looparrowright$ | \looparrowright
$ \curvearrowleft $ | \curvearrowleft | $ \circlearrowleft $| \circlearrowleft | $\upuparrows$ | \upuparrows
$ \curvearrowright $ | \curvearrowright | $ \circlearrowright $| \circlearrowright | $\downdownarrows$ | \downdownarrows
$ \upharpoonleft $ | \upharpoonleft | $ \rightsquigarrow $| \rightsquigarrow | $\multimap$ | \multimap
$ \upharpoonright $ | \upharpoonright | $ \leftrightsquigarrow $| \leftrightsquigarrow

<br>

**表15 AMS 二元否定关系符和箭头**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | - | - | -
$ \nless $ | \nless | $ \lneq $| \lneq | $ \nleq $| \nleq | $\varsubsetneqq$ | \varsubsetneqq
$ \ngtr $ | \ngtr | $ \gneq $| \gneq | $ \ngeq $| \ngeq | $\varsubsetneqq$ | \varsubsetneqq
$ \nleqslant $ | \nleqslant | $ \lneqq $| \lneqq | $ \nmid $| \nmid | $\nsubseteqq$ | \nsubseteqq
$ \ngeqslant $ | \nngeqslantgtr | $ \gneqq $| \gneqq | $ \nparallel $| \nparallel | $\nsupseteqq$ | \nsupseteqq
$ \lvertneqq $ | \lvertneqq | $ \nleqq $| \nleqq | $ \lnsim $| \lnsim | $\nshortmid$ | \nshortmid
$ \gvertneqq $ | \gvertneqq | $ \ngeqq $| \ngeqq | $ \gnsim $| \gnsim | $\nshortparallel$ | \nshortparallel
$ \lnapprox $ | \lnapprox | $ \nsim $| \nsim | $ \lnapprox $| \lnapprox | $\nprec$ | \nprec
$ \gnapprox $ | \gnapprox | $ \ncong $| \ncong | $ \gnapprox $| \gnapprox | $\nsucc$ | \nsucc
$ \npreceq $ | \npreceq | $ \nvdash $| \nvdash | $ \nVdash $| \nVdash | $\precneqq$ | \precneqq
$ \nsucceq $ | \nsucceq | $ \nvDash $| \nvDash | $ \nVDash $| \nVDash | $\succneqq$ | \succneqq
$ \precnsim $ | \precnsim | $ \precnapprox $| \precnapprox | $ \subsetneq $| \subsetneq | $\ntriangleleft$ | \ntriangleleft
$ \succnsim $ | \succnsim | $ \succnapprox $| \succnapprox | $ \supsetneq $| \supsetneq | $\ntriangleright$ | \ntriangleright
$ \varsubsetneq $ | \varsubsetneq | $ \nsubseteq $| \nsubseteq | $ \subsetneqq $| \subsetneqq | $\ntrianglelefteq$ | \ntrianglelefteq
$ \varsupsetneq $ | \varsupsetneq | $ \nsupseteq $| \nsupseteq | $ \supsetneqq $| \supsetneqq | $\ntrianglerighteq$ | \ntrianglerighteq
$ \nleftarrow $ | \nleftarrow | $ \nrightarrow $| \nrightarrow | $ \nleftrightarrow $| \nleftrightarrow
$ \nLeftarrow $ | \nLeftarrow | $ \nRightarrow $| \nRightarrow | $ \nLeftrightarrow $| \nLeftrightarrow


<br>

**表16 AMS 二元运算符**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ \dotplus $ | \dotplus | $ \centerdot $| \centerdot | $\divideontimes$ | \divideontimes
$ \ltimes $ | \ltimes | $ \rtimes $| \rtimes | $\smallsetminus$ | \smallsetminus
$ \doublecup $ | \doublecup | $ \doublecap $| \doublecap | $\doublebarwedge$ | \doublebarwedge
$ \veebar $ | \veebar | $ \barwedge $| \barwedge | $\circleddash$ | \circleddash
$ \boxplus $ | \boxplus | $ \boxminus $| \boxminus | $\circledcirc$ | \circledcirc
$ \boxtimes $ | \boxtimes | $ \boxdot $| \boxdot | $\rightthreetimes$ | \rightthreetimes
$ \intercal $ | \intercal | $ \circledast $| \circledast | $\leftthreetimes$ | \leftthreetimes
$ \curlyvee $ | \curlyvee | $ \curlywedge $| \curlywedge

<br>

**表17 AMS 其他符号**



符号|LaTex命令 | 符号 | LaTex命令 | 符号 |LaTex命令
- |- | - | - | - | -
$ \hbar $ | \hbar | $ \hslash $| \hslash | $\Bbbk$ | \Bbbk
$ \square $ | \square | $ \blacksquare $| \blacksquare | $\circledS$ | \circledS
$ \vartriangle $ | \vartriangle | $ \blacktriangle $| \blacktriangle | $\complement$ | \complement
$ \triangledown $ | \triangledown | $ \blacktriangledown $| \blacktriangledown | $\Game$ | \Game
$ \lozenge $ | \lozenge | $ \blacklozenge $| \blacklozenge | $\bigstar$ | \bigstar
$ \angle $ | \angle | $ \measuredangle $| \measuredangle
$ \diagup $ | \diagup | $ \diagdown $| \diagdown | $\backprime$ | \backprime
$ \nexists $ | \nexists | $ \Finv $| \Finv | $\varnothing$ | \varnothing
$ \eth $ | \eth | $ \sphericalangle $| \sphericalangle | $\mho$ | \mho


<br>

<iframe src="http://nbviewer.jupyter.org/github/liyangbit/python_data_analysis/blob/master/ipynb/LaTex.ipynb" width="700" height="1200"></iframe>



**参考文献：**
1. 一份不太简短的 LaTex2e 介绍
1. Markdown 中使用 LaTex， Binean的博客

<br>

对我的文章感兴趣的朋友，可以关注我的微信公众号（ID：PyDataRoad），接收我的更新通知。

<div align="center">
    <img src="/images/qrcode.jpg" width="200">
</div>
