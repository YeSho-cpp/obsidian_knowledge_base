# 注意事项

1. 使用`$`，即行中公式时，`数学公式`与`$`连接处不要有空格，否则公式不会显示。即`$ 数学公式 $` 不显示公式。
2. 使用`$$`，即居中公式时，数学公式与`$$`连接处可以有空格。
3. 使用`$$`时，上方要空一行。

# 插入公式

左对齐公式(行中公式): `$数学公式$`
居中公式（独立公式):`$$数学公式$$`


注意： 注意事项请参照目录章节中的注意事项子章节。

左对齐例子：`$x+y=z$`

$x+y=z$

居中对齐例子：`$$x+y=z$$`
$$x+y=z$$


# 注释
`%`为单行注释。

例子：
```latex
$$
%第一个极限
\lim_{n \to +\infty} \frac{1}{n(n+1)}
\quad %空一格
and %英文单词and
\quad %空一格
%第2个极限
\lim_{x\leftarrow{example} \infty} \frac{1}{n(n+1)}
$$
```

显示：

$$
%第一个极限
\lim_{n \to +\infty} \frac{1}{n(n+1)}
\quad %空一格
and %英文单词and
\quad %空一格
%第2个极限
\lim_{x\leftarrow{example} \infty} \frac{1}{n(n+1)}
$$


# 编号
Markdown编辑器在公式末尾使用`\tag{编号}`来实现公式手动编号，大括号内的内容可以自定义。

例子：
```latex
$$
x+y=z
\tag{1}
$$
```

显示：
$$
x+y=z
\tag{1}
$$
# 转义字符

在公式中输入`_`或`^`等符号时，会产生上下标功能，若想输入符号本身则需要转义字符\，写法为`\+`字符，示例如下：

例子：
```latex
$$
% \ 为转义字符
home\_name=honor
$$
```

$$
% \ 为转义字符
home\_name=honor
$$
# 换行与对齐

## 换行
使用`\\`进行换行，最后一行的`\\`可写可不写。

例子：
```latex
$$
f(x)=2x+1 \\
=2+1 \\
=3
$$
```

$$
f(x)=2x+1 \\
=2+1 \\
=3
$$
## 对齐
使用`\begin{aligned}`进行对齐，`&`表示对齐位置，一般都在=前面。

例子：
```latex
\begin{aligned}
f(x)&=2x+1 \\
&=2+1 \\
&=3
\end{aligned}
```

# 空格

`\quad`：空一格
`\qquad`：空两格
例子：
`$$x \quad y \qquad z$$`

显示：


$$x \quad y \qquad z$$

# 上下标
`^`表示上标，`_` 表示下标。如果上下标的内容多于一个字符，需要用`{}`将这些内容括成一个整体。上下标可以嵌套，也可以同时使用。

例子：
`$$x^{y^z_w}=(1+{\rm e}^x)^{-2xy^w}$$`

显示：
$$x^{y^z_w}=(1+{\rm e}^x)^{-2xy^w}$$



# 绝对值与范数
## 绝对值
`|、\vert、\mid`可以表示绝对值。由以下示例可以看出，使用`|`或`\vert`效果相同，使用`\mid`在字母与符号之间的间隔较大，不美观，因此推荐使用`|`或`\vert`。

示例：
```
$|x|$
$\vert x \vert$
$\mid x \mid$
```

显示：
$|x|$
$\vert x \vert$
$\mid x \mid$

## 范数  

使用方法与绝对值相同，只是连续输入2个绝对值符号而已，示例如下。

示例：  
`$$L_p=||x||_p$$`  
或  
`$$L_p=\vert\vert x \vert\vert_p$$`

显示：
$$L_p=||x||_p$$

# 括号
## 括号
`()、[]、|`表示符号本身，使用 `\{\}` 来表示 `{}`。当要显示大号的括号或分隔符时，要用 `\left` 和 `\right` 命令，如`$\left(表达式\right)$`，大号的括号详见下一节）。

一些特殊的括号：

|特殊括号| 输入 | 显示 |
|:-:|:-:|:-:|
| 尖括号 | `$\langle表达式\rangle$` | $\langle表达式\rangle$ |
| 向上取整 | `$\lceil表达式\rceil$` | $\lceil表达式\rceil$ |
| 向下取整 | `$\lfloor表达式\rfloor$` | $\lfloor表达式\rfloor$ |
| 大括号 | `$\lbrace表达式\rbrace$` | $\lbrace表达式\rbrace$ |

例子：
`$$f(x,y,z) = 3y^2z \left( 3+\frac{7x+5}{1+y^2} \right)$$`

