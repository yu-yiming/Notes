# Electrodynamics II

本笔记是对应 *UIUC PHYS 436 Electromagnetic Fields II*，以及 *UIUC PHYS 435 Electromagnetic Fields I* 最后一部分的学习笔记，其中包括了麦克斯韦方程组、守恒定律、电磁波、辐射以及狭义相对论相关的知识点。由于本篇是物理笔记，其中的数学定理并不一定提供证明，即使有证明也不一定是严格的。主要参考了 *Introduction to Electrodynamics, 4th Edition: David J. Griffiths*，文中大量的例题、图片都来自该教材，章节的编排也基本按照这本书的设计。

[TOC]

$$
\newcommand{\marginbox}[1]{\fbox{$\hphantom{1} {#1} \vphantom{1\over1} \hphantom{1}$}}\nonumber
\newcommand{\rcur}[0]{\mathscr{r}}
\newcommand{\brcur}[0]{\boldsymbol{\mathscr{r}}}
\newcommand{\unit}[1]{\hat{\boldsymbol{#1}}}
\newcommand{\tensor}[1]{\overleftrightarrow{\boldsymbol{#1}}}
$$

## 电动力学

### 欧姆定律

为了让电荷移动产生电流，我们需要对其施加力。假设对单位电荷的力是 $\mathbf{f}$，它产生的电流密度则是：
$$
\begin{equation*}
	\mathbf{J} = \sigma \mathbf{f}
\end{equation*} \tag{7.1} \label{current-density-by-force}
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
\begin{equation*}
	\mathbf{J} = \sigma(\mathbf{E} + \mathbf{v}\times\mathbf{B})
\end{equation*} \tag{7.2} \label{current-density-by-em-field}
$$
由于多数情况下磁场都要远小于电场（反例如等离子体），因此我们省略磁场这一项：
$$
\begin{equation*}
	\marginbox{\mathbf{J} = \sigma \mathbf{E}}
\end{equation*} \tag{7.3} \label{current-density-by-electric-field}
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
\begin{equation*}
	\marginbox{V = IR}
\end{equation*} \tag{7.4} \label{ohm's-law}
$$
此处的 $R$ 称为电极之间媒介的 **电阻（Resistance）**，单位为 **欧姆（Ohm）**，记作 $\Omega$。它和媒介的类型、几何性质有关。

如果我们冷静下来仔细考虑欧姆定律的合理性，会发现它有些荒谬。电荷受到电场作用开始加速移动；它产生的电流密度也因此增加，这样下去最终的电流应该是无穷大才对？然而根据欧姆定律，这会产生一个恒定的电流（也就意味着电荷的速度是恒定的），这似乎并不合理。

实际上这是合理的，因为我们需要考虑电荷移动路径上的其他粒子，它们会对电荷造成阻力（发生“碰撞”）。一个类比就是沿着一条路加速行驶，却在每个十字路口停下来。虽然你确实一直在加速，但是每过一段距离就要重复这个从零加速的过程，因此你的 *平均速度* 是恒定的。欧姆定律中的 $I$ 正是反映了这个平均速度。让我们尝试建模计算这个平均速度。假设每段从零加速到特定速度的时间是 $t$，长度是 $\lambda$，加速度为 $a$，那么根据牛顿运动定律，下面的式子应该成立：
$$
t = \sqrt{\frac{2\lambda}{a}}
$$
由于电荷进行的是匀加速，我们有：
$$
v_\text{ave} = \frac{1}{2}at = \sqrt{\frac{\lambda a}{2}}
$$
呃，这和我们预料的可能也不太一样，因为欧姆定律任何电流应该和电势差（也即电场）大小成正比，但这里却是和电场（也即加速度）的平方根大小成正比。事实上，如果考虑热力学性质，我们应该使用下面的公式：
$$
t = \frac{\lambda}{v_\text{thermal}}
$$
此时得到的平均速度就是：
$$
v_\text{ave} = \frac{1}{2}at = \frac{a\lambda}{2v_\text{thermal}}
$$
如果假设单位体积的分子数量为 $n$，单个分子中自由电子个数为 $f$，质量为 $m$，则电流密度是：
$$
\begin{equation*}
	\mathbf{J} = nfe\mathbf{v}_\text{ave} = nfe\frac{\lambda}{2v_\text{thermal}}\frac{\mathbf{F}}{m} = \frac{nf\lambda e^2}{2mv_\text{thermal}}\mathbf{E} \tag{7.5}
\end{equation*}
$$
这是一个描述电导率的模型，其正确描述了导体电导率与移动的电荷个数以及温度的相关性，但并不是精准的公式。

此外，由于电荷运动时经历的碰撞，一部分能量会变为热能，其功率可以通过 **焦耳热定律（Joule Heating Law）** 给出：
$$
\begin{equation*}
	P = VI = I^2R
\end{equation*} \tag{7.6} \label{joule-heating-law}
$$

### 电动势

电路中，电流是由两种力产生的，一个是来自电源的力 $\mathbf{f}_s$，另一个则是用于调节电流的静电力，其依然来源于电源的力。故：
$$
\begin{equation*}
	\mathbf{f} = \mathbf{f}_s + \mathbf{E}
\end{equation*} \tag{7.7}
$$
电源中的力可能来源于化学中的作用力（电池）、光（光电池）、机械压力（压电晶体）等等。无论机制如何，围绕电路一圈的环路积分始终是：
$$
\begin{equation*}
	\marginbox{\mathcal{E} = \oint \mathbf{f}\cdot d\mathbf{l} = \oint \mathbf{f}_s\cdot d\mathbf{l}}
\end{equation*} \tag{7.8} \label{emp}
$$
其中 $\oint\mathbf{E}\cdot d\mathbf{l} = 0$ 被约去了。我们将 $\mathcal{E}$ 称为 **电动势（Electromotive Force, emf）**。最理想的情况，当电源处不存在阻力时（理想电动势源，此时有 $\sigma = \infty$），对电势的净力是 $\mathbf{f} = 0$，我们得到 $\mathbf{E} = -\mathbf{f}_s$。因此电源两端的电势差是：
$$
\begin{equation*}
	V = -\int_a^b\mathbf{E}\cdot d\mathbf{l} = \int_a^b\mathbf{f}_s\cdot d\mathbf{l} = \oint\mathbf{f}_s\cdot d\mathbf{l} = \mathcal{E}
\end{equation*} \tag{7.9}
$$
因此电路中，电源提供的电势差等于电池的电动势。我们也可以理解成，电源对单位电荷做功的大小为 $\mathcal{E}$，这样从负极到负极能够达到 $V = \mathcal{E}$ 的电势差。

#### 动生电动势

一个极为常见的电源是 **发电机（Generator）**，其中用到了 **动生电动势（Motional Emf）**的机制。下面这个非常简单的模型中，阴影部分是方向指入屏幕的磁场 $\mathbf{B}$，一个宽为 $h$ 的环形电路正在向右方以 $v$ 匀速移动，$R$ 是一个电阻为防止无穷大的电流。

<img src="graphs/ed2_7-1.png" alt="ed2_7-1" style="zoom:50%;" />

此时在切割磁感线的 $a-b$ 端会产生电动势：
$$
\begin{equation*}
	\mathcal{E} = \oint\mathbf{f}_\text{mag}\cdot d\mathbf{l} = vBh
\end{equation*} \tag{7.10} \label{emf-simple}
$$
这个电动势的方向是沿着电路顺时针的，可以使用右手定则来确认：如果用四根手指蜷曲的方向表示产生的电流在环路中的方向，那么大拇指方向和磁场一致。看起来磁场力就是电动势的来源。不过这并不准确，因为磁场从来不可能做功。实际上做功的是拉着电路运动的力，这个力用来抵消磁场对 $a-b$ 段上运动的电荷（方向为右上，同时考虑电动势的作用以及 $v$）的磁力，后者方向为左上。如果我们设 $a-b$ 段电荷竖直方向移动速度为 $\mathbf{u}$，实际移动速度为 $\mathbf{w}$，两者的夹角为 $\theta$，则单位电荷做功为：
$$
\int\mathbf{f}_\text{pull}\cdot d\mathbf{l} = (uB)\left(\frac{h}{\cos\theta}\right)\sin\theta = vBh = \mathcal{E}
$$
现在，让我们推导一个相比 $(\ref{emf-simple})$ 更加普适的电动势推导方式。首先让我们定义 **磁通量（Magnetic Flux）** 的概念：
$$
\begin{equation*}
	\marginbox{\Phi = \int\mathbf{B}\cdot d\mathbf{a}}
\end{equation*} \tag{7.11} \label{magnetic-flux}
$$
对于前面描述的简单模型，我们有 $\Phi = Bhx$，而它和电动势的关系似乎是：
$$
\begin{equation*}
	\marginbox{\mathcal{E} = -\frac{d\Phi}{dt}}
\end{equation*} \tag{7.12} \label{flux-rule}
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

### 电磁感应

#### 法拉第定律

1831 年，法拉第做了包括下面三个实验在内的多个实验

- 将一个导线环路拉出匀强磁场，后者保持不变。
- 令匀强磁场离开导线环路，后者保持静止。
- 改变匀强磁场大小，其和导线环路的相对位置不变。

上面三种情形下，导线中都会产生电流。因此法拉第认为：*变化的磁场会产生电场*。在狭义相对论的启示下，前两种情况的确是等价的，而第一种情况正是我们此前提到的动生电动势。不过当时并没有相对论的概念，因此法拉第认为第一个实验表明磁场产生了电动势（因为导线环路在运动，而磁场作用于运动的电荷），而第二个实验则表明磁场在运动时会产生电场（因为导体环路是 *静止的*，因此只可能是电场的作用）。第三个实验则证实了他的猜测。

根据法拉第的实验，电动势应该等于磁通量的变化率（这里的电动势是通过电场的环路积分定义的），即：
$$
\begin{equation*}
	\mathcal{E} = \oint \mathbf{E}\cdot d\mathbf{l} = -\frac{d\Phi}{dt} = -\int\frac{\partial \mathbf{B}}{\partial t}\cdot d\mathbf{a}
\end{equation*} \tag{7.13}
$$
这便是 **法拉第定律（Faraday's Law）**。如果利用斯托克斯定理，我们可以将其写为微分的形式：
$$
\begin{equation*}
	\nabla\times\mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}
\end{equation*} \tag{7.14} \label{faraday's-law-differential}
$$
在匀强磁场中，可以看到 $\oint \mathbf{E}\cdot d\mathbf{l} = 0$，这正是我们此前在静电学中的结论。

我们可以用 **广义通量定律（Universal Flux Rule）** 来概括上面三个实验中出现的现象，其和 $(\ref{flux-rule})$ 形式完全一致：
$$
\begin{equation*}
	\mathcal{E} = -\frac{d\Phi}{dt}
\end{equation*} \tag{7.15} \label{universal-flux-rule}
$$
即无论因为任何原因，当通过某个环路的磁通量变化时，就会在该环路上产生大小为 $\mathcal{E}$ 的电动势。这个定律在上面的三个实验中体现的方式不尽相同。第一个实验只包含磁场和洛伦兹力（此时这个电动势是“磁生”的），而后两个是通过法拉第定律得到的“电生”的电动势。两者的等价性其实相当巧合——这些都会在狭义相对论中得到解释，我们这里只要注意在经典电磁学中它们的区别即可。

如果我们观察感应电流产生的磁通量，会发现它们总是和磁通量变化量相反。比如一个减小的磁通量 $\Phi$（假定某个方向为正向），其感应产生电流的磁通量 $\Phi_i$ 总是大于 $0$ 的。这可以通过 **楞次定律（Lenz's Law）** 概括： *感应产生的磁通量总是尝试抵消改变的磁通量*。

#### 感应电场

根据我们此前的讨论，对于纯粹的法拉第电场（即完全通过磁场产生的电场），下面的等式成立：
$$
\nabla \cdot \mathbf{E} = 0 \qquad \nabla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}
$$
它们和下面的等式形式上是等价的：
$$
\nabla \cdot \mathbf{B} = 0 \qquad \nabla\times\mathbf{B} = \mu_0\mathbf{J}
$$
因此我们可以令 $\mathbf{E}, -\frac{\partial \mathbf{B}}{\partial t}$ 分别代替 $\mathbf{B}, \mu_0\mathbf{J}$，并利用毕奥·萨伐特定律得到下面等式：
$$
\begin{equation*}
	\mathbf{E} = -\frac{1}{4\pi}\int\frac{(\partial \mathbf{B}/\partial t)\times\unit{\rcur}}{\rcur^2}\,d\tau = -\frac{1}{4\pi}\frac{\partial}{\partial t}\int\frac{\mathbf{B}\times\unit{\rcur}}{\rcur^2}\,d\tau
\end{equation*} \tag{7.16} \label{biot-savart-law-electric-field}
$$
同时，类比安培定律 $\oint\mathbf{B}\cdot d\mathbf{l} = \mu_0I_\text{enc}$，我们也有积分形式的法拉第定律：
$$
\begin{equation*}
	\oint\mathbf{E}\cdot d\mathbf{l} = -\frac{d\Phi}{d t}
\end{equation*} \tag{7.17} \label{faraday's-law-integral}
$$
最后值得一题的是，在法拉第定律应用的情形总是存在一个 *变化的* 磁场，此时静磁学中我们常用的定律，如毕奥·萨伐特定律、安培定律等都不再准确。幸运的是，在磁场不剧烈震荡，或我们关心无限远处的电磁场时，利用静磁场公式得到的结果误差并不大。我们称这些可以将磁场公式在法拉第定律右侧使用的情形称为 **准静态（Quasistatic）**。通常来讲，只有在考虑电磁波和辐射时我们才不得不仔细考虑如何处理巨大的误差。

> **例**：无限长直导线上有随时间细微变化的电流 $I(t)$。求其产生的感应电场。

> **解**：按照准静态情形分析，磁场强度应为：
> $$
> \mathbf{B} = \frac{\mu_0I}{2\pi s}\unit{s}
> $$
> 再次类比磁场与电流，我们可以得到：
> $$
> \begin{align*}
> \oint\mathbf{E}\cdot d\mathbf{l} = E(s_0)l - E(s)l &= -\frac{d}{dt}\int\mathbf{B}\cdot d\mathbf{a} \\
> &= -\frac{\mu_0I}{2\pi}\frac{dI}{dt}\int_{s_0}^s\frac{1}{s'}\,ds' = -\frac{\mu_0I}{2\pi}\frac{dI}{dt}(\ln{s} - \ln{s_0})
> \end{align*}
> $$
> 因此有：
> $$
> \begin{equation*}
> 	\mathbf{E}(s) = \left(\frac{\mu_0}{2\pi}\frac{dI}{dt}\ln{s} + K\right)\unit{z}
> \end{equation*} \tag{7.18}
> $$
> 此处 $K$ 是一个和 $s$ 无关的常数，不过它还是应该和 $t$ 有关。
>
> 稍等，这个答案并不合理，难不成在无穷远处感应电场无穷大？这个结论是我们“滥用”准静态导致的。由于电磁场以光速 $c$ 运动， 因此每一时刻传递到某点 $s$ 的并不是现在的，而是一段时间以前的磁场（更确切来说，是在某个时间段内，从导线的许多不同位置传过来的磁场）。因此当 $s$ 很大时，此前我们的计算就不准确了。假设 $\tau$ 是令 $I$ 发生“大量”变化的时间，则当：
> $$
> s \ll c\tau \tag{7.19}
> $$
> 成立时，我们可以放心地假设准静态。

#### 电感

假设有两个闭合导线，其中一根导线通恒温电流 $I_1$，其产生的磁场是 $\mathbf{B}_1$。由于通电导线的形状非常复杂，我们难以求出具体的 $\mathbf{B}$，但根据毕奥·萨伐特定律可以得到：
$$
\mathbf{B}_1 = \frac{\mu_0}{4\pi}I_1\oint\frac{d\mathbf{l}_1\times\unit{\rcur}}{\rcur^2}
$$
这是和 $I_1$ 成正比的一个量。此时对于通过另一个导线形成曲面的磁通量：
$$
\begin{equation*}
	\Phi_2 = \int\mathbf{B}_1\cdot d\mathbf{a}_2 = M_{21}I_1
\end{equation*} \tag{7.20}
$$
将 $I_1$ 提出来，剩下的项 $M_{21}$ 只和导线的性质有关，我们称其为两个导线的 **互感（Mutual Inductance）**。这个称呼让我们不禁猜测 $M_{21} = M_{12}$。下面尝试推导出互感的计算公式：
$$
\begin{align*}
	\Phi_2 = \int\mathbf{B}_1\cdot d\mathbf{a}_2 = \int(\nabla\times\mathbf{A}_1)\cdot d\mathbf{a}_2 &= \oint \mathbf{A}_1\cdot d\mathbf{l}_2 \\
	&= \frac{\mu_0}{4\pi}I_1\oint\left(\oint\frac{d \mathbf{l}_1}{\rcur}\right)\cdot d\mathbf{l}_2 \\
	&= \frac{\mu_0}{4\pi}I_1\oint\oint\frac{d\mathbf{l}_1\cdot d\mathbf{l}_2}{\rcur}
\end{align*}
$$
因此互感可以表示为下面的双重积分：
$$
\begin{equation*}
	M = \frac{\mu_0}{4\pi}\oint\oint\frac{d\mathbf{l}_1 \cdot d\mathbf{l}_2}{\rcur} \tag{7.21} \label{neumann-formula}
\end{equation*}
$$
这个公式也被称为 **纽曼公式（Neumann Formula）**。虽然其应用得很少，但证实了我们前面所提到的两点：

- 互感是一个几何性质，只和导线的大小、形状和相对位置等相关。
- 互感是对称的，也即 $M_{21} = M_{12}$。也即，当任一导线通恒温电流电流 $I$ 时，其在另一根导线围成的曲面产生的磁通量等于另一根导线同样通电流 $I$ 时，在该导线围成曲面产生的磁通量。这是一个很神奇的结论。

依然是上面的设置，假设通电导线上的电流发生变化，根据法拉第定律，另一根导线应该相应产生一个感应电动势：
$$
\begin{equation*}
	\mathcal{E}_2 = -\frac{d\Phi_2}{dt} = -M\frac{dI_1}{dt} \tag{7.22}
\end{equation*}
$$
这里假设准静态，因此用到了 $(7.20)$。这个结论颇有意思：任何时候某个电流环路的变化都会在其它导线上产生电动势。事实上它自身也会受到这个变化影响，我们可以仿照之前的推理给出下面的公式：
$$
\begin{equation*}
	\Phi = LI
\end{equation*} \tag{7.23} \label{self-inductance}
$$
此处 $L$ 称为 **自感（Self Inductance）**，或 **电感（Inductance）**，其单位是 **亨利（Henry, H）**，$\text{H} = \text{V}\cdot \text{s}/\text{A}$。对于任何导线环路，在电流产生变化时，其对自己都会产生一个感应电动势，也被称为 **反电动势（Back Emf）**：
$$
\begin{equation*}
	\mathcal{E} = -L\frac{dI}{dt}
\end{equation*} \tag{7.24} \label{emf-by-self-inductance}
$$
电感是改变电流的“阻碍”，某种程度上，我们可以将其与力学中的质量相对照（后者是改变速度的“阻碍”）。

#### 磁场能量

由于反电动势的存在，我们需要一定能量才能让一个导线环路拥有一定的电流。这里我们不考虑流失的热能。由于这个功等同于对抗反电动势，因此有：
$$
\frac{dW}{dt} = -\mathcal{E}I = -LI\frac{dI}{dt}
$$
两边进行积分后得到：
$$
\begin{equation*}
	W = \frac{1}{2}LI^2
\end{equation*} \tag{7.25} \label{magnetic-field-energy}
$$
可见一个电路的能量无关于给其充能的过程，只和几何性质（电感）及最终的电流有关。和电学中得到全空间的电场能量类似，我们也可以尝试通过微积分的方法得到只和磁场强度有关的公式：
$$
LI = \Phi = \int \mathbf{B}\cdot d \mathbf{a} = \int(\nabla\times \mathbf{A})\cdot d\mathbf{a} = \oint \mathbf{A}\cdot d \mathbf{l}
$$
然后将 $LI$ 代入 $(\ref{magnetic-field-energy})$ 中：
$$
W = \frac{1}{2}I\oint\mathbf{A}\cdot d \mathbf{l} = \frac{1}{2}\oint \mathbf{A}\cdot\mathbf{I}\,dl
$$
可以将这个公式扩展到电流体密度的形式：
$$
\begin{equation*}
	W = \frac{1}{2}\int_\mathcal{V}\mathbf{A}\cdot\mathbf{J}\,d\tau
\end{equation*} \tag{7.26}
$$
下面我们将尝试用 $\mathbf{B}$ 代替 $\mathbf{A}$ 和 $\mathbf{J}$。首先根据安培定律可以进行一步化简：
$$
W = \frac{1}{2\mu_0}\int \mathbf{A}\cdot(\nabla\times \mathbf{B})\,d\tau
$$
其中的 $\mathbf{A}\cdot(\nabla\times \mathbf{B})$ 可以参考下面的乘法定律：
$$
\nabla\cdot(\mathbf{A}\times\mathbf{B}) = \mathbf{B}\cdot(\nabla\times\mathbf{A}) - \mathbf{A}\cdot(\nabla\times \mathbf{B})
$$
再根据磁矢势定理，最终有：
$$
W = \frac{1}{2\mu_0}\left(\int_\mathcal{V} B^2\,d\tau - \oint_\mathcal{S}(\mathbf{A}\times\mathbf{B})\cdot d\mathbf{a}\right)
$$
其中 $\mathcal{S}$ 是 $\mathcal{V}$ 周围的曲面。这里我们还是采用和分析电场能量时一样的方法，将 $\mathcal{V}$ 取全空间，此时第二项会无限趋近于 $0$（道理是一样的，面积增长不如势和场之积下跌的速度快），因此第一项就是全空间的磁场能量：
$$
\begin{equation*}
	W = \frac{1}{2\mu_0}\int B^2\,d\tau
\end{equation*} \tag{7.27} \label{magnetic-field-energy-all-space}
$$
看到这里可能会有一个疑问，磁场并不做功，那么构建磁场为什么需要能量呢？答案就在我们推导的过程当中：尽管磁场不做功，但是变化的磁场会产生电场，而电场会做功 *抵抗* 我们构建磁场的行为。

### 麦克斯韦方程组

#### 麦克斯韦对安培定律的修正

至此，我们已经几乎引入了麦克斯韦方程组的所有内容。事实上这正是麦克斯韦之前的电磁场方程，其描述了电场和磁场的散度与梯度：
$$
\begin{align*}
	\text{(i)}& \quad \nabla\cdot \mathbf{E} = \frac{1}{\epsilon_0}\rho \tag{高斯定律}\\
	\text{(ii)}& \quad \nabla\cdot \mathbf{B} = 0 \\
	\text{(iii)}& \quad \nabla\times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t} \tag{法拉第定律} \\
	\text{(iv)}& \quad \nabla\times \mathbf{B} = \mu_0\mathbf{J} \tag{安培定律}
\end{align*}
$$
然而，这几个公式之间有不一致的地方：它们理应满足矢量微积分中的运算规律（如旋度的散度恒为 $0$），但考虑安培定律在非恒稳电流的情形（实际上此时该定律是失效的）：
$$
\nabla\cdot(\nabla\times\mathbf{B}) = \mu_0(\nabla\cdot \mathbf{J})
$$
左侧恒为 $0$，但右侧确不恒为 $0$。为了解决这一点，我们可以先将电流密度的散度求出来：
$$
\nabla\cdot \mathbf{J} = -\frac{\partial \rho}{\partial t} = -\frac{\partial}{\partial t}(\epsilon_0\nabla\cdot \mathbf{E}) = -\nabla\cdot\left(\epsilon_0\frac{\partial \mathbf{E}}{\partial t}\right)
$$
因此，安培定律可以被修正为：
$$
\begin{equation*}
	\marginbox{\nabla\times\mathbf{B} = \mu_0\mathbf{J} + \mu_0\epsilon_0\frac{\partial \mathbf{E}}{\partial t}}
\end{equation*} \tag{7.28} \label{revised-ampere's-law}
$$
这个谜一样的辅助项是符合事实的（毕竟谁都不能违背数学定律），但在法拉第的时代始终没有被发现。这是因为它在通常的电磁学实验中很难检测出来。不过涉及电磁波的传播时，这一项会变得至关重要。同时，这一项和法拉第定律有对称的美感：它暗示变化的电场同样也能产生磁场。

我们定义 **位移电流（Displacement Current）** 为：
$$
\begin{equation*}
	\marginbox{\mathbf{J}_d = \epsilon_0\frac{\partial \mathbf{E}}{\partial t}}
\end{equation*} \tag{7.29} \label{displacement-current}
$$
它大体可以理解成某一时刻电流偏移量的微元（更合适的名字或许是位移电流密度，毕竟它并不是电流），在安培定律理应适用却得出不合理结果时可以作为辅助。作为例子，可以参考一个闭合电路上的一个电容。被导线穿过的长方形安培环路上总有：
$$
\oint \mathbf{B}\cdot d\mathbf{l} = \mu_0I_\text{enc}
$$
然而如果考虑一个从电容之间通过的平面，其边缘和之前我们选择的环路一样，此时穿过该平面的电流为 $0$，电场也应该是$0$ 才对，按照静电学中的安培定律，这似乎是一个悖论。不过现在我们有位移电流的帮助，便有：
$$
\begin{equation*}
	\oint\mathbf{B}\cdot d\mathbf{l} = \mu_0I_\text{enc} + \mu_0\epsilon_0\int\left(\frac{\partial \mathbf{E}}{\partial t}\right)\cdot d\mathbf{a}
\end{equation*} \tag{7.30}
$$
即使选择的平面令 $I_\text{enc} = 0$，位移电流这一项依然可以积分出正确的结果。

#### 麦克斯韦方程组

现在我们得到了麦克斯韦方程的完整清单：
$$
\begin{split}
	\text{(i)}& \quad \nabla\cdot \mathbf{E} = \frac{1}{\epsilon_0}\rho & \text{（高斯定律）}\\
	\text{(ii)}& \quad \nabla\cdot \mathbf{B} = 0 \\
	\text{(iii)}& \quad \nabla\times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t} &  \text{（法拉第定律）} \\
	\text{(iv)}& \quad \nabla\times \mathbf{B} = \mu_0\mathbf{J} + \mu_0\epsilon_0\frac{\partial \mathbf{E}}{\partial t} \qquad & \text{（麦克斯韦修正的安培定律）}
\end{split} \tag{7.31} \label{maxwell's-equations}
$$
再加上运动方程，即洛伦兹力定律：
$$
\begin{equation*}
	\mathbf{F} = q(\mathbf{E} + \mathbf{v}\times\mathbf{B})
\end{equation*} \tag{7.32} \label{lorentz-force-law}
$$
我们至此便总结了经典电动力学中的所有公式。所有其它公式都可以从上面的公式推出。下面我们将给出该方程组的一些变形。首先，电磁场的来源 *只可能* 是电荷或电流。因此如果将“源”统一写在右侧，其它项（电磁场的梯度、散度和变化率）写在左侧，麦克斯韦方程组是下面的形式：
$$
\begin{matrix}
	\text{(i)} \quad \nabla\cdot \mathbf{E} = \dfrac{1}{\epsilon_0}\rho & \quad &
	\text{(iii)}  \quad \nabla\times\mathbf{E} + \dfrac{\partial \mathbf{B}}{\partial t} = \mathbf{0} \phantom{xxxxx} \\
	\text{(ii)} \quad \nabla\cdot \mathbf{B} = 0 \phantom{xx} & \quad & \text{(iv)} \quad \nabla\times\mathbf{B} - \mu_0\epsilon_0\dfrac{\partial \mathbf{E}}{\partial t} = \mu_0\mathbf{J}
\end{matrix} \tag{7.33}
$$
总得来说，麦克斯韦方程组描述了电磁场是如何由电荷产生的，而洛伦兹力公式描述了电磁场如何反过来作用于电荷。

#### 磁荷

当考虑 $\rho = 0$ 且 $\mathbf{J} = \mathbf{0}$ 时，麦克斯韦方程组会变为高度对称的形式：
$$
\begin{matrix}
	\nabla \cdot \mathbf{E} = 0 & \quad\quad & \nabla\times\mathbf{E} = -\dfrac{\partial \mathbf{B}}{\partial t} \phantom{xx} \\
	\nabla \cdot \mathbf{B} = 0 & \quad\quad & \nabla\times\mathbf{B} = \mu_0\epsilon_0\dfrac{\partial \mathbf{E}}{\partial t}
\end{matrix}
$$
此时如果将 $\mathbf{E}$ 用 $\mathbf{B}$ 代替，将 $\mathbf{B}$ 用 $-\mu_0\epsilon_0\mathbf{E}$ 代替，我们会发现上下两组方程对换了。然而这个对称性在引入非零的 $\rho$ 与 $\mathbf{J}$ 就会被轻易破坏。这让我们不禁希望在麦克斯韦方程组中加上虚拟的“磁荷”与“磁流“项：
$$
\begin{matrix}
	\text{(i)} \quad \nabla\cdot\mathbf{E} = \dfrac{1}{\epsilon_0}\rho_e & \quad\quad\quad &
	\text{(iii)} \quad \nabla\times\mathbf{E} = -\mu_0\mathbf{J}_m - \dfrac{\partial \mathbf{B}}{\partial t} \phantom{xx}\\
	\text{(ii)} \quad \nabla\cdot\mathbf{B} = \mu_0\rho_m & \quad\quad\quad & \text{(iv)} \quad \nabla\times \mathbf{B} = \mu_0\mathbf{J}_e + \mu_0\epsilon_0\dfrac{\partial \mathbf{E}}{\partial t}
\end{matrix} \tag{7.34} \label{maxwell's-equations-with-magnetic-charge}
$$
其中 $\rho_e$ 与 $\mathbf{J}_e$ 则等同于此前的 $\rho$ 和 $\mathbf{J}$，而 $\rho_m$ 和 $\mathbf{J}_m$ 是我们新引入的量。然而，迄今为止没有任何一项物理实验中得到非零的磁荷与磁流，磁矢势多极展开的单极子项始终是 $0$。这或许暗示我们自然界中，*的确* 不存在与电荷相对应的磁荷。

#### 物质中的麦克斯韦方程组

当考虑到物质中的电极化和磁极化现象时，我们需要使用束缚电荷和束缚电流这两个概念，这是我们在静电学和静磁学中就熟悉的内容：
$$
\begin{align*}
	\rho_b = -\nabla\cdot\mathbf{P} && \mathbf{J}_b = \nabla\times \mathbf{M}
\end{align*} \tag{7.35}
$$
但在非静态情形下，束缚电荷也会产生电流。考虑一个极小的极化物体，其携带的束缚电荷面密度 $\sigma_b = P$，方向在两边完全相反。此时如果 $P$ 进行了微小变化，两边的电流变化等价于：
$$
dI = \frac{\partial \sigma_b}{\partial t}da_\perp = \frac{\partial P}{\partial t}da_\perp
$$
将 $da_\perp$ 移到左侧，我们就得到了电极化产生的电流体密度：
$$
\begin{equation*}
	\mathbf{J}_p = \frac{\partial \mathbf{P}}{\partial t}
\end{equation*} \tag{7.36} \label{polarization-current}
$$
注意 $\mathbf{J}_p$ 和束缚电流 $\mathbf{J}_b$ 没有任何关系。前者是电极化变化时，电荷的运动导致的；后者则通常和带电物体的旋转相关。不难验证极化电流满足守恒性：
$$
\nabla\cdot\mathbf{J}_p = \nabla\cdot\frac{\partial \mathbf{P}}{\partial t} = \frac{\partial}{\partial t}(\nabla\cdot\mathbf{P}) = -\frac{\partial \rho_b}{\partial t}
$$
可见极化电流保证了束缚电荷的守恒性。

另一边，磁极化发生变化时，并不会产生额外的”极化磁流“（诸如此类的东西），而是会直接影响 $\mathbf{J}_b$。至此，我们得到了完整的电荷密度与电流密度公式：
$$
\begin{align*}
\begin{split}
	\rho &= \rho_f + \rho_b = \rho_f - \nabla\cdot\mathbf{P} \\
	\mathbf{J} &= \mathbf{J}_f + \mathbf{J}_b + \mathbf{J}_p = \mathbf{J}_f + \nabla\times\mathbf{M} + \frac{\partial \mathbf{P}}{\partial t}
\end{split}
\end{align*} \tag{7.37}
$$
将它们代入原始的麦克斯韦方程组，我们得到：
$$
\begin{matrix}
	\text{(i)} \quad \nabla\cdot\mathbf{D} = \rho_f & \quad\quad & 
	\text{(iii)} \quad \nabla\times\mathbf{E} = - \dfrac{\partial \mathbf{B}}{\partial t} \phantom{xxx} \\
	\text{(ii)} \quad \nabla\cdot\mathbf{B} = 0 \phantom{x}& \quad\quad & 
	\text{(iv)} \quad \nabla\times\mathbf{H} = \mathbf{J}_f + \dfrac{\partial \mathbf{D}}{\partial t}
\end{matrix} \tag{7.38} \label{maxwell's-equations-in-matter}
$$
相比原来的方程组，常数的噪声消失了（$\epsilon_0$ 和 $\mu_0$），但也因此引入了两个新的场，某种程度上让公式成分更加复杂。对于线性介质，我们有：
$$
\begin{align*}
	\mathbf{D} = \epsilon\mathbf{E} && \mathbf{H} = \frac{1}{\mu}\mathbf{B}
\end{align*} \tag{7.39}
$$
其中 $\epsilon = \epsilon_0(1 + \chi_e)$，$\mu = \mu_0(1 + \chi_m)$，而 $\chi_e, \chi_m$ 和介质特性有关。

#### 边界条件

这一章中，我们似乎一直没有提到边界条件。但边界条件对求解电磁场是必须的，因为所有电磁场都符合 $(\ref{maxwell's-equations-in-matter})$，只有边界条件将它们区分开。为了更好理解，我们首先列出麦克斯韦方程组的积分形式：
$$
\begin{matrix}
	\displaystyle \text{(i)} \quad \oint_\mathcal{S} \mathbf{D}\cdot d\mathbf{a} = Q_\text{enc} & \quad\quad &
	\displaystyle \text{(iii)} \quad \oint_\mathcal{P} \mathbf{E}\cdot d\mathbf{l} = -\frac{d}{dt}\int_\mathcal{S}\mathbf{B}\cdot d\mathbf{a} \phantom{xxxxx} \\
	\displaystyle \text{(ii)} \quad \oint_\mathcal{S}\mathbf{B}\cdot d\mathbf{a} = 0 \phantom{xxx} & \quad\quad &
	\displaystyle \text{(iv)} \quad \oint_\mathcal{P} \mathbf{H}\cdot d\mathbf{l} = I_{f\text{enc}} + \frac{d}{dt}\int_\mathcal{S}\mathbf{D}\cdot d\mathbf{a}
\end{matrix} \tag{7.40} \label{maxwell's-equations-integral}
$$
考虑一个极薄的包围带电平面的立方体，则根据高斯定律有：
$$
\mathbf{D}_\text{top}\cdot\mathbf{a} - \mathbf{D}_\text{bottom}\cdot\mathbf{a} = \sigma_fa
$$
因此：
$$
D_\text{top}^\perp - D_\text{bottom}^\perp = \sigma_f \tag{7.41}
$$
类似地，我们可以轻松得到：
$$
B_\text{top}^\perp - B_\text{bottom}^\perp = 0 \tag{7.42}
$$
接着，考虑一个极小的通过通电平面的环路，我们有：
$$
\mathbf{E}_\text{top}\cdot\mathbf{l} - \mathbf{E}_\text{bottom}\cdot\mathbf{l} = -\frac{d}{dt}\int_\mathcal{S}\mathbf{B}\cdot d\mathbf{a}
$$
右边项在 $\mathcal{S}\to 0$ 时也趋于 $0$，因此我们有：
$$
\mathbf{E}_\text{top}^\parallel - \mathbf{E}_\text{bottom}^\parallel = \mathbf{0} \tag{7.43}
$$
类似地，对于 $\mathbf{H}$ 右侧非零的只有：
$$
I_{f\text{enc}} = \mathbf{K}_f\cdot(\unit{n}\times\mathbf{l}) = (\mathbf{K}_f\times\unit{n})\cdot\mathbf{l}
$$
因此：
$$
\mathbf{H}_\text{top}^\parallel - \mathbf{H}_\text{bottom}^\parallel = \mathbf{K}_f\times\unit{n} \tag{7.44}
$$
特别地，当线性介质中且不存在自由电荷或自由电流时，所有场都是连续的。



## 守恒律

本章我们将探索电动力学中守恒的量，即能量、动量和角动量。我们在此前应该已经见识过电荷的守恒性，这里再推导一遍。首先，单位时间内通过体积 $\mathcal{V}$ 的边界 $\mathcal{S}$ 的电荷量是：
$$
\begin{equation*}
	\frac{dQ}{dt} = -\oint_\mathcal{S}\mathbf{J}\cdot d\mathbf{a}
\end{equation*} \tag{8.1}
$$
利用 $dQ = \rho\,d\tau$ 以及散度定理，我们可以得到：
$$
\begin{equation*}
	\int_\mathcal{V}\frac{\partial \rho}{\partial t}\,d\tau = -\int_\mathcal{V}\nabla\cdot\mathbf{J}\cdot d\tau
\end{equation*} \tag{8.2}
$$
去掉两遍的积分，就有了电荷守恒定律：
$$
\begin{equation*}
	\marginbox{\frac{\partial \rho}{\partial t} = -\nabla \cdot\mathbf{J}}
\end{equation*} \tag{8.3} \label{conservation-of-charge}
$$
尽管我们是通过常识推出的这个结论，它也可以通过麦克斯韦方程推出（上一章中有提到过）。接下来，我们将使用类似地方法描述能量、动量和角动量的守恒性，因此需要建立例如“能量密度”、“动量密度”和“角动量密度”，以及“能量流”、“动量流”、“角动量流”这样的物理量来对应电荷和电流。

### 能量守恒

#### 坡印廷定理

在静电场的能量这一节中（参见上一篇笔记），我们推导出了静电荷分布在全空间的能量：
$$
W_e = \frac{\epsilon_0}{2}\int E^2\,d\tau
$$
我们也通过电磁感应推出了磁场在全空间的能量：
$$
W_m = \frac{1}{2\mu_0}\int B^2\,d\tau
$$
因此将两个公式综合起来就是电磁场在全空间的能量密度：
$$
\begin{equation*}
	\marginbox{u = \frac{1}{2}\left(\epsilon_0E^2 + \frac{1}{\mu_0}B^2\right)}
\end{equation*} \tag{8.4} \label{electromagnetic-energy-density}
$$
接下来我们将验证这个公式，并给出能量在电动力学的守恒律。假设给定一个电荷和电流的设置使得电磁场可以由两个时间相关的函数 $\mathbf{E}$ 和 $\mathbf{B}$ 描述。根据洛伦兹力定律，我们可以给出一个微元时间内电磁场对其中一个电荷 $q$ 的做功：
$$
dW = \mathbf{F}\cdot d\mathbf{l} = q(\mathbf{E} + \mathbf{v}\times\mathbf{B})\cdot \mathbf{v}\,dt = q\mathbf{E}\cdot\mathbf{v}\,dt
$$
这里再次展现出磁场力不做功的特性。对于电荷的微元，我们有 $dq = \rho\,d\tau$，且 $\rho\mathbf{v} = \mathbf{J}$。因此在体积 $\mathcal{V}$ 中的电荷受到做功的功率是：
$$
\begin{equation*}
	\frac{dW}{dt} = \int_\mathcal{V}\mathbf{E}\cdot\mathbf{J}\,d\tau
\end{equation*} \tag{8.5}
$$
从这个式子我们也可以看出单位时间内电磁场对单位体积中电荷的做功是 $\mathbf{E}\cdot\mathbf{J}$。利用安培-麦克斯韦定律 $(\ref{revised-ampere's-law})$，我们可以将 $\mathbf{J}$ 换为 $\mathbf{E}$ 和 $\mathbf{B}$：
$$
\mathbf{E}\cdot\mathbf{J} = \frac{1}{\mu_0}\mathbf{E}\cdot(\nabla\times\mathbf{B}) - \epsilon_0\mathbf{E}\cdot\frac{\partial \mathbf{E}}{\partial t} \tag{8.6}
$$
再由矢量微分的乘法定律：
$$
\nabla\times(\mathbf{E}\times\mathbf{B}) = \mathbf{B}\cdot(\nabla\times\mathbf{E}) - \mathbf{E}\cdot(\nabla\times\mathbf{B})
$$
利用法拉第定律 $(\ref{faraday's-law-differential})$，可以得到：
$$
\mathbf{E}\cdot(\nabla\times\mathbf{B}) = -\mathbf{B}\cdot\frac{\partial \mathbf{B}}{\partial t} - \nabla\cdot(\mathbf{E}\times\mathbf{B})
$$
注意到下面这个有趣的微分规则：
$$
\mathbf{F}\cdot\frac{\partial \mathbf{F}}{\partial t} = \frac{1}{2}\frac{\partial }{\partial t}(F^2)
$$
因此将我们可以将 $(8.6)$ 代换为：
$$
\begin{equation*}
	\mathbf{E}\cdot\mathbf{J} = -\frac{1}{2}\frac{\partial}{\partial t}\left(\epsilon_0 E^2 + \frac{1}{\mu_0}B^2\right) - \frac{1}{\mu_0}\nabla\cdot(\mathbf{E}\times\mathbf{B})
\end{equation*} \tag{8.7}
$$
从这个式子的形式我们就能意识到离最终结果很近了。对两边进行积分可以得到：
$$
\begin{equation*}
	\frac{dW}{dt} = -\frac{d}{dt}\int_\mathcal{V}\frac{1}{2}\left(\epsilon_0 E^2 + \frac{1}{\mu_0}B^2\right) \,d\tau - \frac{1}{\mu_0}\oint_\mathcal{S}(\mathbf{E}\times\mathbf{B})\cdot d\mathbf{a}
\end{equation*} \tag{8.8} \label{poynting's-theorem}
$$
这就是 **坡印廷定理（Poynting's Theorem）**，是电动力学中的功能原理。其中第一项就是体积 $\mathcal{V}$ 中电磁场的能量的变化率，而第二项（根据能量守恒的思想）显然应该反映了能量运出这个体积边界 $\mathcal{S}$ 的速率。我们将它的微分，也即单位时间内被电磁场从单位面积传送出去的能量称为 **坡印廷矢量（Poynting Vector）**：
$$
\begin{equation*}
	\marginbox{\mathbf{S} \equiv \frac{1}{\mu_0}(\mathbf{E}\times\mathbf{B})}
\end{equation*} \tag{8.9} \label{poynting-vector}
$$
因此 $\mathbf{S}\cdot d\mathbf{a}$ 就是通过某个微元面积的法矢量，即“能量通量”。所以我们也可以将 $\mathbf{S}$ 称为 **能量通量密度（Energy Flux Density）**。至此，电磁场的功率可以写成下面的形式：
$$
\begin{equation*}
	\frac{dW}{dt} = -\frac{d}{dt}\int_\mathcal{V}u\,d\tau - \oint_\mathcal{S}\mathbf{S}\cdot d\mathbf{a}
\end{equation*} \tag{8.10}
$$
当 $\mathcal{V}$ 以内不受功的影响，比如它除了边界之外不存在任何电荷时，我们实际上没有做任何功。此时：
$$
\int\frac{\partial u}{\partial t}\,d\tau = -\oint\mathbf{S}\cdot d\mathbf{a} = -\int\nabla\cdot \mathbf{S}\,d\tau
$$
去掉两边的积分，得到：
$$
\begin{equation*}
	\marginbox{\frac{\partial u}{\partial t} = -\nabla\cdot \mathbf{S}}
\end{equation*} \tag{8.12} \label{conservation-of-energy}
$$
这就是电磁场中的能量守恒定律。如果和电荷守恒定律进行对比，不难发现能量密度 $u$ 和电荷密度 $\rho$ 以及能量通量密度 $\mathbf{S}$ 和电流密度 $\mathbf{J}$ 之间的联系。

当然，在大部分情况下，电磁能是没办法守恒的：只要存在任何电荷，就会对其做功，发生能量转换。

### 动量守恒

#### 电动力学中的牛顿第三定律

如果你有过尝试构建简单的电动力学模型，就会发现磁场力似乎不满足牛顿第三定律：假设两个在某一时刻分别沿着 $x$ 和 $y$ 轴向原点移动的电荷 $q_1$ 和 $q_2$，它们受到对方产生的电场和磁场如下：

<img src="graphs/ed2_8-1.png" alt="ed2_8-1" style="zoom:70%;" />

电场力之间是方向相反的，但是磁场力并非如此……难道要抛弃牛顿第三定律？这就意味着我们也要抛弃动量守恒！我们不可能做出这样的妥协。为了让电动力学中的动量守恒，我们需要定义电磁场的动量。经历了电磁场的能量之后，这可能也没有多令人惊讶了。

#### 麦克斯韦应力张量

下面让我们从数学上推导电磁场对其中电荷的力，首先从洛伦兹力定律开始：
$$
\mathbf{F} = \int_\mathcal{V}(\mathbf{E} + \mathbf{v}\times\mathbf{B})\rho\,d\tau = \int_\mathcal{V}(\rho\mathbf{E} + \mathbf{J}\times\mathbf{B})\,d\tau \tag{8.13}
$$
我们将其中单位体积的力单独提出来分析。根据高斯定律和安培-麦克斯韦定律可以得到：
$$
\mathbf{f} = \epsilon_0(\nabla\cdot\mathbf{E})\mathbf{E} + \left(\frac{1}{\mu_0}\nabla\times\mathbf{B} - \epsilon_0\frac{\partial \mathbf{E}}{\partial t}\right)\times \mathbf{B}
$$
根据下面的等式：
$$
\frac{\partial}{\partial t}(\mathbf{E}\times\mathbf{B}) = \frac{\partial \mathbf{E}}{\partial t}\times\mathbf{B} + \mathbf{E}\times\frac{\partial \mathbf{B}}{\partial t}
$$
以及法拉第定律 $(\ref{faraday's-law-differential})$，我们有：
$$
\frac{\partial \mathbf{E}}{\partial t}\times\mathbf{B} = \frac{\partial }{\partial t}(\mathbf{E}\times\mathbf{B}) + \mathbf{E}\times(\nabla\times\mathbf{E})
$$
根据下面的等式：
$$
\mathbf{F}\times(\nabla\times\mathbf{F}) = \frac{1}{2}\nabla(F^2) - (\mathbf{F}\cdot\nabla)\mathbf{F}
$$
我们可以将 $\mathbf{E}$ 和 $\mathbf{B}$ 都代入里面的 $\mathbf{F}$。这样就得到了最后的等式：
$$
\mathbf{f} = \epsilon_0 [(\nabla\cdot\mathbf{E})\mathbf{E} + (\mathbf{E}\cdot\nabla)\mathbf{E}] + \frac{1}{\mu_0}[(\nabla\cdot\mathbf{B})\mathbf{B} + (\mathbf{B}\cdot\nabla)\mathbf{B}] - \frac{1}{2}\nabla\left(\epsilon_0 E^2 + \frac{1}{\mu_0}B^2\right) - \epsilon_0\frac{\partial}{\partial t}(\mathbf{E}\times\mathbf{B}) \tag{8.14}
$$
这也太复杂了……好在我们可以引入 **麦克斯韦应力张量（Maxwell Stress Tensor）** 来显著简化上面的式子。记：
$$
\begin{equation*}
	T_{ij} \equiv \epsilon_0\left(E_iE_j - \frac{1}{2}\delta_{ij} E^2\right) + \frac{1}{\mu_0}\left(B_iB_j - \frac{1}{2}\delta_{ij} B^2\right)
\end{equation*} \tag{8.15} \label{maxwell-stress-tensor}
$$
其中 $\delta_{ij}$ 被称为 **克罗内克符号（Kronecker Delta）**，它仅在 $i = j$ 时取 $1$，也即 $\delta_{xx} = \delta_{yy} = \delta_{zz} = 1$。是故：
$$
T_{xx} = \frac{1}{2}\epsilon_0(E_x^2 - E_y^2 - E_z^2) + \frac{1}{2\mu_0}(B_x^2 - B_y^2 - B_z^2) \\
T_{xy} = \epsilon_0(E_xE_y) + \frac{1}{\mu_0}(B_xB_y)
$$
由于 $T_{ij}$ 有两个下标，相当于描述了两个方向（对比矢量只描述一个方向），我们将其称为二阶张量，并记为 $\tensor{T}$。二阶张量可以和矢量进行点积，但是两个方向得到的结果不同：
$$
\left(\mathbf{a}\cdot\tensor{T}\right)_j = \sum_{i = x, y, z} a_{i}T_{ij} \qquad \left(\tensor{T}\cdot\mathbf{a}\right)_j = \sum_{i = x, y, z}T_{ji}a_i \tag{8.16}
$$
从这个运算规则我们可以得到：
$$
\left(\nabla\cdot\tensor{T}\right)_j = \epsilon_0\left[(\nabla\cdot\mathbf{E})E_j + (\mathbf{E}\cdot\nabla)E_j - \frac{1}{2}\nabla_jE^2\right] + \frac{1}{\mu_0}\left[(\nabla\cdot\mathbf{B})B_j + (\mathbf{B}\cdot\nabla)B_j - \frac{1}{2}\nabla_jB^2\right]
$$
我们再和 $(8.14)$ 结合，得到：
$$
\begin{equation*}
	\mathbf{f} = \nabla\cdot\tensor{T} - \epsilon_0\mu_0\frac{\partial \mathbf{S}}{\partial t}
\end{equation*} \tag{8.17}
$$
对两边进行积分，回到我们一开始的洛伦兹力：
$$
\begin{equation*}
	\mathbf{F} = \oint_\mathcal{S}\tensor{T}\cdot d\mathbf{a} - \epsilon_0\mu_0\frac{d}{dt}\int_\mathcal{V}\mathbf{S}\,d\tau
\end{equation*} \tag{8.18}
$$
在静态情况下第二项（变化率）为零，此时力可以表示为体积 $\mathcal{V}$ 的边界 $\mathcal{S}$ 处张量 $\tensor{T}$ 的环面积分：
$$
\begin{equation*}
	\mathbf{F} = \oint_\mathcal{S}\tensor{T}\cdot d\mathbf{a}
\end{equation*} \tag{8.19}
$$
因此张量 $\tensor{T}$ 在物理上的意义是在某个平面书上单位面积的力，即 **应力（Stress）**。$T_{ij}$ 代表的是在 $i$ 方向单位面积的作用力对 $j$ 方向的平面分量的作用。因此 $T_{xx}, T_{yy}, T_{zz}$ 代表的是 $x, y, z$ 方向上的 **压强（Pressure）**，而 $T_{xy}, T_{yz}$ 等则代表了不同方向上的 **剪应力（Shear）**。

#### 动量守恒

牛顿第二定律将作用在某物体上的力描述为其动量的变化率，即：
$$
\mathbf{F} = \frac{d\mathbf{p_\text{mech}}}{dt} = -\epsilon_0\mu_0\frac{d}{dt}\int_\mathcal{V}\mathbf{S}\,d\tau + \oint_\mathcal{S}\tensor{T}\cdot d\mathbf{a} \tag{8.20}
$$
这里的 $\mathbf{p}_\text{mech}$ 指的是粒子在体积 $\mathcal{V}$ 中的机械动量。我们不难注意到它和坡印廷定理 $(\ref{poynting's-theorem})$ 的相似性，因此也可以进行类似的解释：第一个积分项代表了体积  $\mathcal{V}$ 中电磁场携带的动量：
$$
\begin{equation*}
	\mathbf{p} = \mu_0\epsilon_0\int_\mathcal{V}\mathbf{S}\,d\tau
\end{equation*} \tag{8.21}
$$
第二个积分项则是单位时间从边界 $\mathcal{S}$ 流出的动量。如果我们将单位体积电磁场的动量记为：
$$
\begin{equation*}
	\mathbf{g} = \epsilon_0\mu_0\mathbf{S} = \epsilon_0(\mathbf{E}\times\mathbf{B})
\end{equation*} \tag{8.22} \label{electromagnetic-momentum}
$$
随后不难得到：
$$
\begin{equation*}
	\marginbox{\frac{\partial \mathbf{g}}{\partial t} = \nabla\cdot\tensor{T}}
\end{equation*} \tag{8.23} \label{conservation-of-momentum}
$$
这就是电磁场中的动量守恒定律。如果探究这个式子左右的含义，就会发现 $\epsilon_0\mu_0\mathbf{S}$ 可以理解为单位体积电磁场的动量，同时 $\mathbf{S}$ 也是单位面积流出的能量（根据能量守恒式）；$-\tensor{T}$ 可以理解为单位面积流出的动量，但 $\tensor{T}$ 也是作用于某平面的应力（根据其定义）。

#### 角动量

至此我们已经得到了电磁场的能量公式 $(\ref{electromagnetic-energy-density})$ 以及动量公式 $(\ref{electromagnetic-momentum})$。角动量可以从动量计算得到：
$$
\begin{equation*}
	\mathbf{\mathscr{l}} = \mathbf{r}\times\mathbf{g} = \epsilon_0\mathbf{r}\times(\mathbf{E}\times\mathbf{B})
\end{equation*} \tag{8.24} \label{electromagnetic-anguar-momentum}
$$
不难发现即使是静态的电磁场依然有动量和角动量（因为 $\mathbf{E}\times\mathbf{B}$ 不为零）。因此只有将电磁场的这些量考虑进来，系统的总动量和总角动量才能守恒。这多少也解释了本节开头牛顿第三定律在电磁场中“不成立”的原因。

#### 磁场不做功

