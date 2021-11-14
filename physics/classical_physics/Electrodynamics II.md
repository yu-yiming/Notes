# Electrodynamics II

本笔记是对应 *UIUC PHYS 436 Electromagnetic Fields II*，以及 *UIUC PHYS 435 Electromagnetic Fields I* 最后一部分的学习笔记，其中包括了电动力学、守恒定律、电磁波、辐射以及狭义相对论相关的知识点。由于本篇是物理笔记，其中的数学定理并不一定提供证明，即使有证明也不一定是严格的。主要参考了 *Introduction to Electrodynamics, 4th Edition: David J. Griffiths*，文中大量的例题、图片都来自该教材，章节的编排也基本按照这本书的设计。

[TOC]

$$
\newcommand{\marginbox}[1]{\fbox{$\hphantom{1} {#1} \vphantom{1\over1} \hphantom{1}$}}\nonumber
\newcommand{\rcur}[0]{\mathscr{r}}
\newcommand{\brcur}[0]{\boldsymbol{\mathscr{r}}}
\newcommand{\unit}[1]{\hat{\boldsymbol{#1}}}
$$

## 电动力学

### 欧姆定律

为了让电荷移动产生电流，我们需要对其施加力。假设对单位电荷的力是 $\mathbf{f}$，它产生的电流密度则是：
$$
\mathbf{J} = \sigma \mathbf{f}
$$
其中 $\sigma$ 是 **电导率（Conductivity）**，和媒介的类型有关。一个相关的概念是 **电阻率（Resistivity）**，记为 $\rho$，定义为电导率的倒数。下面是一些常见媒介的电阻率：

| 材料     | 电阻率（$\ohm\cdot\text{m}$） | 材料                 | 电阻率（$\ohm\cdot\text{m}$） |
| -------- | ----------------------------- | -------------------- | ----------------------------- |
| 银       | $1.59\times 10^{-8}$          | 海水                 | $0.2$                         |
| 铜       | $1.68\times10^{-8}$           | 锗                   | $0.46$                        |
| 金       | $2.21\times10^{-8}$           | 钻石                 | $2.7$                         |
| 铝       | $2.65\times10^{-8}$           | 硅                   | $2500$                        |
| 铁       | $9.61\times10^{-8}$           | 纯水                 | $8.3\times10^3$               |
| 汞       | $9.61\times10^{-7}$           | 玻璃                 | $10^9 - 10^{14}$              |
| 镍铬合金 | $1.08\times10^{-6}$           | 橡胶                 | $10^{13} - 10^{15}$           |
| 锰       | $1.44\times10^{-6}$           | 特富龙（聚四氟乙烯） | $10^{22} - 10^{24}$           |
| 石墨     | $1.6\times10^{-5}$            | 完美导体（不存在）   | $0$                           |

电荷受到力的来源可能是多样的，比如重力、化学力等。本篇中我们只对电磁力感兴趣，所以根据洛伦兹定律有：
$$
\mathbf{J} = \sigma(\mathbf{E} + \mathbf{v}\times\mathbf{B})
$$
由于多数情况下磁场都要远小于电场（反例如等离子体），因此我们省略磁场这一项：
$$
\marginbox{\mathbf{J} = \sigma \mathbf{E}}
$$
这就是 **欧姆定律（Ohm's Law）**，即电流密度和电导率及施加电场成正比。回忆在前一篇（*Electrodynamics I*）中学到的内容，在导体中的电场处处为零，彼时我们假定没有电流存在，因此 $\mathbf{J} = \mathbf{0}$。现在我们可以断定，对于完美导体（$\sigma = \infty$），即使有电流存在，其内部电场依然处处为零（因为 $\mathbf{E} = \mathbf{J}/\sigma$）。在现实情况下可以理解为，由于导体的电导率非常大，我们只需要近乎为零的电场就能产生一定的电流。

对于恒稳电流和电导率不变的情况下，我们有：
$$
\nabla\cdot \mathbf{E} = \frac{1}{\sigma}\nabla\cdot\mathbf{J} = 0
$$
根据高斯定律，电流媒介内部不存在任何净电荷，所以如果有电荷就一定出现在表面。这个式子也暗示在恒稳电流下，拉普拉斯方程依然有效（$\nabla^2 V = 0$）。

> **例**：一个长度为 $L$ 的圆柱体电阻截面积为 $A$，且其材料的电导率是 $\sigma$。现在假设沿着轴方向施加了一个电场 $\mathbf{E}$，其在圆柱体两端产生的电势差为 $V$。求产生的电流。

> **解**：电阻内部的电场应当是匀强的（我们将马上给出证明），因此电流密度也是匀强的：
> $$
> I = JA = \sigma EA = \frac{\sigma A}{L}V
> $$

实际上，对于任意的 **电极（Electrode）** 对，从一端流向另一端的电流都正比于两端的电势差。这也被称为欧姆定律，其形式是我们在中学阶段就已经见过的：
$$
\marginbox{V = IR}
$$
此处的 $R$ 称为电极之间媒介的 **电阻（Resistance）**，单位为 **欧姆（Ohm）**，记作 $\ohm$。它和媒介的类型、几何性质有关。

如果我们冷静下来仔细考虑欧姆定律的合理性，会发现它有些荒谬。电荷受到电场作用开始加速移动；它产生的电流密度也因此加速增加，这样不就应该变为无穷大么？然而同样也是欧姆定律生成这会产生一个恒定的电流（也就意味着电荷的速度是恒定的），这似乎并不合理。

实际上这是合理的，一个类比就是沿着一条路加速行驶，却在每个十字路口停下来。虽然你确实一直在加速，但是每过一段距离就要重复这个从零加速的过程，因此你的 *平均速度* 是恒定的。

### 电动势

电路中，电流是由两种力产生的，一个是来自电源的力 $\mathbf{f}_s$，另一个则是用于调节电流的静电力，其依然来源于电源的力。故：
$$
\mathbf{f} = \mathbf{f}_s + \mathbf{E}
$$
电源中的力可能来源于化学中的作用力（电池）、光（光电池）、机械压力（压电晶体）等等。无论机制如何，围绕电路一圈的环路积分始终是：
$$
\label{emp}
\marginbox{\mathcal{E} = \oint \mathbf{f}\cdot d\mathbf{l} = \oint \mathbf{f}_s\cdot d\mathbf{l}}
$$
其中我们把 $\oint\mathbf{E}\cdot d\mathbf{l} = 0$ 约去了。我们将 $\mathcal{E}$ 称为 **电动势（Electromotive Force, emf）**。当电源处不存在阻力时（理想电动势源），对电势的净力是 $0$（此时有 $\sigma = \infty$），我们得到 $\mathbf{E} = -\mathbf{f}_s$。因此电源两端的电势差是：
$$
V = -\int_a^b\mathbf{E}\cdot d\mathbf{l} = \int_a^b\mathbf{f}_s\cdot d\mathbf{l} = \oint\mathbf{f}_s\cdot d\mathbf{l} = \mathcal{E}
$$
因此电路中，电源提供的电势差等于电池的电动势。我们也可以理解成，电源对单位电荷做功的大小为 $\mathcal{E}$，这样从负极到负极能够达到 $V = \mathcal{E}$ 的电势差。

### 动生电动势

一个极为常见的电源是 **发电机（Generator）**，其中用到了 **动生电动势（Motional Emf）**的机制。下面这个非常简单的模型中，阴影部分是方向指入屏幕的磁场 $\mathbf{B}$，一个宽为 $h$ 的环形电路正在向右方以 $v$ 匀速移动，$R$ 是一个电阻为防止无穷大的电流。

<img src="graphs/ed2_7-1.png" alt="ed2_7-1" style="zoom:50%;" />

此时在切割磁感线的 $a-b$ 端会产生电动势：
$$
\label{emf-simple}
\mathcal{E} = \oint\mathbf{f}_\text{mag}\cdot d\mathbf{l} = vBh
$$
这个电动势的方向是沿着电路顺时针的，可以使用右手定则来确认：如果用四根手指的方向表示产生的电流方向，那么大拇指方向和磁场一致。看起来磁场力就是电动势的来源。不过这并不准确，因为磁场从来不可能做功。实际上做功的是拉着电路运动的力，这个力用来抵消磁场对 $a-b$ 段上运动的电荷（方向为右上，同时考虑电动势的作用以及 $v$）的磁力，后者方向为左上。如果我们设 $a-b$ 段电荷竖直方向移动速度为 $\mathbf{u}$，实际移动速度为 $\mathbf{w}$，两者的夹角为 $\theta$，则单位电荷做功为：
$$
\int\mathbf{f}_\text{pull}\cdot d\mathbf{l} = (uB)\left(\frac{h}{\cos\theta}\right)\sin\theta = vBh = \mathcal{E} \nonumber
$$
现在，让我们推导一个相比 $(\ref{emf-simple})$ 更加普适的电动势推导方式。首先让我们定义 **磁通量（Magnetic Flux）** 的概念：
$$
\label{magnetic-flux}
\marginbox{\Phi = \int\mathbf{B}\cdot d\mathbf{a}}
$$
对于前面描述的简单模型，我们有 $\Phi = Bhx$，而它和电动势的关系似乎是：
$$
\marginbox{\mathcal{E} = -\frac{d\Phi}{dt}}
$$
事实上，对于任意的发电机模型，上面的公式都是成立的。我们将在下面证明这一点：

> **证**：请看下面的参考图：
>
> <img src="graphs/ed2_7-2.png" alt="ed2_7-2" style="zoom:40%;" />
>
> 我们将 $dt$ 间隔的两个磁通量之差近似为对导线上每一点前后连线组成的无数个微小面积 $da$ 的磁通量之和：
> $$
> d\Phi = \Phi_\text{ribbon} = \int_\text{ribbon}\mathbf{B}\cdot d\mathbf{a} \nonumber
> $$
> 现在先着手考虑上面某一点 $P$ 和 $dt$ 事件后的 $P'$，参考上图左侧列出的 $da$ 分析图，我们有：
> $$
> d\mathbf{a} = (\mathbf{v}\times d\mathbf{l})\,dt \nonumber
> $$
> 由磁通量定义 $(\ref{magnetic-flux})$ 有：
> $$
> \frac{d\Phi}{dt} = \oint\mathbf{B}\cdot(\mathbf{v}\times d\mathbf{l}) \nonumber
> $$
> 按照我们此前对 $\mathbf{u}$ 和 $\mathbf{w}$ 的定义，有：
> $$
> \frac{d\Phi}{dt} = \oint\mathbf{B}\cdot(\mathbf{w}\times d\mathbf{l}) = -\oint(\mathbf{w}\times\mathbf{B})\cdot d\mathbf{l} = -\oint\mathbf{f}_\text{mag}\cdot d\mathbf{l} \nonumber
> $$
> 最后一项恰好是电动势的定义。