显示：
$$f(x,y,z) = 3y^2z \left( 3+\frac{7x+5}{1+y^2} \right)$$
## 大括号
使用 `\left`和 `\right`来创建自动匹配高度的括号，包含 `(圆括号)、[方括号]、|绝对值|`

例子：
```latex
$$
f\left(
   \left[
     \frac{
       1+\left\{x,y\right\}
     }{
       \left(
          \frac{x}{y}+\frac{y}{x}
       \right)
       \left(u+1\right)
     }+a
   \right]^{3/2}
\right)
$$
```

显示：
$$
f\left(
   \left[
     \frac{
       1+\left\{x,y\right\}
     }{
       \left(
          \frac{x}{y}+\frac{y}{x}
       \right)
       \left(u+1\right)
     }+a
   \right]^{3/2}
\right)
$$
# 分式
通常使用 `\frac {分子} {分母}` 命令产生一个分式，分式可嵌套。
便捷情况可直接输入`\frac ab`来快速生成一个 $\frac ab$ 
如果分式很复杂，亦可使用 分子 `\over 分母`命令，此时分式仅有一层。

例子：
`$$\frac{a-1}{b-1} \quad and \quad {a+1\over b+1}$$`

显示：
$$\frac{a-1}{b-1} \quad and \quad {a+1\over b+1}$$

# 根式
`\sqrt [根指数] {被开方数}`

注意：缺省根指数时为2

例子：
`$$\sqrt{2} \quad and \quad \sqrt[n]{x+y}$$`

显示：
$$\sqrt{2} \quad and \quad \sqrt[n]{x+y}$$
# 对数
`\log_{对数底数}{表达式}`
表达式的大括号可省略

显示：
$$\log_{x+y}(z+1)$$
# 省略号
数学公式中常见的省略号有两种，`\ldots` 表示与文本底线对齐的横向省略号` …` \ldots…，`\cdots` 表示与文本中线对齐的横向省略号 `⋯` ,`\vdots`表示纵向省略号 `⋮` ,`\ddots`表示斜向省略号 `⋱` 。

例子：
`$$f(x_1,x_2,\underbrace{\ldots}_{\rm ldots} ,x_n) = x_1^2 + x_2^2 + \underbrace{\cdots}_{\rm cdots} + x_n^2$$`
显示：
$$f(x_1,x_2,\underbrace{\ldots}_{\rm ldots} ,x_n) = x_1^2 + x_2^2 + \underbrace{\cdots}_{\rm cdots} + x_n^2$$
# 最值

`\max_{下标表达式}{最值表达式}`表示最大值，`\min_{下标表达式}{最值表达式}`表达最小值。  
例子：  
`$$||x||_\infty=\max_{1\leq i\leq n}{|x_i|}$$`

显示：
$$||x||_\infty=max_{1\leq i\leq n}{|x_i|}$$
# 方程组和分段函数
## 方程组
方程组有2种方式，分别是`\begin{aligned}`和`\begin{cases}`方式，`&`表示对齐位置，推荐使用`\begin{cases}`方式，使用方法如下：

`\begin{aligned}`方式：可以使方程组根据=对齐   

```latex \\
$$
\left\{
\begin{aligned}
a+b&=2 \\
a-b&=4 \\
\end{aligned}
\right.
$$
```

显示：
$$
\left\{
\begin{aligned}
a+b&=2 \\
a-b&=4 \\
\end{aligned}
\right.
$$


`\begin{cases}`方式（推荐）：简便，但无法根据=对齐

```latex
$$
\begin{cases}
a+b=2 \\
a-b=4 \\
\end{cases}
$$
```
显示：

$$
\begin{cases}
a+b=2 \\
a-b=4 \\
\end{cases}
$$

## 分段函数

分段函数可以通过`\begin{cases}`方式实现，不同的是方程式和条件之间要用`&`符号隔开。

例子：
```latex
$$
y =
\begin{cases}
\sin(x)       & x<0 \\
x^2 + 2x +4   & 0 \leq x < 1 \\
x^3           & x \geq 1 \\
\end{cases}
$$
```

$$
y=\begin{cases}
\sin(x) & x<0 \\
x^2+2x+4 & 0\leq x<1 \\
x^3 & x\geq1
\end{cases}
$$
# 累加和累乘

使用 `\sum_{下标表达式}^{上标表达式}{累加表达式}`来输入一个累加。  
与之类似，使用 `\prod \bigcup \bigcap`来分别输入累乘、并集和交集。  
此类符号在行内显示时上下标表达式将会移至右上角和右下角。

例子：
`$$\sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R$$`
显示：
$$
\sum_{i=1}^{n}{\frac{1}{i^2}} \quad and \quad \prod_{i=1}^{n}\frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2}R
$$

