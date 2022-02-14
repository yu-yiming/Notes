# Intro to Quantum Mechanics I

本篇是对应 *UIUC PHYS 486 Quantum Physics I* 的学习笔记，其中包括了波函数等知识。参考书籍是 *Introduction to Quantum Mechanics, 3rd Edition: David J. Griffiths*。

[TOC]

$$
\newcommand{\marginbox}[1]{\fbox{$\hphantom{1} {#1} \vphantom{1\over1} \hphantom{1}$}}\nonumber
\newcommand{\unit}[1]{\hat{\boldsymbol{#1}}}
\newcommand{\bracket}[1]{\left\langle {#1} \right\rangle}
$$



## 波函数与统计基础

### 薛定谔方程

经典力学中，我们用函数 $F(x, t)$ 来描述一维空间中粒子的受力。通过牛顿第二定律 $F = m\ddot{x}$ 解出其运动方程 $x(t)$ 后，我们可以进一步得到速度 $v = \dot{x}$，动量 $p = m\dot{x}$，以及动能 $T = \frac{1}{2}m\dot{x}^2$。如果 $F$ 是一个保守力（我们通常只对它们感兴趣），那就可以定义势能 $V = -\int F\,dx$。因此牛顿第二定律等效于：
$$
\begin{equation*}
	\frac{d^2x}{dt^2} = -\frac{\partial V}{\partial x}
\end{equation*} \tag{1.1} \label{newton's-law}
$$
在量子力学中，我们处理一维粒子的方式会“怪异”许多。波函数 $\Psi(x, t)$ 代替 $x(t)$ 成为描述粒子运动性质的方程，其满足的方程即是 **薛定谔方程（Schrodinger Equation）**：
$$
\begin{equation*}
	i\hbar\frac{\partial \Psi}{\partial t} = -\frac{\hbar^2}{2m}\frac{\partial^2\Psi}{\partial x^2} + V\Psi
\end{equation*} \tag{1.2} \label{schrodinger-equation}
$$
本章的主要内容就是探究波函数的意义，并和经典力学类似，从波函数逐渐得到量子力学中速度、动量以及动能等概念的定义。

### 波函数的统计理解

按照经典力学的诠释，粒子在某一时刻一定出现在某个位置，这也是为什么通过运动方程 $x(t)$ 就能通晓某个粒子任意时刻的状态。然而量子力学中，粒子是分布在全空间中的（在一维情况下就是整条数轴上），波函数模的平方 $|\Psi(x, t)|^2$ 描述的是粒子某一时刻在某点出现的概率。之所以使用模，是因为波函数可能是一个复变函数。因此，在给定范围内粒子出现的概率是：
$$
P(a < x < b) = \int_a^b |\Psi(x, t)|^2\,dx
$$
从图像上来看，这个概率就是 $|\Psi|^2$ 曲线下方围成的面积。为了让波函数定义有效，我们需要令：
$$
\begin{equation*}
	\int_{-\infty}^{+\infty} |\Psi(x, t)|^2\,dx = 1
\end{equation*} \tag{1.3} \label{normalization-of-schrodinger-equation}
$$
有这样的限制也在意料之中，因为如果 $\Psi$ 是薛定谔方程 $(\ref{schrodinger-equation})$ 的解，那么对任意常数 $k$，$k\Psi$ 也是该方程的解。所以只要找到了某个薛定谔方程的解，就可以找到某个常数 $k$ 满足上面的式子。我们将这个过程称为 **归一化（Normalization）**。还有一件事情需要确定，那就是上面这个积分似乎是和 $t$ 无关的，但显然 $t$ 理应对 $|\Psi(x, t)|^2$ 产生影响。如果我们在某个时刻（比如 $t = 0$）归一化 $\Psi$，如何保证在其它时刻波函数依然满足上面的式子呢？我们只需要证明下面这个式子：
$$
\frac{d}{dt}\int_{-\infty}^{+\infty} |\Psi(x, t)|^2\,dx = \int_{-\infty}^{+\infty}\frac{\partial}{\partial t}|\Psi(x, t)|^2\,dx = 0
$$
由 $|\Psi| = \Psi\Psi^*$，我们得到：
$$
\begin{align*}
	\frac{\partial }{\partial t}|\Psi|^2 = \frac{\partial}{\partial t}(\Psi^*\Psi) &= \Psi^*\frac{\partial \Psi}{\partial t} + \frac{\partial \Psi^*}{\partial t}\Psi \\
	&= \Psi^*\left(\frac{i\hbar}{2m}\frac{\partial^2\Psi}{\partial x^2} - \frac{i}{\hbar}V\Psi\right) + \left(-\frac{i\hbar}{2m}\frac{\partial^2\Psi^*}{\partial x^2} + \frac{i}{h}V\Psi^*\right)\Psi \\
	&= \frac{i\hbar}{2m}\left(\Psi^*\frac{\partial \Psi}{\partial x^2} - \frac{\partial \Psi^*}{\partial x^2}\Psi\right) \\
	&= \frac{\partial}{\partial x}\left[\frac{i\hbar}{2m}\left(\Psi^*\frac{\partial \Psi}{\partial x} - \frac{\partial \Psi^*}{\partial x}\Psi\right)\right]
\end{align*}
$$
因此，原始的积分为：
$$
\frac{d}{dt}\int_{-\infty}^{+\infty}|\Psi(x, t)|^2\,dx = \frac{i\hbar}{2m}\left.\left(\Psi^*\frac{\partial \Psi}{\partial x} - \frac{\partial \Psi^*}{\partial x}\Psi\right)\right|_{-\infty}^{+\infty}
$$
这里我们可以简单地认为 $\Psi$ 和 $\Psi^*$ 在无穷远处一定是 $0$（否则它们不可能满足归一化条件），因此这个积分的结果就是 $0$。一些数学家可能会找到病态函数作为反例，不过那些情况在物理中不存在。至此我们就证明了归一化的波函数对于任一时刻都有效。

波函数的出现为量子力学加入了很大的 **非决定性（Indeterminacy）**；即使我们得到了粒子的全部信息（即波函数 $\Psi$），也没法准确预测它的精确位置（尽管在进行实验测量时 *一定* 会得到精确的位置）。我们能得到的只有其统计学信息。自上世纪量子力学诞生以来，物理学家和哲学家对这种现象始终抱有迷惑，不知道是自然界的真相还是理论上的瑕疵。他们关于“测量粒子并发现它在点 $C$ ，那么在测量的前一刻（很短时间之前）它究竟在哪里”问题的合理解答大致有三种，让我们了解一下：

- **现实主义**：这个粒子就应该在 $C$ 点。这是爱因斯坦的观点，也是最符合直觉的观点。然而如果这是真实情况，量子力学就是一个不完整的理论，因为波函数没法告诉我们粒子的真实位置。现实主义的观点在于，虽然实验上无法得到粒子的准确位置，但是理论上它们位置一定是确定的。因此 $\Psi$ 并不能完整地表示粒子的状态，一定有一个“隐藏变量”。
- **保守主义**：这个粒子不在任何地方。测量的动作让粒子不得不出现在一个位置（虽然我们没法解释它为什么出现在这里）。这也被称为 **哥本哈根解释（Copenhagen Interpretation）**，由波尔和海森堡提出。这也是从提出至今最多物理学家支持的解释。不过如果这个观点是正确的，那么 *测量* 这个动作就会拥有非常诡异的意义；过去一个世纪的争论中，都没有为其作出清楚的解释。
- **不可知论**：拒绝做出回答。这个观点并非看上去那么愚蠢：我们应该如何验证粒子在测量前一刻的位置？唯一的验证方法就是进行一次测量，但这就不再符合“测量之前”了。上世纪的几十年间，这是大多数物理学家的”退却形态“：如果你问我粒子在测量前究竟在哪里，我会使用哥本哈根解释；如果你要刨根问底，那我就以不可知论应对，并结束回答。

1964 年是一个重要的时间点，约翰·贝尔的实验揭示了粒子在测量前位置的确定与否会造成可观测的区别，因此不可知论瞬间失去了市场。因此这变成了前两种解释的战场。尽管没有实验否定第一种观点，但多数实验肯定了第二种观点。因此本篇也假设哥本哈根解释是正确的理解：粒子在测量前没有准确的位置；测量发生时才会产生确切的结果。

还有一个另一个非常类似的问题，即在进行一次观测后，如果立即再进行一次观测，粒子的位置应该在哪里？这回所有人的观点都是一样的：和第一次测量的位置一样。保守主义对此的解释是，第一次测量会导致粒子的波函数 **坍缩（Collapse）**，此时波函数除了某一点上的概率为 $1$ 外，其余位置概率均为零。

值得一提的是，量子力学假设的均是低速情形，也即忽略相对论效应；这也让它的理论显得不够完整。但批判量子力学的不足绝不是我们在此的原因：经典力学的局限性也不妨碍它在宏观低速条件下的简洁有效；同样地，量子力学在微观低速条件下可以准确描述粒子的运动，因此值得我们去思考与学习。作为拓展，**量子场论（Quantum Field Theory, QFT）** 将经典场论、量子力学、狭义相对论结合到一起，是更加“一统”的学说；但这超出本篇笔记涵盖的内容了。

### 概率论

#### 离散变量

让我们首先来考虑 **离散变量（Discrete Variable）** 情况下的概率问题。离散指的是，变量的分布是不连续的。比如一群人的年龄分布、生日分布等。对于一个离散的随机分布，样本总数等于离散随机变量每一个可能取值的样本数量总和：
$$
\begin{equation*}
	N = \sum_{j=0}^\infty N(j)
\end{equation*} \tag{1.4}
$$
某一点的 **概率（Probability）** 是：
$$
\begin{equation*}
	P(j) = \frac{N(j)}{N}
\end{equation*} \tag{1.5}
$$
其中 $j$ 是随机变量的一个取值。如果要计算随机变量的平均值，可以使用加权平均：
$$
\begin{equation*}
	\langle j \rangle = \frac{\sum jN(j)}{N} = \sum_{j=0}^\infty jP(j)
\end{equation*} \tag{1.6} \label{expectation-value}
$$
我们将其称为 **期望值（Expectation Value）**。类似地，随机变量平方的期望是它们加权平方和的平均值：
$$
\begin{equation*}
	\bracket{j^2} = \sum_{j=0} j^2P(j)
\end{equation*} \tag{1.7}
$$
统计学中，我们希望用一个值表示数据的分散程度；比如 $1, 1, 1$ 就要比 $1, 2, 3$ 要更加集中。第一个能想到的方式是求出每一个变量和平均值的差：
$$
\begin{equation*}
	\Delta j = j - \bracket{j}
\end{equation*} \tag{1.8}
$$
不过如果尝试求差值的期望，会发现其恒等于零：
$$
\bracket{\Delta j} = \sum_{j=0}^\infty (j - \bracket{j})P(j) = \sum_{j=0}^\infty jP(j) - \bracket{j}\sum_{j=0}^\infty P(j) = \bracket{j} - \bracket{j} = 0
$$
这一点不难理解。一个解决方法是求 $|\Delta j|$ 的期望，但是很少有人喜欢处理大量的绝对值。因此我们使用的是差值平方的期望，我们将其称为 **方差（Variance）**，记为 $\sigma^2$：
$$
\begin{equation*}
	\sigma^2 = \bracket{(\Delta j)^2}
\end{equation*} \tag{1.9} \label{variance}
$$
至于 $\sigma$ 则被称为 **标准差（Standard Deviation）**。方差的计算式如下：
$$
\begin{align*}
	\sigma^2 &= \bracket{(\Delta j)^2} = \sum (j - \bracket{j})^2P(j) \\
	&= \sum \left(j^2 - 2j\bracket{j}\right)P(j) \\
	&= \sum \left(j^2 - 2j\bracket{j} + \bracket{j}^2\right)P(j) \\
	&= \sum j^2P(j) - 2\bracket{j}\sum jP(j) + \bracket{j}^2\sum P(j) \\
	&= \bracket{j^2} - 2\bracket{j}\bracket{j} + \bracket{j}^2 \\
	&= \bracket{j^2} - \bracket{j}^2
\end{align*}
$$
进行平方根运算后得到：
$$
\begin{equation*}
	\sigma = \sqrt{\bracket{j^2} - \bracket{j}^2}
\end{equation*} \tag{1.10} \label{standard-deviation}
$$
这个结论也暗示了平方的期望值总是不小于期望值的平方：
$$
\begin{equation*}
	\bracket{j^2} \ge \bracket{j}^2
\end{equation*} \tag{1.11}
$$
等号当且仅当所有样本的值都一样的时候才成立。

#### 连续变量

现在，让我们扩展到 **连续变量（Continuous Variable）** 的情形。通常，离散公式到连续公式只需要将求和符号转换为积分符号，这是因为积分本来就是求和的一个极限。首先，让我们定义 **概率密度（Probability Density）** 为极小段区间的概率，即：
$$
\begin{equation*}
	\rho(x) = \lim_{\Delta x \to 0}\frac{P(x + \Delta x) - P(x)}{\Delta x}
\end{equation*} \tag{1.12} \label{probability-density}
$$
因此，在概率密度的图像中，曲线在一个区间中围成的面积就是变量出现在该区间中的概率：
$$
\begin{equation*}
	P(a < x < b) = \int_a^b \rho(x)\,dx
\end{equation*}
$$
有了概率的定义，我们不难给出其它相应的公式：
$$
\begin{align*}
\begin{split}
	\int_{-\infty}^{+\infty}\rho(x)\,dx &= 1 \\
	\bracket{x} &= \int_{-\infty}^{+\infty} x\rho(x)\,dx \\
	\bracket{f(x)} &= \int_{-\infty}^{+\infty} f(x)\rho(x)\,dx \\
	\sigma^2 &= \bracket{x^2} - \bracket{x}^2
\end{split} \tag{1.13}
\end{align*}
$$
在量子力学中，波函数模的平方就是一个概率密度函数，也即我们令：
$$
\begin{equation*}
	\rho(x) = |\Psi(x, t)|^2
\end{equation*} \tag{1.14}
$$

### 算子

从此前得到的公式中我们可以得到波函数描述的位置期望值：
$$
\begin{equation*}
	\bracket{x} = \int_{-\infty}^{+\infty} x|\Psi(x, t)|^2\,dx
\end{equation*} \tag{1.15}
$$
这里计算出来的值并不是某一时刻粒子所处的位置，而是此时刻这个粒子位置的期望值。换句话说，这是 *无数个* 处于 $\Psi$ 状态的粒子（即可以被这个波函数描述的粒子）在被观测后所在位置的平均值。注意，这和一个粒子被观测多次时位置的平均值是不同的：第一次观测后，粒子的波函数就会坍缩。此后观测的时间点如果相距非常小，我们会得到相同的位置。

为了计算出这个期望值的变化率，我们可以对上面的式子求导，过程颇似于之前对归一化时间无关的证明，因此不表：
$$
\begin{equation*}
	\bracket{v} = \frac{d\bracket{x}}{dt} = -\frac{i\hbar}{2m}\int\left(\Psi^*\frac{\partial \Psi}{\partial x} - \frac{\partial \Psi^*}{\partial x}\Psi\right)\,dx = -\frac{i\hbar}{m}\int\Psi^*\frac{\partial \Psi}{\partial x}\,dx
\end{equation*} \tag{1.16}
$$
这个式子的含义是粒子速度的期望值。不过，粒子的速度在量子力学中似乎并没有意义，毕竟我们甚至不知道它上一秒在哪里！之后我们会讲解如何从波函数构建速度的概率密度，但现在我们先将上面的公式当作粒子速度的期望值。

动量可以简单通过 $\bracket{p} = m\bracket{v}$ 得到：
$$
\begin{equation*}
	\bracket{p} = m\frac{d\bracket{x}}{dt} = -i\hbar \int\left(\Psi^*\frac{\partial \Psi}{\partial x}\right)\,dx
\end{equation*} \tag{1.17}
$$
可以看到相比经典力学中的公式，量子力学里相同的概念表示起来要复杂不少。不过我们观察到它们之间有相似的部分，因此我们采用了全新的记法：
$$
\begin{align*}
\begin{split}
	\bracket{x} &= \int\Psi^*[x]\Psi\,dx \\
	\bracket{p} &= \int\Psi^*[-i\hbar\ (\partial/\partial x)]\Psi\,dx
\end{split} \tag{1.18}
\end{align*}
$$
此时，方括号包括的是一个 **算子（Operator）**，它会将后面的函数转换为另一个函数。这些公式除了算子以外的部分是完全相同的。因此我们可以通过算子来表示量子力学中的物理量。这里，我们不加证明地给出一个数学公式（可以将其看作公理）：
$$
\begin{equation*}
	\bracket{Q(x, p)} = \int\Psi^*\left[Q\left(x, -i\hbar\frac{\partial}{\partial x}\right)\right]\Psi\,dx
\end{equation*} \tag{1.19}
$$
也即将其原始公式中的 $p$ 全部替换为 $-i\hbar\ (\partial/\partial x)$ 即可。举个例子就是动能：
$$
\begin{equation*}
	\bracket{T} = -\frac{\hbar^2}{2m}\int\Psi^*\frac{\partial^2 \Psi}{\partial x^2}\,dx
\end{equation*} \tag{1.20}
$$

### 测不准原理

假设你拿着一根非常长的绳子的一端并不断抖动，绳子应该会呈现出某种”运动状态“，就好像一个波一样。此时如果有人问题”绳子的波具体在哪里“，你可能没法回答，因为绳子上波无处不在（极端情况下）！但如果他问你的是”绳子的波长是多少“，你或许可以给出一个合理的答案。另一种情况，假设你对一根非常长的静止绳子突然施加一段力，它会形成一个”鼓包“，此时你或许可以回答”波在哪里“，但它的波长是没有定义的（极端情况下）。所以我们面临一个抉择：如果想要知道波长，我们就得接受波在一个范围，而不是一个点；如果想要知道位置，那就得认识到”波“并不存在，因此没有波长的概念。任何中间情况，两者都没法得到确切的值。

**德布罗意公式（De Broglie Formula）** 描述了粒子动量与波长的关系：
$$
\begin{equation*}
	p = \frac{h}{\lambda} = \frac{2\pi\hbar}{\lambda}
\end{equation*} \tag{1.21}
$$
因此前面描述的波长和动量相关；测出的波长越准确，得到的动量也就越准确。**测不准原理（The Uncertainty Principle）** 定量叙述如下：
$$
\begin{equation*}
	\sigma_x\sigma_p \ge \frac{\hbar}{2}
\end{equation*} \tag{1.22}
$$
其中 $\sigma_x$ 和 $\sigma_p$ 分别是位置和动量分布的标准差。我们会在后续章节证明这个公式。



## 时间无关的薛定谔方程

### 静止状态

本节中，让我们尝试得到薛定谔方程的解。首先回顾薛定谔方程：
$$
\begin{equation*}
	i\hbar\frac{\partial \Psi}{\partial t} = -\frac{\hbar^2}{2m}\frac{\partial^2\Psi}{\partial x^2} + V\Psi
\end{equation*} \tag{2.1}
$$
我们如果不加说明，通篇都假设 $V = V(x)$，即势能与时间无关。一个典型的偏微分方程解法是 **分离变量法（Separation of Variables）**，假设 $\Psi$ 可以表示成两个分别是 $x$ 和 $t$ 的函数的积：
$$
\begin{equation*}
	\Psi(x, t) = \psi(x)\varphi(t)
\end{equation*} \tag{2.2}
$$
将其代入原方程后得到：
$$
\begin{equation*}
	i\hbar\frac{1}{\varphi}\frac{d\varphi}{dt} = -\frac{\hbar^2}{2m}\frac{1}{\psi}\frac{d^2\psi}{dx^2} + V
\end{equation*} \tag{2.3}
$$
此时令等式两边相等的唯一可能性是左侧和右侧都等于某个常数。设：
$$
\begin{equation*}
	E = i\hbar\frac{1}{\varphi}\frac{d\varphi}{dt} \implies \frac{d\varphi}{dt} = -\frac{iE}{\hbar}\varphi
\end{equation*} \tag{2.4}
$$
将 $d\varphi/dt$ 用 $E$ 代替后得到：
$$
\begin{equation*}
	-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} + V\psi = E\psi
\end{equation*} \tag{2.5} \label{time-independent-schrodinger-equation}
$$
至此，我们将一个偏微分方程转化为了两个常微分方程 $(2.4)$ 和 $(2.5)$。其中第一个方程非常好解，我们可以轻松地得到：
$$
\begin{equation*}
	\varphi(t) = e^{-iEt/\hbar}
\end{equation*} \tag{2.6}
$$
这里我们省略了常数，这个常数会在 $\psi(x)$ 中补偿回来。第二个方程称为 **时间无关的薛定谔方程（Time-Independent Schrodinger Equation）**，我们需要其给出具体的势能函数 $V(x)$ 才能进一步求解。本章接下来的内容便是如何解不同设置下的时间无关的薛定谔方程。

首先，让我们回顾一下分离变量法。为什么我们用这种方式来解薛定谔方程？答案可以是：这是我们最简单的方法；不过更重要的是，通过这种方式得到的解有更好的性质。下面来分析一二：

- 它们代表了 **静止状态（Stationary State）**。尽管最后得到的波函数 $\Psi(x, t) = \psi(x)\exp(-iEt/\hbar)$ 是时间相关的，但是它的概率密度不是：
  $$
  |\Psi(x, t)|^2 = \Psi^*\Psi = \psi^*e^{iEt/\hbar}\psi e^{-iEt/\hbar} = |\psi(x)|^2 \tag{2.7}
  $$
  类似地，任意变量的期望值都是时间无关的：
  $$
  \bracket{Q(x, p)} = \int \psi^*\left[Q\left(x, -i\hbar\frac{d}{dx}\right)\right]\psi\,dx \tag{2.8}
  $$
  特别地，由于 $\bracket{x}$ 是常数，动量 $\bracket{p} = 0$。此外不难发现，这些情况下我们可以直接用 $\psi(x)$ 代替 $\Psi(x, t)$。
  
- 它们是总能量确定的状态。经典力学中，我们用 **哈密顿量（Hamiltonian）** 来表示总能量：
  $$
  H(x, p) = \frac{p^2}{2m} + V(x) \tag{2.10}
  $$
  我们可以得到其对应的算子，即 **哈密顿算子（Hamiltonian Operator）**：
  $$
  \hat{H} = -\frac{\hbar^2}{2m}\frac{\partial^2}{\partial x^2} + V(x)
  $$
  因此，我们得到了时间无关的薛定谔方程的最简形式：
  $$
  \begin{equation*}
  	\hat{H}\psi = E\psi
  \end{equation*} \tag{2.11}
  $$
  注意到总能量的期望值是：
  $$
  \bracket{H} = \int\psi^*\hat{H}\psi\,dx = E\int|\psi|^2\,dx = E\int|\Psi|^2\,dx = E
  $$
  且总能量平方的期望值是：
  $$
  \bracket{H^2} = \int\psi^*\hat{H}^2\psi\,dx = \int\psi^*\hat{H}(E\psi)\,dx = E\int\psi^*\hat{H}\psi\,dx = E^2\int|\psi|^2\,dx = E^2
  $$
  发现总能量的标准差是 $0$，因此分离变量法得到的解，在每次测量其总能量时，得到的结果都是相同的：
  $$
  \sigma_H = \sqrt{\bracket{H^2} - \bracket{H}^2} = 0 \tag{2.12}
  $$

- 它们的线性组合依然是原方程的解。事实上，对于 $(\ref{time-independent-schrodinger-equation})$，我们可以得到无数个解 $\psi_1(x), \psi_2(x), ...$，简记为 $\{\psi_n(x)\}$。它们对应的总能量是 $\{E_n\} = E_1, E_2, ...$。所以每个解可以写为：
  $$
  \Psi_i(x, t) = \psi_i(x)e^{-iE_it/\hbar}
  $$
  它们的线性组合就是方程的通解：
  $$
  \begin{equation*}
  	\Psi(x, t) = \sum_{n=1}^\infty c_n\psi_n(x)e^{-iE_nt/\hbar}
  \end{equation*} \tag{2.13} \label{general-solution-to-time-independent-schrodinger-equation}
  $$
  其中 $\{c_n\}$ 是一系列任意常数。 

将这一节的内容总结起来，就是对于已知势能函数 $V(x)$ 以及初始状态 $\Psi(x, 0)$ 的薛定谔方程，我们可以首先尝试解时间无关的薛定谔方程；在得到无限个解 $\{\psi_n(x)\}$ 以及它们对应的总能量 $\{E_n\}$ 后，将 $t = 0$ 代入通解以对应初始条件 $\Psi(x, 0)$，即令：
$$
\Psi(x, 0) = \sum_{n=1}^\infty c_n\psi_n(x) \tag{2.14}
$$
最后，将 $\varphi(t)$ 放回式子，就得到：
$$
\begin{equation*}
	\Psi(x, t) = \sum_{n=1}^\infty c_n\psi_n(x)e^{-iE_nt/\hbar} = \sum_{n=1}^\infty c_n\Psi_n(x, t)
\end{equation*} \tag{2.15}
$$
其中：
$$
\begin{equation*}
	\Psi_n(x, t) = \psi_n(x)e^{-iE_nt/\hbar}
\end{equation*} \tag{2.16}
$$
需要注意的是，此前介绍的，概率密度和期望值的时间无关性只对每个 $\Psi_n(x, t)$ 有效；通解形式并不一定满足时间无关。在通解中，$|c_n|^2$ 的含义是对应总能量出现的概率（下一章再进行解释），因此它们的和应该是 $1$：
$$
\begin{equation*}
	\sum_{n=1}^\infty |c_n|^2 = 1
\end{equation*} \tag{2.17}
$$
此外，总能量的期望值满足：
$$
\begin{equation*}
	\bracket{H} = \sum_{n=1}^\infty |c_n|^2E_n
\end{equation*} \tag{2.18}
$$
由于 $c_n$ 是与时间无关的常数，因此每一种能量的概率，以及总能量的期望值和时间都无关。这是 **能量守恒（Energy Conservation）** 在量子力学中的体现。

### 无限方井

让我们考虑一种势能分布：
$$
\begin{equation*}
	V(x) = \begin{cases}
		0, & 0 \le x \le a \\
		\infty & x < 0\ \text{或}\ x > a
	\end{cases}
\end{equation*} \tag{2.19}
$$
这种情况下，除了 $x = 0$ 或 $x = a$ 两个地方，其余位置的粒子受力均为零，而两个边界处受力为无穷大。如果类比成经典力学中的情形，就是空间中完全光滑轨道上有两个完全弹性的障碍物，物体只会在两个障碍物之间运动。

我们将这种设置称为 **无限方井（Infinite Square Well, ISW）**。正如其名，粒子仿佛被“困在”一个无限深的方井中无法出来。在方井中，由于势能为零，我们可以代入 $(\ref{time-independent-schrodinger-equation})$ 得到：
$$
\begin{equation*}
	-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} = E\psi
\end{equation*} \tag{2.20} \label{schrodinger-equation-of-infinite-square-well}
$$
这是一个典型的二阶常微分方程，其通解是：
$$
\begin{equation*}
	\psi(x) = A\sin kx + B\cos kx \qquad k = \frac{\sqrt{2mE}}{\hbar}
\end{equation*} \tag{2.21}
$$
为了确定上面的常数 $A, B$，我们需要给出一系列 **边界条件（Boundary Conditions）**。我们通常假定 $\psi$ 和 $d\psi/dx$ 是连续的，此处由于 $V$ 可能是 $\infty$，所以只有第一个条件是成立的。此时有：
$$
\psi(0) = \psi(a) = 0
$$
带入到原式中，可以得到 $B = 0$ 以及 $ka = \pm n\pi$，所以以下形式的 $k$ 都可以满足要求：
$$
\begin{equation*}
	k_n = \frac{n\pi}{a} \quad n = 1, 2, 3, ...
\end{equation*} \tag{2.22}
$$
这样我们也能确定总能量：
$$
\begin{equation*}
	E_n = \frac{\hbar^2k_n^2}{2m} = \frac{n^2\pi^2\hbar^2}{2ma^2}
\end{equation*} \tag{2.23}
$$
为了得到 $A$ 的确定值，我们需要对 $\psi$ 进行归一化：
$$
\int_0^a |A|^2\sin^2 kx\,dx = |A|^2\frac{a}{2} = 1 \implies |A|^2 = \frac{2}{a}
$$
我们直接取 $A = \sqrt{2/a}$。这样就得到了无限方井内的解：
$$
\begin{equation*}
	\psi_n(x) = \sqrt{\frac{2}{a}}\sin\left(\frac{n\pi x}{a}\right)
\end{equation*}
$$
下面是在 $n = 1, 2, 3$ 时的图像：

![qm1_2-1](graphs/qm1_2-1.png)

可以看到，$n$ 对应着图像中波峰/波谷出现的数量。我们将 $n = 1$ 称为 **基态（Ground State）**，其它的情形则称为 **激发态（Excited State）**。这些状态之间有一些共同点：

- 它们关于方井中心交替呈现 **奇性** 与 **偶性**，也即镜面对称和中心对称。比如 $\psi_1, \psi_3, ...$ 为偶，$\psi_2, \psi_4, ...$ 为奇。

- 它们与 $0$ 的交点数量（称为 **结点**）逐个增加。

- 它们之间相互 **正交（Orthogonal）**，定义为：
  $$
  \begin{equation*}
  	\int\psi_m(x)^*\psi_n(x)\,dx = 0 \qquad (m \ne n)
  \end{equation*}
  $$
  证明如下：
  $$
  \begin{align*}
  	\int \psi_m(x)^*\psi_n(x)\,dx 
  	&= \frac{2}{a}\int_0^a\sin\left(\frac{m\pi x}{a}\right)\sin\left(\frac{n\pi x}{a}\right)\,dx \\
  	&= \frac{1}{a}\int_0^a\left[\cos\left(\frac{(m-n)\pi x}{a}\right) - \cos\left(\frac{(m+n)\pi x}{a}\right)\right]\,dx \\
  	&= \left.\left[\cos\left(\frac{(m-n)\pi x}{a}\right) - \frac{1}{(m+n)\pi}\sin\left(\frac{(m+n)\pi x}{a}\right)\right]\right|_0^a \quad\text{(Suppose $m\ne n$)}\\
  	&= \frac{1}{\pi}\left[\frac{\sin{[(m-n)\pi]}}{m-n} - \frac{\sin{[(m+n)\pi]}}{m+n}\right] \\
  	&= 0
  \end{align*}
  $$
  当 $m = n$ 时，上面的积分会得到 $1$。因此我们可以将其记为：
  $$
  \int \psi_m(x)^*\psi_n(x)\,dx = \delta_{mn}
  $$
  此处记号 $\delta_{mn}$ 是 **克罗内克函数**，定义为：
  $$
  \begin{equation*}
  	\delta_{mn} =
  	\begin{cases}
  		0 & m \ne n \\
  		1 & m = n
  	\end{cases}
  \end{equation*}
  $$

- 它们是 **完备的（Complete）**，即任何函数 $f(x)$ 都可以表示为它们的线性和（我们不在本篇笔记中证明这一点）：
  $$
  \begin{equation*}
  	f(x) = \sum_{n=1}^\infty c_n\psi_n(x) = \sqrt{\frac{2}{a}}\sum_{n=1}^\infty c_n\sin\left(\frac{n\pi x}{a}\right)
  \end{equation*}
  $$
  这种表示方式被称为 $f(x)$ 的 **傅立叶级数（Fourier Series）**。这个性质也被称为 **狄利克雷定理（Dirichlet's Theorem）**。为了确定这里的常数 $c_n$，我们需要用到一个小技巧：
  $$
  \int\psi_m(x)^*f(x)\,dx = \sum_{n=1}^\infty c_n\int\psi_m(x)^*\psi_n(x)\,dx = \sum_{n=1}^\infty c_n\delta_{mn} = c_m
  $$

上面这四个性质不仅对无限方井，对大多数的势能分布都满足。尤其是正交性和完备性非常有用，我们多数情况下会假设它们成立。

至此，我们得到了无限方井的单个状态的通解（时间相关）：
$$
\begin{equation*}
	\Psi_n(x, t) = \sqrt{\frac{2}{a}}\sin\left(\frac{n\pi x}{a}\right)\exp\left(-i\frac{n^2\pi^2\hbar}{2ma^2}t\right)
\end{equation*}
$$
所有状态叠加起来的通解（时间相关）则是：
$$
\begin{equation*}
	\Psi(x, t) = \sum_{n=1}^\infty c_n\sqrt{\frac{2}{a}}\sin\left(\frac{n\pi x}{a}\right)\exp\left(-i\frac{n^2\pi^2\hbar}{2ma^2}t\right)
\end{equation*}
$$
为了得到 $c_n$，我们可以令 $t = 0$，然后根据正交性得到：
$$
\begin{equation*}
	c_n = \sqrt{\frac{2}{a}}\int_0^a\sin\left(\frac{n\pi x}{a}\right)\Psi(x, 0)\,dx
\end{equation*}
$$
这里的 $c_n$ 满足模平方之和为 $1$，我们可以通过归一化条件得到该结论：
$$
\begin{align*}
	1 = \int |\Psi(x, 0)|^2\,dx
	&= \int \left[\sum_{m=1}^\infty c_m\psi_m(x)\right]^*\left[\sum_{n=1}^\infty c_n\psi_n(x)\right]\,dx \\
	&= \sum_{m=1}^\infty \sum_{n=1}^\infty c_m^*c_n\int\psi_m(x)^*\psi_n(x)\,dx \\
	&= \sum_{m=1}^\infty \sum_{n=1}^\infty c_m^*c_n\delta_{mn} \\
	&= \sum_{n=1}^\infty |c_n|^2
\end{align*}
$$
同时能量的期望是：
$$
\begin{align*}
	\langle H\rangle 
	&= \int \Psi^* \hat{H}\Psi\,dx \\
	&= \int \left(\sum c_m\psi_m\right)^*\hat{H}\left(\sum c_n\psi_n\right)\,dx \\
	&= \sum\sum c_m^*c_nE_n \int \psi_m^*\psi_n\,dx \\
	&= \sum|c_n|^2E_n
\end{align*}
$$

### 有限方井

一个类似无限方井的设定是 **有限方井（Finite Square Well）**，其势能分布如下：
$$
\begin{align*}
	V(x) = 
	\begin{cases}
		-V_0 & -a \le x \le a \\
		0 & |x| > a
	\end{cases}
\end{align*}
$$
此处 $V_0$ 是某个常数。针对 $E < 0$ 或 $E > 0$，我们可以将问题分为 **约束状态（Bound State）**和 **分散状态（Scattering State）**。我们先研究前一种情形。当 $x < -a$ 时，$V(x) = 0$，此时有：
$$
-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} = E\psi \implies \frac{d^2\psi}{dx^2} = \kappa^2\psi \quad \text{其中 $\kappa = \frac{\sqrt{-2mE}}{\hbar}$}
$$
它的通解是：
$$
\psi(x) = Ae^{-\kappa x} + Be^{\kappa x}
$$
由于第一项会在 $x \to \infty$ 时无意义，得到 $A = 0$。所以当 $x < -a$ 时有：
$$
\psi(x) = Be^{\kappa x}
$$
类似地，当 $x > a$ 时我们有：
$$
\psi(x) = Ce^{-\kappa x}
$$
在 $|x| \le a$ 时，$V(x) = -V_0$，此时有：
$$
-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} = (V_0 + E)\psi \implies \frac{d^2\psi}{dx^2} = -l^2\psi \quad \text{其中 $l = \frac{\sqrt{2m(E + V_0)}}{\hbar}$}
$$
它的通解是：
$$
\psi(x) = D\sin(lx) + E\cos(lx)
$$
考虑 $V$ 的对称性，为了让 $\psi$ 和 $\frac{d\psi}{dx}$ 在 $\pm a$ 处连续，我们最终得到的应该是类似于下面的结果（注意 $x$ 的范围和前面使用的不太一样）：
$$
\psi(x) = 
\begin{cases}
	Ce^{-\kappa x} & x > a \\
	E\cos(lx) & 0 < x < a \\
	\psi(-x) & x < 0
\end{cases}
$$
其中第二项取余弦而非正弦是因为它在 $x = 0$ 处的梯度是连续的。将边界条件代入，有：
$$
\begin{align*}
\begin{cases}
	Ce^{-\kappa a} = E\cos(la)  \\
	-\kappa Ce^{-\kappa a} = -lE\sin(la)
\end{cases}
\implies
\kappa = l\tan{la}
\end{align*}
$$
回忆 $l$ 是一个和 $E$ 相关的变量，因此这个公式给出了所有可能的 $E$。为了得到更好的形式，记 $z = la$，$z_0 = \frac{a}{\hbar}\sqrt{2mV_0}$。由我们对 $\kappa$ 和 $l$ 的定义，有 $\kappa^2 + l^2 = \frac{2mV_0}{\hbar^2}$，也即 $\kappa^2 + l^2 = (\frac{z_0}{a})^2$，将 $l = \frac{z}{a}$ 代入后得到：
$$
\tan{z} = \sqrt{\left(\frac{z_0}{z}\right)^2 - 1}
$$
再次回顾上式中变量的含义：$z_0$ 是和问题设置有关的常量，$z$ 是和 $E$ 相关的量。下面是当 $z_0 = 8$ 时的示意图：

<img src="graphs/qm1_2-2.png" alt="qm1_2-2" style="zoom:80%;" />

可以看到，只有 $\tan{z} = \sqrt{(z_0/z)^2-1}$ 的地方，$z$ 对应的能量才是允许取的值。让我们针对 $z_0$ 的两种极端情形讨论有限方井的性质：

- 深且宽的方井：当 $z_0$ 很大时，上图中的 $\sqrt{(z_0/z)^2-1}$ 图像会向上移动到较高的地方，此时它与 $z$ 轴的交点也会大幅向右移动。观察 $\tan{z}$ 的图像，可以发现它和 $\sqrt{(z_0/z)^2 - 1}$ 的交点会在大约 $z_n = n\pi/2$ 的位置，此时对应的能量是：
  $$
  \begin{equation*}
  	E_n + V_0 \approx \frac{n^2\pi^2\hbar^2}{2m(2a)^2} \quad \text{其中 $n$ 是奇数}
  \end{equation*}
  $$
  等式右侧这个形式我们应该比较熟悉：它完美对应了宽度为 $2a$ 的无限方井中允许取的能量里一半的情形。这也符合我们的预期：当方井深度无限加大时（$V_0 \to \infty$），它就类似于一个无限方井。

- 浅且窄的方井：当 $z_0$ 变小时，能取得的状态会越来越少；当 $z_0 < \frac{\pi}{2}$ 时只有一个。有趣的事，无论 $z_0$ 多么小，始终至少有一个合法的状态。

现在研究 $E > 0$，即分散状态下的方井。此时对于 $x < -a$ 有：
$$
\begin{equation*}
	\psi(x) = Ae^{ikx} + B^{-ikx} \qquad \text{其中 $k = \frac{\sqrt{2mE}}{\hbar}$}
\end{equation*}
$$


### 自由粒子

这一节讨论没有任何约束的环境，即：
$$
V(x) = 0
$$
此时薛定谔方程变为：
$$
-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} = E\psi \implies \frac{d^2\psi}{dx^2} = -k^2\psi \qquad \text{其中 $k = \frac{\sqrt{2mE}}{\hbar}$}
$$
其通解为（值得一提的是，我们使用了指数形式而非三角函数形式，其原因很快就会说明）：
$$
\begin{equation*}
	\psi(x) = Ae^{ikx} + Be^{-ikx}
\end{equation*}
$$
至此，一切和无限方井的情况相同，但现在我们没有任何边界条件，所以不需要确定 $A$ 和 $B$ 的值。时间相关的波函数解即是：
$$
\begin{equation*}
	\Psi(x, t) = A^{ik\left(x - \frac{\hbar k}{2m}t\right)} + Be^{-ik\left(x + \frac{\hbar k}{2m}\right)}
\end{equation*}
$$
对这个方程进行简单分析，可以看出第一项是一个从左向右传播的波，第二项是从右向左传播的波，它们速度相同方向相反（大小和 $k$ 有关）。因此我们可以根据 $k$ 的值确定波函数：
$$
\begin{equation*}
	\Psi_k(x, t) = Ae^{i\left(kx - \frac{\hbar k^2}{2m}t\right)}
\end{equation*}
$$
这里的 $k$ 是任何满足下列的常数：
$$
\begin{equation*}
	k = \pm \frac{\sqrt{2mE}}{\hbar}
\end{equation*}
$$
根据德布罗意公式 $p = \frac{2\pi\hbar}{\lambda}$，我们可以计算出波的动量：
$$
\begin{equation*}
	p = \hbar k
\end{equation*}
$$
波的传递速度是（通过求波函数中 $x$ 和 $t$ 的系数比值得到）：
$$
\begin{equation*}
	v_\text{quantum} = \frac{\hbar |k|}{2m} = \sqrt{\frac{E}{2m}}
\end{equation*}
$$
我们注意到它和经典物理中速度的区别（通过动能公式 $E = \frac{1}{2}mv^2$ 得到）：
$$
\begin{equation*}
	v_\text{classical} = \sqrt{\frac{2E}{m}} = 2v_\text{quantum}
\end{equation*}
$$
这个差异究竟是为什么？此外，如果我们尝试对自由粒子的波函数归一化，会发现无法完成：
$$
\begin{equation*}
	\int_{-\infty}^\infty \Psi_k\Psi_k\,dx = |A|^2\int_{-\infty}^\infty\,dx = +\infty
\end{equation*}
$$
对此的解释是，$\Psi_k$ 无法代表自由粒子的一个合理状态：这和无限方井是不同的。我们需要将所有 $k$ 的情形都考虑在内才能得到合理的状态。由于 $k$ 不是离散的，我们需要求积分而非求和：
$$
\begin{equation*}
	\Psi(x, t) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^{\infty}\phi(k)e^{i(kx - \frac{\hbar k^2}{2m}t)}\,dk
\end{equation*}
$$
上式中凭空出现的 $\frac{1}{\sqrt{2\pi}}\phi(k)$ 是用来对这个积分归一化的参数。为了算出这个 $\phi(k)$，我们只需要尝试将 $\Psi(x, 0)$ 归一化。根据 **普朗歇尔定理（Plancherel's Theorem）**，有：
$$
f(x) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^\infty F(k)e^{ikx}\,dk \Longleftrightarrow F(k) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^\infty f(x)e^{-ikx}\,dx
$$
这里 $F(k)$ 是 $f(x)$ 的 **傅立叶变换（Fourier Transform）**，$f(x)$ 是 $F(k)$ 的 **逆傅立叶变换（Inverse Fourier Tranform）**。这样我们就得到了 $\phi(k)$ 的公式：
$$
\begin{equation*}
	\phi(k) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^\infty \Psi(x, 0)e^{-ikx}\,dx
\end{equation*}
$$
