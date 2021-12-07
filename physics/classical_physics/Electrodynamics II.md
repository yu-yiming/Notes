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
此处的 $R$ 称为电极之间媒介的 **电阻（Resistance）**，单位为 **欧姆（Ohm）**，记作 $\ohm$。它和媒介的类型、几何性质有关。

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
其中我们把 $\oint\mathbf{E}\cdot d\mathbf{l} = 0$ 约去了。我们将 $\mathcal{E}$ 称为 **电动势（Electromotive Force, emf）**。最理想的情况，当电源处不存在阻力时（理想电动势源，此时有 $\sigma = \infty$），对电势的净力是 $\mathbf{f} = 0$，我们得到 $\mathbf{E} = -\mathbf{f}_s$。因此电源两端的电势差是：
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
最后值得一题的是，在法拉第定律应用的情形总是存在一个 *变化的* 磁场，此时静磁学中我们常用的定律，如毕奥·萨伐特定律、安培定律等都不再准确。不过幸运的是，在磁场不剧烈震荡，或我们关心无限远处的电磁场时，利用静磁场公式得到的结果误差并不大。我们称这些可以将磁场公式在法拉第定律右侧使用的情形称为 **准静态（Quasistatic）**。通常来讲，只有在考虑电磁波和辐射时我们才不得不仔细考虑如何处理巨大的误差。

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