# 矢量

使用 `\vec{矢量}`来自动产生一个矢量。  
也可以使用 `\overrightarrow`等命令自定义字母上方的符号。

例子：  
`$$\vec{a} \cdot \vec{b}=0\$$`

显示：

$$\vec{a} \cdot \vec{b}$$
例子：  
`$$\overleftarrow{xy} \quad and \quad \overleftrightarrow{xy} \quad and \quad \overrightarrow{xy}$$`

显示：
$$\overleftarrow{xy} \quad and \quad \overleftrightarrow{xy} \quad and \quad \overrightarrow{xy}$$
极限
`\lim_{变量 \to 表达式} 表达式`
如有需求，可以更改 \to 符号至任意符号。

例子：
`$$\lim_{n \to +\infty} \frac{1}{n(n+1)} \quad and \quad \lim_{x\leftarrow{example} \infty} \frac{1}{n(n+1)}$$`

显示：
$$\lim_{n \to +\infty} \frac{1}{n(n+1)} \quad and \quad \lim_{x\leftarrow{example} \infty} \frac{1}{n(n+1)}$$
# 几何形状符号

- 正方形：◻（`\square`）
	- $\square x$

- 三角形：△（`\triangle`）
	- $\triangle x$

# 导数、梯度、积分、偏导

## 导数  
`${\rm d}x$`或`${\text d}x$`或`$\text{d}x$`

${\rm d}x$或${\text d}x$或$\text{d}x$
## 偏导  
`$\frac{\partial y}{\partial x}$`

$\frac{\partial y}{\partial x}$

**梯度**  
`$\nabla f(x)$`

$\nabla f(x)$
## 积分

`\int_积分下限^积分上限 {被积表达式}`

例子：  
`$$\int_0^1 {x^2} \,{\rm d}x$$`

显示：
$$
\int_0^1 {x^2} {\rm d}x
$$
# 矩阵

## 基础矩阵

使用`\begin{matrix}…\end{matrix}` 这样的形式来表示矩阵，在`\begin` 与`\end` 之间加入矩阵中的元素即可。矩阵的行之间使用`\\` 分隔，`\\`表示换行，列之间使用`&` 分隔，`&`表示对齐位置。

例子：
```latex
$$
\begin{matrix}
1 & x & x^2 \\
1 & y & y^2 \\
1 & z & z^2 \\
\end{matrix}
$$
```

显示：
$$
\begin{matrix}
1 & x & x^2 \\
1 & y & y^2 \\
1 & z & z^2 \\
\end{matrix}
$$

## 带括号的矩阵

**使用`\left` 与`\right` 表示括号**

如果要对矩阵加括号，可以像上文中提到的一样，使用`\left` 与`\right` 配合表示括号符号。

例子：
```latex
$$
\left[
\begin{matrix}
1 & x & x^2 \\
1 & y & y^2 \\
1 & z & z^2 \\
\end{matrix}
\right]
$$
```
显示：
$$
\left[
\begin{matrix}
1 & x & x^2 \\
1 & y & y^2 \\
1 & z & z^2 \\
\end{matrix}
\right]
$$
## 使用特殊的`matrix`

带括号的矩阵也可以使用特殊的`matrix` 。即替换`\begin{matrix}…\end{matrix}` 中`matrix` 为`pmatrix` ，`bmatrix` ，`Bmatrix` ，`vmatrix` , `Vmatrix` 。

1. pmatrix：`$\begin{pmatrix}1 & 2 \\ 3 & 4\\ \end{pmatrix}$`
	$\begin{pmatrix}1 & 2 \\ 3 & 4\\ \end{pmatrix}$
2. bmatrix：`$\begin{bmatrix}1 & 2 \\ 3 & 4\\ \end{bmatrix}$`
	$\begin{bmatrix}1 & 2 \\ 3 & 4\\ \end{bmatrix}$
3. Bmatrix：`$\begin{Bmatrix}1 & 2 \\ 3 & 4\\ \end{Bmatrix}$`
	$\begin{Bmatrix}1 & 2 \\ 3 & 4\\ \end{Bmatrix}$
4. vmatrix：`$\begin{vmatrix}1 & 2 \\ 3 & 4\\ \end{vmatrix}$`
	$\begin{vmatrix}1 & 2 \\ 3 & 4\\ \end{vmatrix}$
