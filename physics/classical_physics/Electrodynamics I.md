# Electrodynamics I

本笔记是对应 *UIUC PHYS 435 Electromagnetic Fields I* 中静电磁学部分的学习笔记，其中包括了矢量微积分、偏微分方程、静电学、静磁学相关的知识点。由于本篇是物理笔记，其中的数学定理并不一定提供证明，即使有证明也不一定是严格的。主要参考了 *Introduction to Electrodynamics, 4th Edition: David J. Griffiths*，文中大量的例题、图片都来自该教材，章节的编排也基本按照这本书的设计。

[TOC]

$$
\newcommand{\marginbox}[1]{\fbox{$\hphantom{1} {#1} \vphantom{1\over1} \hphantom{1}$}}\nonumber
\newcommand{\rcur}[0]{\mathscr{r}}
\newcommand{\brcur}[0]{\boldsymbol{\mathscr{r}}}
\newcommand{\unit}[1]{\hat{\boldsymbol{#1}}}
$$



**电动力学（Electrodynamics）** 研究的核心问题就是已知一系列 **源电荷（Source Charge）**，它们对另一个 **测试电荷（Test Charge）** 的作用是怎么样的。其中源电荷和测试电荷的初始状态（位置，速度）给定时，我们需要尝试计算出测试电荷的运动轨迹。这个问题可以通过 **叠加原理（Principle of Superposition）** 显著简化：由于电磁力和电荷的电量成线性比例，我们可以将一个个源电荷孤立出来处理。如果源电荷 $q_i$ 对测试电荷 $Q$ 的力为 $\mathbf{F}_i$，则所有源电荷都存在的情况下 $Q$ 受到的力就是 $\sum \mathbf{F}_i$。实验表明，一个源电荷 $q$ 对测试电荷 $Q$ 的电磁力通常与它们的电量、速度、 $q$ 的加速度以及两者的距离 $\mathscr{r}$ 相关。由于两者可能都在运动，考虑所有因素的分析会异常复杂。所以，我们将一步步构建电动力学中影响作用力的因素，从简单的问题中理解电磁场的性质。

## 矢量分析

在开始电动力学的学习之前，我们首先应该熟悉 **矢量微积分（Vector Calculus）**，它在本文的公式中占有极大的篇幅。许多物理量除了大小，还有方向的性质，比如一段位移 **r**，我们不仅关心它的大小（距离），也关心它的起点和终点。我们将这样的物理量称为称为 **矢量（Vector）**，而其它的物理量（比如温度、质量等）被称为 **标量（Scalar）**。

让我们做符号格式的约定。本篇全文中，所有的符号如果没有经过粗体处理，如 $x, \alpha, B$，都是标量；相反如果经过粗体处理，如 $\mathbf{z}, \mathbf{A}$ 则视为矢量。和矢量有关的概念和记号如下：

- 一个矢量 $\mathbf{A}$ 的大小 $|\mathbf{A}|$ 会被简记为 $A$。
- 与矢量 $\mathbf{A}$ 大小相同，方向相反的矢量记为 $-\mathbf{A}$。
- 与矢量 $\mathbf{A}$ 方向相同，长度为其 $k$ 倍的矢量记为 $k\mathbf{A}$。
- 与矢量 $\mathbf{A}$ 方向相同，单位长度的矢量记为 $\hat{\mathbf{A}}$。
- 大小为 $0$ 的矢量记为 $\mathbf{0}$，注意它和 $0$ 的区别。

### 矢量的运算

矢量的加减法可以参考下面的图片。这个法则是基于一个公理：只要两个矢量拥有相同的大小和方向，它们就是相等的矢量。因此我们可以通过平移矢量得到期望的图形：

<img src="graphs/ed1_1-1.png" alt="ed1_1-1" style="zoom:35%;" />

可以看到，矢量的加减法非常符合我们的直觉：只需要将两个矢量的头尾相连就可以得到它们的和；而两个矢量的差等于一个矢量加上另一个矢量的逆。

矢量的 **点乘（Dot Product）** 描述了两个矢量在同一方向共同的作用效果，大小为两个矢量大小之积乘以它们夹角的余弦，即：
$$
\begin{equation}
	\marginbox{\mathbf{A}\cdot \mathbf{B} = AB\cos\theta} \label{dot-product}\tag{1.1}
\end{equation}
$$
可以参考下图：

<img src="graphs/ed1_1-2.png" alt="ed1_1-2" style="zoom:40%;" />

特别地有 $\mathbf{A}\cdot\mathbf{A} = A^2$。从几何角度理解，点乘是将一个矢量投影到另一个矢量的方向上之后两者的乘积。物理学中的功 $W = \int\mathbf{F}\cdot\,d\mathbf{l}$ 是比较典型的使用点乘的例子。通过点乘的定义我们可以轻易地得到余弦定理：
$$
\begin{equation*}
c^2 = \mathbf{c}\cdot\mathbf{c} = (\mathbf{a} - \mathbf{b})\cdot(\mathbf{a} - \mathbf{b}) = \mathbf{a}\cdot\mathbf{a} - 2\mathbf{a}\cdot\mathbf{b} + \mathbf{b}\cdot\mathbf{b} = a^2 + b^2 -2ab\cos\theta
\end{equation*}
$$
矢量的 **叉乘（Cross Product）** 定义为垂直于两个矢量所在平面的矢量，大小为两个矢量大小之积乘以它们夹角的正弦，方向则满足 **右手定则（Right-Hand Rule）**：
$$
\begin{equation}
	\marginbox{\mathbf{A}\times\mathbf{B} = AB\sin\theta\hat{\mathbf{n}}} \label{cross-product}\tag{1.2}
\end{equation}
$$
其中 $\hat{\mathbf{n}}$ 是满足右手定则的法矢量，该定则描述如下：将四指指向第一个矢量的方向，大拇指处于同一平面并垂直于它们，然后使得四指能通过较小角度转到第二个矢量的方向时，大拇指的方向就是 $\hat{\mathbf{n}}$ 的方向。为了更好说明这个方向，我们以笛卡尔坐标系为例：

<img src="graphs/ed1_1-3.png" alt="ed1_1-3" style="zoom:40%;" />

上图中，通过叉乘定义有：
$$
\begin{equation}
	\hat{\mathbf{x}}\times\hat{\mathbf{y}} = \hat{\mathbf{z}} \qquad \hat{\mathbf{y}}\times\hat{\mathbf{z}} = \hat{\mathbf{x}} \qquad \hat{\mathbf{z}}\times\hat{\mathbf{x}} = \hat{\mathbf{y}} \tag{1.3}
\end{equation}
$$
特别地有 $\mathbf{A}\times\mathbf{A} = \mathbf{0}$。此外，通过右手定则不难发现叉乘不满足交换律，即 $\mathbf{A}\times\mathbf{B} = -\mathbf{B}\times\mathbf{A}$。

### 矢量的分量表示

在 $n$ 维空间中，对于线性无关的 $n$ 个矢量 $\hat{\mathbf{e}}_1,...,\hat{\mathbf{e}}_n$，对给定的 $n$ 维矢量 $\mathbf{A}$ 总能找到唯一的 $n$ 元组 $(A_1,...,A_n)$ 使得下式成立：
$$
\begin{equation}
	\mathbf{A} = \sum_{i=1}^n A_i\hat{\mathbf{e}}_i \tag{1.4}
\end{equation}
$$
因此，我们对笛卡尔坐标系中的矢量 $\mathbf{A}$ 总能写成下面的形式：
$$
\begin{equation}
	\mathbf{A} = A_x\hat{\mathbf{x}} + A_y\hat{\mathbf{y}} + A_z\hat{\mathbf{z}} \tag{1.5}
\end{equation}
$$
我们可以根据此定义给出矢量加减法、点乘和叉乘在笛卡尔坐标系中的计算公式：
$$
\begin{align}
\begin{split}
	\mathbf{A} + \mathbf{B} &= (A_x + B_x)\hat{\mathbf{x}} + (A_y + B_y)\hat{\mathbf{y}} + (A_z + B_z)\hat{\mathbf{z}} \\
	\mathbf{A} \cdot \mathbf{B} &= A_xB_x + A_yB_y + A_zB_z \\
	\mathbf{A} \times \mathbf{B} &= (A_yB_z - A_zB_y)\hat{\mathbf{x}} + (A_zB_x - A_xB_z)\hat{\mathbf{y}} + (A_xB_y - A_yB_x)\hat{\mathbf{z}}
\end{split} \tag{1.6}
\end{align}
$$
其中叉乘的公式可以通过行列式变得更加优雅：
$$
\begin{equation}
\mathbf{A}\times\mathbf{B} = 
\begin{vmatrix}
	\hat{\mathbf{x}} & \hat{\mathbf{y}} & \hat{\mathbf{z}} \\
	A_x & A_y & A_z \\
	B_x & B_y & B_z
\end{vmatrix}
\end{equation} \label{cross-product-determinant}\tag{1.7}
$$
现在计算点乘变得如此简单，我们可以利用点乘的定义得到两个矢量之间的夹角：
$$
\begin{equation}
\cos{\theta} = \frac{\mathbf{A}\cdot\mathbf{B}}{AB} = \frac{A_xB_x + A_yB_y + A_zB_z}{\sqrt{(A_x^2 + A_y^2 + A_z^2)(B_x^2 + B_y^2 + B_z^2)}}
\end{equation} \label{angle-by-dot-product}\tag{1.8}
$$
最后让我们介绍两种常见的矢量连乘公式：$\mathbf{A}\cdot(\mathbf{B}\times\mathbf{C})$ 和 $\mathbf{A}\times(\mathbf{B}\times\mathbf{C})$。其中第一个式子的结果是一个标量，$\mathbf{B}\times\mathbf{C}$ 得到了垂直于 $\mathbf{B}$、$\mathbf{C}$ 所在平面的矢量 $BC\sin\theta\hat{\mathbf{n}}$，随后它与 $\mathbf{A}$ 的点乘正好得到了 $\mathbf{A}$ 到 $\mathbf{B}$、$\mathbf{C}$ 所在平面的距离。通过下面的图片来理解，我们不难发现这个结果就是所示平行六面体的体积：

<img src="graphs/ed1_1-4.png" alt="ed1_1-4" style="zoom:40%;" />

另一个 $\mathbf{A}\times(\mathbf{B}\times\mathbf{C})$ 可以通过叉乘在笛卡尔坐标系的计算方式强行得到下面的结果：
$$
\begin{equation}
\mathbf{A}\times(\mathbf{B}\times\mathbf{C}) = \mathbf{B}(\mathbf{A}\cdot\mathbf{C}) - \mathbf{C}(\mathbf{A}\cdot\mathbf{B})
\end{equation} \label{triple-product}\tag{1.9}
$$
 类似地也有：
$$
\begin{equation}
(\mathbf{A}\times\mathbf{B})\times\mathbf{C} = -\mathbf{A}(\mathbf{B}\cdot\mathbf{C}) + \mathbf{B}(\mathbf{A}\cdot\mathbf{C})
\end{equation}
$$
通过上面两个公式，我们就可以得到任意的矢量点乘叉乘混合的结果了。

在进入更深层次的矢量分析之前，让我们确立物理学中的 **位置（Position）**、**位移（Displacement）** 和 **分离矢量（Separation Vector）** 等概念和它们的记号。位置矢量指的是空间中从原点指向某一点的矢量，通常记为 $\mathbf{r}$。电动力学中通常关心两个位置矢量，它们是 **场源点（Source Point）**，记为 $\mathbf{r}'$ 和 **场点（Field Point）**，记为 $\mathbf{r}$。从场源点指向场点的矢量则被称为分离矢量，记为 $\boldsymbol{\mathscr{r}}$，它可以定义为：
$$
\begin{equation}
\brcur = \mathbf{r} - \mathbf{r}'
\end{equation} \label{field-position-notation} \tag{1.10}
$$
位移是指表明位置移动的矢量 $\Delta\mathbf{r} = \mathbf{r}_1 - \mathbf{r}_0$。当这个量取无穷小时我们就得到了无穷小位移矢量 $d\mathbf{l}$：
$$
\begin{equation}
d\mathbf{l} = d\mathbf{r} = dx\,\hat{\mathbf{x}} + dy\,\hat{\mathbf{y}} + dz\,\hat{\mathbf{z}} 
\end{equation} \label{infinitesimal-length} \tag{1.11}
$$

### 矢量微分

#### 梯度

我们默认已经对标量的函数与微分比较熟悉了。对于函数 $f(x)$，它的导数 $\frac{df}{dx}$ 表明其 **斜率（Slope）** 的特征。现在假设函数 $T(x, y, z)$ ，它的“斜率”显得不是那么明显，我们需要考虑它所有方向上单位长度引起的变化：
$$
\begin{equation}
dT = \frac{\partial T}{\partial x}\,dx + \frac{\partial T}{\partial y}\,dy + \frac{\partial T}{\partial z}\,dz
\end{equation} \label{total-derivative} \tag{1.12}
$$
我们只需要考虑 $x, y, z$ 三个方向即可，因为所有其他方向的变化率都可以通过它们三个的线性组合得到。注意到上面的公式和点乘的相似性。将它重写为矢量点乘的形式：
$$
\begin{equation}
	dT = \left(\frac{\partial T}{dx}\hat{\mathbf{x}} + \frac{\partial T}{dx}\hat{\mathbf{y}} + \frac{\partial T}{dx}\hat{\mathbf{z}}\right)\cdot(dx\,\hat{\mathbf{x}} + dy\,\hat{\mathbf{y}} + dz\,\hat{\mathbf{z}}) = \nabla T\cdot d\mathbf{l}
\end{equation}
$$
 $\nabla$ 是引入的新记号，我们称其为 **Del 算子（Del Operator）**，其定义如下：
$$
\begin{equation}
\marginbox{\nabla = \hat{\mathbf{x}}\frac{\partial}{\partial x} + \hat{\mathbf{y}}\frac{\partial}{\partial y} + \hat{\mathbf{z}}\frac{\partial}{\partial z}}
\end{equation} \label{del-operator} \tag{1.13}
$$
可以看到 del 算子和微分算子 $\frac{d}{dx}$ 非常相似。后文中我们将利用 del 算子重新建立微积分中的一些定理，可以时常将其和微分算子对比。

当 $\nabla$ 使用在标量函数前时，会遵循类似于乘法分配律的规则得到：
$$
\begin{equation}
\nabla T = \left(\hat{\mathbf{x}}\frac{\partial}{\partial x} + \hat{\mathbf{y}}\frac{\partial}{\partial y} + \hat{\mathbf{z}}\frac{\partial}{\partial z}\right)T = \frac{\partial T}{dx}\hat{\mathbf{x}} + \frac{\partial T}{dx}\hat{\mathbf{y}} + \frac{\partial T}{dx}\hat{\mathbf{z}}
\end{equation} \label{gradient-cartesian} \tag{1.14}
$$
我们将这个矢量函数称为 $T$ 的 **梯度（Gradient）**。根据 (15) 式我们也可以发现，梯度是使得 $dT$ 最大的 $d\mathbf{l}$ 方向。如果更加生动地说明，梯度就是站在某点时最陡的（向高处的）方向，而梯度的大小就是这个方向上的斜率。

#### 散度

由于 Del 算子的矢量性质，我们可以尝试得到它和矢量函数的点乘和叉乘。点乘得到的是矢量函数的 **散度（Divergence）**：
$$
\begin{equation}
\nabla\cdot\mathbf{v} = \left(\hat{\mathbf{x}}\frac{\partial}{\partial x} + \hat{\mathbf{y}}\frac{\partial}{\partial y} + \hat{\mathbf{z}}\frac{\partial}{\partial z}\right)\cdot(v_x\hat{\mathbf{x}} + v_y\hat{\mathbf{y}} + v_z\hat{\mathbf{z}}) = \frac{\partial v_x}{\partial x} + \frac{\partial v_y}{\partial y} + \frac{\partial v_z}{\partial z}
\end{equation} \label{divergence-cartesian} \tag{1.15}
$$
散度的几何意义是某点上一个矢量函数的发散程度。我们不加证明地给出其等价的定义：
$$
\begin{equation}
\marginbox{\nabla\cdot\mathbf{v} = \lim_{V\to 0}\frac{1}{V}\oint_\mathcal{S}\mathbf{v}\cdot d\mathbf{a}}
\end{equation} \label{divergence} \tag{1.16}
$$
也即一个闭合曲面中矢量场的通量（穿过该曲面的积分）与闭合曲面形成体积 $V$ 之比在 $V\to 0$ 的极限。在静电场的章节中我们会再一次看到类似的定义。

#### 旋度

**旋度（Curl）** 则描述了一个矢量函数的旋转程度：
$$
\begin{align}
\nabla\times\mathbf{v} = \left(\hat{\mathbf{x}}\frac{\partial}{\partial x} + \hat{\mathbf{y}}\frac{\partial}{\partial y} + \hat{\mathbf{z}}\frac{\partial}{\partial z}\right)\times(v_x\hat{\mathbf{x}} + v_y\hat{\mathbf{y}} + v_z\hat{\mathbf{z}}) =
\begin{vmatrix}
	\hat{\mathbf{x}} & \hat{\mathbf{y}} & \hat{\mathbf{z}} \\
	\partial_x & \partial_y & \partial_z \\
	v_x & v_y & v_z
\end{vmatrix}  \\
= \left(\frac{\partial v_z}{\partial y} - \frac{\partial v_y}{\partial z}\right)\hat{\mathbf{x}} + \left(\frac{\partial v_x}{\partial z} - \frac{\partial v_z}{\partial x}\right)\hat{\mathbf{y}} + \left(\frac{\partial v_y}{\partial x} - \frac{\partial v_x}{\partial y}\right)\hat{\mathbf{z}}
\end{align} \label{curl-cartesian} \tag{1.17}
$$

下面是它的数学定义：
$$
\begin{equation}
	\marginbox{\nabla\times \mathbf{v} = \lim_{S\to 0}\frac{1}{S}\oint_\mathcal{P} \mathbf{v}\cdot d\mathbf{l}}
\end{equation} \label{curl} \tag{1.18}
$$
可见，这是一个曲面边界的环路积分与该曲面形成面积 $S$ 之比在 $S \to 0$ 的极限。

#### 矢量微分的运算定律

现在让我们探索矢量微分的运算规律。标量函数的微分定律可以简单总结如下：
$$
\begin{align}
\begin{split}
	\frac{d}{dx}(f + g) &= \frac{df}{dx} + \frac{dg}{dx} \\
	\frac{d}{dx}(kf) &= k\frac{df}{dx} \\
	\frac{d}{dx}(fg) &= f\frac{dg}{dx} + g\frac{df}{dx} \\
	\frac{d}{dx}\left(\frac{f}{g}\right) &= \frac{g\frac{df}{dx} - f\frac{dg}{dx}}{g^2}
\end{split}
\end{align} \label{derivative-rules} \tag{1.19}
$$
在矢量微分中，也有相当类似的定律，比如加法定律：
$$
\begin{align}
\begin{split}
	\nabla(f + g) &= \nabla f + \nabla g \\
	\nabla\cdot(\mathbf{A} + \mathbf{B}) &= \nabla\cdot\mathbf{A} + \nabla\cdot\mathbf{B} \\
	\nabla\times(\mathbf{A} + \mathbf{B}) &= \nabla\times\mathbf{A} + \nabla\times\mathbf{B}
\end{split}
\end{align} \label{vector-derivate-add-rules} \tag{1.20}
$$
标量乘法定律：
$$
\begin{align}
\begin{split}
    \nabla(kf) &= k\nabla f \\
    \nabla\cdot(k\mathbf{A}) &= k(\nabla\cdot\mathbf{A}) \\
    \nabla\times(k\mathbf{A}) &= k(\nabla\times\mathbf{A})
\end{split}
\end{align} \label{vector-derivative-scalar-multiply-rules} \tag{1.21}
$$
梯度定律：
$$
\begin{align}
\begin{split}
	\nabla(fg) &= f\nabla g + g\nabla f \\
	\nabla(\mathbf{A}\cdot\mathbf{B}) &= \mathbf{A}\times(\nabla\times\mathbf{B}) + \mathbf{B}\times(\nabla\times\mathbf{A}) + (\mathbf{A}\cdot\nabla)\mathbf{B} + (\mathbf{B}\cdot\nabla)\mathbf{A} \end{split}
\end{align} \label{vector-derivative-gradient-rules} \tag{1.22}
$$
以及矢量乘法定律：
$$
\begin{align}
\begin{split}
	\nabla\cdot(f\mathbf{A}) &= f(\nabla\cdot A) + \mathbf{A}\cdot(\nabla f) \\
	\nabla\cdot(\mathbf{A}\times\mathbf{B}) &= \mathbf{B}\cdot(\nabla\times\mathbf{A}) - \mathbf{A}\cdot(\nabla\times\mathbf{B}) \\
	\nabla\times(f\mathbf{A}) &= f(\nabla\times \mathbf{A}) - \mathbf{A}\times(\nabla f) \\
	\nabla\times(\mathbf{A}\times\mathbf{B}) &= (\mathbf{B}\cdot\nabla)\mathbf{A} - (\mathbf{A}\cdot\nabla)\mathbf{B} + \mathbf{A}(\nabla\cdot{B}) - \mathbf{B}(\nabla\cdot\mathbf{A})
\end{split}
\end{align} \label{vector-derivative-vector-multiply-rules} \tag{1.23}
$$
上面这一系列定律都可以通过矢量的运算定律以及梯度、散度和旋度的定义得到。

#### 二阶导数

我们可以使用两次 del 算子以得到函数的二阶导数。其中最常用的是梯度的散度，我们称其为 **拉普拉斯（Laplacian）**：
$$
\begin{align}
	\nabla\cdot(\nabla T) &= \left(\hat{\mathbf{x}}\frac{\partial}{\partial x} + \hat{\mathbf{y}}\frac{\partial}{\partial y} + \hat{\mathbf{z}}\frac{\partial}{\partial z}\right)\cdot\left(\frac{\partial T}{dx}\hat{\mathbf{x}} + \frac{\partial T}{dx}\hat{\mathbf{y}} + \frac{\partial T}{dx}\hat{\mathbf{z}}\right) \\ 
	&= \frac{\partial^2T}{\partial x^2} + \frac{\partial^2 T}{\partial y^2} + \frac{\partial^2 T}{\partial z^2}
\end{align} \label{laplacian-cartesian} \tag{1.24}
$$
我们经常将上面式子简写为 $\nabla^2 T$，其中 $\nabla^2$ 是一个新定义的算子，称为 **拉普拉斯算子（Laplacian Operator）**：
$$
\begin{equation}
	\marginbox{\nabla^2 = \frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2} + \frac{\partial^2}{\partial z^2}}
\end{equation} \label{laplacian-operator} \tag{1.25}
$$
偶尔我们也会对矢量使用拉普拉斯算子，此时它代表对矢量的不同分量分别进行不同方向的求拉普拉斯：
$$
\begin{equation}
	\nabla^2\mathbf{v} = (\nabla^2v_x)\hat{\mathbf{x}} + (\nabla^2v_y)\hat{\mathbf{y}} + (\nabla^2v_z)\hat{\mathbf{z}}
\end{equation} \label{vector-laplacian} \tag{1.26}
$$
除了梯度的散度，我们还可能得到：

- 梯度的旋度。我们可以通过其定义证明：
  $$
  \begin{equation}
  	\nabla\times(\nabla T) = \mathbf{0}
  \end{equation} \label{curl-of-gradient} \tag{1.27}
  $$
  这是一个很重要的结论。通常被用来判断一个矢量场 $\nabla T$ 是否是某个标量场 $T$ 的梯度。
  
- 散度的梯度，即 $\nabla(\nabla\cdot \mathbf{v})$。遗憾的是，在物理学中这个表达式很少被用到，因此对它的兴趣寥寥。但注意散度的梯度 *不等于* 梯度的散度！

- 旋度的散度。我们可以通过其定义证明：
  $$
  \begin{equation}
  	\nabla\cdot(\nabla\times\mathbf{v}) = 0 \label{divergence-of-curl} \tag{1.28}
  \end{equation}
  $$
  
- 旋度的旋度。其计算可以参考矢量连乘公式：
  $$
  \begin{equation}
  	\nabla\times(\nabla\times \mathbf{v}) = \nabla(\nabla\cdot\mathbf{v}) - \nabla^2\mathbf{v}
  \end{equation} \label{curl-of-curl} \tag{1.29}
  $$
  此式也常作为矢量的拉普拉斯定义式。

### 矢量积分

在电动力学中，我们最常见到的几种积分即是 **线积分（Line Integral）**、**面积分（Surface Integral）** 和 **体积分（Volume Integral）**。我们下面进行简单的展示说明：

- 线积分：沿着一个路径的积分，定义如下：
  $$
  \begin{equation}
  \int_\mathbf{a}^\mathbf{b}\mathbf{v}\cdot d\mathbf{l}
  \end{equation} \label{line-integral} \tag{1.30}
  $$
  其中 $\mathbf{v}$ 是一个矢量函数，$d\mathbf{l}$ 则是无穷小位移矢量，描述了积分路径的方向，可以参考下图：

  <img src="graphs/ed1_1-5.png" alt="ed1_1-5" style="zoom:40%;" />
  
  当线积分的初始点 $\mathbf{a}$ 和终止点 $\mathbf{b}$ 重合时，这就变成了一个闭合曲线，此时我们将线积分记为下面的形式：
  $$
  \begin{equation}
  	\oint \mathbf{v}\cdot d\mathbf{l}
  \end{equation} \label{closed-line-integral} \tag{1.31}
  $$
  
- 面积分：垂直作用于一个曲面的积分：
  $$
  \begin{equation}
  	\int_\mathcal{S}\mathbf{v}\cdot d\mathbf{a}
  \end{equation} \label{surface-integral} \tag{1.32}
  $$
  其中 $\mathcal{S}$ 是积分针对的曲面，$d \mathbf{a}$ 则是一个无穷小面积的法矢量。显然一块面积的法矢量有两个，因此此处可能会产生歧义，在积分时应该明确说明面积的方向。如果 $\mathcal{S}$ 是闭合的，我们就可以将面积分记为：
  $$
  \begin{equation}
  	\oint \mathbf{v}\cdot d\mathbf{a}
  \end{equation} \label{closed-surface-integral} \tag{1.33}
  $$
  我们对面积分还有一个常用的称呼。考虑将 $\mathbf{v}$ 视为单位时间单位截面上某个流体的质量时，那么 $\int\mathbf{v}\cdot d\mathbf{a}$ 能够描述单位时间通过某个面的流体质量。我们因此也将其称为 **通量（Flux）**。
  
- 体积分：对某个体积的积分：
  $$
  \begin{equation}
  	\int_\mathcal{V}T\,d\tau
  \end{equation} \label{volume-integral} \tag{1.34}
  $$
  这里 $T$ 是一个标量函数，而 $d\tau$ 是一个无穷小体积。在笛卡尔坐标系中可以轻易得到：
  $$
  \begin{equation}
  	d\tau = dx\,dy\,dz
  \end{equation} \label{infinitesimal-volume-cartesian} \tag{1.35}
  $$
  偶尔我们也会遇到矢量函数的体积分：
  $$
  \begin{equation}
  	\int\mathbf{v}\,d\tau = \hat{\mathbf{x}}\int v_x\,d\tau + \hat{\mathbf{y}}\int v_y\,d\tau + \hat{\mathbf{z}}\int v_z\,d\tau
  \end{equation} \label{vector-volume-integral} \tag{1.36}
  $$

#### 微积分基本定理

虽然大家它可能已经很熟悉了，但是为了和后面将要提到的其它基本定理比较，这里还是将 **微积分基本定理（The Fundamental Theorem of Calculus）** 写出来：
$$
\begin{equation}
	\int_a^b\left(\frac{df}{dx}\right)\,dx = f(b) - f(a)
\end{equation} \label{fundamental-theorem} \tag{1.37}
$$
其几何意义是显然的。下面让我们对矢量微积分中导数的三种变形：梯度、散度和旋度分别给出类似的基本定理：

#### 梯度基本定理

在一个线积分中，任取一段无穷小的路径 $d\mathbf{l}$ 都有 $dT = \nabla T \cdot d\mathbf{l}$（我们此前在 (15) 式中已经给出）。因此对于完整的线积分：
$$
\begin{equation}
	\int_\mathbf{a}^\mathbf{b}(\nabla T)\cdot d\mathbf{l} = T(\mathbf{b}) - T(\mathbf{a})
\end{equation} \label{gradient-theorem} \tag{1.38}
$$
这也即是 **梯度基本定理（The Fundamental Theorem for Gradients）**。其几何意义在于，空间中任意两点的“高度差”等价于连接它们的路径上每个“高度”变化的和。和微积分基本定理对比我们可以发现，它们的特征都在于，一个积分的结果只取决于边界点，和中间过程（路径）无关。物理学中，如果一个矢量场的线积分和路径无关，我们就称其为 **保守的（Conservative）**。从梯度基本定理来看，一个矢量场保守的充分条件是这个场是某个标量场的梯度。我们可以从该定理迅速得到一个推论 ：
$$
\begin{equation}
	\oint (\nabla T)\cdot d\mathbf{l} = 0
\end{equation} \label{closed-line-integral-of-gradient} \tag{1.39}
$$

#### 散度基本定理

**散度基本定理（The Fundamental Theorem for Divergences）** 将面积分和散度的体积分联系到了一起：
$$
\begin{equation}
	\int_\mathcal{V}(\nabla\cdot\mathbf{v})\,d\tau = \oint_\mathcal{S}\mathbf{v}\cdot d\mathbf{a}
\end{equation} \label{divergence-theorem} \tag{1.40}
$$
它也被称为 **高斯定理（Gauss's Theorem）**、**格林定理（Green's Theorem）** 或 **散度定理（Divergence Theorem）**。其几何意义在于，一个闭合曲面的通量等于该曲面包围的面积内总量的变化值。同样地，这也将一个积分（等式左侧）的结果取决于其边界区域（等式右侧的曲面）。

#### 旋度基本定理

**旋度基本定理（The Fundamental Theorem for Curls）** 将旋度对某曲面的通量和该曲面边界的线积分联系到了一起：
$$
\begin{equation}
	\int_\mathcal{S}(\nabla\times \mathbf{v})\cdot d\mathbf{a} = \oint_\mathcal{P}\mathbf{v}\cdot d\mathbf{l}
\end{equation} \label{stokes-theorem} \tag{1.41}
$$
这也被称为 **斯托克斯定理（Stokes' Theorem）**。其几何意义在于，一个曲面内旋度的总量等于其边界处一圈的变化之和。通过图片能够更好理解这一点：

<img src="graphs/ed1_1-6.png" alt="ed1_1-6" style="zoom:50%;" />

和梯度基本定理类似，我们发现旋度的通量和所选曲面无关，只和曲面边界有关；对于闭合曲面，我们总有：
$$
\begin{equation}
	\oint(\nabla \times\mathbf{v}) \cdot d\mathbf{a} = 0
\end{equation} \label{closed-surface-integral-of-curl} \tag{1.42}
$$

#### 分部积分

通过对微分乘法定律 $(fg)' = fg' + gf'$ 的应用，我们可以得到 **分部积分（Integration by Parts）**的积分方法：
$$
\begin{equation}
	\int_a^b f\left(\frac{dg}{dx}\right)\,dx = -\left.\int_a^b g\left(\frac{df}{dx}\right)\,dx + fg\right|_a^b
\end{equation} \label{integrate-by-parts} \tag{1.43}
$$
类似地，我们也可以将矢量微分中介绍的乘法定律变为积分的形式，比如：
$$
\int\nabla\cdot(f\mathbf{A})\,d\tau = \int f(\nabla\cdot\mathbf{A})\,d\tau + \int\mathbf{A}\cdot(\nabla f)\,d\tau\nonumber
$$
根据散度定理我们可以进一步得到：
$$
\begin{equation}
	\int_\mathcal{V} f(\nabla\cdot\mathbf{A})\,d\tau = -\int_\mathcal{V}\mathbf{A}\cdot(\nabla f)\,d\tau + \oint_\mathcal{S} f\mathbf{A}\cdot d\mathbf{a}
\end{equation} \label{integrate-by-parts-divergence} \tag{1.44}
$$
看起来非常抽象，但分部积分确实是一个非常强大的积分工具，我们会时常用到。

### 曲线坐标系

此前我们没有着重强调微积分所用的坐标系，对 del 算子的定义则只使用了笛卡尔坐标系。对于笛卡尔坐标系，多数公式可以很轻松地进行变形（比如 $da = dx\,dy$ 等），这是因为它的三个坐标方向是始终不变的，我们不再展开说明。**曲线坐标系（Curvilinear Coordinates）** 与其不同，它的坐标网格中存在曲线。这里我们将详细介绍两种物理学中极为常用的曲线坐标系。

#### 球坐标系

我们可以通过空间中某点和原点的距离 $r$、和 $z$ 轴的夹角 $\theta$，以及其径矢 $\mathbf{r}$ 对 $x$-$y$ 平面投影与 $x$ 轴的夹角 $\phi$ 来唯一确定这个点，使得下面和笛卡尔坐标系的关系式成立：
$$
\begin{align}
	x = r\sin{\theta}\cos{\theta} && y = r\sin{\theta}\sin{\theta} && z = r\cos\theta
\end{align}
$$
如下图所示：

<img src="graphs/ed1_1-7.png" alt="ed1_1-7" style="zoom:40%;" />

图中也给出了这三个量的单位矢量方向，都是使其增长的方向且相互垂直，这样就能够将任一个空间中的矢量 $\mathbf{A}$ 表示为 $A_r\hat{\mathbf{r}} + A_\theta\hat{\mathbf{\theta}} + A_\phi\hat{\mathbf{\phi}}$  的形式。和笛卡尔坐标系对比后可以发现，球坐标系中的单位矢量是不固定的（即不是常量）。它们每一个都取决于 $r, \theta, \phi$ 的值，具体关系如下：
$$
\begin{align}
\begin{split}
	\hat{\mathbf{r}} &= \sin\theta\cos\phi\hat{\mathbf{x}} + \sin\theta\sin\phi\hat{\mathbf{y}} + \cos\theta\hat{\mathbf{z}} \\
	\hat{\boldsymbol{\theta}} &= \cos\theta\cos\phi\hat{\mathbf{x}} + \cos\theta\sin\phi\hat{\mathbf{y}} - \sin\theta\hat{\mathbf{z}} \\
	\hat{\boldsymbol{\phi}} &= -\sin\phi \hat{\mathbf{x}} + \cos\phi\hat{\mathbf{y}}
\end{split}
\end{align} \label{spherical-basis-from-cartesian} \tag{1.45}
$$
因此进行微分时我们要考虑它们的偏导数，实际上就是下面这些：
$$
\begin{align}
	\frac{\partial \hat{\mathbf{r}}}{\partial \theta} = \hat{\boldsymbol{\theta}} && \frac{\partial \hat{\boldsymbol{\theta}}}{\partial \theta} = -\hat{\mathbf{r}} 
\end{align} \label{derivatives-of-spherical-basis} \tag{1.46}
$$
接下来，让我们给出所有球坐标系中所有和微积分相关的公式。首先我们尝试得到 $d\mathbf{l}$，也即无穷小的位移。这可以通过对三个方向 $\hat{\mathbf{r}}$ 、$\hat{\boldsymbol{\theta}}$、$\hat{\boldsymbol{\phi}}$ 上的分量求和得到。我们可以通过下图理解这三个方向上的分量：

<img src="graphs/ed1_1-8.png" alt="ed1_1-8" style="zoom:45%;" />

因此我们有：
$$
\begin{equation}
	d\mathbf{l} = dr\hat{\mathbf{r}} + r\,d\theta\hat{\boldsymbol{\theta}} + r\sin\theta\,d\phi\hat{\boldsymbol{\phi}}
\end{equation} \label{infinitesimal-length-spherical} \tag{1.47}
$$
通过这三个分量我们还可以得到 $d\tau$，也即它们的乘积：
$$
\begin{equation}
	d\tau = r^2\sin\theta\,dr\,d\theta\,d\phi
\end{equation} \label{infinitesimal-volume-spherical} \tag{1.48}
$$
对于面积 $da$ 则取决于它的方向。如果 $r$ 保持不变，则它是 $\hat{\boldsymbol{\theta}}$ 与 $\hat{\boldsymbol{\phi}}$ 方向的两个分量的乘积（这种情况比较常见）：
$$
\begin{equation}
	d\mathbf{a} = r^2\sin\theta\,d\theta\,d\phi\hat{\mathbf{r}}
\end{equation} \label{infinitesimal-surface-spherical} \tag{1.49}
$$
如果 $\theta$ 保持不变，则公式如下：
$$
d\mathbf{a} = r\,dr\,d\phi\hat{\boldsymbol{\theta}}
$$
然后让我们尝试得到球坐标系下的 del 算子。我们从笛卡尔坐标系中 del 算子的定义开始：
$$
\begin{align*}
	\nabla T &= \frac{\partial T}{\partial x}\hat{\mathbf{x}} + \frac{\partial T}{\partial y}\hat{\mathbf{y}} + \frac{\partial T}{\partial z}\hat{\mathbf{z}} \\
	&= \left[\frac{\partial T}{\partial r}\left(\frac{\partial r}{\partial x}\right) + \frac{\partial T}{\partial \theta}\left(\frac{\partial \theta}{\partial x}\right) + \frac{\partial T}{\partial \phi}\left(\frac{\partial \phi}{\partial x}\right)\right]\hat{\mathbf{x}} + ... \\
\end{align*}
$$
第二步中我们省略了 $\hat{\mathbf{y}}$ 和 $\hat{\mathbf{z}}$ 的项，但是它们和第一项是类似的，都是对某一分量分解为球坐标系下三个分量的和。通过代入前面介绍过的公式，我们可以得到 del 算子相关的所有公式：
$$
\begin{align}
\nabla T &= \frac{\partial T}{\partial r}\hat{\mathbf{r}} + \frac{1}{r}\frac{\partial T}{\partial \theta}\hat{\boldsymbol{\theta}} + \frac{1}{r\sin\theta}\frac{\partial T}{\partial \phi}\hat{\boldsymbol{\phi}} \\

\nabla \cdot \mathbf{v} &= \frac{1}{r^2}\frac{\partial}{\partial r}(r^2v_r) + \frac{1}{r\sin\theta}\frac{\partial}{\partial \theta}(\sin\theta v_\theta) + \frac{1}{r\sin\theta}\frac{\partial v_\phi}{\partial \phi} \\

\nabla\times \mathbf{v} &= \frac{1}{r\sin\theta}\left(\frac{\partial}{\partial \theta}(\sin\theta v_\phi) - \frac{\partial v_\theta}{\partial \phi}\right)\hat{\mathbf{r}} + \frac{1}{r}\left(\frac{1}{\sin\theta}\frac{\partial v_r}{\partial \phi} - \frac{\partial }{\partial r}(rv_\phi) \right)\hat{\boldsymbol{\theta}} \\
&+ \frac{1}{r}\left(\frac{\partial}{\partial r}(rv_\theta) - \frac{\partial v_r}{\partial \theta}\right)\hat{\boldsymbol{\phi}} \\

\nabla^2 T &= \frac{1}{r^2}\frac{\partial}{\partial r}\left(r^2\frac{\partial T}{\partial r}\right) + \frac{1}{r^2\sin\theta}\frac{\partial}{\partial \theta}\left(\sin\theta\frac{\partial T}{\partial \theta}\right) + \frac{1}{r^2\sin^2\theta}\frac{\partial^2 T}{\partial \phi^2}
\end{align} \label{derivatives-spherical-coordinates} \tag{1.50}
$$

#### 柱坐标系

**柱坐标系（Cylindrical Coordinates）** 是对平面中极坐标系的三维扩张，它由和直角坐标系中相同含义的 $z$，和 $z$ 轴的距离 $s$ 和与球坐标系中相同含义的 $\phi$ 构成。下面是它和笛卡尔坐标系的关系：
$$
x = s\cos{\phi} \qquad y = s\sin{\phi} \qquad z = z
$$
与球坐标系类似地，柱坐标系中的单位矢量会随着 $(s, \phi, z)$ 的变化而改变；它们的表达式如下：
$$
\begin{align}
\begin{split}
	\unit{s} &= \cos\phi \unit{x} + \sin\phi \unit{y} \\
	\unit{\phi} &= -\sin\phi\unit{x} + \cos\phi\unit{y} \\
	\unit{z} &= \unit{z}
\end{split}
\end{align} \label{cylindrical-basis-from-cartesian} \tag{1.51}
$$
下面是一张示意图：

<img src="graphs/ed1_1-9.png" alt="ed1_1-9" style="zoom:40%;" />

无穷小的位移是：
$$
\begin{equation}
	d\mathbf{l} = ds\unit{s} + s\,d\phi\unit{\phi} + dz\unit{z}
\end{equation} \label{infinitesimal-length-cylindrical} \tag{1.52}
$$
我们同样可以得到 $d\tau$：
$$
\begin{equation}
	d\tau = s\,ds\,d\phi\,dz
\end{equation} \label{infinitesimal-volume-cylindrical} \tag{1.53}
$$
和球坐标系类似，我们可以得到柱坐标系下的梯度、散度、旋度的公式：
$$
\begin{align}
\nabla T &= \frac{\partial T}{\partial s}\unit{s} + \frac{1}{s}\frac{\partial T}{\partial \phi}\unit{\phi} + \frac{\partial T}{\partial z}\unit{z} \\
\nabla\cdot \mathbf{v} &= \frac{1}{s}\frac{\partial}{\partial s}(sv_s) + \frac{1}{s}\frac{\partial v_\phi}{\partial \phi} + \frac{\partial v_z}{\partial z} \\
\nabla\times\mathbf{v} &= \left(\frac{1}{s}\frac{\partial v_z}{\partial \phi} - \frac{\partial v_\phi}{\partial z}\right)\unit{s} + \left(\frac{\partial v_s}{\partial s} - \frac{\partial v_z}{\partial s}\right)\unit{\phi} + \frac{1}{s}\left(\frac{\partial}{\partial s}(sv_\phi) - \frac{\partial v_s}{\partial \phi}\right)\unit{z} \\
\nabla^2T &= \frac{1}{s}\frac{\partial}{\partial s}\left(s\frac{\partial T}{\partial s}\right) + \frac{1}{s^2}\frac{\partial^2 T}{\partial \phi^2} + \frac{\partial^2 T}{\partial z^2}
\end{align} \label{derivatives-cylindrical} \tag{1.54}
$$

### 狄拉克函数

本节中让我们介绍一个数学工具。以一个例子作为导引：

> **例**：求球坐标系中，$\mathbf{v} = \frac{1}{r^2}\unit{r}$ 的散度。

> **解**：利用 $(\ref{derivatives-spherical-coordinates})$ 的散度公式，我们可以作如下计算：
> $$
> \nabla\cdot\mathbf{v} = \frac{1}{r^2}\frac{\partial }{\partial r}\left(r^2\frac{1}{r^2}\right) = 0 \nonumber
> $$
> 不过这个结论似乎违背了我们的直觉：这个矢量场的散度显然不应该是 $0$，因为根据散度定理，考虑闭合的曲面积分：
> $$
> \oint\mathbf{v}\cdot\,d\mathbf{a} = \int\left(\frac{1}{R^2}\unit{r}\right)\cdot(R^2\sin\theta\,d\theta\,d\phi\unit{r}) = 4\pi \nonumber
> $$
> 这和我们此前得到的散度并不符合（无论怎么积分，$0$ 都只能得到 $0$）。这个区别源于 $r = 0$ 这一点。当 $r \ne 0$ 时，矢量场的散度确实为 $0$，但是由于 $r = 0$ 时 $v = \infty$，我们就不能通过公式直接计算出该点的散度。一个形象的例子是质点的密度（仅当在该点时存在无穷大的密度）。

#### 一维的狄拉克函数

定义 **狄拉克函数（Dirac Delta Function）** 为仅在 $x = 0$ 时不为 $0$ 的函数：
$$
\delta(x) = 
\begin{cases}
	0, \qquad \text{若 $x \ne 0$} \\
	\infty, \quad\ \ \text{若 $x = 0$}
\end{cases} \label{dirac-function} \tag{1.55}
$$
它有一个非常重要的性质：
$$
\label{dirac-function-property}
\int_{-\infty}^\infty\delta(x)\,dx = 1 \tag{1.56}
$$
狄拉克函数并不是严格意义上的函数，而是一个函数序列的极限。我们可以通过构造一系列函数不断逼近狄拉克函数，并对它们的积分求极限。对于一个常规的连续函数 $f(x)$，它和狄拉克函数的乘积满足一个有趣的性质：
$$
\int_{-\infty}^\infty f(x)\delta(x)\,dx = f(0)
$$
这个结果是显而易见的（因为 $\delta(x)$ 在 $x \ne 0$ 的地方处处为 $0$）。如果对狄拉克函数进行平移，我们就能得到一个更加通用的结论：
$$
\int_{-\infty}^\infty f(x)\delta(x - a)\,dx = f(a) \tag{1.57}
$$

#### 三维的狄拉克函数

我们可以通过一维的狄拉克函数轻易构造三维的版本：
$$
\begin{equation}
	\delta^3(\mathbf{r}) = \delta(x)\delta(y)\delta(z) 
\end{equation} \label{3d-dirac-function} \tag{1.58}
$$
此时它应该满足下面的性质：
$$
\int\delta^3(\mathbf{r})\,d\tau = \int_{-\infty}^\infty\int_{-\infty}^\infty\int_{-\infty}^\infty\delta(x)\delta(y)\delta(z)\,dx\,dy\,dz = 1 \tag{1.59}
$$
和一个常规函数结合后我们也可以得到下面的结论：
$$
\int f(\mathbf{r})\delta^3(\mathbf{r} - \mathbf{a})\,d\tau = f(\mathbf{a}) \tag{1.60}
$$
有了以上铺垫，我们就可以给出之前遇到的矢量场的散度了：
$$
\nabla\cdot\left(\frac{\unit{r}}{r^2}\right) = 4\pi\delta^3(\mathbf{r})
$$
对于 $\rcur = \mathbf{r} - \mathbf{r}'$，上面的式子可以转化为：
$$
\nabla\cdot\left(\frac{\unit{\rcur}}{\rcur^2}\right) = 4\pi\delta^3(\brcur)
$$
由于 $\frac{1}{\rcur}$ 的梯度恰好是 $-\frac{\unit{\rcur}}{\rcur^2}$，我们可以得到：
$$
\nabla^2 \frac{1}{\rcur} = -4\pi\delta^3(\brcur) \tag{1.61}
$$
这个公式在电磁学中会非常常用。

### 矢量场论简介

最后，让我们将前面的知识总结起来，并作为后文电磁场理论的引言。自从法拉第的实验发现后，电场和磁场就常常被联系到一起。麦克斯韦则通过数学工具（即矢量微积分）将两者用优美的数学公式联系到一起。不过这些公式虽美，也引发了一个问题：我们究竟还需要多少条件才能通过麦克斯韦方程唯一确定一个矢量场（我们显然还需要条件，因为这些方程对一切电磁场生效）？就比如想要知道一个矢量场 $\mathbf{V}$，已经有的条件是：
$$
\nabla\cdot\mathbf{V} = C \qquad \nabla\times\mathbf{V} = \mathbf{D} \qquad \nabla\cdot\mathbf{D} = 0 \nonumber
$$
第三个方程是我们根据第二个条件和 del 算子性质得到的结论。此时我们显然不能得到 $\mathbf{V}$。事实上，我们还需要一系列 **边界条件（Boundary Condition）**，比如通常包括无穷远处 $\mathbf{V} \to \mathbf{0}$。后续学习电磁场时，我们在介绍它们的微积分性质后，会给出为了求出它们需要的边界条件。

此外，我们已经认识到梯度的旋度和旋度的散度恒为零了，因此在看到一个矢量场的散度或旋度为零时，我们能自然地得到一些很好的性质。下面是两个相关的定理：

- **无旋场（Irrotational Field）**：一个矢量场满足下面的条件中的任何一条时，就同时满足其它条，并被称为无旋场：
  - $\nabla\times\mathbf{F} = \mathbf{0}$ 处处成立。
  - $\int_\mathbf{a}^\mathbf{b}\mathbf{F}\cdot\,d\mathbf{l}$ 和路径无关，只与起始点和终止点有关。
  - $\oint\mathbf{F}\cdot\,d\mathbf{l} = 0$ 对任意环路成立。
  - $\mathbf{F}$ 是某个标量场的梯度，即 $\mathbf{F} = -\nabla V$（这里的负号可以简单和功与能相联系；当我们希望矢量场做功时，它的势能就会相应减少。这里的 $V$ 包含有势能的意思。作为类比，重力势能在高处更高，但是顺着重力方向向下运动时，势能就会降低转化为动能）。同时这个 $V$ 不是唯一的，任意常数 $C$ 都能满足 $\mathbf{F} = -\nabla(V + C)$。
- **无源场（Solenoidal Field）**：一个矢量场满足下面的条件中的任何一条时，就同时满足其它条，并被称为无源场：
  - $\nabla\cdot\mathbf{F} = 0$ 处处成立。
  - $\int\mathbf{F}\cdot\,d\mathbf{a}$ 在给定边界线时，与曲面形状无关。
  - $\oint\mathbf{F}\cdot\,d\mathbf{a} = 0$ 对任意闭合曲面成立。
  - $\mathbf{F}$ 是某个矢量场的旋度，即 $\mathbf{F} = \nabla\times\mathbf{A}$。这个 $\mathbf{A}$ 不是唯一的，任意标量场 $f$ 都能满足 $\mathbf{F} = \nabla\times(\mathbf{A} + \nabla f)$。

此外，任意矢量场 $\mathbf{F}$ 都能表示为：
$$
\mathbf{F} = -\nabla V + \nabla\times \mathbf{A}
$$

我们会在后文中看到它们的身影。

## 静电场概述

**静电学（Electrostatics）** 研究静止源电荷的电场问题，换句话说，这是研究与时间 *无关* 的电场性质。这种情况下我们可以立刻给出源电荷对测试电荷的力。通过 **库仑定律（Coulomb's Law）** 描述，即静电荷吸引另一个电荷产生的力正比于两电荷的乘积并反比于两电荷距离的平方，公式是：
$$
\begin{equation}
    \marginbox{\mathbf{F} = \frac{1}{4\pi\epsilon_0}\frac{qQ}{\mathscr{r}^2}\hat{\boldsymbol{\mathscr{r}}}}
\end{equation} \label{coulomb's-law} \tag{2.1}
$$
其中 $\epsilon_0$ 是一个常数，称为 **真空介电常数（Permittivity of Free Space）**，值是 $8.85\times10^{-12}\text{C}^2\cdot\text{N}^{-1}\cdot\text{m}^{-2}$。本篇中，记源电荷的位置为 $\mathbf{r}'$，测试电荷的位置为 $\mathbf{r}$，而两者之间的矢量（从源电荷指向测试电荷）为 $\boldsymbol{\mathscr{r}} = \mathbf{r} - \mathbf{r}'$。库仑定律和叠加原理组成了静电学的全部核心内容。之后我们引入的概念和公式都是它们的数学延伸。

### 电场强度

考虑有 $n$ 个源电荷 $q_1,...,q_n$ 距离测试电荷 $\mathscr{r}_1,...,\mathscr{r}_n$。此时对测试电荷 $Q$ 的静电力为：
$$
\begin{align}
\begin{split}
\mathbf{F} &= \sum_{i=1}^n \mathbf{F}_i \\
&= \sum_{i=1}^n\frac{1}{4\pi\epsilon_0}\frac{q_iQ}{\mathscr{r}_i^2}\hat{\boldsymbol{\mathscr{r}_i}} \\
&= \frac{Q}{4\pi\epsilon_0}\sum_{i=1}^n\frac{q_i}{\mathscr{r}_i^2}\hat{\boldsymbol{\mathscr{r}_i}} 
\end{split}
\end{align} \tag{2.2}
$$
可以看到其中有一个和测试电荷无关的量，我们定义其为源电荷的 **电场强度（Electric Field）**：
$$
\begin{align}
\mathbf{F} &= Q\mathbf{E} \\
\mathbf{E} &= \frac{1}{4\pi\epsilon_0}\sum_{i=1}^n\frac{q_i} {\mathscr{r}_i^2}\hat{\boldsymbol{\mathscr{r}_i}} 
\end{align}\label{coulomb's-law-discrete} \tag{2.3}
$$
通过第一个定义式我们可以认识到，电场强度的值就是静电场中某点上，单位电荷受到源电荷的力的大小。让我们用一个例子来熟悉电场强度的求解：

> **例**：有两个距离为 $d$ 的电荷，电量都为 $q$，求它们连线的中垂线上的电场强度分布，见下图 (a)。
>
> <img src="graphs/ed1_2-1.png" alt="ed1_2-1" style="zoom:50%;" />

> **解**：可以通过库仑定律推导出的电场强度计算式得到 $\mathbf{E} = \frac{1}{4\pi\epsilon_0}\frac{q}{\mathscr{r}^2}(\hat{\boldsymbol{\mathscr{r}_1}} + \hat{\boldsymbol{\mathscr{r}_2}})$。由于两个源电荷的对称性，最后形成的电场强度只有 $z$ 方向分量，所以我们可以得到答案：
> $$
> \begin{align*}
> \mathbf{E} &= E_z\hat{\mathbf{z}} = \frac{1}{4\pi\epsilon_0}\frac{2q}{\mathscr{r}^2}\cos\theta \\
> &= \frac{1}{2\pi\epsilon_0}\frac{qz}{[z^2 + (d/2)^2]^{3/2}}\hat{\mathbf{z}}
> \end{align*}
> $$

当源电荷连续分布时，$(\ref{coulomb's-law-discrete})$ 式中的求和符号就应该换成积分符号：
$$
\mathbf{E} = \frac{1}{4\pi\epsilon_0}\int\frac{1}{\mathscr{r}^2}\hat{\boldsymbol{\mathscr{r}}}\,dq
$$
这里的 $dq$ 在实际计算时，要根据电荷分布的具体情况来代换成对线、对面、对体的积分，可以参考下面的图：

<img src="graphs/ed1_2-2.png" alt="ed1_2-2" style="zoom:40%;" />

在 (b)、(c)、(d) 的情况下，$dq$ 分别可以写为 $\lambda\,dl$、$\sigma\,da$、$\rho\,d\tau$，其中 $\lambda$、$\sigma$、$\rho$ 分别是电荷的线密度、面密度和体密度。其中最常用的就是体密度。我们有时会把下面这个公式称为库仑定律：
$$
\begin{equation}
	\marginbox{\mathbf{E}(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\int\frac{\rho(\mathbf{r}')}{\mathscr{r}^2}\hat{\boldsymbol{\mathscr{r}}}\,d\tau}
\end{equation} \label{coulomb's-law-integral} \tag{2.4}
$$
让我们同样通过一个例子来理解连续分布的电荷产生的电场强度：

> **例**：有一条电荷均匀分布的直线段，长度为 $2L$，电荷密度为 $\lambda$，求其中垂线上的电场强度分布。

> **解**：我们可以借用前文中的例子的结论，即距离为 $d$ 的两个电荷 $q$ 在其连线中垂线上产生的电场强度为：
> $$
> \mathbf{E} = \frac{1}{2\pi\epsilon_0}\frac{qz}{[z^2 + (d/2)^2]^{3/2}}\hat{\mathbf{z}}\nonumber
> $$
> 因此，对于长度为 $2L$ 的电荷均匀分布的直线，我们可以将其看作很多对 $\lambda dl$：
> $$
> \begin{align*}
> \mathbf{E} &= \int_0^L \frac{1}{2\pi\epsilon_0}\frac{\lambda z}{(z^2 + l^2)^{3/2}}\,dl \\
> &= \frac{1}{2\pi\epsilon_0}\frac{\lambda L}{z\sqrt{z^2 + L^2}}\hat{\mathbf{z}}
> \end{align*}
> $$
> 当 $L \to \infty$ 时，我们可以得到：
> $$
> \mathbf{E} = \frac{\lambda}{2\pi\epsilon_0 z}
> $$

### 静电场的散度和旋度

#### 静电场的散度

考虑一个曲面 $\mathcal{S}$，静电场 $\mathbf{E}$ 对其的通量是：
$$
\begin{equation}
	\Phi_\mathbf{E} = \int_\mathcal{S}\mathbf{E}\cdot d\mathbf{a}
\end{equation} \label{electric-flux} \tag{2.5}
$$
从某种方式来理解，这个公式计算的是穿过 $\mathcal{S}$ 的磁感线数量。当 $\mathcal{S}$ 是闭合的时候可以得到：
$$
\oint_\mathcal{S}\mathbf{E}\cdot d\mathbf{a} = \int\frac{1}{4\pi\epsilon_0}\left(\frac{q}{r^2}\hat{\mathbf{r}}\right)\cdot(r^2\sin\theta\,d\theta\,d\phi\hat{\mathbf{r}}) = \frac{q}{4\pi\epsilon_0}\int_0^{2\pi}d\phi\int_0^\pi\sin{\theta}\,d\theta = \frac{1}{\epsilon_0}q\nonumber
$$
当 $\mathcal{S}$ 中存在多个源电荷时，我们可以利用叠加原理得到更为通用的结论，这就是 **高斯定理（Gauss's Law）**：
$$
\begin{equation}
	\oint \mathbf{E}\cdot d\mathbf{a} = \frac{Q_\text{enc}}{\epsilon_0}
\end{equation} \label{gauss's-law} \tag{2.6}
$$
其中 $Q_\text{enc}$ 是曲面内所有电荷的总量。这个美妙的结论充分显示了数学以及大自然的美感。事实上，所有满足距离平方反比的矢量场都能够得到类似的结论，我们这里不展开说明了。

现在利用散度定理，即 $(\ref{divergence-theorem})$ 式，此时有：
$$
\int_\mathcal{V}(\nabla\cdot\mathbf{E})\,d\tau = \oint_S\mathbf{E}\cdot d\mathbf{a} = \frac{1}{\epsilon_0}\int_\mathcal{V}\rho\,d\tau
$$
右边这个等式就是我们之前得到的高斯定理，其中 $\rho$ 是曲面内的电荷体密度。将最左侧和最右侧的积分号去掉，我们就得到了**微分形式的高斯定理（Gauss's Law in Differential Form）**：
$$
\begin{equation}
	\nabla\cdot \mathbf{E} = \frac{\rho}{\epsilon_0}
\end{equation} \label{gauss's-law-derivative} \tag{2.7}
$$
这个公式是 **麦克斯韦方程组** 的一部分，且在 $\mathbf{E}$ 与 $\rho$ 和时间有关时依然有效。上面的结论其实可以从纯数学角度得到。根据库仑定律，即 $(\ref{coulomb's-law-integral})$，我们可以在左侧乘上 del 算子得到其散度：
$$
\nabla\cdot \mathbf{E} = \frac{1}{4\pi\epsilon_0}\int\nabla\cdot\left(\frac{\hat{\boldsymbol{\mathscr{r}}}}{\mathscr{r}^2}\right)\rho(\mathbf{r}')\,d\tau\nonumber
$$
其中的散度是我们此前计算过的：
$$
\nabla\cdot\left(\frac{\hat{\boldsymbol{\mathscr{r}}}}{\mathscr{r}^2}\right) = 4\pi\delta^3(\hat{\boldsymbol{\mathscr{r}}})
$$
代入前面的式子我们就能轻易地得到微分形式的高斯定理。

高斯定理在求解高度对称的电荷分布产生的电场时非常方便，我们可以通过构建“高斯面”来快速求解。其中高度对称可以完全归纳为下面三种：

- 球对称：将高斯面设置为同心球。
- 柱对称：将高斯面设置为同轴柱。
- 平面对称：将高斯面设置为一个平行并包括平面的小盒子。

> **例**：有一个电荷密度均匀为 $\sigma$ 的无限平面，求它产生的电场强度。

> **解**：构建高斯面，如下图所示：
>
> <img src="graphs/ed1_2-3.png" alt="ed1_2-3" style="zoom:40%;" />
>
> 电场在这个盒子垂直于平面的面上的通量为 $0$。因此我们只需要考虑两个平行的平面：
> $$
> \int\mathbf{E}\cdot\,d\mathbf{a} = \frac{Q_\text{enc}}{\epsilon_0} \implies E\cdot2A = \frac{\sigma\cdot A}{\epsilon_0}\nonumber
> $$
> 因此我们不难得到：
> $$
> \mathbf{E} = \frac{\sigma}{2\epsilon_0}\unit{n}
> $$

不过我们也要认识到高斯定律的局限性，当我们无法保证曲面的通量是匀强电场时，就无法利用高斯定律了。

#### 静电场的旋度

为了得到静电场的旋度，我们可以从电场的线积分出发。为了简化说明，我们考虑单个点电荷产生的电场 $\mathbf{E}$：
$$
\mathbf{E} = \frac{1}{4\pi\epsilon_0}\frac{q}{r^2}\unit{r} \nonumber
$$
我们利用球坐标系进行求解，并将这个点电荷设置在原点，即 $\mathbf{r}' = \mathbf{0}$（因为更加方便计算）。此时计算线积分：
$$
\int_\mathbf{a}^\mathbf{b}\mathbf{E}\cdot\,d\mathbf{l} = \frac{1}{4\pi\epsilon_0}\int_\mathbf{a}^\mathbf{b}\frac{q}{r^2}\,dr = \frac{1}{4\pi\epsilon_0}\left(\frac{q}{r_a} - \frac{q}{r_b}\right)
$$
我们发现这个结果只和起始点和终止点有关（是不是有点熟悉？没错，梯度基本定理，也即 $(\ref{gradient-theorem})$），当两点重合时，这个值就是 $0$。再利用斯托克斯定理 $(\ref{stokes-theorem})$，我们得到下式：
$$
\begin{equation}
	\marginbox{\nabla\times\mathbf{E} = \mathbf{0}}
\end{equation} \tag{2.8}
$$
当考虑多个电荷产生的电场时，利用叠加原理，就能得知任意分布的静电场的旋度均为 $\mathbf{0}$。

### 电势

我们在矢量分析的章节中已经提到过，旋度为零的矢量场，其环路积分为零，因此电场从某点到另一点的路径积分始终相等（即与路径无关）。我们可以通过下式定义 **电势（Electric Potential）**：
$$
\begin{equation}
	\marginbox{V(\mathbf{r}) = -\int_\mathcal{O}^\mathbf{r}\mathbf{E}\cdot\,d\mathbf{l}}
\end{equation} \label{electric-potential} \tag{2.9}
$$
其中 $\mathcal{O}$ 是任选的参照点，在具体问题中我们会具体设置。如果 $\mathcal{O}$ 是确定的，那么 $V$ 就只和 $\mathbf{r}$，也即场点的位置有关了。两点之间的 **电势差（Electric Potential Difference）** 则可以通过上面的定义快速得到：
$$
\begin{equation}
	V(\mathbf{b}) - V(\mathbf{a}) = -\int_\mathcal{O}^\mathbf{b}\mathbf{E}\cdot d\mathbf{l} + \int_\mathcal{O}^\mathbf{a}\mathbf{E}\cdot d\mathbf{l} = -\int_\mathbf{a}^\mathbf{b}\mathbf{E}\cdot d\mathbf{l}
\end{equation} \label{electric-potential-difference} \tag{2.10}
$$
可以看到电势差和选取的参照点无关。事实上，如果我们改用另一个参照点 $\mathcal{O}'$，在 $\mathbf{r}$ 点的新旧势只差一个常数：
$$
V'(\mathbf{r}) = -\int_{\mathcal{O}'}^\mathbf{r}\mathbf{E}\cdot d\mathbf{l} = -\int_{\mathcal{O}'}^\mathcal{O}\mathbf{E}\cdot d\mathbf{l} - \int_\mathcal{O}^\mathbf{r}\mathbf{E}\cdot d\mathbf{l} = K + V(\mathbf{r})
$$
而两点间的电势差则保持不变。静电学中，我们习惯将 $\mathcal{O}$ 设在无穷远处，并令 $V(\mathcal{O}) = 0$。不过在一些特殊情况下，这可能会导致 $V(\mathbf{r}) = \infty$，此时我们需要选择其它参照点。

现在回到电势差，利用梯度基本定理：
$$
-\int_\mathbf{a}^\mathbf{b}\mathbf{E}\cdot d\mathbf{l} = V(\mathbf{b}) - V(\mathbf{a}) = \int_\mathbf{a}^\mathbf{b}(\nabla V)\cdot d\mathbf{l}
$$
将两端积分去掉，就可以得到电势定义的积分形式：
$$
\begin{equation}
	\marginbox{\mathbf{E} = -\nabla V}
\end{equation} \label{electric-field-from-potential} \tag{2.11}
$$
电势是电磁场中的重要概念，它作为一个易于分析的标量，可以唯一地确定电场；同时它惊人地满足叠加原理，即多个点电荷产生电场的电势等于它们分别产生的电势之和：
$$
V_\text{tot} = \sum_{i=1}^nV_i
$$
这比作为矢量的电场要易于处理得多。因此后文中我们常常首先确定电荷分布产生的电势，随后再确定电场（下一章将着重介绍这个思路）。

#### 电荷分布的电势

电势的定义式 $(\ref{electric-potential})$ 似乎通常意义不大，因为如果我们已经知道电场强度，就没必要知道电势的大小了。下面我们通过库仑定律 $(\ref{coulomb's-law})$ 来推出电荷分布的电势。微元路径积分是：
$$
\mathbf{E}\cdot d\mathbf{l} = \frac{1}{4\pi\epsilon_0}\frac{q}{r^2}\,dr \nonumber
$$
在 $\mathbf{r}$ 点的电势则是：
$$
V(r) = -\int_\mathcal{O}^\mathbf{r} \mathbf{E}\cdot d\mathbf{l} = -\frac{1}{4\pi\epsilon_0}\int_\infty^r\frac{q}{r'^2}\,dr' = \frac{1}{4\pi\epsilon_0}\frac{q}{r}
$$
注意上式中我们利用了 $V(\infty) = 0$。因此，对于给定电荷密度为 $\rho(\mathbf{r}')$ 的一个体积，其电势为：
$$
\begin{equation}
	\marginbox{V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\int\frac{\rho(\mathbf{r}')}{\rcur}\,d\tau'}
\end{equation} \label{electric-potential-integral} \tag{2.12}
$$
上面便是任意静电场的电势通解。其中的 $\rho(\mathbf{r}')\,d\tau'$ 可以被替换为 $\lambda(\mathbf{r}')\,dl'$ 或 $\sigma(\mathbf{r}')\,da'$。再次强调，这个结论是建立在 $V(\infty) = 0$ 的条件下的。

#### 边界条件

静电学问题中，我们通常知道电荷的分布 $\rho$，随后尝试得到电场强度 $\mathbf{E}$。除了高度对称的情况下可以使用高斯定律轻松求解，其余时候我们都得想方法得到电势 $V$，随后再通过 $\mathbf{E} = -\nabla V$ 得到电场强度。

现在让我们考虑一个带电为 $\sigma$ 的平面附近的电场强度分布。我们可以构建一个高斯面方便求解。穿过上下两个面的电场之差应该满足：
$$
\begin{equation}
	E_\text{above}^\perp - E_\text{below}^\perp = \frac{\sigma}{\epsilon_0}
\end{equation} \label{electric-field-boundary-condition} \tag{2.13}
$$
对于平行方向的电场，可以考虑一个垂直并穿过平面的环路。只有 $E_\text{above}^\parallel = E_\text{below}^\parallel$ 时才能保证环路积分为零。

至于电势 $V_\text{above}$ 和 $V_\text{below}$，根据电势差公式 $(\ref{electric-potential-difference})$，当两者非常接近时，应该满足 $V_\text{above} = V_\text{below}$。电势梯度在垂直于平面方向的分量则和电场强度类似，是不连续的：
$$
\frac{\partial V_\text{above}}{\partial n} - \frac{\partial V_\text{below}}{\partial n} = -\frac{\sigma}{\epsilon_0} \tag{2.14}
$$
其中 $\frac{\partial V}{\partial n} = \nabla V\cdot\unit{n}$，被称为电势的 **法向导数（Normal Derivative）**。不难发现这个正规导数恰是电场垂直于平面方向分量的相反矢量。

### 静电学中的功与能

假设目前已知电场强度 $\mathbf{E}$，测试电荷为 $Q$，求将它从 $\mathbf{a}$ 点移动到 $\mathbf{b}$ 点需要做多少功呢？由于测试电荷受到的电场力为 $\mathbf{F} = Q\mathbf{E}$，我们施加的力（至少）应该大小相同方向相反，为 $-Q\mathbf{E}$。整个过程我们做的功是：
$$
W = \int_\mathbf{a}^\mathbf{b}\mathbf{F}\cdot d\mathbf{l} = -Q\int_\mathbf{a}^\mathbf{b}\mathbf{E}\cdot d\mathbf{l} = Q(V(\mathbf{b}) - V(\mathbf{a}))
$$
为了得到电场和测试无关的做功性质，我们将 $W$ 比上 $Q$，得到：
$$
\frac{W}{Q} = V(\mathbf{b}) - V(\mathbf{a})
$$
可见电场中，电势差决定了做功需要的大小。如果考虑初始点为无穷远处（电势为 $0$），我们就得到了构建这个电荷系统所需要的能量，也即电势能：
$$
\begin{equation}
	W = QV(\mathbf{r})
\end{equation} \label{electric-configuration-energy} \tag{2.15}
$$
从这个公式来看，电势就是电荷系统中单位电荷的电势能。

#### 离散电荷分布的能量

第一个电荷不需要任何能量，因为一开始不存在电场；第二个电荷需要克服第一个电荷的电场力，因此我们需要：
$$
W_2 = \frac{1}{4\pi\epsilon_0}q_2\left(\frac{q_1}{\rcur_{12}}\right)
$$
第三个电荷需要同时克服两个电场力做功：
$$
W_3 = \frac{1}{4\pi\epsilon_0}q_3\left(\frac{q_1}{\rcur_{13}} + \frac{q_2}{\rcur_{23}}\right)
$$
第四个电荷需要同时克服三个电场力做功：
$$
W_4 = \frac{1}{4\pi\epsilon_0}q_4\left(\frac{q_1}{\rcur_{14}} + \frac{q_2}{\rcur_{24}} + \frac{q_3}{\rcur_{34}}\right)
$$
以此类推，我们最后可以得到一个通式：
$$
\begin{equation}
	W = \frac{1}{4\pi\epsilon_0}\sum_{i=1}^n\sum_{j>i}^n\frac{q_iq_j}{\rcur_{ij}}
\end{equation} \label{electric-configuration-energy-discrete} \tag{2.16}
$$
也可以等价写成：
$$
\begin{equation}
	W = \frac{1}{2}\sum_{i=1}^nq_i\sum_{j\ne i}^n\frac{1}{4\pi\epsilon_0}\frac{q_j}{\rcur_{ij}} = \frac{1}{2}\sum_{i=1}^nq_iV(\mathbf{r}_i)
\end{equation} \label{electric-configuration-energy-discrete-alt} \tag{2.17}
$$

#### 连续电荷分布的能量

当电荷分布连续时，我们可以将 $(\ref{electric-configuration-energy-discrete-alt})$ 简单改成：
$$
\begin{equation}
	W = \frac{1}{2}\int\rho V\,d\tau
\end{equation} \label{electric-energy-continuous} \tag{2.18}
$$
下面我们尝试将其改写成 $E$ 的关系式，根据高斯定理，我们有：
$$
W = \frac{\epsilon_0}{2}\int(\nabla\cdot\mathbf{E})V\,d\tau
$$
利用分部积分法 $(\ref{integrate-by-parts-divergence})$，我们可以得到：
$$
W = \frac{\epsilon_0}{2}\left(-\int\mathbf{E}\cdot(\nabla V)\,d\tau + \oint V\mathbf{E}\cdot d\mathbf{a}\right) = \frac{\epsilon_0}{2}\left(\int_\mathcal{V}E^2\,d\tau + \oint_\mathcal{S}V\mathbf{E}\cdot d\mathbf{a}\right)\nonumber
$$
这里有一个小技巧。E 大致是 $r^{-2}$ 数量级，$V$ 是 $r^{-1}$ 数量级，而 $\mathcal{S}$ 是 $r^2$ 数量级。上面的第一个积分由于 $E^2 > 0$，无论 $\mathcal{V}$  取多大都能保证单调递增；但第二个积分由于内部总体呈 $r^{-1}$ 数量级，随着 $\mathcal{S}$ 变大，这个积分会越来越小。因此我们只需要考虑无穷远处的一个 $\mathcal{V}$ 和其边界 $\mathcal{S}$，就可以忽略第二个积分了。因此在全空间中，静电场的能量是：
$$
\begin{equation}
	W = \frac{\epsilon_0}{2}\int E^2\,d\tau
\end{equation} \label{electric-energy-all-space} \tag{2.19}
$$
等等，我们这里得到的似乎是一个永远为正的能量；反观 $(\ref{electric-configuration-energy-discrete-alt})$，其结果显然可能是负数，这其中一定存在问题。让我们来好好考察以上这些公式的异同：

- 静电场能量的连续形式公式 $(\ref{electric-energy-continuous})$ 似乎就和离散形式 $(\ref{electric-configuration-energy-discrete-alt})$ 意义不同。后者明确计算每个电荷和 *除自己以外* 的电荷的电势，但前者则统统计算进来。从这种角度看，连续形式公式更加 *全面* 地描述了电场的能量，它甚至考虑了每个电荷自己的能量。离散公式则更倾向于假设电荷已经存在，而计算将它们移动成特定分布时需要的能量。不过，$(\ref{electric-energy-all-space})$ 在计算点电荷的能量时会出现尴尬的一幕：
  $$
  W = \frac{\epsilon_0}{2}\frac{1}{(4\pi\epsilon_0)^2}\int\left(\frac{q}{r^2}\right)^2r^2\sin\theta\,dr\,d\theta\,d\phi = \frac{q^2}{8\pi\epsilon_0}\int_0^\infty\frac{1}{r^2}\,dr = \infty
  $$

- 即使是前后有推导关系的 $(\ref{electric-energy-continuous})$ 和 $(\ref{electric-energy-all-space})$ 也在形式上有微妙的差异。前者将能量局限于 $\rho \ne 0$，也即有电荷分布的地方，而后者则在整个空间中对电场进行积分。究竟是电荷，还是电场拥有能量呢？在更大的背景下，后者是更正确的结论。事实上，电场在全空间的能量密度（单位体积的能量）是：
  $$
  \begin{equation*}
  	U = \frac{\epsilon_0}{2}E^2
  \end{equation*} \tag{2.20} \label{electric-field-energy-density}
  $$
  这和 $(\ref{electric-energy-all-space})$ 相照应。
  
- 最后，令人惊讶（但并不难接受）的是，静电场能量不满足叠加原理。考虑两个电场 $\mathbf{E}_1$ 和 $\mathbf{E}_2$ 的叠加：
  $$
  \begin{align}
  W_\text{tot} &= \frac{\epsilon_0}{2}\int (\mathbf{E}_1 + \mathbf{E}_2)^2\,d\tau = \frac{\epsilon_0}{2}\int(E_1^2 + E_2^2 + 2\mathbf{E}_1\cdot\mathbf{E}_2)\,d\tau \nonumber \\
  &= W_1 + W_2 + \epsilon_0\int\mathbf{E}_1\cdot\mathbf{E}_2\,d\tau
  \end{align} \tag{2.21}
  $$

### 导体

金属 **导体（Conductor）** 中，电子可以自由移动的。我们假设导体中存在 *无穷* 个自由电荷。此时它会有一些奇特的性质：

- 在导体内 $\mathbf{E} = \mathbf{0}$。这是因为一旦存在外加电场 $\mathbf{E}_0$，导体中的电荷会进行自由移动（此时不是我们暂时研究的静电场），直到 **感应电荷（Induced Charge）** 产生的电场和外加电场中和为止。
- 在导体内 $\rho = 0$。这是通过上一条以及高斯定律得到的。
- 所有净电荷都分布在导体表面。这是上一条的直接结论。这似乎有些魔幻，因为显然相同电荷之间会互相排斥，因此我们只能假定它们之间有合理的距离。
- 导体附近的电场垂直于导体表面，否则电荷会不断移动，直到剔除电场的切向分量位置。
- 导体表面是一个等势面，否则电荷会不断移动，直到形成等势面为止。

#### 感应电荷

当我们将一个电荷 $+q$ 靠近一个不带电的导体时，它们会相互吸引。这是因为 $+q$ 会排斥导体中的正电荷，并留下负电荷；由于剩下的负电荷和 $+q$ 更近，所以总体呈现吸引效果。这里，我们分析一个比较特殊的情况。考虑一个金属球壳内部存在一个电荷 $q$。此时根据高斯定律，在球壳以内和以外都应该存在电场。球壳内侧会产生总量为 $-q$ 的感应电荷。这样，在导体中任何包围其内侧曲面的高斯面中总不存在任何净电荷。由导体的性质可以知道，在球壳外侧会产生总量为 $q$ 的电荷，因此包围球壳外侧曲面的高斯面中，净电荷总为 $q$，且导体本身呈电中性。

这给我们一个比较令人欣喜的结果，那就是带空洞的导体（空洞中存在净电荷）的外部电场和空洞的位置无关。此外，如果导体中某个空洞内没有净电荷，那么此处的电场也为 $\mathbf{0}$，且在空洞表面不会有任何电荷（为了理解这一点，可以考虑一个连接空洞表面一对正负电荷 $a, b$ 的环路。此时显然有 $\int_a^b \mathbf{E}\cdot d\mathbf{l} \ne 0$，然而环路剩余的部分均在导体中，那里 $\mathbf{E} = \mathbf{0}$，如下图所示。因此，电场的环路积分非零，这是不可能的），这也是 **法拉第笼（Faraday Cage）** 的原理。

<img src="graphs/ed1_2-4.png" alt="ed1_2-4" style="zoom:50%;" />

#### 导体的面电荷

由于导体内部的电场为零，此前讨论的边界条件 $(\ref{electric-field-boundary-condition})$ 告诉我们导体表面的电场为：
$$
\begin{equation*}
	\mathbf{E} = \frac{\sigma}{\epsilon_0}\unit{n}
\end{equation*} \tag{2.22}
$$
类似地，通过另一条边界条件可以得到：
$$
\begin{equation*}
	\sigma = -\epsilon_0\frac{\partial V}{\partial n}
\end{equation*} \tag{2.23}
$$
因此只要我们可以计算出导体表面的电场或电势，就能得到相应的电荷面密度。由于电场的存在，这些面电荷会受到静电力。我们记单位面积受到的力为 $\mathbf{f}$，即 $\mathbf{f} = \sigma\mathbf{E}$。此处的 $\mathbf{E}$ 是外部电场，满足：
$$
\begin{equation*}
	\mathbf{E}_\text{ave} = \frac{1}{2}\sigma(\mathbf{E}_\text{above} + \mathbf{E}_\text{below})
\end{equation*} \tag{2.24}
$$
这是因为上表面的电场是外部电场 *加上* 面电荷产生的电场，而下表面的电场是外部电场 *减去* 面电荷产生的电场，因此两者的平均值便是纯粹的外部电场。在导体的设定下，$\mathbf{E}_\text{above}$ 是前面给出的电场，而 $\mathbf{E}_\text{below} = \mathbf{0}$，因此：
$$
\begin{equation*}
	\mathbf{f} = \frac{\sigma^2}{2\epsilon_0}\unit{n}
\end{equation*} \tag{2.25}
$$
我们将其称为 **静电压（Electrostatic Pressure）**，其倾向于将导体“拉入”电场中。如果用电场来表示这个量（套用 $(2.22)$ 公式）则是：
$$
\begin{equation*}
	P = \frac{\epsilon_0}{2}E^2
\end{equation*} \tag{2.26}
$$
注意此公式只在导体表面处成立。巧合的是，这个公式和 $(\ref{electric-field-energy-density})$ 完全一致。

#### 电容

考虑两个分别带 $+Q$ 和 $-Q$ 电荷的导体。由于导体表面是等势面，我们可以定义它们之间的电势差为：
$$
V = V_+ - V_- = -\int_{(-)}^{(+)}\mathbf{E}\cdot d\mathbf{l}
$$
现在让我们尝试得到和电场 $\mathbf{E}$ 有关的信息。根据库仑定律 $(\ref{coulomb's-law-integral})$，我们知道电场和电荷密度成正比；而电荷密度是和电荷量成正比的（这或许存疑，但事实如此且不难接受）。因此，我们发现电势差和两个导体带的电荷量成正比。我们将两者的比值定义为 **电容量（Capacitance）**，或简称为 **电容**（这个简称非常常用，但是有时会与 **电容（Capacitor）** ，即本模型中的这两个导体组成的结构混淆，因此在必要时我会将前者称为电容量）：
$$
\begin{equation*}
	C \equiv \frac{Q}{V}
\end{equation*} \tag{2.27} \label{capacitance}
$$
电容是一个只与导体几何性质（大小、形状、两个导体间的距离等）有关的量，**SI** 单位是 **法拉第（Farads, $\text{F}$）**，满足 $\text{F} = \text{C}/\text{V}$。这是一个很大的量，更常用的单位是微法（$10^{-6}$）或皮法（$10^{-12}$）。电容的定义保证其一定是一个正值。

有的时候，我们会说某个导体的电容如何如何。此时是默认另一个导体为无限半径的球形壳。

最后，让我们计算电容的能量。为了形成一个电容，我们需要将电荷从一个导体移动到另一个导体。假设某一刻目标导体上的电荷为 $q$，则有：
$$
dW = \left(\frac{q}{C}\right)\,dq
$$
因此：
$$
W = \int_0^Q\left(\frac{q}{C}\right)\,dq = \frac{1}{2}\frac{Q^2}{C}
$$
将 $Q = CV$ （这里的 $V$ 是最终状态下的电势差）代入则有：
$$
\begin{equation*}
	W = \frac{1}{2}CV^2
\end{equation*} \tag{2.28} \label{energy-of-capacitor}
$$


## 电势

静电学所感兴趣的就是求出一个静止的电荷分布产生的电场。我们已经介绍过下面这个公式了：
$$
\def\rcurs{{\mbox{$\resizebox{.09in}{.08in}{\includegraphics[trim= 1em 0 14em 0,clip]{ScriptR}}$}}}
\def\brcurs{{\mbox{$\resizebox{.09in}{.08in}{\includegraphics[trim= 1em 0 14em 0,clip]{BoldR}}$}}}
\begin{equation*}
	\mathbf{E}(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\int\frac{\boldsymbol{\hat{\mathscr{r}}}}{\mathscr{r}}\rho(\mathbf{r}')\,d\tau'
\end{equation*}
$$
不过这个积分通常解起来比较困难。更流行的做法是先求出电势，然后再逆推回电场：
$$
\begin{align*}
	V(\mathbf{r}) &= \frac{1}{4\pi\epsilon_0}\int\frac{1}{\mathscr{r}}\rho(\mathbf{r}')\,d\tau \\
	\mathbf{E}(\mathbf{r}) &= -\nabla V
\end{align*}
$$
坏消息是，即使是这个积分也不见得好算。等式右侧的 $\rho$ 本身可能比较复杂，因为我们能够控制的往往只是总电荷。所以，下面这个微分形式的公式，即 **泊松方程（Poisson's Equation)** 可能更加有用：
$$
\begin{equation}
	\nabla^2V = -\frac{\rho}{\epsilon_0}
\end{equation} \label{poisson's-equation} \tag{3.1}
$$
这个公式配合一些边界条件时和积分形式的公式是等价的。通常，我们对电荷密度为 $0$ 的地方更感兴趣。换句话说，虽然某些地方周围存在电荷，但我们只关注于不存在电荷的地方。此时等式右侧为 $0$，我们也就将公式化为了更加友好的形式：
$$
\begin{equation}
	\nabla^2V = 0
\end{equation} \label{laplace's-equation} \tag{3.2}
$$
这便是 **拉普拉斯方程（Laplace's Equation）**。本节中将用一些篇幅，首先介绍这个偏微分方程的性质和求解。

### 拉普拉斯方程

拉普拉斯方程在物理学中非常常见，它的解被称为 **调和函数（Harmonic Function）**。接下来我们会发现它的两个重要性质：

- 在其中某个点的解等于其周围等距离边界上的解的平均值。
- 不会出现局部极值。换句话说，方程解的极值只会出现在边界。

为了方便类比和阐述，让我们从一维中的拉普拉斯方程开始，一步步介绍到三维。

#### 一维的拉普拉斯方程

$$
\begin{equation}
	\frac{d^2 V}{dx^2} = 0
\end{equation} \label{1d-laplace's-equation} \tag{3.3}
$$

这甚至只是一个常微分方程。其通解是显然的：
$$
V(x) = mx + b
$$
其几何意义是一条直线。对照之前我们提到的方程性质，不难发现其都被满足了。

#### 二维的拉普拉斯方程

$$
\begin{equation}
	\frac{\partial^2 V}{\partial x^2} + \frac{\partial^2 V}{\partial y^2} = 0
\end{equation} \label{2d-laplace's equation} \tag{3.4}
$$

这次我们就没办法得到一个简单的通解了（具体的解我们会在后面讨论）。不过其依然遵守方程的两个性质：
$$
\begin{equation}
	V(x, y) = \frac{1}{2\pi R}\underset{\text{circle}}{\oint}V\,dl
\end{equation} \tag{3.5}
$$
（这不是 $V$ 的解！）可以看到任一点的解都是其周围以之为中心任取的圆边界上解的平均值。受这个特性启发，我们可以设计出一个计算电势的算法：首先确定边界上所有点的电势，并对内部特定网格上的点上的电势进行合理猜测。之后，根据这些值更新周围点的平均电势。多次迭代后就能够得到确定的电势。

有趣的事实：二维中的调和函数是最小化通过给定边界线曲面面积的曲面。

#### 三维的拉普拉斯方程

$$
\begin{equation}
	\frac{\partial^2 V}{\partial x^2} + \frac{\partial^2 V}{\partial y^2} + \frac{\partial^2 V}{\partial z^2} = 0
\end{equation} \label{3d-laplace's-equation} \tag{3.6}
$$

同样地，三维中的拉普拉斯方程满足下面的性质：
$$
\begin{equation}
	V(\mathbf{r}) = \frac{1}{4\pi R^2}\underset{\text{sphere}}{\oint}V\,da
\end{equation} \tag{3.7}
$$

#### 边界条件、导体和唯一性定理

我们至今都没有给出拉普拉斯方程的通解（除了一维的情形），实际上我们甚至不知道在给定多少边界条件时能够确定一个方程的解。通过之前介绍的调和函数性质，我们猜测如果确定了一个闭合范围边界上的值，就能够确定这个闭合区域中所有的解：

> **第一唯一性定理**：某个不包含电荷的体积 $\mathcal{V}$ 中的电势由这个体积的边界 $\mathcal{S}$ 上的电势唯一地确定。

> **证**：
>
> 假设在定理的条件下有两个 *不同的* 解 $V_1, V_2$ 使得 $\nabla^2 V_1 = \nabla^2 V_2 = 0$ 在 $\mathcal{V}$ 中成立，令 $V_3 \equiv V_1 - V_2$，此时有 $\nabla^2 V_3 = \nabla^2 V_1 - \nabla^2 V_2 = 0$，因此 $V_3$ 也是拉普拉斯方程的解。然而由于在 $\mathcal{S}$ 上恒有 $V_3 = V_1 - V_2 = 0$，由于拉普拉斯方程没有局部极值的性质，我们得到 $V_3 \equiv 0$。这也即是 $V_1 = V_2$，得证。

事实上，利用第一唯一性定理的证明可以得到更强的结论。前面的讨论都是基于不存在任何电荷的 $\mathcal{V}$ 进行的（为了满足拉普拉斯方程）。但是对于包含任意电荷的体积 $\mathcal{V}$，此时 $\nabla^2 V = -\frac{\rho}{\epsilon_0}$，我们都能得到一样的结果。最终结论如下：

> **推论**：某个体积 $\mathcal{V}$ 的电势由其中的电荷密度 $\rho$ 以及边界 $\mathcal{S}$ 上的电势唯一地确定。

现在让我们考虑一些导体。对于带电的导体来说，我们通常只知道其电量而非电势。如果我们确定每个导体上的电量，以及这些导体之间区域的电荷密度，此时我们能否得到确定的电场？事实上，只需要知道总电量和电荷密度，就可以得到确定的电场：

> **第二唯一性定理**：某个被导体包围的，电荷密度为 $\rho$ 的体积 $\mathcal{V}$ 中的电势由 $\mathcal{V}$ 中所有导体的总电量唯一地确定。

> **证**：
>
> 假设在定理的条件下有两个 *不同的* 解 $\mathbf{E}_1, \mathbf{E}_2$ 使得 $\nabla\cdot \mathbf{E}_1 = \nabla\cdot\mathbf{E}_2 = \frac{\rho}{\epsilon_0}$ 在 $\mathcal{V}$ 中不包括导体的部分成立。此时对于任一个包围的导体表面 $\mathcal{S}_i$，根据高斯定理有 $\oint_{\mathcal{S}_i}\mathbf{E}_1\cdot d\mathbf{a} = \oint_{\mathcal{S}_i}\mathbf{E}_2\cdot d\mathbf{a} = \frac{Q_i}{\epsilon_0}$。令 $\mathbf{E}_3 = \mathbf{E}_1 - \mathbf{E}_2$，则有 $\oint_{\mathcal{S}_i}\mathbf{E}_3\cdot d\mathbf{a} = 0$。由于每个导体表面都是等势面，$V_3$ 应该是一个常数。根据散度定理同时有：$\int_{\mathbf{V}_i}\nabla\cdot(V_3\mathbf{E}_3)\,d\tau = 0$。由 $del$ 算子和矢量的运算性质可以最终得到 $-\int_{\mathcal{V}_3}(E_3)^2\,d\tau = 0$。这个积分显然是非负的，因此我们得到 $E_3 = 0$。这也即是 $\mathbf{E}_1 = \mathbf{E}_2$，得证。

### 图像法

**图像法（Method of Image）** 是通过对称性解决静电场问题的方法，它通过引入电荷关于某个平面镜面对称的相反电荷来得到某个问题的等效情景。但是前者明显更容易解决，因为在平面上的电势总为零。下面我们就从一个图像法的经典应用来展示其奇妙之处。

#### 经典图像问题

考虑一个电荷 $q$ 举例一个接地的无限导体平面，求在有电荷这半边空间的电势。如果只是从直觉分析这个问题，可能会觉得非常棘手，因为电荷 $q$ 会吸引导体中的异性电荷，而我们甚至不知道会有多少电荷被吸引，并在导体平面上呈现什么分布。不过数学上，我们的目标是当 $q$ 在 $(0, 0, d)$ 时 ，求解 $z > 0$ 时的泊松方程，边界条件如下：

- $z = 0$ 时  $V = 0$，因为导体接地。
- 远离 $q$ 时，也即 $x^2 + y^2 + z^2 \gg d^2$ 时， $V \to 0$。

根据第一唯一性定理，我们可以根据这些边界条件确定唯一的电势，从而确定唯一的电场。不过即使如此，我们也需要想办法得到一个解。考虑一个完全不同的题设，导体平面根本不存在，并且在 $(0, 0, -d)$ 处存在另一个 $-q$ 电荷。此时我们可以轻松地写出空间中任一点的电势：
$$
V(x, y, z) = \frac{q}{4\pi\epsilon_0}\left(\frac{1}{\sqrt{x^2 + y^2 + (z - d)^2}} + \frac{1}{\sqrt{x^2 + y^2 + (z + d)^2}}\right)
$$
呃，但这和我们想要解决的问题有什么关系呢？事实上，当 $z > 0$ 时这碰巧也是无限导体平面问题的解！因为在后面的这种设定下，之前的边界条件依然成立，而第一唯一性定理并不关心边界条件以外的任何信息（电荷分布等），所以可以肯定这就是该问题的解。

#### 感应表面电荷

在已经知道电势的情况下，我们可以轻易地得到导体平面上的电荷密度：
$$
\sigma = -\nabla V = \left.-\epsilon_0\frac{\partial V}{\partial z}\right|_{z=0} = \frac{-qd}{2\pi(x^2 + y^2 + d^2)^{3/2}}
$$
不出所料，感应电荷是负值且在原点处最大。我们可以利用这个结果计算总的感应电荷：
$$
Q = \int\sigma\,da = \int_0^{2\pi}\int_0^\infty \frac{-qd}{2\pi(x^2 + y^2 + d^2)^{3/2}}r\,dr\,d\phi = -q
$$
这点也不出所料。

#### 力与能量

现在让我们看看这个电荷如何被导体平面的感应电荷吸引。计算电场力并没有什么复杂的：
$$
\mathbf{F} = -\frac{1}{4\pi\epsilon_0}\frac{q^2}{(2d)^2}\hat{\mathbf{z}}
$$
这个电场的能量可以通过计算将一个电荷 $q$ 从无限远处拉到 $d$ 所做的功来得到：
$$
W = \int_\infty^d \mathbf{F}\cdot d\mathbf{l} = \frac{1}{4\pi\epsilon_0}\int_\infty^d\frac{q^2}{4z^2}\,dz = -\frac{1}{4\pi\epsilon_0}\frac{q^2}{4d^2}
$$

#### 接地球形导体

我们再来看一个利用图像法解决静电场问题的例子。有一个接地的球形导体半径为 $R$，距离球心 $a$ 处有一个电荷 $q$，求导体外部的电势分布。

同样地，让我们考虑另一个情景，在球心和 $q$ 中间距离球心 $\frac{R^2}{a}$ 处放置一个电量为 $q' = -\frac{R}{a}q$ 的电荷。此时球外的电势可以写成：
$$
V = \frac{1}{4\pi\epsilon_0}\left(\frac{q}{\mathscr{r}} + \frac{q'}{\mathscr{r}'}\right) = \frac{q}{4\pi\epsilon_0}\left(\frac{1}{\sqrt{r^2 + a^2 - 2ra\cos{\theta}}} - \frac{1}{\sqrt{R^2 + (ra/R)^2 - 2ra\cos{\theta}}}\right)
$$
在球面上，即 $r = R$ 时，显然也有 $V = 0$，因此和原题设的边界条件一致。此时电荷受力为：
$$
\mathbf{F} = -\frac{1}{4\pi\epsilon_0}\frac{q^2Ra}{(a^2 - R^2)^2}\hat{\mathbf{r}}
$$

 ### 分离变量法

#### 笛卡尔坐标系

现在介绍一种直接解决拉普拉斯方程的方式，它基于一个简单的假设：调和函数能够表示成一系列一元函数的积，即如下形式（我们先从二维中的拉普拉斯方程讲起）：
$$
V(x, y) = X(x)Y(y) \tag{3.8}
$$
这当然不是拉普拉斯方程的通解；但是根据这个假设我们可以轻松地进一步求解：将 (84) 式插入原方程：
$$
Y\frac{d^2X}{dx^2} + X\frac{d^2Y}{dY^2} = 0 \implies \frac{X''(x)}{X(x)} = -\frac{Y''(y)}{Y(y)} = k^2
$$
这里的 $k$ 是某个常数。这样我们就将一个偏微分方程转化为两个常微分方程。它们的解是：
$$
\begin{align}
	X(x) = Ae^{kx} + Be^{-kx} && Y(y) = C\sin{ky} + D\sin{ky}
\end{align}
$$
因此原方程的解是：
$$
\begin{equation}
	V(x, y) = (Ae^{kx} + Be^{-kx})(C\sin{ky} + D\sin{ky})
\end{equation} \label{2d-laplace's-equation-general-solution} \tag{3.9}
$$
为了能进一步求解这个方程，我们设定四个边界条件（这只是某一种特殊情形，后面根据问题的不同设置我们需要不同的边界条件）：

- $y = 0$ 时 $V = 0$。
- $y = a$ 时 $V = 0$。
- $x = 0$ 时  $V = V_0(y)$。
- $x \to \infty$ 时 $V \to 0$。

这实际上是一个非常基本的情形，图示如下：

<img src="graphs/ed1_3-1.png" alt="ed1_3-1" style="zoom:40%;" />

我们将边界条件中的前两条代入 $(3.9)$ 式，可以得到：
$$
V(x, y) = Ce^{-kx}\sin{ky}, \quad k = \frac{n\pi}{a}, \quad n = 1, 2, ...
$$
过程中可以意识到上面 $(3.9)$ 为了贴合边界条件，将 $x$ 设计为指数函数、$y$ 设计为三角函数。因为上面的 $n$ 是任取的正整数，也就是说我们得到了无穷多个解 $V_1, V_2, ...$。由于原方程是线性的（可以简单证明），我们可以写出分离变量法得到的通解：
$$
V(x, y) = \sum_{n=1}^\infty C_ne^{-\frac{n\pi x}{a}}\sin{\frac{n\pi y}{a}}
$$
边界条件的第三条要求 $V(0, y) = V_0(y)$。我们可以根据傅立叶级数的知识对 *任意* 的 $V_0(y)$ 进行例如 $(3.9)$ 式（取 $x = 0$ ）的构造（这和函数的 **完备性（Completeness）** 有关，具体证明比较复杂，我们这里不进行说明）。最后一个问题是 $C_n$ 的求解，这用到了一个小技巧。我们将 $(3.9)$ 式稍稍改写一下，引入 $n' \in \Z^+$：
$$
\sum_{n=1}^\infty C_n\int_0^a\sin{\frac{n\pi y}{a}}\sin{\frac{n'\pi y}{a}}\,dy = \int_0^aV_0(y)\sin{\frac{n'\pi y}{a}}\,dy \tag{3.10}
$$
左式的值惊人地简单，在 $n' \ne n$ 时均为 $0$。在 $n' = n$ 时则是 $\frac{a}{2}C_n$（这被称为函数的 **正交性（Orthogonality）**）。这样我们就得到了 $C_n$ 的计算式：
$$
C_n = \frac{2}{a}\int_0^aV_0(y)\sin{\frac{n\pi y}{a}}\,dy
$$
让我们举例子来说明：

> **例**：用分离变量法解拉普拉斯方程，边界条件和前文中列出的一致，除了 $V(0, y) = V_0$ 是一个常数。

> **解**：因为边界条件 $V(0, y) = V_0$，我们可以进一步化简 (91) 式：
> $$
> C_n = \frac{2V_0}{a}\int_0^a\sin{\frac{n\pi y}{a}}\,dy = \frac{2V_0}{n\pi}(1 - \cos{n\pi})
> = \begin{cases}
> 0, \quad \text{if $n$ is even} \\\\
> \dfrac{4V_0}{n\pi}, \quad \text{if $n$ is odd}
> \end{cases}
> $$
> 代入 (89) 式我们得到：
> $$
> V(x, y) = \frac{4V_0}{\pi}\sum_{n = 1, 3,...}\frac{1}{n}e^{-\frac{n\pi x}{a}}\sin{\frac{n\pi y}{a}}
> $$
> 这个级数实际上可以进一步简化为：
> $$
> V(x, y) = \frac{2V_0}{\pi}\arctan{\frac{\sin{\frac{\pi y}{a}}}{\sinh{\frac{\pi x}{a}}}}
> $$
> 图示如下：
>
> <img src="graphs/ed1_3-2.png" alt="ed1_3-2" style="zoom:50%;" /><img src="graphs/ed1_3-3.png" alt="ed1_3-3" style="zoom:40%;" />
>
> 右图中，(a), (b), (c), (d) 分别对应着 $n = 1, 5, 10, 100$ 的情形。可以看到随着 $n$ 变大，$V$ 越来越贴近于 $V_0$。

> **例**：用分离变量法解拉普拉斯方程，边界条件如下：
>
> - $y = 0$ 及 $y = a$ 时 $V = 0$。
> - $x = \pm b$ 时 $V = V_0$ 为一个常数。

> **解**：虽然边界条件不同，我们同样可以得到 (87) 式。之后代入边界条件化简为：
> $$
> V(x, y) = C\cosh{\frac{n \pi x}{a}}\sin{\frac{n \pi y}{a}}
> $$
> 线性组合后得到：
> $$
> V(x, y) = \sum_{n = 1}^\infty C_n\cosh{\frac{n\pi x}{a}}\sin{\frac{n \pi y}{a}}
> $$
> 其中 $C_n$ 的求解步骤和之前介绍的类似，最后我们得到：
> $$
> V(x, y) = \frac{4V_0}{\pi}\sum_{n=1,3,...}\frac{1}{n}\frac{\cosh{\frac{n\pi x}{a}}}{\cosh{\frac{n\pi b}{a}}}\sin{\frac{n\pi y}{a}}
> $$
> 如下图所示：
>
> <img src="graphs/ed1_3-4.png" alt="ed1_3-4" style="zoom:45%;" />

> **例**：一个无限长的接地金属管道（长宽分别为 $a, b$），在某一个截面处的电势为 $V_0(y, z)$。求该管道的电势分布。图示如下：
>
> <img src="graphs/ed1_3-5.png" alt="ed1_3-5" style="zoom:50%;" />

> **解**：这里我们需要解一个三维的拉普拉斯方程，即 $(\ref{3d-laplace's-equation})$ 式。利用分离变量法，我们假设 $V(x, y, z) = X(x)Y(y)Z(z)$。代入原方程后得到：
> $$
> \frac{1}{X}\frac{d^2 X}{dx^2} + \frac{1}{Y}\frac{d^2 Y}{dy^2} + \frac{1}{Z}\frac{d^2 Z}{dz^2} = 0
> $$
> 设：
> $$
> \begin{align}
> 	\frac{d^2 X}{dx^2} = (k^2+l^2)X\quad \frac{d^2 Y}{dy^2} = -k^2Y \quad \frac{d^2Z}{dz^2} = -l^2Z
> \end{align}
> $$
> 对于这三个常微分方程，我们可以轻易地得到下面的解：
> $$
> \begin{align}
> 	X(x) &= Ae^{\sqrt{k^2+l^2}x} + Be^{-\sqrt{k^2+l^2}x} \\
>     Y(y) &= C\sin{ky} + D\cos{ky} \\
>     Z(z) &= E\sin{lz} + F\cos{lz}
> \end{align} \label{3d-laplace's-equation-general-solution} \tag{3.11}
> $$
> 代入边界条件，我们可以得到下面的解：
> $$
> 	V(x, y, z) = Ce^{-\pi\sqrt{(n/a)^2 + (m/b)^2}x}\sin{\frac{n\pi y}{a}}\sin{\frac{m\pi z}{b}}
> $$
> 由于 $m, n$ 都是任取的正整数，根据拉普拉斯方程的线性，通解可以写成级数的形式：
> $$
> V(x, y, z) = \sum_{n=1}^\infty\sum_{m=1}^\infty C_{n, m}e^{-\pi\sqrt{(n/a)^2+(m/b)^2}x}\sin{\frac{n\pi y}{a}}\sin{\frac{m\pi z}{b}}
> $$
> 同时，边界条件 $V(0, y, z) = V_0(y, z)$，可以帮助我们得到 $C_{n, m}$ 的算式：
> $$
> C_{n, m} = \frac{4}{ab}\int_0^a\int_0^bV_0(y, z)\sin{\frac{n\pi y}{a}}\sin{\frac{m\pi z}{b}}\,dy\,dz
> = \begin{cases}
> 0 \quad \text{if $n$ or $m$ is even} \\\\
> \dfrac{16V_0}{\pi^2nm} \quad \text{if $n$ and $m$ are odd}
> \end{cases}
> $$
> 最后的式子是：
> $$
> V(x, y, z) = \frac{16V_0}{\pi^2}\sum_{n, m = 1, 3, ...}\frac{1}{nm}e^{-\pi\sqrt{(n/a)^2 + (m/b)^2}x}\sin{\frac{n\pi y}{a}}\sin{\frac{m\pi z}{b}}
> $$

#### 球坐标系

此前的边界都是平行于坐标轴的平面，因此使用笛卡尔坐标系非常合适。但是我们同样对以球面为边界的电势问题感兴趣。球坐标系中，拉普拉斯方程是以下的形式：
$$
\begin{equation}
\frac{1}{r^2}\frac{\partial}{\partial r}\left(r^2\frac{\partial V}{\partial r}\right) + \frac{1}{r^2\sin{\theta}}\frac{\partial }{\partial \theta}\left(\sin{\theta}\frac{\partial V}{\partial \theta}\right) + \frac{1}{r^2\sin^2\theta}\frac{\partial^2 V}{\partial \phi^2} = 0
\end{equation} \label{3d-laplace's-equation-spherical} \tag{3.12}
$$
这个式子令人望而却步。我们假设这个式子和 $\phi$ 无关，这样我们可以将其简化为：
$$
\begin{equation}
	\frac{\partial }{\partial r}\left(r^2\frac{\partial V}{\partial r}\right) + \frac{1}{\sin{\theta}}\frac{\partial}{\partial \theta}\left(\sin{\theta}\frac{\partial V}{\partial \theta}\right) = 0
\end{equation} \label{3d-laplace-equation-spherical-reduced} \tag{3.13}
$$
利用分离变量法，我们假设：
$$
V(r, \theta) = R(r)\Theta(\theta)
$$
插入 $(\ref{3d-laplace-equation-spherical-reduced})$ 式，我们可以得到：
$$
\frac{1}{R}\frac{d}{dr}\left(r^2\frac{dR}{dr}\right) + \frac{1}{\Theta\sin{\theta}}\frac{d}{d\theta}\left(\sin{\theta}\frac{d\Theta}{d\theta}\right) = 0
$$
我们同样用一个常数来代替每一项：
$$
\frac{1}{R}\frac{d}{dr}\left(r^2\frac{dR}{dr}\right) = -\frac{1}{\Theta\sin{\theta}}\frac{d}{d\theta}\left(\sin{\theta}\frac{d\Theta}{d\theta}\right) = l(l + 1)
$$
这里，常数 $l(l + 1)$ 是为了之后答案简洁而故意设成这样。下面是这两个常微分方程的解：
$$
\begin{align}
	R(r) = Ar^l + \frac{B}{r^{l+1}} && \Theta(\theta) = P_l(\cos{\theta})
\end{align}
$$
其中 $P_l(x)$ 被称为 **勒让德多项式（Legendre Polynomial）**，其通常可以通过 **罗德里格公式（Rodrigues Formula）** 计算得到：
$$
\begin{equation}
	P_l(x) = \frac{1}{2^ll!}\left(\frac{d}{dx}\right)^l(x^2 - l)^l
\end{equation} \label{rodrigues-formula} \tag{3.14}
$$
作为举例，下面是前几个勒让德多项式：
$$
\begin{align*}
	P_0(x) &= 1 \\
	P_1(x) &= x \\
	P_2(x) &= \frac{1}{2}(3x^2 - 1) \\
	P_3(x) &= \frac{1}{2}(5x^3 - 3x) \\
	P_4(x) &= \frac{1}{8}(35^4 - 30x^2 + 3)
\end{align*}
$$
其典型的性质是：

- $P_l(1) = 1$，这也是它的系数 $\frac{1}{2^ll!}$ 的功劳。
- 第奇数个勒让德多项式只有 $x$ 的奇数次幂；第偶数个勒让德多项式只有 $x$ 的偶数次幂。这对函数的奇偶性会有一定启示。

这样我们就得到了球坐标系下不考虑方位角 $\phi$ 的拉普拉斯方程的通解：
$$
\begin{equation}
	V(r, \theta) = \sum_{l = 0}^\infty\left(A_lr^l + \frac{B_l}{r^{l+1}}\right)P_l(\cos\theta)
\end{equation} \label{3d-laplace's-equation-spherical-general-solution} \tag{3.15}
$$
这里需要注意的是，勒让德多项式并不是 $\Theta(\theta)$ 唯一的解（原方程是二阶的偏微分方程），但它们在 $\theta = 0$ 或 $\theta = \pi$ 时会得到物理中不成立的值，因此我们不考虑。作为例子，$l = 0$ 时另外有一个解 $\Theta(\theta) = \ln\left(\tan{\frac{\theta}{2}}\right)$。

下面举一些例子说明：

> **例**：求解球坐标系中的拉普拉斯方程，其中在球表面处的电势为 $V_0(\theta)$。

> **解**：为了不让电势在原点爆掉，$B_l$ 应该是 $0$。因此通解是：
> $$
> V(r, \theta) = \sum_{l=0}^\infty A_lr^lP_l(\cos{\theta})
> $$
> 同时为了满足边界条件 $V(R, \theta) = V_0(\theta)$，根据勒让德多项式在 $-1 \le x \le 1$ 的完备性和正交性（不作证明），我们有：
> $$
> \int_{-1}^1P_l(x)P_{l'}(x)\,dx = \int_0^\pi P_l(\cos{\theta})P_{l'}(\cos{\theta})\sin{\theta}\,d\theta
> = \begin{cases}
> 0, \quad \text{if $l' \ne l$} \\\\
> \dfrac{2}{2l + 1}, \quad \text{if $l' = l$}
> \end{cases}
> $$
> 因此对前式（取 $r = R$）两侧都乘上 $P_{l'}(\cos{\theta})$ 并积分后，可以得到：
> $$
> A_l = \frac{2l + 1}{2R^l}\int_0^\pi V_0(\theta)P_l(\cos{\theta})\sin\theta\,d\theta
> $$
> 这个式子显然没有解析解。事实上我们习惯的解法是直接通过边界条件的式子得到通用解。以 $V_0(\theta) = k\sin^2(\frac{\theta}{2})$ 为例（$k$ 是一个常数）。通过半角公式我们可以得到：
> $$
> V_0(\theta) = \frac{k}{2}(1 - \cos\theta) = \frac{k}{2}[P_0(\cos\theta) - P_1(\cos\theta)]\nonumber
> $$
> 把这个式子和 $V(R, \theta) = \sum_{l=0}^\infty A_lR^lP_l(\cos\theta) = V_0(\theta)$ 对比，就可以得到 $A_0 = \frac{k}{2}, A_1 = -\frac{k}{2R}$，再代入通解：
> $$
> V(r, \theta) = \frac{k}{2}\left(1 - \frac{r}{R}\cos\theta\right)
> $$

### 多极展开

#### 远距离的电势估计

在一个电荷分布的很远处，我们可以将这个电荷分布看作一个点电荷来计算电势，这没有问题（我们时常也用这个来检查计算出来的电势）；此外，如果 $Q \approx 0$ ，我们大可将电势估计为 $0$，因为在电荷分布的很远处，通常电势都会非常小。下面我们构建一个常见的模型来深入探讨这个情景。

一个 **电偶极子（Electric Dipole）** 由一对距离为 $d$ 的正负电荷构成，电量为 $\pm q$。图片描述如下：

<img src="graphs/ed1_3-6.png" alt="ed1_3-6" style="zoom:45%;" />

下面我们尝试得到在电偶极子远处的电势：
$$
V(\mathbf{r}) = \frac{1}{4\pi \epsilon_0}\left(\frac{q}{\mathscr{r}_+} - \frac{q}{\mathscr{r}_-}\right)\nonumber
$$
根据余弦定理，并考虑 $d \gg r$ 我们有：
$$
\mathscr{r}_{\pm} = r^2 + \left(\frac{d}{2}\right)^2 \mp rd\cos\theta = r^2\left(1 \mp \frac{d}{r}\cos\theta + \frac{d^2}{4r^2}\right) \approx r^2\left(1 \mp \frac{d}{r}\cos\theta\right)\nonumber
$$
取倒数后相减得到：
$$
\frac{1}{\mathscr{r}_+} - \frac{1}{\mathscr{r}_-} \approx \frac{d}{r^2}\cos\theta\nonumber
$$
这样就得到了电偶极子在远处的电势分布：
$$
\begin{equation}
	V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\frac{qd\cos\theta}{r^2}
\end{equation} \label{electric-dipole-potential} \tag{3.16}
$$
这个结果可能还有些出乎意料，因为电势的衰减速度居然和距离成平方反比。实际上，如果对于类似设置的 **四极子（Quadrupole）**、**八极子（Octopole）** 进行分析，它们的电势衰减速度分别为立方反比和四次方反比。

<img src="graphs/ed1_3-7.png" alt="ed1_3-7" style="zoom:50%;" />

现在让我们考虑一个更加通用的场景。对于一个电荷分布，取原点到源和场的径矢 $\mathbf{r}'$ 和 $\mathbf{r}$，源到场的矢量为 $\mathscr{r}$（这正是我们一直以来的设置）。设 $\mathbf{r}$ 和 $\mathbf{r}'$ 的夹角为 $\alpha$。根据余弦定理有：
$$
\mathscr{r}^2 = r^2 + r'^2 - 2rr'\cos\alpha = r^2\left[1 + \left(\frac{r'}{r}\right)^2 - 2\left(\frac{r'}{r}\right)\cos\alpha\right]
$$
我们记：
$$
\epsilon = \left(\frac{r'}{r}\right)\left(\frac{r'}{r} - 2\cos\alpha\right) \tag{3.17}
$$
这样原始就可以写为 $\mathscr{r} = r\sqrt{1 + \epsilon}$ 的形式了。根据二项式定理我们可以将其展开为多项式：
$$
\begin{align}
\frac{1}{\mathscr{r}} &= \frac{1}{r}\left(1 - \frac{1}{2}\epsilon + \frac{3}{8}\epsilon^2 - \frac{5}{16}\epsilon^3 + ... \right) \nonumber\\
&= \frac{1}{r}\left[1 - \frac{1}{2}\left(\frac{r'}{r}\right)\left(\frac{r'}{r} - 2\cos\alpha\right) + \frac{3}{8}\left(\frac{r'}{r}\right)^2\left(\frac{r'}{r} - 2\cos\alpha\right)^2 - \frac{5}{16}\left(\frac{r'}{r}\right)^3\left(\frac{r'}{r} - 2\cos\alpha\right)^3 + ...\right] \nonumber\\
&= \frac{1}{r}\left[1 + \left(\frac{r'}{r}\right)\cos\alpha + \left(\frac{r'}{r}\right)^2\left(\frac{3\cos^2\alpha - 1}{2}\right) + \left(\frac{r'}{r}\right)^3\left(\frac{5\cos^3\alpha - 3\cos\alpha}{2}\right) + ...\right] \nonumber\\
&= \frac{1}{r}\sum_{n=0}^\infty \left(\frac{r'}{r}\right)^nP_n(\cos\alpha)
\end{align} \label{expansion-of-1-over-r} \tag{3.18}
$$
中间真的非常巧合，每一项的系数正好是勒让德多项式（事实上，我们也将 $\frac{1}{r}$ 称为勒让德多项式的生成函数）。这样，我们就得到了任意电荷分布在远处的产生的电势，我们也将其称为电势 $\frac{1}{r}$ 的 **多极展开（Multipole Expansion）**：
$$
\begin{equation}
V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\sum_{n=0}^\infty \frac{1}{r^{n+1}}\int r'^nP_n(\cos\alpha)\rho(\mathbf{r}')\,d\tau'
\end{equation} \label{electric-multipole-expansion} \tag{3.18}
$$

#### 单极子和偶极子

在 $r \gg 0$ 时，多极展开公式被第一项主导，也就是单极子的电势近似（当 $r' = 0$ 时，这个式子就是准确的电势公式）：
$$
V_\text{mon}(\mathbf{r}) = \frac{1}{4\pi \epsilon_0}\frac{Q}{r}
$$
当总电荷 $Q \to 0$ 时，主导的就是第二项了（因为第一项趋于 $0$）：
$$
V_\text{dip}(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\frac{1}{r^2}\int r'\cos\alpha\rho(\mathbf{r}')\,d\tau'
$$
由于 $r'\cos\alpha = \hat{\mathbf{r}}\cdot\mathbf{r}'$，我们可以将上式写得更为简洁：
$$
V_\text{dip}(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\frac{1}{r^2}\hat{\mathbf{r}}\cdot\int\mathbf{r}'\rho(\mathbf{r}')\,d\tau'
$$
其中和 $\mathbf{r}$ 无关的项称为 **偶极矩（Dipole Moment）**：
$$
\begin{equation}
	\marginbox{\mathbf{p} = \int\mathbf{r}'\rho(\mathbf{r}')\,d\tau'}
\end{equation} \label{electric-dipole-moment} \tag{3.19}
$$
这样，也可以将满足偶极子分布的电势写成：
$$
\begin{equation}
	V_\text{dip}(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\frac{\mathbf{p}\cdot\hat{\mathbf{r}}}{r^2}
\end{equation} \label{electric-dipole-potential-alt} \tag{3.20}
$$
偶极矩取决于电荷分布的集合特性（比如说形状、大小和密度）。如果考虑离散的电荷分布，偶极矩的定义如下式：
$$
\begin{equation}
	\mathbf{p} = \sum_{i=1}^nq_i\mathbf{r}_i'
\end{equation} \label{electric-dipole-moment-discrete} \tag{3.21}
$$
特别地，只有正负两个电荷存在时（此时我们称其为 **物理偶极子（Physical Dipole）**，它们产生的偶极矩将只和它们之间的距离有关：
$$
\begin{equation}
	\mathbf{p} = q\mathbf{r}_+' - q\mathbf{r}_-' = q\mathbf{d}
\end{equation} \label{physical-dipole} \tag{3.22}
$$
结合 $(\ref{electric-dipole-potential-alt})$，我们可以得到与上一节相同的结论。注意这里是一个估计值；当 $r$ 变大或者 $d$ 变小时，这个估计会更加精确。当 $d \to 0$ 时，我们可以得到一个 **完美偶极子（Perfect Dipole）**，有时也称为 **纯偶极子（Pure Dipole）**。此时为了不让偶极矩削减至零，我们还应该让 $q \to \infty$，保证 $qd$ 是一个固定的值。此时我们可以放心地使用 $(\ref{electric-dipole-potential-alt})$ 式了。

#### 电偶极子的电场

现在我们假设一个完美偶极子在原点，偶极矩沿 $z$ 方向。此时 $(\ref{electric-dipole-potential-alt})$ 可以在球坐标系中简化为：
$$
\begin{equation*}
	V_\text{dip}(r, \theta) = \frac{p\cos\theta}{4\pi\epsilon_0r^2}
\end{equation*} \tag{3.23} \label{electric-potential-of-electric-dipole}
$$
求它每个方向的梯度的负数即是该方向的电场，综合起来就是：
$$
\begin{equation}
	\textbf{E}_\text{dip}(r, \theta) = \frac{p}{4\pi\epsilon_0r^3}(2\cos\theta\hat{\mathbf{r}} + \sin\theta\hat{\boldsymbol{\theta}})
\end{equation} \tag{3.24} \label{electric-field-of-electric-dipole}
$$
可以看到偶极子场和距离的三次方成反比；类似地我们能得到四极子场和距离的四次方、八极子场和距离的五次方……成反比。这和我们此前得到的结论（电偶极子的电势和平方成反比，四极子和八极子则是平方反比和立方反比）互洽。

下面是偶极子场的示意图，可以看到它非常相似于磁场：

<img src="graphs/ed1_3-8.png" alt="ed1_3-8" style="zoom:40%;" />

左侧是一个完美偶极子的电场，右侧是一个物理偶极子的电场。



## 物质中的电场

这一章，让我们研究物质中的电场。我们将物质分为两种，**导体（Conductor）** 和 **绝缘体（Insulator）**（或称为 **电介质（Dielectric）**。前者充斥了可以自由移动的电子，它们不依附于任一个原子核；后者中的电子则紧紧关联于一个原子核，只能在原子范围内适量移动。虽然电介质中的电子并不容易移动，但它 *依然* 会受电场影响运动，主要以两种形式，**伸展** 和 **旋转**，我们很快就会提到它们。

### 极化

当把一个中性的粒子放到电场 $\mathbf{E}$ 中时，会发生什么？你可能会下意识认为无事发生。但由于原子中存在电子以及带正电的原子核，它们受到电场影响会以相反的方向运动：电子逆着电场方向，而原子核顺着电场方向。电场足够强的情况下，一些电子会完全脱离原子核，使其成为离子；其余的时候，电子和原子核会达到一个微妙的平衡：电场 $\mathbf{E}$ 虽然让它们相互分开，但是它们之间的电荷力会互相吸引。此时我们称这个原子被 **极化（Polarize）** 了。回忆我们上一章介绍的概念，这就形成了一个偶极矩，其方向和 $\mathbf{E}$ 一致。这个偶极矩在电场不足以 **离子化（Ionize）** 原子前，和电场强度成正比：
$$
\begin{equation*}
	\mathbf{p} = \alpha \mathbf{E}
\end{equation*} \tag{4.1} \label{polarizability}
$$
其中常数 $\alpha$ 被称为 **原子极化度（Atomic Polarizability）**，它和原子类型相关。下表给出了一些常见的原子的极化度：
$$
\fbox{
$\begin{array}{cccccccccc}
\text{H} & \text{He} & \text{Li} & \text{Be} & \text{C} & \text{Ne} & \text{Na} & \text{Ar} & \text{K} & \text{Cs} \\
0.667 & 0.205 & 24.3 & 5.60 & 1.67 & 0.396 & 24.1 & 1.64 & 43.4 & 59.4
\end{array}
$
}
$$
如果你对化学有一定了解，可以同时联系这些原子的化学性质。

对于分子，极化就变得复杂了。分子中的每个原子都会发生极化且在极化程度和方向相关。以 $\text{CO}_2$ 为例，如果沿着它的轴施加电场，极化度就是 $4.5\times 10^{-40} \,\text{C}^2\cdot\text{m}/\text{N}$。而垂直于这个方向的电场则会使其拥有 $2.0\times 10^{-40}\,\text{C}^2\cdot\text{m}/\text{N}$ 的极化度。对于其它方向的极化度可以通过叠加原理得到：
$$
\mathbf{p} = \alpha_{\perp}\mathbf{E}_\perp + \alpha_{\parallel}\mathbf{E}_\parallel \nonumber
$$
$\text{CO}_2$ 的分子模型还算简单（因为它的三个原子成一条直线），如果是最通用的情况，就会形成下面这样的 **极化度张量（Polarizabiliy Tensor）**：
$$
\begin{cases}
	p_x = \alpha_{xx}E_x + \alpha_{xy}E_y + \alpha_{xz}E_z \\
	p_y = \alpha_{yx}E_x + \alpha_{yy}E_y + \alpha_{yz}E_z \\
	p_z = \alpha_{zx}E_x + \alpha_{zy}E_y + \alpha_{zz}E_z
\end{cases} \tag{4.2}
$$

#### 极化分子的对齐

现在考虑一个没有那么对称的情况，水分子 $\text{H}_2\text{O}$。它的结构大致如下：

<img src="graphs/ed1_4-1.png" alt="ed1_4-1" style="zoom:50%;" />

我们不给出具体的计算过程，但水分子的偶极矩大概为 $6.1\times 10^{-30}\ \text{C}\cdot\text{m}$，这是相当大的，也因此使水成为优秀的溶剂。另一方面，由于水分子在图示的方向上并不对称，在匀强电场下会产生一个力矩。将两个氢原子的质心与氧原子质心连线的中点视作原点，可以列出下面的等式：
$$
\begin{align}
\begin{split}
\mathbf{N} &= (\mathbf{r}_+\times\mathbf{F}_+) + (\mathbf{r}_-\times\mathbf{F}_-) \\
&= \frac{1}{2}\mathbf{d}\times q\mathbf{E} + (-\frac{1}{2}\mathbf{d})\times (-q\mathbf{E}) \\
&= q\mathbf{d}\times\mathbf{E}
\end{split}
\end{align} \tag{4.3}
$$
结合物理偶极子偶极矩的计算式 $(\ref{physical-dipole})$，我们有：
$$
\begin{equation*}
	\mathbf{N} = \mathbf{p}\times\mathbf{E}
\end{equation*} \tag{4.4} \label{dipole-torque}
$$
当电场不是匀强的时候，偶极子会受到一个非零的合外力 $\mathbf{F} = q\Delta \mathbf{E}$。 $\Delta \mathbf{E} = (\nabla E_x)\cdot\mathbf{d} + (\nabla E_y)\cdot\mathbf{d} + (\nabla E_z)\cdot\mathbf{d}$，故 $\mathbf{F} = (\nabla E_x)\cdot\mathbf{p} + (\nabla E_y)\cdot\mathbf{p} + (\nabla E_z)\cdot \mathbf{p} = \nabla(\mathbf{E}\cdot\mathbf{p})$。为了防止造成 $\mathbf{p}$ 也要参与微分的错觉，我们通常使用下面的等价形式：
$$
\begin{equation*}
	\mathbf{F} = (\mathbf{p}\cdot\nabla)\mathbf{E}
\end{equation*} \tag{4.5} \label{force-on-electric-dipole}
$$
此时，如果依然将偶极子的质心作为原点，我们得到的力矩依然是 $(\ref{dipole-torque})$ 这个式子；如果以其它点为原点，就要考虑合外力，最终的力矩是 $\mathbf{N} = (\mathbf{p}\times \mathbf{E}) + (\mathbf{r}\times\mathbf{F})$。

综上，我们考察了原子或分子在电场下产生的极化现象。无论是原子的“趋离子化”，还是分子受到的力矩，都让它们朝着某个方向进行变化。我们用 **极化强度（Polarization）** 来量化这个变化，它表示单位体积内偶极矩的大小，记作 $\mathbf{P}$。随后我们将利用这个物理量研究一个极化物体内的电场，

#### 束缚电荷

根据电偶极子产生电势的公式 $(\ref{electric-dipole-potential-alt})$ 和极化强度定义，我们可以给出极化物体产生的电势：
$$
\begin{equation*}
	V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\int_\mathcal{V}\frac{\mathbf{P}(\mathbf{r}')\cdot\hat{\boldsymbol{\mathscr{r}}}}{\mathscr{r}^2}\,d\tau'
\end{equation*} \tag{4.6}
$$
注意到 $\dfrac{\hat{\boldsymbol{\mathscr{r}}}}{\mathscr{r}^2} =\nabla'\left(\dfrac{1}{\mathscr{r}}\right)$，其中 $\nabla'$ 指的是对源坐标求梯度，因此上面的式子可以写成：
$$
V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\int_\mathcal{V}\mathbf{P}(\mathbf{r}')\nabla'\left(\frac{1}{r}\right)\,d\tau'
$$
利用分部积分法和散度定理，即公式 $(\ref{integrate-by-parts-divergence})$，我们可以得到：
$$
\begin{equation*}
	V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\oint_\mathcal{S}\frac{1}{\mathscr{r}}\mathbf{P}(\mathbf{r}')\cdot\,d\mathbf{a}' - \frac{1}{4\pi\epsilon_0}\int_\mathcal{V}\frac{1}{\mathscr{r}}(\nabla'\cdot\mathbf{P}(\mathbf{r}'))\,d\tau
\end{equation*} \tag{4.7}
$$
可以看到左侧比较像一个电荷分布的面积分，右侧则比较像一个体积分。对比电势的计算公式 $(\ref{electric-potential-integral})$，我们设出两个虚拟的电荷密度：
$$
\begin{align}
\begin{split}
	\sigma_b &= \mathbf{P}\cdot\hat{\mathbf{n}} \\
	\rho_b &= -\nabla\cdot\mathbf{P}
\end{split}
\end{align} \tag{4.8} \label{bound-charge-density}
$$
此时我们可以将公式写为更加简洁的形式：
$$
\begin{equation*}
	V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\oint_\mathcal{S}\frac{1}{\mathscr{r}}\sigma_b\,da' + \frac{1}{4\pi\epsilon_0}\int_\mathcal{V}\frac{1}{\mathscr{r}}\rho_b\,d\tau'
\end{equation*} \tag{4.9} \label{electric-potential-of-bound-charge}
$$
这个结论告诉我们，一个极化物体的电势分布等价于一个拥有特定电荷密度的面积分和特定电荷密度的体积分之和。我们将这些“虚拟”的电荷称为 **束缚电荷（Bound Charge）**。不过事实上，这些电荷并非完全抽象；它们就来自于极化物体中的一个个极化分子。让我们假设一个特殊的情形：一系列偶极子首尾相连在一条直线上，此时中间的所有电荷可以认为都互相抵消，只剩下首尾两个电荷。因此这个奇怪的结构等价于其仅在头部有一个正电荷而结尾有一个负电荷，如下图所示：

<img src="graphs/ed1_4-2.png" alt="ed1_4-2" style="zoom:50%;" />

我们将这样的两个电荷称为束缚电荷，因为它们距离相对较远，不能相互抵消。受这个结构启发，我们可以考察一个平行于 $\mathbf{P}$ 的管道（对于任一个极化物体，我们都能将其分为许多个这样的管道）。对其中的一个横截面为 $A$，长度为 $d \to 0$ 的小块（体积就是 $Ad$），它的偶极矩就是 $P(Ad)$。再根据物理偶极矩的公式 $(\ref{physical-dipole})$，有 $q = PA$。由于中间的电荷都互相抵消了，最后剩下的就是管道最外部的面上的电荷。考虑到这个面可能不垂直于 $\mathbf{P}$，我们需要乘上 $\cos\theta$ 作为参数，也即 $q = PA_\text{end}\cos\theta = \mathbf{P}\cdot\hat{\mathbf{n}}A$。所以单位面积上的电荷就是 $\sigma_b = \mathbf{P}\cdot\hat{\mathbf{n}}$。示意图如下：

<img src="graphs/ed1_4-3.png" alt="ed1_4-3" style="zoom:40%;" />

当电场并非是匀强的时候，$\mathbf{P}$ 会产生非零的散度。此时任取的一个体积 $\mathcal{V}$ 内的净电荷等于被“推出”其边界 $\mathcal{S}$ 的电荷总量取负（回忆前面我们得到的 $q = PA$），因此我们可以列出：
$$
\int_\mathcal{V}\rho_b\,d\tau = -\oint_\mathcal{S}\mathbf{P}\cdot d\mathbf{a}
$$
再根据散度定理 $(\ref{divergence-theorem})$，我们得到：
$$
\int_\mathcal{V}\rho_b\,d\tau = -\int_\mathcal{V}(\nabla\cdot\mathbf{P})\,d\tau
$$
由于 $\mathcal{V}$ 是任取的，我们可以将积分符号摘掉，这样也就得到了 $\rho_b = -\nabla\cdot\mathbf{P}$。

> **例**：求一个匀强电场下被极化的球产生的电势。

> **解**：不失一般性，假设极化强度 $\mathbf{P} = P\hat{\mathbf{z}}$。此时束缚电荷的面密度是 $\sigma = \mathbf{P}\cdot\hat{\mathbf{n}} = P\cos\theta$。利用之前（上一章）我们的结论，有：
> $$
> V(r, \theta) = \begin{cases}
> 	\dfrac{P}{3\epsilon_0}r\cos\theta\quad\quad \text{for $r \le R$} \\
> 	\dfrac{P}{3\epsilon_0}\dfrac{R^3}{r^2}\cos\theta\quad\, \text{for $r \ge R$}
> \end{cases}\nonumber
> $$
> 此时在球内的电场是：
> $$
> \mathbf{E} = -\nabla V = -\frac{P}{3\epsilon_0}\hat{\mathbf{z}} = -\frac{1}{3\epsilon_0}\mathbf{P}
> $$
> 可以看到这是一个匀强的电场。另一方面，在球外，产生的电势等价于球心的一个完美偶极子产生的电势，即：
> $$
> V = \frac{1}{4\pi\epsilon_0}\frac{\mathbf{p}\cdot\hat{\mathbf{r}}}{r^2}
> $$
> 这里的 $\mathbf{p}$ 等于整个球体产生的偶极矩：
> $$
> \mathbf{p} = \frac{4}{3}\pi R^3\mathbf{P}
> $$
> 下面是一个示意图。可以参考上一章结尾给出的图：
>
> <img src="graphs/ed1_4-4.png" alt="ed1_4-4" style="zoom:30%;" />

#### 电介质中的电场

上一节的一些论述似乎有值得思考的问题。对 $(4.6)$ 式的使用暗示我们将极化物体按照多个完美偶极子理解（实际上并非如此），且 $\mathbf{P}$ 作为一个（连续的）密度函数用于表示离散的分子形成的偶极子是否有欠考虑？在物体以外，我们通常可以接受这样的差异（因为 $\rcur$ 比正负电子对的间距 $d$ 大多了，此时物理偶极子 *约等于* 完美偶极子），但在物体内（恰巧也是我们上一节中积分的范围），其适用性存疑，本节将对此进行分析。

物质中微观尺度下的电场是异常复杂的。因为在靠近电荷的地方，电场会变得极为庞大，然而稍微偏离（并靠近另一个电荷）时，电场的方向会发生大幅变化。更不要说考虑电荷的移动时，下一秒的电场分布就截然不同。因此，我们对微观下的电场兴趣不大（事实上也难以计算），而更注重（相对）宏观尺度的电场。比如，我们会将水是做一个连续流体（而忽略其分子结构），此时无需考虑水分子中电子的运动和相互作用。此时，我们通常会将某一点的场定义为一个包含该点的足够大的微小空间（比如几千个分子）中场的平均值。这样既能反应物体局部的特征，也能避免某点的突变影响数据。因此，当我们描述“物质中的电场”时，指的是宏观意义上的电场。类似的概念我们在物理中遇到过一些了，比如密度（我们求的当然不是原子的密度，更不是原子间“真空”的密度，而是一系列分子组成的物质质量比上它们占的体积）。

现在让我们重新回顾一个极化物体中的电场。让我们专注于一个半径为 $R$ 的球体。它受到的电场可以被分为两个部分，即外部的偶极矩产生的电场和内部的偶极矩产生的电场：
$$
\mathbf{E} = \mathbf{E}_\text{out} + \mathbf{E}_\text{in}
$$
对一个球体的平均电场等于该球体在球心处受到的电场，因此：
$$
\begin{equation*}
	V_\text{out} = \frac{1}{4\pi\epsilon_0}\underset{\text{outside}}{\int}\frac{\mathbf{P}(\mathbf{r}')\cdot\unit{\rcur}}{\rcur^2}\,d\tau'
\end{equation*}
$$
这里使用该公式并无问题，因为我们可以假设球以外的电偶极矩和球心距离足够远。另一边，内部的偶极矩产生的电场可以通过球平均场得到：
$$
\mathbf{E}_\text{in} = -\frac{1}{4\pi\epsilon_0}\frac{\mathbf{p}}{R^3}
$$
我们再将 $\mathbf{p} = \mathbf{P}\int\,d\tau'$ 代回上式，就得到：
$$
\begin{equation*}
	\mathbf{E}_\text{in} = -\frac{1}{3\epsilon_0}\mathbf{P}
\end{equation*} \tag{4.10} \label{electric-field-inside-dielectric-sphere}
$$
由于我们选取的 $R$ 足够小，因此 $\mathbf{P}$ 相比其体积分小很多。因此我们最终得到的就是上一节的公式。这里有关电介质球体内部电场的结论，即 $(\ref{electric-field-inside-dielectric-sphere})$ 也颇有意思：无论实际上极化分布有多复杂，其在一个球体中的平均值等价于均匀分布的极化强度。

### 电位移

#### 电介质中的高斯定律

如果我们综合考虑一个电介质中的电荷，它可能包括束缚电荷和之前我们已经熟悉的 **自由电荷（Free Charge）**：
$$
\begin{equation*}
	\rho = \rho_b + \rho_f
\end{equation*} \tag{4.10}
$$
根据高斯定律和束缚电荷密度的计算公式，我们有：
$$
\epsilon_0\nabla\cdot\mathbf{E} = -\nabla\cdot\mathbf{P} + \rho_f
$$
将 del 算子移到同一边，我们可以得到：
$$
\nabla\cdot(\epsilon_0\mathbf{E} + \mathbf{P}) = \rho_f
$$
这样就可以引入 **电位移（Electric Displacement）**的概念了：
$$
\begin{equation*}
	\marginbox{\mathbf{D} \equiv \epsilon_0\mathbf{E} + \mathbf{P}}
\end{equation*} \tag{4.11} \label{electric-displacement}
$$
之前的式子也可以写为高斯定律的微分形式：
$$
\begin{equation*}
	\nabla\cdot \mathbf{D} = \rho_f
\end{equation*} \tag{4.12} \label{divergence-of-D}
$$
其对应的积分形式便是：
$$
\begin{equation*}
	\oint\mathbf{D}\cdot\,d\mathbf{a} = Q_{f_\text{enc}}
\end{equation*} \tag{4.13} \label{gauss's-law-of-D}
$$
虽然电位移 $\mathbf{D}$ 和电场强度 $\mathbf{E}$ 从公式上来看非常相似，它们多数情况下有完全不同的性质。首先，不存在关于电位移的库仑定律 $(\ref{coulomb's-law})$，即电位移不能通过对距离平方反比求体积分得到。此外，它也不存在与电势对等的概念，这是因为电位移的旋度不恒等于 $\mathbf{0}$：
$$
\begin{equation*}
	\nabla\times\mathbf{D} = \epsilon_0(\nabla\times\mathbf{E}) + (\nabla\times\mathbf{P}) = \nabla\times\mathbf{P}
\end{equation*} \tag{4.14}
$$
因此通常情况下，它不是任何物理量的梯度，因此也就不存在类似于电势的概念了。

#### 边界条件

我们可以轻松地得到电介质存在时的边界条件：
$$
\begin{equation*}
	D_\text{above}^\perp - D_\text{below}^\perp = \sigma_f
\end{equation*} \tag{4.15}
$$
由电位移的旋度等于极化强度的旋度可得：
$$
\begin{equation*}
	\mathbf{D}_\text{above}^\parallel - \mathbf{D}_\text{below}^\parallel = \mathbf{P}_\text{above}^\parallel - \mathbf{P}_\text{below}^\parallel
\end{equation*} \tag{4.16}
$$

### 线性电介质

#### 电极化率与介电率

让我们回顾 $(\ref{polarizability})$，探究极化产生的原因。大多数情况下，我们可以将这个式子写成：
$$
\begin{equation*}
	\mathbf{P} = \epsilon_0\chi_e\mathbf{E}
\end{equation*} \tag{4.17} \label{electric-susceptibility}
$$
其中 $\chi_e$ 是介质的 **电极化率（Electric Susceptibility）**，它取决于物质的微观结构，以及温度等环境因素。 $\mathbf{E}$ 是考虑了极化产生的电场之后的总电场强度。我们将所有满足 $(\ref{electric-susceptibility})$ 的物质称为 **线性电介质（Linear Dielectric）**。此时我们可以进一步得到电位移和总电场强度的关系：
$$
\begin{equation*}
	\mathbf{D} = \epsilon_0E + \mathbf{P} = \epsilon_0(1 + \chi_e)\mathbf{E} = \epsilon\mathbf{E}
\end{equation*} \tag{4.18} \label{E-and-D}
$$
其中将 $\epsilon_0(1 + \chi_e)$ 记为 $\epsilon$，称为 **介电率（Permittivity）**。和 $\epsilon_0$ 对比就能知道真空中 $\chi_0 = 0$，这很符合直觉。我们也将物质的介电率和真空介电率的比值记作 $\epsilon_r$，称为 **相对介电率（Relative Permittivity）**，或 **电介质常数（Dielectric Constant）**。下面是一些常见物质的电介质常数：

| 物质     | 电介质常数 | 物质                 | 电介质常数 | 物质             | 电介质常数 |
| :------- | :--------- | -------------------- | ---------- | ---------------- | ---------- |
| 真空     | 1          | 氦气                 | 1.000065   | 氢气             | 1.000254   |
| 干燥空气 | 1.000536   | 水蒸气（100 摄氏度） | 1.00589    | 苯               | 2.28       |
| 钻石     | 5.7-5.9    | 盐                   | 5.9        | 硅               | 11.7       |
| 甲醇     | 33.0       | 水                   | 80.1       | 冰（-30 摄氏度） | 104        |
| 钽铌酸钾 | 34000      |                      |            |                  |            |

由于电位移现在和电场强度成正比，我们可以做出合理猜测：此时电位移的旋度为 $\mathbf{0}$。可惜这是错误的结论。在不同电介质的边界处，$\mathbf{P}$ 跨越两种电介质的环路积分并不一定为 $0$，因此其旋度，也即 $\mathbf{D}$ 的旋度非零。不过，如果整个空间都由一种 *同质* 的（即介电率为常数）线性电介质充满，则我们确实可以认为 $\mathbf{D}$ 是无旋的。此时总有：
$$
\begin{equation*}
	\mathbf{E} = \frac{1}{\epsilon}\mathbf{D} = \frac{1}{\epsilon_r}\mathbf{E}_\text{vac}
\end{equation*} \tag{4.19}
$$
其中 $\mathbf{E}_\text{vac}$ 是假设电介质为真空时的电场强度。这意味着我们可以通过库仑定律来计算电位移，以及电介质中的电场强度。比如假设一个自由电荷 $q$ 嵌入了一个巨大的电介质中，此时它产生的电场强度是：
$$
\begin{equation*}
	\mathbf{E} = \frac{1}{4\pi \epsilon}\frac{q}{r^2}\unit{r}
\end{equation*} \tag{4.20}
$$
这个式子展示了一个有趣的现象，即这个电荷四周的电荷受到的力要小于在真空中所受到的（因为 $\epsilon > \epsilon_0$）。这是因为电介质极化产生的束缚电荷在该电荷周围部分抵消了其电荷量。

#### 边界条件

在同质的线性电介质中，束缚电荷体密度和自由电荷体密度总是成正比的：
$$
\begin{equation*}
	\rho_b = -\nabla\cdot\mathbf{P} = -\nabla\cdot\left(\epsilon_0\frac{\chi_e}{\epsilon}\mathbf{D}\right) = -\left(\frac{\chi_e}{1 + \chi_e}\right)\rho_f
\end{equation*} \tag{4.21}
$$
特别地，如果自由电荷不在电介质内，那么总有 $\rho = 0$，所有的净电荷一定在电介质表面。此时我们可以继续使用拉普拉斯方程。需要满足的边界条件有：
$$
\begin{equation*}
	\epsilon_\text{above}E_\text{above}^\perp - \epsilon_\text{below}E_\text{below}^\perp = \sigma_f
\end{equation*} \tag{4.22}
$$
或者等价的：
$$
\begin{equation*}
	\epsilon_\text{above}\frac{\partial V_\text{above}}{\partial n} - \epsilon_\text{below}\frac{\partial V_\text{below}}{\partial n} = \sigma_f
\end{equation*} \tag{4.23}
$$


同时，电势应该是连续的：
$$
\begin{equation*}
	V_\text{above} = V_\text{below}
\end{equation*} \tag{4.24}
$$

#### 电介质系统中的能量

我们此前已经知道电容中的能量公式 $(\ref{energy-of-capacitor})$。现在考虑被线性电介质包围的电容，其电容量为：
$$
\begin{equation*}
	C = \epsilon_r C_\text{vac}
\end{equation*} \tag{4.25} \label{capacitance-in-dielectric}
$$
此结论从电容和电场成正比的性质不难得到。显然我们对电容充电所需的能量变多了。下面我们尝试推导该能量用电场表示的形式。首先，假设电介质从始至终不移动。当我们加入自由电荷，使电荷密度从 $\rho_f$ 增加了 $\Delta \rho_f$ 时，我们有：
$$
\begin{align*}
	\Delta W = \int (\Delta \rho_f) V\,d\tau &= \int\nabla\cdot(\Delta \mathbf{D})V\,d\tau \\
	&= \int\nabla\cdot[(\Delta\mathbf{D})V]\,d\tau + \int(\Delta\mathbf{D})\cdot \mathbf{E}\,d\tau \\
	&= \int_\mathcal{S}V(\Delta \mathbf{D})\cdot d\mathbf{a} + \frac{1}{2}\int_\mathcal{V}\Delta (\mathbf{D}\cdot\mathbf{E})\,d\tau
\end{align*}
$$
当我们令 $\mathcal{S}, \mathcal{V} \to \infty$ 时，第一项消失了，而第二项变为：
$$
\begin{equation*}
	W = \frac{1}{2}\int\mathbf{D}\cdot\mathbf{E}\,d\tau
\end{equation*} \tag{4.26} \label{energy-in-dielectric}
$$
我们不难发现它和此前得到的 $(\ref{electric-energy-all-space})$ 的区别，对于线性电介质而言在于一个系数 $\epsilon_r$。从物理意义上来讲，$(\ref{energy-in-dielectric})$ 是将自由电荷放置为特定分布，使得电介质得到相应电场时需要的能量；$(\ref{electric-energy-all-space})$ 则是同时考虑自由电荷和束缚电荷，将其放置为特定分布时需要的能量（换言之，其完全忽略了电介质的存在，而只考虑电荷分布。此时电介质中束缚电荷间的能量（类似于弹力）就会被忽略），因此通常我们不会这样计算)。

此外，$(\ref{energy-in-dielectric})$ 通常只适用于线性电介质。对于一些带有损耗的系统，构建电场的能量可以是任意大的（因为此时功和路径有关）。

#### 电介质的受力

前面章节中介绍[导体](#导体)时曾计算过导体所受的静电力。电介质虽然没法像导体那样强烈地反应，但是产生的束缚电荷和导体中的感应电荷是类似的。还是假设线性电介质添入平行板电容器的情形，此时在 *绝大多数* 地方，电场都是匀强的。此时由于电介质两边带的电荷相反且数量相等，似乎受到的合力为零才对。但是实际上要复杂一些。在平行板的边缘处存在 **边缘场（Fringing Field）**，如下图所示：

![ed1_4-5](graphs/ed1_4-5.png)

在研究电容的大多数情况下我们都会将其忽略，但此时它是产生作用的唯一来源。这个场的计算颇为复杂，因此此处我们用其它方式来求电介质的手里。考虑我们施加力 $F_\text{pull}$ 将电介质从电容板中抽出，则有：
$$
dW = F_\text{pull}\,dx \implies F = -F_\text{pull} = -\frac{dW}{dx}
$$
如果电容的电荷为 $Q$ 且保持不变，则电容的总能量为：
$$
W = \frac{1}{2}CV^2 = \frac{1}{2}\frac{Q^2}{C}
$$
假设电介质抽出的方向 $x$ 上电容板的长度为 $l$，电容板间距为 $d$，则有：
$$
C = \frac{\epsilon_0w}{d}(\epsilon_rl - \chi_ex) \tag{4.27}
$$
代入前面的式子，有：
$$
\begin{align*}
	F = -\frac{dW}{dx} &= \frac{1}{2}\frac{Q^2}{C^2}\frac{dC}{dx} \\
	&= \frac{1}{2}V^2\frac{dC}{dx} \\
	&= -\frac{\epsilon_0\chi_ew}{2d}V^2 \tag{4.28}
\end{align*} 
$$
这里需要额外注意的是，计算电容能量时使用的是 $\frac{Q^2}{2C}$ 而非 $\frac{1}{2}CV^2$。前者将 $Q$ 视为常数而后者将 $V$ 视为常数，两者微分得到的结果是不一样的。这是因为 $Q$ 可以很自然地保持不变，而 $V$ 保持不变则需要将电容接通电源，此时电源会对系统做功，因此算出来的是错误的结果。改正后的算式应该是：
$$
\begin{align*}
	F = -\frac{dW}{dx} + V\frac{dQ}{dx} = &-\frac{1}{2}V^2\frac{dC}{dx} + V^2\frac{dC}{dx} \\
	&= \frac{1}{2}V^2\frac{dC}{dx} \\
	&= -\frac{\epsilon_0\chi_ew}{2d}V^2
\end{align*}
$$

这和之前的结论是吻合的。

## 静磁场概览

让我们回忆一下电动力学研究的问题，即一个测试电荷 $Q$ 在一系列源电荷 $q_i$ 的作用下的运动规律。在前面几章静电场的学习中，我们大概了解了源电荷均为静止时，其激发的场的性质。接下来的两章中，我们将探索特定分布的 $q_i$ 在匀速直线运动状态下， $Q$ 收到的影响，而这就是 **静磁场（Magnetostatics）** 主要研究的内容。

### 洛伦兹力

#### 磁场力和磁场

通过实验发现，两端通电导线在电流方向相同时会相吸，反之则相斥，究竟是什么导致这种现象呢？如果将测试电荷静止放在通电导线附近，则不会发生任何现象。这说明这种未知的相互作用只发生在都进行相对运动的电荷之间，我们称其为 **磁场力（Magnetic Force）**。正如静止电荷四周会分布电场 $\mathbf{E}$，运动的电荷四周会分布磁场 $\mathbf{B}$。分布情况如下图所示：

​                 <img src="graphs/ed1_5-1.png" alt="ed1_5-1" style="zoom:50%;" />              <img src="graphs/ed1_5-2.png" alt="ed1_5-2" style="zoom:50%;" />

右图清晰地解释了两条通电导线磁力的示意图。需要注意 **磁场强度（Magnetic Field）** 是为了描述矢量 $\mathbf{v}$ 和 $\mathbf{F}$ 而“捏造“出来的矢量。事实上，它是一个 **伪矢量（Pseudovector）**，其表现为，将其它设定进行镜像变换后，$\mathbf{B}$ 并不一定是原来的镜像变换，如下图所示：

<img src="graphs/ed1_5-3.png" alt="ed1_5-3" style="zoom:10%;" />

**洛伦兹力定律（Lorentz Force Law）** 声称，速度为 $\mathbf{v}$ 的测试电荷 $Q$ 受到的磁场力和 $Q$、$\mathbf{v}$ 以及磁场强度 $\mathbf{B}$ 成正比，方向和 $\mathbf{v}\times\mathbf{B}$ 相同：
$$
\begin{equation*}
	\marginbox{\mathbf{F} = Q(\mathbf{v}\times \mathbf{B})}
\end{equation*} \tag{5.1} \label{lorentz-force-law}
$$
这可以认为是静磁学的一个公理（类似于库仑定律），我们无需证明。如果同时考虑电场 $\mathbf{E}$，上式就变为：
$$
\begin{equation*}
	\mathbf{F} = Q(\mathbf{E} + \mathbf{v}\times\mathbf{B})
\end{equation*} \tag{5.2} \label{electromagnetic-force}
$$
我们接下来的任务就是研究如何求得其中的 $\mathbf{E}$ 和 $\mathbf{B}$。

> **例**：下面我们以 **回旋加速器（Cyclotron）** 为例，展示电磁场同时存在的情形。其中的带电粒子会进行螺旋前进的运动，其中向心力由磁场提供。示意图如下：
>
> <img src="graphs/ed1_5-4.png" alt="ed1_5-4" style="zoom:50%;" />
>
> 假设磁场强度为 $\mathbf{B}$, 带电粒子电荷为 $Q$, 回旋加速器的半径是 $R$，粒子的速度与磁场垂直的分量为 $v$ （注意到平行方向的速度并不会影响磁场力），求粒子的动量。

> **解**：这个本质就是一个向心问题：
> $$
> \begin{align*}
> 	QvB = m\frac{v^2}{R} \implies p = QBR
> \end{align*}
> $$

> **例**：**摆线运动（Cycloid Motion）**，考虑两个相互垂直的电场 $\mathbf{E} = E\hat{\mathbf{z}}$ 和磁场 $\mathbf{B} = B\hat{\mathbf{x}}$，以及一个初始静止在原点的正电荷。求它的运动轨迹。

> **解**：该电荷只会受到 $y, z$ 方向的力，可以列出下面的式子：
> $$
> \mathbf{v}\times\mathbf{B} = 
> \begin{vmatrix}
> 	\hat{\mathbf{x}} & \hat{\mathbf{y}} & \hat{\mathbf{z}} \\
> 	0 & \dot{x} & \dot{z} \\
> 	B & 0 & 0
> \end{vmatrix} 
> = B\dot{z}\hat{\mathbf{y}} - B\dot{y}\hat{\mathbf{z}}
> \nonumber
> $$
> 将电场的影响考虑上去，则有：
> $$
> \begin{align*}
>     \mathbf{F} &= Q(\mathbf{E} + \mathbf{v} \times \mathbf{B}) = Q((E - B\dot{y})\hat{\mathbf{z}} - B\dot{y}\hat{\mathbf{z}}) \\
>     &= m\mathbf{a} = m(\ddot{y}\hat{\mathbf{y}} + \ddot{z}\hat{\mathbf{z}})
> \end{align*}
> $$
> 这样就列出了运动方程：
> $$
> \begin{align*}
> 	QB\dot{z} = m\ddot{y} && Q(E - B\dot{y}) = m\ddot{z}
> \end{align*}
> $$
> 设 **摆线频率（Cyclotron Frequency）** 为：
> $$
> \omega = \frac{QB}{m}
> $$
> 我们就得到了简化的运动方程：
> $$
> \begin{align*}
> 	\ddot{y} = \omega\dot{z} && \ddot{z} = \omega\left(\frac{E}{B} - \dot{y}\right)
> \end{align*}
> $$
> 这个方程的通解是：
> $$
> \begin{cases}
> 	y(t) = C_1\cos{\omega t} + C_2\sin{\omega t} + \dfrac{E}{B}t + C_3 \\
> 	z(t) = C_2\cos{\omega t} - C_2\sin{\omega t} + C_4
> \end{cases}
> $$
> 根据边界条件 $y(0) = z(0) = 0, \dot{y}(0) = \dot{z}(0) = 0$，我们最终得到：
> $$
> \begin{cases}
> 	y(t) = \dfrac{E}{\omega B}(\omega t - \sin{\omega t}) \\
> 	z(t) = \dfrac{E}{\omega B}(1 - \cos{\omega t})
> \end{cases}
> $$
> 为了更好理解这个答案，令：
> $$
> R = \frac{E}{\omega B}
> $$
> 则下面的等式成立：
> $$
> (y - R\omega t)^2 + (z - R)^2 = R^2
> $$
> 因此它的轨迹是一个以 $(0, R\omega t, R)$ 为圆心，半径为 $R$ 的圆。由于 $y = R\omega t = \dfrac{E}{B}t$ 随时间变化而移动，因此可以理解为这个圆沿着 $\hat{\mathbf{y}}$ 以 $\dfrac{E}{B}$ 的速度滚动。示意图如下：
>
> <img src="graphs/ed1_5-5.png" alt="ed1_5-4" style="zoom:50%;" />

#### 电流

**电流（Current）** 是指单位时间内通过某点的电荷数量，单位是 **安培（Amperes, $\text{A}$）**，满足 $1\ \text{A} = 1\ \text{C}/\text{s}$。需要注意的是，电流的方向和电荷类型有关；负电荷（比如电子）移动方向和电流方向是相反的。对于电荷线密度为 $\lambda$ 的电线，它的电流可以通过下式计算：
$$
\begin{equation*}
	\mathbf{I} = \lambda\mathbf{v}
\end{equation*} \tag{5.3} \label{line-current}
$$
我们可以由此推出洛伦兹力公式和电流的关系：
$$
\begin{align*}
	\mathbf{F} &= \int(\mathbf{v}\times\mathbf{B})\,dq = \int(\mathbf{v}\times\mathbf{B})\lambda\,dl = \int(\mathbf{I}\times\mathbf{B})\,dl \nonumber \\
&= \int I(d\mathbf{l}\times\mathbf{B}) \tag{5.4}
\end{align*}
$$
当电流从平面中通过时（如下图所示），我们可以类似地定义 **面电流密度（Surface Current Density）**，记为 $\mathbf{K}$：

<img src="graphs/ed1_5-6.png" alt="ed1_5-6" style="zoom:40%;" />

计算公式是：
$$
\begin{equation*}
	\mathbf{K} = \sigma\mathbf{v} = \frac{\mathbf{dI}}{dl_\perp}
\end{equation*} \tag{5.5} \label{surface-current-density}
$$
洛伦兹力为：
$$
\begin{equation*}
	\mathbf{F} = \int(\mathbf{K}\times\mathbf{B})\,da
\end{equation*} \tag{5.6}
$$
三维空间中，可以定义 **体电流密度（Volume Current Density）**，记为 $\mathbf{J}$，图示如下：

<img src="graphs/ed1_5-7.png" alt="ed1_5-7" style="zoom:50%;" />

类似地我们有：
$$
\begin{align*}
	\mathbf{J} &= \rho \mathbf{v} = \frac{d\mathbf{I}}{da_\perp} \tag{5.7} \label{volume-current-density} \\
	\mathbf{F} &= \int(\mathbf{J}\times\mathbf{B})\,d\tau \tag{5.8}
\end{align*}
$$
我们可以通过电流密度计算通过截面 $\mathcal{S}$ 的电流：
$$
\begin{equation*}
	I = \int_\mathcal{S}\mathbf{J}\cdot\,d\mathbf{a}
\end{equation*} \tag{5.9}
$$
根据散度定理我们有：
$$
\int_\mathcal{S}\mathbf{J}\cdot\,d\mathbf{a} = \int_\mathcal{V}(\nabla\cdot\mathbf{J})\,d\tau \nonumber
$$
这是单位时间内从体积 $\mathcal{V}$ 中 *离开* 的电荷量。它应该和单位时间进入 $\mathcal{V}$ 的电荷量相同，因此：
$$
\int_\mathcal{V}(\nabla\cdot\mathbf{J})\,d\tau = -\frac{d}{dt}\int_\mathcal{V}\rho\,d\tau = -\int_\mathcal{V}\frac{\partial \rho}{\partial t}\,d\tau \nonumber
$$
因此我们得到下面的重要结论：
$$
\begin{equation*}
	\marginbox{\nabla\cdot\mathbf{J} = -\frac{\partial \rho}{\partial t}}
\end{equation*} \tag{5.10} \label{continuity-equation}
$$
我们也将其称为 **连续性方程（Continuity Equation）**，其描述了局部电荷的守恒性。

最后让我们总结一下计算电流时几个等价的式子：
$$
\begin{equation*}
	\sum_i Nq_i\mathbf{v}_i = \int_\mathcal{L}N\mathbf{I}\,dl = \int_\mathcal{S}N\mathbf{K}\,da = \int_\mathcal{V}N\mathbf{J}\,d\tau
\end{equation*} \tag{5.11}
$$
其中 $N$ 是任意的函数。这个和我们之前讨论电荷分布时，$dq = \lambda\,dl = \sigma\,da = \rho\,d\tau$ 是类似的。

### 毕奥·萨伐尔定律

我们至今一直将 $\mathbf{B}$ 当作已知的量，然而它究竟是多大呢？我们暂时只关注静磁场中 $\mathbf{B}$ 的大小。我们感兴趣的是会产生静磁场 **恒定电流（Steady Current）**，即电流方向和大小均保持不变。使得静磁场成立的重要一点是导线的任一处的电荷密度变化率为 $0$：
$$
\begin{equation*}
	\frac{\partial \rho}{\partial t} = 0 \qquad \frac{\partial \mathbf{J}}{\partial t} = \mathbf{0}
\end{equation*} \tag{5.12}
$$
需要注意单独的电荷 $q$ 匀速直线运动时 *不会* 产生静磁场，因为每当它移动到另一点，都会改变它运动轨迹上的电荷分布。一个理想的产生静磁场的模型是通恒定电流的无限长直导线。导线中任意一处的电流都是常数 $I$。此外，根据连续性方程 $(\ref{continuity-equation})$，我们能知道静磁场中：
$$
\begin{equation*}
	\nabla \cdot \mathbf{J} = 0
\end{equation*} \tag{5.13}
$$
**毕奥·萨伐尔定律（Biot-Savart Law）** 定义静磁场的磁场强度 $\mathbf{B}$ 为：
$$
\begin{equation*}
	\mathbf{B}(\mathbf{r}) = \frac{\mu_0}{4\pi}\int\frac{\mathbf{I}\times\boldsymbol{\mathscr{r}}}{\mathscr{r}^2} = \frac{\mu_0I}{4\pi}\int\frac{d\mathbf{l}'\times\unit{\rcur}}{\rcur^2}
\end{equation*} \tag{5.14} \label{biot-savart-law}
$$
其中 $\mu_0$ 被称为 **真空磁导率（Permeability of Free Space）**，其值为精确的：
$$
\mu_0 = 4\pi\times 10^{-7}\ \text{N}/\text{A}^2
$$
这是因为这个值使用来定义电流单位安培的（真空中两条恒定电流通过的长直导线，其相互之间的磁场力就是 $1\times 10^{-7}\ \text{N}$），随后再用来定义电荷单位库仑，以及真空介电率。磁场强度本身的单位则是 **特斯拉（Tesla, $\text{T}$）**，其等价于 $\text{N}/(\text{A}\cdot\text{m})$。

> **例**：求距离通恒定电流的无限长直导线为 $s$ 处的磁场强度。图示如下：
>
> <img src="graphs/ed1_5-8.png" alt="ed1_5-8" style="zoom:50%;" />

> **解**：通过上图，我们发现 $d\mathbf{l}'\times\boldsymbol{\mathscr{r}}$ 方向指向纸页外，且满足 $dl'\sin\alpha = dl'\cos\theta$。由于 $l' = s\tan\theta$，我们有 $dl' = s\sec^2\theta\,d\theta$。另外，由 $\mathscr{r}\cos\theta = s$，有 $\dfrac{1}{\mathscr{r}^2} = \dfrac{\cos^2\theta}{s^2}$，此时我们就可以约化毕奥·萨伐尔公式了：
> $$
> \begin{align*}
> B &= \frac{\mu_0 I}{4\pi}\int_{\theta_1}^{\theta}\frac{\cos^2\theta}{s^2}s\sec^2\cos\theta\,d\theta \\
> &= \frac{\mu_0I}{4\pi s}\int_{\theta_1}^{\theta_2}\cos\theta\,d\theta \\
> &= \frac{\mu_0I}{4\pi s}(\sin\theta_2 - \sin\theta_1) \tag{5.15}
> \end{align*}
> $$
> 而这仅是一小段电线产生的磁场强度。当取无限长的导线段时，$\theta_1 = -\pi/2, \theta_2 = \pi/2$，我们得到：
> $$
> \begin{equation*}
> 	B = \frac{\mu_0I}{2\pi s}
> \end{equation*} \tag{5.16}
> $$
> 它的方向是 $\unit{\phi}$。

根据上面这个例子的结论，我们就可以解释本章开头，两条通电直导线相互作用的现象了。假设它们相距 $d$，这样一条导线对另一条的作用力就是（根据洛伦兹力公式）：
$$
F = I_2\left(\frac{\mu_0I_1}{2\pi s}\right)\int\,dl \nonumber
$$
对于一小段电线的作用力则是：
$$
\begin{equation*}
	f = \frac{\mu_0}{2\pi} \frac{I_1I_2}{d}
\end{equation*} \tag{5.17}
$$
我们可以将此和库仑定律进行对比，有高度的相似性。

### 静磁场的散度和旋度

根据我们前面对于无限长直导线产生的磁场强度的结论，对于一个半径 $s$ 的磁场环路积分是：
$$
\oint \mathbf{B}\cdot d\,\mathbf{l} = \oint\frac{\mu_0I}{2\pi s}\,dl = \mu_0I
$$
有意思地是，这个积分和 $s$ 无关。这个结论和电场 $\mathbf{E}$ 的通量类似。如果存在多段电线时，不难得到下面这个和高斯定律类似的结论：
$$
\oint\mathbf{B}\cdot\,d\mathbf{l} = \mu_0I_\text{enc}
$$
环路中包含的总电流 $I_\text{enc}$ 也可以通过电流密度得到：
$$
I_\text{enc} = \int\mathbf{J}\cdot\,d\mathbf{a}
$$
根据斯托克斯定理，我们有：
$$
\int(\nabla\times\mathbf{B})\cdot\,d\mathbf{a} = \oint\mathbf{B}\cdot\,d\mathbf{l} = \mu_0I_\text{enc} = \mu_0\int\mathbf{J}\cdot\,d\mathbf{a}\nonumber
$$
拆掉最两侧的积分号后，我们就得到了：
$$
\begin{equation*}
	\nabla\times\mathbf{B} = \mu_0\mathbf{J}
\end{equation*}
$$
简单回顾，我们对于无限长直导线的磁场推出了这样的结论，但实际上它对任意磁场都有效。下面我们将通过毕奥·萨伐尔定律再次得到这个结论。

#### 安培定律

首先我们考察磁场的散度：
$$
\begin{align*}
	\nabla\cdot\mathbf{B} &= \frac{\mu_0}{4\pi}\int\nabla\cdot\left(\mathbf{J}\times\frac{\unit{\mathscr{r}}}{\rcur^2}\right)\,d\tau' \\
	&= \frac{\mu_0}{4\pi}\int\frac{\unit{\mathscr{r}}}{\rcur^2}\cdot(\nabla\times\mathbf{J})\,d\tau' - \frac{\mu_0}{4\pi}\int\mathbf{J}\cdot\left(\nabla\times\frac{\unit{\mathscr{r}}}{\rcur^2}\right)\,d\tau'
\end{align*}
$$
上面第一项中 $\nabla\times\mathbf{J} = 0$，第二项中那个旋度为 $\mathbf{0}$。因此我们得到了：
$$
\begin{equation*}
	\marginbox{\nabla\cdot\mathbf{J} = 0}
\end{equation*} \tag{5.18} \label{divergence-of-magnetic-field}
$$
 这说明磁场的散度是 0。接下来考察磁场的旋度：
$$
\begin{align*}
\nabla\times\mathbf{B} &= \frac{\mu_0}{4\pi}\int\nabla\times\left(\mathbf{J}\times\frac{\unit{\mathscr{r}}}{\rcur^2}\right)\,d\tau' \\
&= \frac{\mu_0}{4\pi}\int\mathbf{J}\left(\nabla\cdot\frac{\unit{\rcur}}{\rcur^2}\right)\,d\tau' - \frac{\mu_0}{4\pi}\int(\mathbf{J}\cdot\nabla)\frac{\unit{\rcur}}{\rcur^2}\,d\tau
\end{align*}
$$
其中的第一项可以进一步化简：
$$
\begin{align*}
\frac{\mu_0}{4\pi}\int\mathbf{J}\left(\nabla\cdot\frac{\unit{\rcur}}{\rcur^2}\right)\,d\tau &=
\frac{\mu_0}{4\pi}\int\mathbf{J}(\mathbf{r'})4\pi\delta^3(\mathbf{r} - \mathbf{r}')\,d\tau' \\
&= \mu_0\mathbf{J}(\mathbf{r})
\end{align*}
$$
第二项在复杂的变形之后会变为 $0$，参考下面的过程。首先将 $\nabla$ 替换为 $\nabla'$：
$$
-(\mathbf{J}\cdot\nabla)\frac{\unit{\rcur}}{\rcur^2} = (\mathbf{J}\cdot\nabla')\frac{\unit{\rcur}}{\rcur^2}\nonumber
$$
对于直角坐标系中的一个坐标，比如 $x$，我们有：
$$
(\mathbf{J}\cdot\nabla')\frac{x - x'}{\rcur^3} = \nabla'\cdot\frac{x - x'}{\rcur^3}\mathbf{J} - \frac{x - x'}{\rcur^3}(\nabla'\cdot\mathbf{J})\nonumber
$$
静磁场中，电流密度的散度为 0，因此右式的第二项为为 0。第一项则可以根据散度定理得到：
$$
\int_\mathcal{V}\nabla'\cdot\frac{x - x'}{\rcur^3}\mathbf{J}\,d\tau' = \oint_\mathcal{S}\frac{x - x'}{\rcur^3}\mathbf{J}\cdot d\mathbf{a}'\nonumber = 0
$$
这样我们就得到了 **安培定律（Ampere's Law)**：
$$
\begin{equation*}
	\marginbox{\nabla\times\mathbf{B} = \mu_0 \mathbf{J}}
\end{equation*} \tag{5.19} \label{amperre's-law-derivative}
$$
它的积分形式我们之前在无限长直导线的例子中见到过了，也即：
$$
\begin{equation*}
	\marginbox{\oint\mathbf{B}\cdot d\mathbf{l} = \mu_0I_\text{enc}}
\end{equation*} \tag{5.20} \label{ampere's-law}
$$
这个定律对于所有的静磁场（恒定电流产生的磁场）都成立。

#### 和静电场的比较

现在我们已经得到了静电磁学最重要的几个公式，它们也是麦克斯韦方程的在一定局限下的形式：

- 静电学：
  - $\nabla\cdot\mathbf{E} = \dfrac{\rho}{\epsilon_0}$ （高斯定律）
  - $\nabla\times\mathbf{E} = \mathbf{0}$。静电场是无旋场。
  - $\mathbf{E} \to \mathbf{0}$，当场点距离场源很远时。
  - $\mathbf{E} = \dfrac{1}{4\pi\epsilon_0}\dfrac{\rho(\mathbf{r}')}{\rcur^2}\hat{\rcur}\,d\tau'$（库仑定律）
- 静磁学：
  - $\nabla\cdot\mathbf{B} = 0$。静磁场是无源场。
  - $\nabla\times\mathbf{E} = \mu_0\mathbf{J}$（安培定律）
  - $\mathbf{B} \to \mathbf{0}$，当场点距离场源很远时。
  - $\mathbf{B} = \dfrac{\mu_0}{4\pi}{\displaystyle\int}\dfrac{\mathbf{J}\times\unit{\rcur}}{\rcur^2}\,d\tau'$ （毕奥·萨伐尔定律）
- 电磁场力：
  - $\mathbf{F} = Q(\mathbf{E} + \mathbf{v}\times\mathbf{B})$（洛伦兹力）

电磁学研究的早期，物理学家倾向于认为磁力也源于类似于电荷的 **磁荷（Magnetic Charge）**，或 **磁单极子（Magnetic Monopole）**，并给出了和库仑定律极为相似的公式。当时是安培首先提出磁场源于运动的一系列电荷。事实上，直到现在都没有实验能证实磁单极子的存在，但一些基本的物理理论中着实需要它们。本篇中我们假定 *不存在* 磁单极子，也即所有磁场都是无源的，只有运动的电荷才能产生并受磁场影响。

现实中，磁场力相比电场力要小得多；通常只有在测试电荷接近光速的情况下两者才相当。不过，对于带有电流的电线来说，电场会因为导线整体呈中性（电流的四周由等量反向电荷组成），产生效果的主要是磁力。

### 磁矢势

正如我们通过静电场无旋引入了电势 $V$ 使得电场是它的负梯度，由静磁场无源我们也可以引入 **磁矢势（Magnetic Vector Potential）** 使得其旋度为磁场：
$$
\begin{equation*}
	\marginbox{\mathbf{B} = \nabla\times\mathbf{A}}
\end{equation*} \tag{5.21} \label{magnetic-vector-poential}
$$
将其代入安培定律：
$$
\nabla\times\mathbf{B} = \nabla\times(\nabla\times\mathbf{A}) = \nabla(\nabla\cdot\mathbf{A}) - \nabla^2\mathbf{A} = \mu_0\mathbf{J}
$$
此处我们断定：
$$
\begin{equation*}
	\marginbox{\nabla\cdot \mathbf{A} = 0}
\end{equation*} \tag{5.22} \label{divergence-of-magnetic-vector-potential}
$$
这是因为对于任意使得 $(\ref{magnetic-vector-poential})$ 成立的 $\mathbf{A}$，$\mathbf{A} + \nabla f$ 同样也能使其成立，其中 $f$ 是任意的标量场。假设 $\nabla\cdot\mathbf{A} \ne 0$，下面证明我们总能找到 $f$ 使得 $\mathbf{A}_0 = \mathbf{A} + \nabla f$ 是无源场，并让 $\mathbf{A}_0$ 成为我们想要的磁矢势：
$$
\begin{align*}
	&\nabla\cdot(\mathbf{A} + \nabla f) = \nabla\cdot\mathbf{A} + \nabla^2f = 0 \\
	\implies &\nabla^2 f = -\nabla\cdot\mathbf{A} \quad \text{（泊松方程）} \\
	\implies &f = \frac{1}{4\pi}\int\frac{\nabla\cdot\mathbf{A}}{\rcur}\,d\tau' \quad \text{（若在无穷远处 $\nabla\cdot\mathbf{A} \to 0$）}
\end{align*}
$$
上面给出了满足在无穷远处 $\nabla\cdot\mathbf{A} \to 0$ 的解；但即使是其它情况，我们也总能找到泊松方程的解。综上，我们可以认为 $(\ref{divergence-of-magnetic-vector-potential})$ 成立。因而，我们可以得到磁矢势的拉普拉斯：
$$
\begin{equation*}
	\marginbox{\nabla^2\mathbf{A} = \mu_0\mathbf{J}}
\end{equation*} \tag{5.23} \label{laplacian-of-magnetic-vector-potential}
$$
这仍然是一个泊松方程。如果在无穷远处 $\mathbf{J} \to \mathbf{0}$，我们可以得到下面的解：
$$
\begin{equation*}
	\mathbf{A}(\mathbf{r}) = \frac{\mu_0}{4\pi}\int\frac{\mathbf{J}(\mathbf{r}')}{\rcur}\,d\tau' = \frac{\mu_0I}{4\pi}\int\frac{1}{\rcur}\,d\mathbf{l}' = \frac{\mu_0}{4\pi}\int\frac{\mathbf{K}}{\rcur}\,da'
\end{equation*} \tag{5.24}
$$


#### 边界条件

现在考虑一个电流密度为 $\mathbf{K}$ 的通电平面附近的磁场分布。对于平面上下的垂直方向磁场 $B_\text{above}^\bot$ 和 $B_\text{below}^\bot$由于磁场在闭合曲面上的积分恒为 $0$，如果在曲面上构造一个极薄的小盒子，我们不难得到：
$$
\begin{equation*}
	B_\text{above}^\bot = B_\text{below}^\bot
\end{equation*} \tag{5.25}
$$
对于磁场的水平方向分量，则可以构造一个穿过曲面的环路，其方向和磁场方向平行并和曲面垂直。此时平面上下的磁场对其的环路积分为：
$$
\begin{align*}
	\oint\mathbf{B}\cdot d\mathbf{l} = (B_\text{above}^\parallel - B_\text{below}^\parallel)l = \mu_0I_\text{enc} = \mu_0Kl \\
	\implies B_\text{above}^\parallel - B_\text{below}^\parallel = \mu_0K \tag{5.26}
\end{align*}
$$
总结下来，我们得到：
$$
\begin{equation*}
	\mathbf{B}_\text{above} - \mathbf{B}_\text{below} = \mu_0(\mathbf{K}\times\unit{n})
\end{equation*} \tag{5.27}
$$
磁矢势和电势在边界条件中类似，也是连续的（令其散度为零的情况下），即：
$$
\begin{equation*}
	\mathbf{A}_\text{above} = \mathbf{A}_\text{below}
\end{equation*} \tag{5.28}
$$
磁矢势的导数同样是不连续的：
$$
\begin{equation*}
	\frac{\partial \mathbf{A}_\text{above}}{\partial n} - \frac{\partial\mathbf{B}_\text{below}}{\partial n} = -\mu_0\mathbf{K}
\end{equation*} \tag{5.29}
$$

#### 多极展开

回忆我们之前对 $\frac{1}{\rcur}$ 进行的展开式 $(\ref{expansion-of-1-over-r})$，我们同样可以得到远距离处磁矢势的多极展开：
$$
\begin{equation*}
	\mathbf{A}(\mathbf{r}) = \frac{\mu_0I}{4\pi}\sum_{n=0}^\infty\frac{1}{r^{n+1}}\oint r'^nP_n(\cos\alpha)\,d\mathbf{l}'
\end{equation*} \tag{5.30} \label{multipole-expansion-vector-potential}
$$
和电势的展开类似，我们将这个级数的第一项称为 **单极子**，第二项称为 **偶极子**，第三项称为 **四极子**，以此类推。不出意料的是，单极子项始终为零，这正是由于磁场的无源性，即 $\nabla\cdot \mathbf{B}$ 决定的。由此也向我们暗示了自然界中不存在磁单极子。所以磁偶极子成为了主导项：
$$
\begin{equation*}
	\mathbf{A}_\text{dip}(\mathbf{r}) = \frac{\mu_0I}{4\pi r^2}\oint r'\cos\alpha\,d\mathbf{l}' = \frac{\mu_0I}{4\pi r^2}\oint (\unit{r}\cdot\mathbf{r}')\,d\mathbf{l}'
\end{equation*} \tag{5.31}
$$
我们可以将 $\unit{r}$ 提出来，写成：
$$
\begin{equation*}
	\mathbf{A}_\text{dip}(\mathbf{r}) = \frac{\mu_0}{4\pi}\frac{\mathbf{m}\times\unit{r}}{r^2}
\end{equation*} \tag{5.32}
$$
其中 $\mathbf{m}$ 是 **磁偶极矩（Magnetic Dipole Moment）**，定义为：
$$
\begin{equation*}
	\marginbox{\mathbf{m} = I\int\,d\mathbf{a} = I\mathbf{a}}
\end{equation*} \tag{5.33} \label{magnetic-dipole-moment}
$$

此处的 $\mathbf{a}$ 是一个带有方向的环路围成的面积，其方向由右手定则确定（四根手指方向与环路方向相同，大拇指向上，方向为面的方向）。在磁矢势多极展开公式 $(\ref{multipole-expansion-vector-potential})$ 中，显然磁偶极矩和原点的选择无关，这和电势在总电荷数为零（单极子为零）时，电偶极矩也和原点的选择无关一致。这也和磁单极子项恒为零相吻合。

一个纯磁偶极子，可以想象为无限小的电流环路。如果将其放置在原点，其产生的磁矢势为：
$$
\begin{equation*}
	\mathbf{A}_\text{dip}(\mathbf{r}) = \frac{\mu_0}{4\pi}\frac{m\sin\theta}{r^2}\unit{\phi}
\end{equation*} \tag{5.34} \label{magnetic-vector-potential-of-magnetic-dipole}
$$
其相应的磁场是：
$$
\begin{equation*}
	\mathbf{B}_\text{dip}(\mathbf{r}) = \frac{\mu_0m}{4\pi r^2}(2\cos\theta\unit{r} + \sin\theta\unit{\theta})
\end{equation*} \tag{5.35} \label{magnetic-field-of-magnetic-dipole}
$$
这个结果和电偶极子激发的电场 $(\ref{electric-field-of-electric-dipole})$ 形式上几乎雷同。



## 物质中的磁场

日常生活中，我们对“磁性”的印象往往来自于冰箱贴、指南针、南北极等看起来和电流八竿子打不着的地方。但实际上，如果我们从微观层面研究，就会发现它们磁性是来源于电荷的运动：没错，电子绕着原子核运动，并存在自旋，此时是会产生磁场的。通常，这些电子的磁场会相互抵消，因此宏观中大多数物体没有磁性。但在一些条件下（比如外加磁场）电子旋转的朝向会趋于一致，此时磁场效应就相对明显了，我们称该物体被 **磁化（Magnetize）** 了。

电极化的方向总是和外加电场方向相同，但磁化就要分具体情况了：**顺磁体（Paramagnet）** 被激发出的磁场和外加磁场相同，**逆磁体（Diamagnet）** 被激发出的磁场和外加电场相反，而 **铁磁体（Ferromagnet）** 是一种特殊的磁体，它在外加磁场消失后依然会保持磁化的状态。永磁铁通常是由铁磁体制成的。本章中，我们会首先对这些不同类型的磁性来源进行分析，然后再引入和 **电位移** 类似的辅助场作为数学工具研究物质中的磁场。

### 磁化

#### 磁偶极子所受力和力矩

磁偶极子在磁场中会受到一个力矩。我们来考虑一个正方形的电流环路在磁场中受到的力矩（这是因为任意的电流环路都可以视作一系列正方形电流环路的加和，如下图右所示）。将这个环路中心置于原点，并且给它一个 $z$ 轴向 $y$ 轴方向的倾斜角。假设磁场方向为 $\unit{z}$，则倾斜的两边所受磁力抵消，我们就只需要计算”跷跷板“两端所受磁力了（如下图左所示）：

<img src="graphs/ed1_6-1.png" alt="ed1_6-1" style="zoom:110%;" /><img src="graphs/ed1_6-2.png" alt="ed1_6-2" style="zoom:110%;" />

每一段受到的磁力都是 $IbB$，但方向相反。利用 $m = Iab$，我们可以得到力矩：
$$
\begin{align}
	\mathbf{N} = \mathbf{m}\times\mathbf{B}
\end{align} \label{magnetic-dipole-torque} \tag{6.1}
$$
这个公式对匀强磁场总是成立；特别地，如果磁偶极子是一个完美偶极子，则对任意磁场都成立。在这个力矩作用下，磁偶极子总会达到 $\mathbf{m}$ 和 $\mathbf{B}$ 方向相同的结果，这就是 **顺磁性（Paramagnetism）** 的由来。原子中的电子就是这样的一个偶极子，因此如果有能够响应外加磁场的电子，物体就会显顺磁性。不过根据量子力学中的泡利不相容原理，原子中的电子时常会两两成对，并呈现正好相反的自旋——因此，顺磁性往往出现在有 *奇数* 个电子的原子或分子中（不过也不要把这个当作最终的、完整的理论，因为我们根本没有考虑电子之间的热碰撞等损耗）。

现在尝试计算这个电流环路受到的合外力。如果是匀强磁场，则合力为 $\mathbf{0}$：
$$
\begin{equation*}
	\mathbf{F} = I\oint(d\mathbf{l}\times\mathbf{B}) = I\oint d\mathbf{l}\times \mathbf{B} = \mathbf{0}
\end{equation*}
$$
如果不是匀强磁场，我们以螺线管为例：

<img src="graphs/ed1_6-4.png" alt="ed1_6-4" style="zoom:110%;" />

对于上图所示的环路，螺线管产生的净力是：
$$
\begin{equation*}
	F = 2\pi IRB\cos\theta
\end{equation*} \tag{6.2}
$$
对于一个磁偶极矩为 $\mathbf{m}$ 的微元环路，在磁场 $\mathbf{B}$ 中受到的力则是：
$$
\begin{equation*}
	\mathbf{F} = \nabla(\mathbf{m}\cdot\mathbf{B})
\end{equation*} \tag{6.3} \label{force-on-magnetic-dipole}
$$
需要特别注意的是，虽然这和电场类似条件下得到的 $(\ref{force-on-electric-dipole})$ 类似，但是磁场中不能写成与其完全一致的形式。让我们尝试对 $(\ref{force-on-magnetic-dipole})$ 进行变形：
$$
\begin{align*}
	\nabla(\mathbf{m}\cdot\mathbf{E}) &= \mathbf{m}\times(\nabla\times\mathbf{B}) + \mathbf{E}\times(\nabla\times\mathbf{m}) + (\mathbf{m}\cdot\nabla)\mathbf{E} + (\mathbf{E}\cdot\nabla)\mathbf{m} \\
	&= \mu_0(\mathbf{m}\times\mathbf{J}) + (\mathbf{m}\cdot\nabla)\mathbf{B}
\end{align*}
$$
可见由于磁场的散度不恒为零，最后会多出一项 $\mu_0(\mathbf{m}\times\mathbf{J})$，这就是问题所在。

#### 磁场对原子轨道的影响

考虑一个电子围绕原子核旋转的模型。假设其轨道半径为 $R$，则周期为 $T = 2\pi R/v$，其中 $v$ 是电子的速度。此时会形成一个电流：
$$
I = \frac{-e}{T} = -\frac{ev}{2\pi R}
$$
由磁偶极矩的计算公式 $(\ref{magnetic-dipole-moment})$ 得到：
$$
\begin{equation*}
	\mathbf{m} = -\frac{1}{2}evR\unit{z}
\end{equation*}
$$
此时如果外加一个磁场 $\mathbf{B}$，由于很难让整个轨道向一侧倾斜，顺磁性的表现会比较差。有另一个效应更加明显：电子的加速或减速。由于磁场参与了电子圆周运动的平衡：
$$
\frac{1}{4\pi\epsilon_0}\frac{e^2}{R^2} + e\bar{v}B = m_e\frac{\bar{v}^2}{R^2}
$$
这里第一项是电子和原子核的静电力，第二项则是磁场对电子的磁力。因此我们可以得到：
$$
e\bar{v}B = m_e\frac{\bar{v}^2}{R^2} - \frac{1}{4\pi\epsilon_0}\frac{e^2}{R^2} = \frac{m_e}{R}(\bar{v} + v)(\bar{v} - v)
$$
其中 $v$ 是磁场不存在时，电子做匀速圆周运动的速度。因此变化的速度为：
$$
\begin{equation*}
	\Delta v = \bar{v} - v = \frac{eRB}{m_e}\frac{\bar{v}}{\bar{v} + v} \approx \frac{eRB}{2m_e}
\end{equation*} \tag{6.4}
$$
这里我们假设 $\Delta v$ 非常小，因此 $\bar{v}$ 可以近似为 $v$。最后，这个速度改变造成的磁偶极矩的变化为：
$$
\begin{equation*}
	\Delta\mathbf{m} = -\frac{e^2R^2}{4m_e}\mathbf{B}
\end{equation*} \tag{6.5}
$$
可见偶极矩向磁场的反方向变化，直到方向完全相反为止。我们将这个性质称为 **逆磁性（Diamagnetism）**。多数情况下逆磁性的效应要比顺磁性弱得多，因此主要在偶数电子数量的原子中观测到。

我们前面给出的公式说明只是一个非常理想且局限的情形（匀速圆周运动，且原子不受磁场影响等），但无论如何设置，其结论（磁偶极矩变化相反于磁场）保持不变。事实上只有借用量子物理的知识才能对其细节进行打磨，这里我们就不多介绍了。

### 磁化物体的场

#### 束缚电流

和极化物体类似，我们定义单位体积中物体的磁偶极矩为其 **磁化强度（Magnetization）**，记作 $\mathbf{M}$。由于单个磁偶极子产生的磁矢势为：
$$
\begin{equation*}
	\mathbf{A}(\mathbf{r}) = \frac{\mu_0}{4\pi}\frac{\mathbf{m}\times\unit{\rcur}}{\rcur^2}
\end{equation*} \tag{6.6}
$$
因此对于一个磁化的物体，其产生的叠加的磁矢势为：
$$
\begin{equation*}
	\mathbf{A}(\mathbf{r}) = \frac{\mu_0}{4\pi}\int\frac{\mathbf{M}(\mathbf{r}')\times\unit{\rcur}}{\rcur^2}\,d\tau'
\end{equation*} \tag{6.7} \label{magnetic-vector-potential-of-magnetized-matter}
$$
随后进行和此前介绍[束缚电荷](#束缚电荷)时用的数学方法类似，得到：
$$
\begin{equation*}
	\mathbf{A}(\mathbf{r}) = \frac{\mu_0}{4\pi}\int_\mathcal{V}\frac{1}{\rcur}[\nabla'\times\mathbf{M}(\mathbf{r}')] + \frac{\mu_0}{4\pi}\oint_\mathcal{S}\frac{1}{\rcur}[\mathbf{M}(\mathbf{r}')\times d\mathbf{a}']
\end{equation*}
$$
我们将其中有电流密度特征的项提取出来：
$$
\begin{align*}
	\mathbf{J}_b &= \nabla\times\mathbf{M} \\
	\mathbf{K}_b &= \mathbf{M}\times\unit{n}
\end{align*} \tag{6.8} \label{bound-current}
$$
由这个定义，我们就可以将前面的公式写成：
$$
\begin{equation*}
	\mathbf{A}(\mathbf{r}) = \frac{\mu_0}{4\pi}\int_\mathcal{V}\frac{\mathbf{J}_b(\mathbf{r}')}{\rcur}\,d\tau' + \frac{\mu_0}{4\pi}\oint_\mathcal{S}\frac{\mathbf{K}_b(\mathbf{r}')}{\rcur}\,da'
\end{equation*} \tag{6.9}
$$
这个结果暗示了，磁化物体的磁矢势等价于通过一个体电流 $\mathbf{J}_b$ 和面电流 $\mathbf{K}_b$ 产生的磁矢势。我们将它们称为 **束缚电流（Bound Current）**。在分析磁化物体时，我们常常通过磁化强度先得到该物体的束缚电流，然后再通过其求出磁场。

#### 束缚电流的物理意义

### 辅助场 $\mathbf{H}$

#### 磁介质中的安培定律

现在综合考虑磁介质中的电流，它应该分为两部分，束缚电流和 **自由电流（Free Current）**：
$$
\begin{equation*}
	\mathbf{J} = \mathbf{J}_b + \mathbf{J}_f
\end{equation*} \tag{6.10}
$$
根据安培定律，我们有：
$$
\frac{1}{\mu_0}(\nabla\times\mathbf{B}) = \nabla\times\mathbf{M} + \mathbf{J}_f
$$
将两个旋度项合并：
$$
\nabla\times\left(\frac{1}{\mu_0}\mathbf{B} - \mathbf{M}\right) = \mathbf{J}_f
$$
这里就可以引入 $\mathbf{H}$ 场了：
$$
\begin{equation*}
	\marginbox{\mathbf{H} \equiv \frac{1}{\mu_0}\mathbf{B} - \mathbf{M}}
\end{equation*} \tag{6.11} \label{H-field}
$$
物质中的安培定律因此可以写成：
$$
\begin{equation*}
	\nabla\times\mathbf{H} = \mathbf{J}_f
\end{equation*} \tag{6.12} \label{ampere's-law-in-matter-derivative}
$$
其积分形式是：
$$
\begin{equation*}
	\oint\mathbf{H}\cdot d\mathbf{l} = I_{f\text{enc}}
\end{equation*} \tag{6.13} \label{ampere's-law-in-matter}
$$
$\mathbf{H}$ 在磁介质中的作用和电位移，即 $\mathbf{D}$ 在电介质中的作用一致。我们对于磁化物体，时常可以通过自由电流求得 $\mathbf{H}$，然后通过其定义得到磁场强度。特别地，对于一些对称的物体，我们可以通过安培定律快速计算出 $\mathbf{H}$。

我们并没有一个合适的名字称呼 $\mathbf{H}$ 场，或许 **辅助场（Auxiliary Field）** 是一个称呼，但更多的时候我们直接称其为 $\mathbf{H}$。一些教材中会将其称为 *磁场强度*，但这样就只能为 $\mathbf{B}$ 起另外一个名字，如 *磁通量密度*、*磁感应强度* 等，但是这些名字显得不太直观，无法体现出 $\mathbf{B}$ 在磁学中的关键地位（毕竟毕奥·萨法尔定律给出的是 $\mathbf{B}$ 而不是 $\mathbf{H}$），就好像将 $\mathbf{E}$ 称为 *电感应强度* 一样不自然。事实上，$\mathbf{D}$ 的命名，*电位移*，也令人迷惑不已。不过 $\mathbf{D}$ 的名称流传已久成为了习惯。

值得一提的趣事是，$\mathbf{H}$ 在物理实验中的重要性比 $\mathbf{D}$ 要重要得多。这是因为实验中涉及磁场时会用到电磁铁，而其束缚电流我们是无法直接测量的。电流表中看到的是自由电流，因此算出来的就是 $\mathbf{H}$，而 $\mathbf{B}$ 则要根据磁介质的性质计算得出，反而不太好得到。另一边，涉及电场时会用到接入电源的电容，此时能够测量的是电源的电压，算出来的就是 $\mathbf{E}$。此时 $\mathbf{D}$ 要根据电介质的性质计算得出，不太好得到。如果有轻松测量电荷的方式，或许 $\mathbf{D}$ 会更加常用一些。

$(\ref{ampere's-law-in-matter})$ 有的时候会诱使我们得到错误的结果。这是因为 $\mathbf{H}$ 的散度并不等于 $0$：
$$
\begin{equation*}
	\nabla\cdot\mathbf{H} = \nabla\cdot(\frac{1}{\mu_0}\mathbf{B} - \mathbf{M}) = -\nabla\cdot\mathbf{M}
\end{equation*} \tag{6.14}
$$
在一些不是完美对称的情形（圆柱、平面、螺线、环形线圈），如磁铁块（六面体），虽然不存在任何束缚电流，但不能断定 $\mathbf{H}$ 等于零，否则此时会有 $\mathbf{B} = \mu_0\mathbf{M}$，因此在磁铁外不存在磁场，显然与我们的直觉相悖。实际上这正是因为磁化强度在一些地方的散度不为零导致的。

#### 边界条件

物质存在时磁场中的边界条件是：
$$
\begin{equation*}
	H_\text{above}^\perp - H_\text{below}^\perp = -(M_\text{above}^\perp - M_\text{below}^\perp)
\end{equation*} \tag{6.15}
$$
平行方向则有：
$$
\begin{equation*}
	\mathbf{H}_\text{above}^\parallel - \mathbf{H}_\text{below}^\parallel = \mathbf{K}_f\times\unit{n}
\end{equation*} \tag{6.16}
$$

### 线性磁介质

#### 磁极化率与磁导率

本章开头的时候我们就介绍了两种磁性。它们共同的特点是和磁场成正比（在磁场不过强的时候）。事实上我们可以将这种性质用下面的形式表示：
$$
\begin{equation*}
	\mathbf{M} = \chi_m\mathbf{H}
\end{equation*} \tag{6.16} \label{magnetic-susceptibility}
$$
我们将 $\chi_m$ 称为 **磁化率（Magnetic Susceptibility）**，而满足这个式子的磁介质被称为 **线性磁介质（Linear Media）**。如果将其和电极化率的式子对比会发现奇怪的不对称性：理论上下面这个式子才是“正确”的定义式：
$$
\mathbf{M} = \frac{1}{\mu_0}\chi_m\mathbf{B}
$$
但由于历史原因，我们使用前一种公式。这种不对称性也延续到了 $\mathbf{B}$ 与 $\mathbf{H}$ 在线性磁介质种的本构关系中：
$$
\begin{equation*}
	\mathbf{B} = \mu_0(\mathbf{H} + \mathbf{M}) = \mu_0(1 + \chi_m)\mathbf{H} = \mu\mathbf{H}
\end{equation*} \tag{6.17} \label{B-and-H}
$$
可以对比发现和 $(\ref{E-and-D})$ 的区别。我们将 $\mu$ 称为物质的 **磁导率（Permeability）**。真空的磁导率是 $\mu_0$，此时 $\chi_m = 0$。磁导率与真空磁导率的比值并不常用。下面是一些常见物质的磁化率：

| 物质     | 磁导率               | 物质     | 磁导率             |
| -------- | -------------------- | -------- | ------------------ |
| 铋       | $-1.7\times 10^{-4}$ | 氧气     | $1.7\times10^{-6}$ |
| 金       | $-3.4\times10^{-5}$  | 钠       | $8.5\times10^{-6}$ |
| 银       | $-2.4\times10^{-5}$  | 铝       | $2.2\times10^{-5}$ |
| 铜       | $-9.7\times10^{-6}$  | 钨       | $7.0\times10^{-5}$ |
| 水       | $-9.0\times10^{-6}$  | 铂       | $2.7\times10^{-4}$ |
| 二氧化碳 | $-1.1\times10^{-8}$  | 液态氧气 | $3.9\times10^{-3}$ |
| 氢气     | $-2.1\times10^{-9}$  | 钆       | $4.8\times10^{-1}$ |

上表中左侧是逆磁体，右侧是顺磁体。

线性磁介质中，我们发现 $\mathbf{M}$ 与 $\mathbf{H}$ 都和 $\mathbf{B}$ 成正比，这是否以为着前两者的散度都为 $0$ 呢？并非如此：
$$
\nabla\cdot\mathbf{H} = \nabla\cdot\left(\frac{1}{\mu}\mathbf{B}\right) = \mathbf{B}\cdot\nabla\left(\frac{1}{\mu}\right)
$$
因此在 $\mu$ 部位常数的时候，依然无法断言 $\mathbf{H}$ 为零。$\mathbf{M}$ 是同理的。即使是 $\mu$ 保持不变的线性磁介质，在边界处 $\mathbf{H}$ 是不连续的，因此其散度必然不是 $0$。

### 铁磁性

最后，让我们对第三种磁性，**铁磁性（Ferromagnetism）** 进行简单的介绍。其它大多数磁体的性质都是存在外加磁场的情况下，磁偶极矩发生定向改变，然后显示出磁性。铁磁体（和顺磁体类似）在产生与磁场相同方向的磁偶极矩后，倾向于 *维持* 当前的状态，即使在磁场消失后依然保持。这是因为铁磁体中的磁偶极子倾向于和其周围磁偶极子方向相同，更深层的原因涉及量子力学，这里不作说明。这个趋同的范围限制在一个 **域（Domain）** 内（大概包含几十亿个偶极子），不同域的偶极子间互不影响，整体的偶极矩方向也各不相同（通常和晶体结构有关）。这也是为什么不是所有的铁制物体都有磁性。但在磁场的作用下，所有域都会倾向于向一个方向对齐，我们也就得到了一个 **永磁铁（Permanent Magnet）**。如果磁场非常强大，使得所有域都对齐到一个方向，我们称该物质 **饱和（Saturated）** 了。

从对铁磁性的描述中不难发现，其磁化过程是不可逆的。初始状态下，铁磁体中域的方向是相对随机的。在磁场作用下，它会被磁化为磁场方向直至饱和。如果我们这时候通相反方向的等量磁场，域的方向只会变为完全反向，不会回到开始的状态。这个变化曲线可以用 **磁滞环路（Hysteresis Loop）** 来表现：

![ed1_6-5](graphs/ed1_6-5.png)

如果需要量化电流和磁化强度的关系，我们常使用另一组关系的磁滞图，即 $\mu_0\mathbf{H}$ （与电流对应，根据安培定律）和 $\mathbf{B}$（因为 $\mathbf{B} = \mu_0(\mathbf{H} + \mathbf{M})$ 中 $\mathbf{H}$ 要比 $\mathbf{M}$ 要小得多，因此直接用 $\mathbf{B}$ 代替 $\mu_0\mathbf{M}$）：

![ed1_6-6](graphs/ed1_6-6.png)

注意这里横坐标是乘过 $10^4$ 之后的结果，因此实际上只需要通很小的电流，就可以在铁磁体中产生巨大的磁场。

## 公式汇总

本篇的主要内容是静电场与静磁场的性质，以及通过矢量分析和偏微分方程等数学工具得到的有趣结论。从中我们应该意识到电磁场中存在大量相似的公式和设定。下面我们将其一一总结起来并进行对比，如下表所示：

| **名称（电）**                                         |                           **公式**                           | **名称（磁）**                                               |                           **公式**                           |
| :----------------------------------------------------- | :----------------------------------------------------------: | ------------------------------------------------------------ | :----------------------------------------------------------: |
| 库仑定律 $\mathbf{E}$                                  | $\displaystyle{\mathbf{E}(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\int\frac{\rho(\mathbf{r}')}{\rcur^2}\unit{\rcur}\,d\tau'}$ | 毕奥·萨伐尔定律 $\mathbf{B}$                                 | $\displaystyle{\mathbf{B}(\mathbf{r}) = \frac{\mu_0}{4\pi}\int\frac{\mathbf{J}(\mathbf{r}')}{\rcur^2}\times\unit{\rcur}\,d\tau'}$ |
| 电荷分布                                               |        $dq = \lambda\,dl = \sigma\,da = \rho\,d\tau$         | 电流分布                                                     | $d(\sum q_i\mathbf{v}_i) = \mathbf{I}\,dl = \mathbf{K}\,da = \mathbf{J}\,d\tau$ |
| 电场散度                                               | $\displaystyle \nabla\cdot \mathbf{E} = \frac{\rho}{\epsilon_0}$ | 磁场散度                                                     |                 $\nabla\cdot \mathbf{B} = 0$                 |
| 静电场旋度                                             |            $\nabla\times\mathbf{E} = \mathbf{0}$             | 静磁场旋度                                                   |         $\nabla\times \mathbf{B} = \mu_0\mathbf{J}$          |
| 高斯定律                                               | $\displaystyle \oint\mathbf{E}\cdot d\mathbf{a} = \frac{Q_\text{enc}}{\epsilon_0}$ | 安培定律                                                     | $\displaystyle \oint\mathbf{B}\cdot d\mathbf{l} = \mu_0I_\text{enc}$ |
| 静电场无旋                                             |     $\displaystyle \oint\mathbf{E}\cdot d\mathbf{l} = 0$     | 磁场无源                                                     |     $\displaystyle \oint\mathbf{B}\cdot d\mathbf{a} = 0$     |
| 电势                                                   | $\displaystyle V(\mathbf{r}) = -\int_\mathcal{O}^\mathbf{r} \mathbf{E}\cdot d\mathbf{l}$ | 磁矢势                                                       | $\displaystyle \mathbf{A}(\mathbf{r}) = \int_\mathcal{O}^\mathbf{r}\mathbf{B}\times d\mathbf{l}$ |
| 电场是电势的梯度                                       |                   $\mathbf{E} = -\nabla V$                   | 磁场是磁矢势的旋度                                           |            $\mathbf{B} = \nabla\times \mathbf{A}$            |
| 库仑定律 $V$                                           | $\displaystyle V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\int\frac{\rho(\mathbf{r}')}{\rcur}\,d\tau'$ | 毕奥·萨伐尔定律 $\mathbf{A}$                                 | $\displaystyle \mathbf{A}(\mathbf{r}) = \frac{\mu_0}{4\pi}\int\frac{\mathbf{J}(\mathbf{r}')}{\rcur}\,d\tau'$ |
| 泊松方程 $V$                                           |    $\displaystyle \nabla^2 V = -\frac{\rho}{\epsilon_0}$     | 泊松方程 $\mathbf{A}$                                        |           $\nabla^2\mathbf{A} = -\mu_0\mathbf{J}$            |
| 边界条件 $\mathbf{E}$                                  | $\displaystyle \mathbf{E}_\text{above} - \mathbf{E}_\text{below} = \frac{\sigma}{\epsilon_0}\unit{n}$ | 边界条件 $\mathbf{B}$                                        | $\mathbf{B}_\text{above} - \mathbf{B}_\text{below} = \mu_0(\mathbf{K}\times\unit{n})$ |
| 边界条件 $V$                                           |              $V_\text{above} = V_\text{below}$               | 边界条件 $\mathbf{A}$                                        |     $\mathbf{A}_\text{above} = \mathbf{A}_\text{below}$      |
| 边界条件 $\displaystyle \frac{\partial V}{\partial n}$ | $\displaystyle \frac{\partial V_\text{above}}{\partial n} - \frac{\partial V_\text{below}}{\partial n} = -\frac{\sigma}{\epsilon_0}$ | 边界条件 $\displaystyle \frac{\partial \mathbf{A}}{\partial n}$ | $\displaystyle \frac{\partial \mathbf{A}_\text{above}}{\partial n} - \frac{\partial\mathbf{A}_\text{below}}{\partial n} = -\mu_0\mathbf{K}$ |
| 电势多极展开                                           |                        简略式见后表 1                        | 磁矢势多极展开                                               |                        简略式见后表 2                        |
| 电势多极展开                                           |                        详细式见后表 3                        | 磁矢势多极展开                                               |                        详细式见后表 4                        |
| 电偶极矩                                               | $\displaystyle \mathbf{p} = \int\mathbf{r}'\rho(\mathbf{r}')\,d\tau'$ | 磁偶极矩                                                     |       $\displaystyle \mathbf{m} = I\int\,d\mathbf{a}'$       |
| 电势的偶极子项                                         | $\displaystyle V_\text{dip} = \frac{1}{4\pi\epsilon_0}\frac{\mathbf{p}\cdot\unit{r}}{r^2}$ | 磁矢势的偶极子项                                             | $\displaystyle \mathbf{A}_\text{dip} = \frac{\mu_0}{4\pi}\frac{\mathbf{m}\times\unit{r}}{r^2}$ |
| 电场（偶极子项）                                       |                      坐标有关式见后表 5                      | 磁场（偶极子项）                                             |                      坐标有关式见后表 6                      |
| 电场（偶极子项）                                       |                      坐标无关式见后表 7                      | 磁场（偶极子项）                                             |                      坐标无关式见后表 8                      |
| 电偶极子受力矩                                         |        $\boldsymbol{N} = \mathbf{p}\times \mathbf{E}$        | 磁偶极子受力矩                                               |        $\boldsymbol{N} = \mathbf{m}\times\mathbf{B}$         |
| 电偶极子受力                                           |       $\mathbf{F} = (\mathbf{p}\cdot\nabla)\mathbf{E}$       | 磁偶极子受力                                                 |       $\mathbf{F} = \nabla(\mathbf{m}\cdot\mathbf{B})$       |
| 极化强度                                               |    $\displaystyle \mathbf{P} = \frac{d\mathbf{p}}{d\tau}$    | 磁化强度                                                     |    $\displaystyle \mathbf{M} = \frac{d\mathbf{m}}{d\tau}$    |
| 电势 $\mathbf{P}$                                      |                           见后表 9                           | 磁矢势 $\mathbf{M}$                                          |                          见后表 10                           |
| 束缚电荷面密度                                         |             $\sigma_b = \mathbf{P}\cdot\unit{n}$             | 束缚电流面密度                                               |          $\mathbf{K}_b = \mathbf{M}\times\unit{n}$           |
| 束缚电荷体密度                                         |              $\rho_b = -\nabla\cdot\mathbf{P}$               | 束缚电流体密度                                               |           $\mathbf{J}_b = \nabla\times\mathbf{M}$            |
| 电势（极化物体）                                       |                          见后表 11                           | 磁矢势（磁化物体）                                           |                          见后表 12                           |
| 电位移                                                 |       $\mathbf{D} = \epsilon_0\mathbf{E} + \mathbf{P}$       | 磁场 $\mathbf{H}$                                            | $\displaystyle \mathbf{H} = \frac{1}{\mu_0}\mathbf{B} - \mathbf{M}$ |
| 电位移的散度                                           |      $\nabla\cdot \mathbf{D} = \rho_f = \rho - \rho_b$       | 磁场 $\mathbf{H}$ 的旋度                                     | $\nabla\times\mathbf{H} = \mathbf{J}_f = \mathbf{J} - \mathbf{J}_b$ |
| 高斯定律 $\mathbf{D}$                                  | $\displaystyle \oint\mathbf{D}\cdot d\mathbf{a} = Q_{f\text{enc}}$ | 安培定律 $\mathbf{H}$                                        | $\displaystyle \oint\mathbf{H}\cdot d\mathbf{l} = I_{f\text{enc}}$ |
| 边界条件 $\mathbf{D}^\perp$                            |   $D_\text{above}^\perp - D_\text{below}^\perp = \sigma_f$   | 边界条件 $\mathbf{H}^\parallel$                              | $\mathbf{H}_\text{above}^\parallel - \mathbf{H}_\text{below}^\parallel = \mu_0(\mathbf{K}_f \times \unit{n})$ |
| 边界条件 $\mathbf{D}^\parallel$                        |                          见后表 13                           | 边界条件 $\mathbf{H}^\perp$                                  |                          见后表 14                           |
| 线性电介质                                             |          $\mathbf{P} = \epsilon_0\chi_e\mathbf{E}$           | 线性磁介质                                                   |               $\mathbf{M} = \chi_m\mathbf{H}$                |
| 介电率                                                 |             $\epsilon = \epsilon_0(1 + \chi_e)$              | 磁导率                                                       |                  $\mu = \mu_0(1 + \chi_m)$                   |
| 本构关系                                               |              $\mathbf{D} = \epsilon\mathbf{E}$               | 本构关系                                                     |                 $\mathbf{B} = \mu\mathbf{H}$                 |
| 相对介电率                                             |   $\displaystyle \epsilon_r = \frac{\epsilon}{\epsilon_0}$   | 相对磁导率                                                   |          $\displaystyle \mu_r = \frac{\mu}{\mu_0}$           |



上面由于篇幅限制，有些公式没有给出。下表给出详细公式：

| **编号** |                           **公式**                           |
| -------- | :----------------------------------------------------------: |
| 1        | $\displaystyle V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\sum_{n=0}^\infty\frac{1}{r^{n+1}}\int r'^nP_n(\cos\alpha)\rho(\mathbf{r}')\,d\tau'$ |
| 2        | $\displaystyle \mathbf{A}(\mathbf{r}) = \frac{\mu_0I}{4\pi}\sum_{n=0}^\infty\frac{1}{r^{n+1}}\oint r'^nP_n(\cos\alpha)\,d\mathbf{l}'$ |
| 3        | $\displaystyle V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\left[\frac{1}{r}\int\rho(\mathbf{r}')\,d\tau' + \frac{1}{r^2}\int r'\cos\alpha\rho(\mathbf{r}')\,d\tau' + \frac{1}{r^3}\int r'^2\frac{1}{2}\left(3\cos^2\alpha - 1\right)\rho(\mathbf{r}')\,d\tau' + ...\right]$ |
| 4        | $\displaystyle \mathbf{A} = \frac{\mu_0I}{4\pi}\left[\frac{1}{r}\oint\,d\mathbf{l}' + \frac{1}{r^2}\oint r'\cos\alpha\,d\mathbf{l}' + \frac{1}{r^3} \oint r'^2\frac{1}{2}(3\cos^2\alpha - 1)\,d\mathbf{l}' + ...\right]$ |
| 5        | $\displaystyle \mathbf{E}(r, \theta) = \frac{p}{4\pi\epsilon_0r^3}(2\cos\theta\unit{r} + \sin\theta\unit{\theta})$ |
| 6        | $\displaystyle\mathbf{B}(r, \theta) = \frac{\mu_0m}{4\pi r^3}(2\cos\theta\unit{r} + \sin\theta\unit{\theta})$ |
| 7        | $\displaystyle \mathbf{E}_\text{dip}(\mathbf{r}) = \frac{1}{4\pi\epsilon_0r^3}[3(\mathbf{p}\cdot\unit{r})\unit{r} - \mathbf{p}] - \frac{1}{3\epsilon_0}\mathbf{p}\delta^3(\mathbf{r})$ |
| 8        | $\displaystyle\mathbf{B}_\text{dip}(\mathbf{r}) = \frac{\mu_0}{4\pi r^3}[3(\mathbf{m}\cdot\unit{r}) - \mathbf{m}] + \frac{2\mu_0}{3}\mathbf{m}\delta^3(\mathbf{r})$ |
| 9        | $\displaystyle V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\int\frac{\mathbf{P}(\mathbf{r}')\cdot\unit{\rcur}}{\rcur^2}\,d\tau'$ |
| 10       | $\displaystyle \mathbf{A}(\mathbf{r}) = \frac{\mu_0}{4\pi}\int\frac{\mathbf{M}(\mathbf{r}')\times\unit{\rcur}}{\rcur^2}\,d\tau'$ |
| 11       | $\displaystyle V(\mathbf{r}) = \frac{1}{4\pi\epsilon_0}\underset{\mathcal{S}}\oint\frac{\sigma_b}{\rcur}\,da' + \frac{1}{4\pi\epsilon_0}\underset{\mathcal{V}}{\int}\frac{\rho_b}{\rcur}\,d\tau'$ |
| 12       | $\displaystyle \mathbf{A}(\mathbf{r}) = \frac{\mu_0}{4\pi}\underset{\mathcal{S}}{\oint}\frac{\mathbf{K}_b(\mathbf{r}')}{\rcur}\,da' + \frac{\mu_0}{4\pi}\underset{\mathcal{V}}{\int}\frac{\mathbf{J}_b(\mathbf{r}')}{\rcur}\,d\tau'$ |
| 13       | $\mathbf{D}_\text{above}^\parallel - \mathbf{D}_\text{below}^\parallel = \mathbf{P}_\text{above}^\parallel - \mathbf{P}_\text{below}^\parallel$ |
| 14       | $H_\text{above}^\perp - H_\text{below}^\perp = -(M_\text{above}^\perp - M_\text{below}^\perp)$ |

我们也给出一些常见的电荷分布产生的电磁场。注意，下面所有带电都指均匀带电，通电指通恒稳电流，即 $\lambda, \sigma, \rho, I, K, J$ 均为常数：

| **分布（电）** | **公式**                                                     | **分布（磁）**   | **公式**                                                  |
| -------------- | ------------------------------------------------------------ | ---------------- | --------------------------------------------------------- |
| 无限带电直导线 | $E = \dfrac{\lambda}{2\pi\epsilon_0s}$                       | 无限通电直导线   | $B = \dfrac{\mu_0I}{2\pi s}$                              |
| 有限带电直导线 | $E = \dfrac{\lambda}{4\pi\epsilon_0s}(\cos\theta_2 - \cos\theta_1)$ | 有限通电直导线段 | $B = \dfrac{\mu_0I}{4\pi s}(\sin\theta_2 - \sin\theta_1)$ |
| 带电环路       | $E = \dfrac{\lambda}{2\epsilon_0}\dfrac{Rz}{(R^2+z^2)^{3/2}}$ | 通电环路         | $B = \dfrac{\mu_0I}{2}\dfrac{R^2}{(R^2+z^2)^{3/2}}$       |
| 带电平面       | $E = \dfrac{\sigma}{2\epsilon_0}$                            | 通电平面         | $B = \dfrac{\mu_0K}{2}$                                   |