5. Vmatrix：`$\begin{Vmatrix}1 & 2 \\ 3 & 4\\ \end{Vmatrix}$`
	$\begin{Vmatrix}1 & 2 \\ 3 & 4\\ \end{Vmatrix}$

## 行列式

方法已经在上一节**带括号的矩阵**中有所介绍，此处只写一个例子。

例子1：使用`\left` 与`\right` 表示括号

```latex
$$
\left|
\begin{matrix}
1 & x & x^2 \\
1 & y & y^2 \\
1 & z & z^2 \\
\end{matrix}
\right|
$$
```

$$
\left|
\begin{matrix}
1 & x & x^2 \\
1 & y & y^2 \\
1 & z & z^2 \\
\end{matrix}
\right|
$$
## 元素省略的矩阵
可以使用`\cdots ：⋯ \cdots⋯，\ddots：⋱ \ddots⋱ ，\vdots：⋮ \vdots⋮ `，来省略矩阵中的元素。

例子：
```latex
$$
\begin{pmatrix}
1&a_1&a_1^2&\cdots&a_1^n\\
1&a_2&a_2^2&\cdots&a_2^n\\
\vdots&\vdots&\vdots&\ddots&\vdots\\
1&a_m&a_m^2&\cdots&a_m^n\\
\end{pmatrix}
$$
```

$$
\begin{pmatrix}
1&a_1&a_1^2&\cdots&a_1^n\\
1&a_2&a_2^2&\cdots&a_2^n\\
\vdots&\vdots&\vdots&\ddots&\vdots\\
1&a_m&a_m^2&\cdots&a_m^n\\
\end{pmatrix}
$$

# 希腊字母

输入 `\小写希腊字母英文全称`和`\首字母大写希腊字母英文全称`来分别输入小写和大写希腊字母。  
对于大写希腊字母与现有字母相同的，直接输入大写字母即可。


|     输入     |    显示     |
| :--------: | :-------: |
|  `\alpha`  |     α     |
|  `\beta`   |     β     |
|  `\gamma`  |     γ     |
|  `\delta`  |     δ     |
| `\epsilon` |     ε     |
|  `\zeta`   |     ζ     |
|   `\eta`   |     η     |
|  `\theta`  |     θ     |
|  `\iota`   |     ι     |
|  `\kappa`  |     κ     |
| `\lambda`  |     λ     |
|   `\mu`    |     μ     |
|   `\nu`    |     ν     |
|   `\xi`    |     ξ     |
| `\omicron` |     ο     |
|   `\pi`    |     π     |
|   `\rho`   |     ρ     |
|  `\sigma`  |     σ     |
|   `\tau`   |     τ     |
| `\upsilon` |     υ     |
|   `\phi`   |     φ     |
|   `\chi`   |     χ     |
|   `\psi`   |     ψ     |
|  `\omega`  |     ω     |
|  `\Omega`  | $\Omega$  |

# 运算符
对于加减除，对应键盘上便可打出来，但是对于乘法，键盘上没有这个符号，所以我们应该输入 `\times` 来显示一个 `× \times×` 号。

普通字符在数学公式中含义一样，除了` # $ % & ~ _ { }` 若要在数学环境中表示这些符号`# $ % & _ { }`，需要分别表示为`\# \$ \% \& \_ \{ \}`，即在个字符前加上转义字符 `\` 。

关系运算符
以下是关系运算符的 Markdown 图表，2列分别公式语言和关系运算符，并且已经交换了位置：

|关系运算符 |公式语言 |
|:-:|:-:|
|$a \pm b$ |`$a \pm b$`|
|$a \times b$|`$a \times b$`|
|$a=b$ |`a=b`|
|$a\neq b$ |`\neq` |
|$a\approx b$| `\approx` |
| $a\sim b$ | `\sim` |
| $a\equiv b$ | `\equiv` |
| $a\leq b$ | `\leq` |
| $a\geq b$ | `\geq` |
| $a\ll b$ | `\ll` |
|$a\gg b$| `\gg` |
|$a \div b$ |`$a \div b$`|
|$a \cdot b$ |`$a \cdot b$`|
|$a \circ b$ |`$a \circ b$`|
|$a \odot b$ |`$a \odot b$`|
|$a \otimes b$ |`$a \otimes b$`|
|$a \oplus b$|`$a \oplus b$`|
|$a\subset b$|`\subset` |
| $a\supset b$ |`\supset` |
| $a\subseteq b$ |`\subseteq`|
| $a\supseteq b$ | `\supseteq` |
| $a\in b$ | `\in` |
|$a\ni b$| `\ni` |


其中，部分公式添加前缀`big`可以放大，删掉`big`前缀即为正常大小。

## 三角运算符

|符号|代码|
|:-:|:-:|
| $\sin x$ | `\sin x` |
| $\cos x$ | `\cos x` |
| $\tan x$ | `\tan x` |
| $\cot x$ | `\cot x` |
| $\sec x$ | `\sec x` |
| $\csc x$ | `\csc x` |

## 微积分运算符

|符号|代码|
|:-:|:-:|
| $\frac{dy}{dx}$ | `\frac{dy}{dx}` |
| $\int_a^b f(x)dx$ | `\int_a^b f(x)dx` |
| $\iint_R f(x,y)dxdy$ | `\iint_R f(x,y)dxdy` |
| $\iiint_V f(x,y,z)dxdydz$ | `\iiint_V f(x,y,z)dxdydz` |
| $\oint_C f(x,y)ds$ | `\oint_C f(x,y)ds` |

## 逻辑运算符、

|符号|代码|
|:-:|:-:|
| $A \land B$ | `A \land B` |
| $A \lor B$ | `A \lor B` |
| $\neg A$ | `\neg A` |
| $A \rightarrow B$ | `A \rightarrow B` |
| $A \Leftrightarrow B$ | `A \Leftrightarrow B` |

## 箭头运算符
以下是常见的箭头运算符以及它们对应的 LaTeX 代码：

|符号|代码|
|:-:|:-:|
| $\rightarrow$ | `\rightarrow` |
| $\leftarrow$ | `\leftarrow` |
| $\Rightarrow$ | `\Rightarrow` |
| $\Leftarrow$ | `\Leftarrow` |
| $\leftrightarrow$ | `\leftrightarrow` |
| $\Leftrightarrow$ | `\Leftrightarrow` |
## 离散符号

以下是一些离散数学中常用的符号及其含义：
### 集合论符号

|符号|含义|
|:-:|:-:|
|$\emptyset$|`$\emptyset$` 空集（不包含元素的集合）|
|$\in$|`$\in$` 属于（表示某个元素属于某个集合）|
|$\notin$|`$\notin$` 不属于（表示某个元素不属于某个集合）|
|$\subseteq$|`$\subseteq$` 子集（表示一个集合的所有元素也属于另一个集合）|


### 谓词逻辑符号

在谓词逻辑中，我们使用以下符号表示量化：

|符号|含义|
|:-:|:-:|
|$\forall$|`$\forall$` 全称量词（对于所有...）|
|$\exists$|`$\exists$` 存在量词（存在...）|

我们还使用以下符号表示定界符：

|符号|含义|
|:-:|:-:|
|$(...)$|`$(...)$` 括号（用于分组）|
|$[\ ]$|`$[\ ]$` 中括号（表示条件，如：$[x>0]$）|
|$\{...\}$|`$\{...\}$` 大括号（表示集合）|
|$:$|`$:$` 冒号（表示成员关系，如：$ x \in S $）|

## 戴帽符号

|符号 |代码|
|:-:|:-:|
| $\hat{a}$ | `\hat{a}` |
|$\widehat{AB}$| `\widehat{AB}` |
| $\check{x}$ | `\check{x}` |
| $\tilde{y}$ | `\tilde{y}` |
| $\widetilde{\Delta A}$ | `\widetilde{\Delta A}` |
| $\vec{v}$ | `\vec{v}` |
| $\dot{x}$ | `\dot{x}` |
| $\ddot{x}$ | `\ddot{x}` |
| $\bar{x}$ |`\bar{x}` |
|$\overline{AB}$|`\overline{AB}`|
|$y^{\prime}$ |`y^{\prime}`|

## 特殊符号
上述内容仅包含一些常用公式及符号，一些不常用的符号可以查找官方文档获取，此处提供一个比较全的LaTeX符号博客：链接，供大家参考。

下方展示一些不常用特殊符号：

无穷大符号：`$\infty$`
$\infty$

领结符号：`$\bowtie$`
$\bowtie$

帽：`$\hat x$`
$\hat x$

范数：`$\ell_p$`
$\ell_p$

箭头备注：`$\xrightarrow{f}$`
$\xrightarrow{f}$
​
上备注：`$\overset{def}{=}$`
$\overset{def}{=}$

下备注：`$\underset{x\in S\subseteq X}{max}$`
$\underset{x\in S\subseteq X}{max}$
​


$$\theta^T*x_{ij}+\phi^T*g+b$$





Lb​=m1​i∈M∑​(∇x​(d^i​)−∇x​(di​))2+m1​i∈M∑​(∇y​(d^i​)−∇y​(di​))2