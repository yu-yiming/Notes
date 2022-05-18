# Intro to Quantum Mechanics I

本篇是对应 *UIUC PHYS 486 Quantum Physics I* 的学习笔记，其中包括了波函数、希尔伯特空间、算符、自旋，和双粒子系统等知识。参考书籍是 *Introduction to Quantum Mechanics, 3rd Edition: David J. Griffiths*。

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
其中的 $\hbar$ 是一个在量子力学中无处不见的常量（大约是 $\epsilon_0$ 和 $\mu_0$ 在电动力学中的常见程度），**约化普朗克常量（Reduced Planck Constant）**，值约为 $1.053473\times 10^{-34}\ \text{J}\cdot\text{s}$。本章的主要内容就是探究波函数的意义，并和经典力学类似，从波函数逐渐得到量子力学中速度、动量以及动能等概念的定义。

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
由 $|\Psi| = \Psi^*\Psi$，我们得到：
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
这里我们可以简单地认为 $\Psi$ 和 $\Psi^*$ 在无穷远处一定是 $0$（否则它们不可能满足归一化条件），因此这个积分的结果就是 $0$。数学家可能会找到一些病态函数作为反例，不过那些情况在物理中不存在。至此我们就证明了归一化的波函数对于任一时刻都有效。

波函数的出现为量子力学加入了很大的 **非决定性（Indeterminacy）**；即使我们得到了粒子的全部信息（即波函数 $\Psi$），也没法准确预测它的精确位置（尽管在进行实验测量时 *一定* 会得到精确的位置）。我们能得到的只有其统计学信息。自上世纪量子力学诞生以来，物理学家和哲学家对这种现象始终抱有迷惑，不知道是自然界的真相还是理论上的瑕疵。他们关于“测量粒子并发现它在点 $C$ ，那么在测量的前一刻（很短时间之前）它究竟在哪里”问题的合理解答大致有三种，让我们了解一下：

- **现实主义**：这个粒子就应该在 $C$ 点。这是爱因斯坦的观点，也是最符合直觉的观点。然而如果这是真实情况，量子力学就是一个不完整的理论，因为波函数没法告诉我们粒子的真实位置。现实主义的观点在于，虽然实验上无法得到粒子的准确位置，但是理论上它们位置一定是确定的。因此 $\Psi$ 并不能完整地表示粒子的状态，一定有一个“隐藏变量”。
- **保守主义**：这个粒子不在任何地方。测量的动作让粒子不得不出现在一个位置（虽然我们没法解释它为什么出现在这里）。这也被称为 **哥本哈根解释（Copenhagen Interpretation）**，由波尔和海森堡提出。这也是从提出至今最多物理学家支持的解释。不过如果这个观点是正确的，那么 *测量* 这个动作就会拥有非常诡异的意义；过去一个世纪的争论中，都没有为其作出清楚的解释。
- **不可知论**：拒绝做出回答。这个观点并非看上去那么愚蠢：我们应该如何验证粒子在测量前一刻的位置？唯一的验证方法就是进行一次测量，但这就不再符合“测量之前”了。上世纪的几十年间，这是大多数物理学家的”退却形态“：如果你问我粒子在测量前究竟在哪里，我会使用哥本哈根解释；如果你要刨根问底，那我就以不可知论应对，并结束回答。

1964 年是一个重要的时间点，约翰·贝尔的实验揭示了粒子在测量前位置的确定与否会造成可观测的区别，因此不可知论瞬间失去了市场。因此这变成了前两种解释的战场。尽管没有实验否定第一种观点，但多数实验肯定了第二种观点。因此本篇也假设哥本哈根解释是正确的理解：粒子在测量前没有准确的位置；测量发生时才会产生确切的结果。

还有一个另一个非常类似的问题，即在进行一次观测后，如果立即再进行一次观测，粒子的位置应该在哪里？这回所有人的观点都是一样的：和第一次测量的位置一样。保守主义对此的解释是，第一次测量会导致粒子的波函数 **坍缩（Collapse）**，此时波函数除了某一点上的概率为 $1$ 外，其余位置概率均为零。

值得一提的是，量子力学假设的均是低速情形，也即忽略相对论效应；这也让它的理论显得不够完整。但批判量子力学的不足绝不是我们在此的原因：经典力学的局限性也不妨碍它在宏观低速条件下的简洁有效；同样地，量子力学在微观低速条件下可以准确描述粒子的运动，因此值得我们去思考与学习。作为拓展，**量子场论（Quantum Field Theory, QFT）** 将经典场论、量子力学、狭义相对论结合到一起，是更加“一统”的学说；但这超出本篇笔记涵盖的内容了。

### 概率论简介

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

### 算符

从此前得到的公式中我们可以得到波函数描述的位置期望值：
$$
\begin{equation*}
	\bracket{x} = \int_{-\infty}^{+\infty} x|\Psi(x, t)|^2\,dx
\end{equation*} \tag{1.15}
$$
这里计算出来的值并不是某一时刻粒子所处的位置，而是此时刻这个粒子位置的期望值。换句话说，这是 *无数个* 处于 $\Psi$ 状态的粒子（即可以被这个波函数描述的粒子）在被观测后所在位置的平均值。注意，这和一个粒子被观测多次时位置的平均值是不同的：第一次观测后，粒子的波函数就会坍缩。此后观测的时间点如果相距非常小，我们会得到相同的位置。

为了计算出这个期望值的变化率，我们可以对上面的式子求导，过程颇似于之前对归一化时间无关的证明，因此不详细列出了：
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
这里，方括号包括的是一个 **算符（Operator）**，它会将后面的函数转换为另一个函数。这些公式除了算符以外的部分是完全相同的。因此我们可以通过算符来表示量子力学中的物理量。这里，我们不加证明地给出一个数学公式（可以将其看作公理）：
$$
\begin{equation*}
	\bracket{Q(x, p)} = \int\Psi^*\left[Q\left(x, -i\hbar\frac{\partial}{\partial x}\right)\right]\Psi\,dx
\end{equation*} \tag{1.19}
$$
也即将其原始公式中的 $p$ 全部替换为 $-i\hbar\ (\partial/\partial x)$ 即可。举个例子就是动能，由 $T = p^2/2m$ 有：
$$
\begin{equation*}
	\bracket{T} = -\frac{\hbar^2}{2m}\int\Psi^*\frac{\partial^2 \Psi}{\partial x^2}\,dx
\end{equation*} \tag{1.20}
$$

最后让我们给出一个有趣的结论。回忆在经典力学中，力可以被定义为动量的变化率：
$$
\begin{equation*}
	F = \frac{dp}{dt}
\end{equation*}
$$
另一方面，力也是势能梯度的负数，因此我们在一维情况下有（实际上这就是 $(\ref{newton's-law})$ 的另一种形式）：
$$
\begin{equation*}
	\frac{dp}{dt} = -\frac{\partial V}{\partial x}
\end{equation*} \tag{1.21}
$$
接下来我们将证明这个公式在量子力学中对于期望值依然成立。左侧可以写为：
$$
\frac{d\langle p\rangle}{dt} = -i\hbar\frac{d}{dt}\int\Psi^*\frac{\partial \Psi}{\partial x}\,dx
$$
由于积分内不包含 $t$ 的积分，我们可以将 $\frac{d}{dt}$ 移动到积分号内。省略积分号和常数有：
$$
\begin{align*}
	\frac{\partial}{\partial t}\left(\Psi^*\frac{\partial \Psi}{\partial x}\right)
		&= \frac{\partial \Psi^*}{\partial t}\frac{\partial \Psi}{\partial x} + \Psi^*\frac{\partial}{\partial x}\frac{\partial \Psi}{\partial t} \\
		&= -\frac{i}{\hbar}\left(-\frac{\hbar^2}{2m}\frac{\partial^2 \Psi^*}{\partial x^2} - V\Psi^*\right)\frac{\partial \Psi}{\partial x} + \Psi^*\frac{\partial}{\partial x}\left(-\frac{i}{\hbar}\right)\left(\frac{\hbar^2}{2m}\frac{\partial^2\Psi}{\partial x^2} + V\Psi\right) \\
		&= -\frac{i}{\hbar}\left[-\frac{\hbar^2}{2m}\frac{\partial^2 \Psi^*}{\partial x^2}\frac{\partial \Psi}{\partial x} - V\Psi^*\frac{\partial \Psi}{\partial x} + \frac{\hbar^2}{2m}\Psi^*\frac{\partial^3 \Psi}{\partial x^3} + \frac{\partial V}{\partial x}\Psi^*\Psi + V\Psi^*\frac{\partial \Psi}{\partial x} \right] \\
		&= \frac{i\hbar}{2m}\left(\Psi^*\frac{\partial \Psi}{\partial x^3} - \frac{\partial^2 \Psi^*}{\partial x^2}\frac{\partial \Psi}{\partial x}\right) - \frac{i}{\hbar}\Psi^*\Psi\frac{\partial V}{\partial x}
\end{align*}
$$
对 $x$ 进行积分，可以得到：
$$
\begin{align*}
	\frac{d\langle p \rangle}{dt}
		&= \frac{\hbar^2}{2m}\int \left(\Psi^*\frac{\partial^3 \Psi}{\partial x^3} - \frac{\partial^2 \Psi^*}{\partial x^2}\frac{\partial \Psi}{\partial x}\right)\,dx - \int\left(\Psi^*\Psi\frac{\partial V}{\partial x}\right)\,dx \\
		&= \frac{\hbar^2}{2m}\left[\left.\Psi^*\frac{\partial^2 \Psi}{\partial x^2}\right|_{-\infty}^\infty - \int \frac{\partial \Psi^*}{\partial x}\frac{\partial^2 \Psi}{\partial x^2}\,dx - \left.\frac{\partial^2\Psi^*}{\partial x^2}\frac{\partial \Psi}{\partial x}\right|_{-\infty}^{\infty} + \int \frac{\partial \Psi^*}{\partial x}\frac{\partial^2 \Psi}{\partial x^2}\,dx\right] - \int\Psi^*\frac{\partial V}{\partial x}\Psi\,dx \\
		&= -\int\Psi^*\frac{\partial V}{\partial x}\Psi\,dx 
\end{align*}
$$
其中我们用到了 $x \to \infty$ 时 $\Psi^*$ 与 $\frac{\partial \Psi^*}{\partial x}$ 均为零的性质。最后的结果和我们定义的算符形式一致，因此得到：
$$
\begin{equation*}
	\frac{d\langle p \rangle}{dt} = \left\langle -\frac{\partial V}{\partial x} \right\rangle
\end{equation*} \tag{1.22} \label{ehrenfest's-theorem}
$$
其称为 **埃伦菲斯特定理（Ehrenfest's Theorem）**。这也告诉了我们，牛顿第二定律中物理量的期望值在量子力学中依然满足同样的数学关系。



### 测不准原理

假设你拿着一根非常长的绳子的一端并不断抖动，绳子应该会呈现出某种”运动状态“，就好像一个波一样。此时如果有人问题”绳子的波具体在哪里“，你可能没法回答，因为绳子上波无处不在（极端情况下）！但如果他问你的是”绳子的波长是多少“，你或许可以给出一个合理的答案。另一种情况，假设你对一根非常长的静止绳子突然施加一段力，它会形成一个”鼓包“，此时你或许可以回答”波在哪里“，但它的波长是没有定义的（极端情况下）。所以我们面临一个抉择：如果想要知道波长，我们就得接受波在一个范围，而不是一个点；如果想要知道位置，那就得认识到”波“并不存在，因此没有波长的概念。任何中间情况，两者都没法得到确切的值。

**德布罗意公式（De Broglie Formula）** 描述了粒子动量与波长的关系：
$$
\begin{equation*}
	p = \frac{h}{\lambda} = \frac{2\pi\hbar}{\lambda}
\end{equation*} \tag{1.23}
$$
因此前面描述的波长和动量相关；测出的波长越准确，得到的动量也就越准确。**测不准原理（The Uncertainty Principle）** 定量叙述如下：
$$
\begin{equation*}
	\sigma_x\sigma_p \ge \frac{\hbar}{2}
\end{equation*} \tag{1.24}
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
这里我们省略了常数，这个常数会在 $\psi(x)$ 中补偿回来。$(\ref{time-independent-schrodinger-equation})$ 称为 **时间无关的薛定谔方程（Time-Independent Schrodinger Equation）**，我们需要其给出具体的势能函数 $V(x)$ 才能进一步求解。本章接下来的内容便是如何解不同设置下的时间无关的薛定谔方程。

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
  我们可以得到其对应的算符，即 **哈密顿算符（Hamiltonian Operator）**：
  $$
  \hat{H} = -\frac{\hbar^2}{2m}\frac{\partial^2}{\partial x^2} + V(x)
  $$
  因此，我们得到了时间无关的薛定谔方程的最简形式（熟悉线性代数的同学可能发现下面这种形式和特征值、特征矢量的相似性；的确如此，我们会在下一章着重介绍量子力学和线性代数的联系）：
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

最后，我们值得讨论一下 $E$ 和 $V$ 的大小关系。回忆时间无关的薛定谔方程 $(\ref{time-independent-schrodinger-equation})$ 可以等价写为：
$$
\begin{equation*}
	\frac{d^2\psi}{dx^2} = \frac{2m}{\hbar^2}[V(x) - E]\psi
\end{equation*} \tag{2.19}
$$
我们断言 $E \ge V_\text{min}$，即系统的 $E$ 一定大于 $V$ 的最小值。假设 $V(x) > E$ 始终成立，对上式两边同时乘一个 $\psi^*$ 后进行积分：
$$
\begin{align*}
	\int \psi^*\frac{d^2\psi}{dx^2}\,dx
		&= \left.\psi^*\frac{\partial \psi}{\partial x}\right|_{-\infty}^\infty - \int\frac{\partial \psi^*}{\partial x}\frac{\partial \psi}{\partial x}\,dx \\
		&= -\int \left|-\frac{\partial \psi}{\partial x}\right|^2\,dx \\
		&< 0 \\
	\int \psi^*\frac{2m}{\hbar^2}[V(x) - E]\psi
		&= \frac{2m}{\hbar}\int [V(x) - E]|\psi|^2\,dx \\
		&> 0
\end{align*}
$$
发现两边不可能相等。因此一定存在 $x$ 使得 $V(x) \le E$。

### 无限方井

让我们考虑一种势能分布：
$$
\begin{equation*}
	V(x) = \begin{cases}
		0, & 0 \le x \le a \\
		\infty & x < 0\ \text{或}\ x > a
	\end{cases}
\end{equation*} \tag{2.20}
$$
这种情况下，除了 $x = 0$ 或 $x = a$ 两个地方，其余位置的粒子受力均为零，而两个边界处受力为无穷大。如果类比成经典力学中的情形，就是空间中完全光滑轨道上有两个完全弹性的障碍物，物体只会在两个障碍物之间运动。

我们将这种设置称为 **无限方井（Infinite Square Well, ISW）**。正如其名，粒子仿佛被“困在”一个无限深的方井中无法出来。在方井中，由于势能为零，我们可以代入 $(\ref{time-independent-schrodinger-equation})$ 得到：
$$
\begin{equation*}
	-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} = E\psi
\end{equation*} \tag{2.21} \label{schrodinger-equation-of-infinite-square-well}
$$
这是一个典型的二阶常微分方程，其通解是：
$$
\begin{equation*}
	\psi(x) = A\sin kx + B\cos kx \qquad k = \frac{\sqrt{2mE}}{\hbar}
\end{equation*} \tag{2.22}
$$
为了确定上面的常数 $A, B$，我们需要给出一系列 **边界条件（Boundary Conditions）**。我们通常假定 $\psi$ 和 $d\psi/dx$ 是连续的，此处由于 $V$ 可能是 $\infty$，所以只有第一个条件是成立的。此时有：
$$
\psi(0) = \psi(a) = 0
$$
带入到原式中，可以得到 $B = 0$ 以及 $ka = \pm n\pi$，所以以下形式的 $k$ 都可以满足要求：
$$
\begin{equation*}
	k_n = \frac{n\pi}{a} \quad n = 1, 2, 3, ...
\end{equation*} \tag{2.23}
$$
这样我们也能确定总能量：
$$
\begin{equation*}
	E_n = \frac{\hbar^2k_n^2}{2m} = \frac{n^2\pi^2\hbar^2}{2ma^2}
\end{equation*} \tag{2.24}
$$
为了得到 $A$ 的确定值，我们需要对 $\psi$ 进行归一化：
$$
\int_0^a |A|^2\sin^2 kx\,dx = |A|^2\frac{a}{2} = 1 \implies |A|^2 = \frac{2}{a}
$$
我们直接取 $A = \sqrt{2/a}$。这样就得到了无限方井内的解：
$$
\begin{equation*}
	\psi_n(x) = \sqrt{\frac{2}{a}}\sin\left(\frac{n\pi x}{a}\right)
\end{equation*} \tag{2.25} \label{wave-function-of-infinite-square-well}
$$
下面是在 $n = 1, 2, 3$ 时的图像：

![qm1_2-1](graphs/qm1_2-1.png)

可以看到，$n$ 对应着图像中波峰/波谷出现的数量。我们将 $n = 1$ 称为 **基态（Ground State）**，其它的情形则称为 **激发态（Excited State）**。这些状态之间有一些共同点：

- 它们关于方井中心交替呈现 **奇性** 与 **偶性**，也即镜面对称和中心对称。比如 $\psi_1, \psi_3, ...$ 为偶，$\psi_2, \psi_4, ...$ 为奇。

- 它们与 $0$ 的交点数量（称为 **结点**）逐个增加。

- 它们之间相互 **正交（Orthogonal）**，定义为：
  $$
  \begin{equation*}
  	\int\psi_m^*(x)\psi_n(x)\,dx = 0 \qquad (m \ne n)
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
  当 $m = n$ 时，根据波函数的归一性上面的积分会得到 $1$。因此我们可以将一般情形的积分结果记为：
  $$
  \begin{equation*}
  	\int \psi_m^*(x)\psi_n(x)\,dx = \delta_{mn}
  \end{equation*} \tag{2.26} \label{orthogonality-of-wave-functions}
  $$
  此处记号 $\delta_{mn}$ 是 **克罗内克函数**，定义为：
  $$
  \begin{equation*}
  	\delta_{mn} =
  	\begin{cases}
  		0 & m \ne n \\
  		1 & m = n
  	\end{cases}
  \end{equation*} \tag{2.27} \label{kronecker-delta}
  $$
  
- 它们是 **完备的（Complete）**，即任何函数 $f(x)$ 都可以表示为它们的线性和（我们不在本篇笔记中证明这一点）：
  $$
  \begin{equation*}
  	f(x) = \sum_{n=1}^\infty c_n\psi_n(x) = \sqrt{\frac{2}{a}}\sum_{n=1}^\infty c_n\sin\left(\frac{n\pi x}{a}\right)
  \end{equation*} \tag{2.28}
  $$
  这种表示方式被称为 $f(x)$ 的 **傅立叶级数（Fourier Series）**。这个性质也被称为 **狄利克雷定理（Dirichlet's Theorem）**。为了确定这里的常数 $c_n$，我们需要用到一个小技巧（也称为 **傅里叶技巧（Fourier's Trick）**）：
  $$
  \begin{equation*}
  	\int\psi_m^*(x)f(x)\,dx = \sum_{n=1}^\infty c_n\int\psi_m^*(x)\psi_n(x)\,dx = \sum_{n=1}^\infty c_n\delta_{mn} = c_m
  \end{equation*} \tag{2.29} \label{fourier's-trick}
  $$

上面这四个性质不仅对无限方井，对大多数的势能分布都满足。尤其是正交性和完备性非常有用，我们多数情况下会假设它们成立。

至此，我们得到了无限方井的单个状态的通解（时间相关）：
$$
\begin{equation*}
	\Psi_n(x, t) = \sqrt{\frac{2}{a}}\sin\left(\frac{n\pi x}{a}\right)\exp\left(-i\frac{n^2\pi^2\hbar}{2ma^2}t\right)
\end{equation*} \tag{2.30}
$$
所有状态叠加起来的通解（时间相关）则是：
$$
\begin{equation*}
	\Psi(x, t) = \sum_{n=1}^\infty c_n\sqrt{\frac{2}{a}}\sin\left(\frac{n\pi x}{a}\right)\exp\left(-i\frac{n^2\pi^2\hbar}{2ma^2}t\right)
\end{equation*} \tag{2.31}
$$
为了得到 $c_n$，我们可以令 $t = 0$，然后根据正交性得到：
$$
\begin{equation*}
	c_n = \sqrt{\frac{2}{a}}\int_0^a\sin\left(\frac{n\pi x}{a}\right)\Psi(x, 0)\,dx
\end{equation*} \tag{2.32}
$$
这里的 $c_n$ 满足模平方之和为 $1$，我们可以通过归一化条件得到该结论：
$$
\begin{align*}
	1 = \int |\Psi(x, 0)|^2\,dx
	&= \int \left[\sum_{m=1}^\infty c_m\psi_m(x)\right]^*\left[\sum_{n=1}^\infty c_n\psi_n(x)\right]\,dx \\
	&= \sum_{m=1}^\infty \sum_{n=1}^\infty c_m^*c_n\int\psi_m(x)^*\psi_n(x)\,dx \\
	&= \sum_{m=1}^\infty \sum_{n=1}^\infty c_m^*c_n\delta_{mn} \\
	&= \sum_{n=1}^\infty |c_n|^2
\end{align*} \tag{2.33}
$$
同时能量的期望是：
$$
\begin{align*}
	\langle H\rangle 
	&= \int \Psi^* \hat{H}\Psi\,dx \\
	&= \int \left(\sum c_m\psi_m\right)^*\hat{H}\left(\sum c_n\psi_n\right)\,dx \\
	&= \sum\sum c_m^*c_nE_n \int \psi_m^*\psi_n\,dx \\
	&= \sum|c_n|^2E_n
\end{align*} \tag{2.34}
$$

### 自由粒子

这一节讨论没有任何约束的环境，即：
$$
V(x) = 0 \tag{2.35}
$$
此时薛定谔方程变为：
$$
-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} = E\psi \implies \frac{d^2\psi}{dx^2} = -k^2\psi \qquad \text{其中 $k = \frac{\sqrt{2mE}}{\hbar}$} \tag{2.36}
$$
其通解为（这里我们采用指数形式，其原因很快就会提到）：
$$
\begin{equation*}
	\psi(x) = Ae^{ikx} + Be^{-ikx}
\end{equation*} \tag{2.37}
$$
至此，一切和无限方井的情况相同，但现在我们没有任何边界条件，所以不需要确定 $A$ 和 $B$ 的值。时间相关的波函数解即是：
$$
\begin{equation*}
	\Psi(x, t) = A^{ik\left(x - \frac{\hbar k}{2m}t\right)} + Be^{-ik\left(x + \frac{\hbar k}{2m}t\right)}
\end{equation*} \tag{2.38}
$$
敏锐的同学可能已经发现，这分明是 **波方程（Wave Equation）** 的通解：
$$
\begin{equation*}
	\frac{\partial^2 \Psi}{\partial t^2} = \frac{\hbar^2 k^2}{4m^2}\frac{\partial^2 \Psi}{\partial x^2}
\end{equation*} \tag{2.39} \label{wave-equation}
$$
再对波函数进行简单分析，可以看出第一项是一个从左向右传播的波，第二项是从右向左传播的波，它们速度相同方向相反（大小即是 $\frac{\hbar k}{2m}$）。因此我们可以根据 $k$ 的值确定波函数（这里实际上就是通过让 $k$ 可以取负值，从而让两项合并到一项）：
$$
\begin{equation*}
	\Psi_k(x, t) = Ae^{i\left(kx - \frac{\hbar k^2}{2m}t\right)}
\end{equation*} \tag{2.40}
$$
这里的 $k$ 是任何满足下列的常数：
$$
\begin{equation*}
	k = \pm \frac{\sqrt{2mE}}{\hbar}
\end{equation*} \tag{2.41}
$$
根据德布罗意公式 $p = \frac{2\pi\hbar}{\lambda}$，我们可以计算出波的动量：
$$
\begin{equation*}
	p = \hbar k
\end{equation*} \tag{2.42}
$$
此前我们已经算出波的传递速度：
$$
\begin{equation*}
	v_\text{quantum} = \frac{\hbar |k|}{2m} = \sqrt{\frac{E}{2m}}
\end{equation*} \tag{2.43}
$$
我们注意到它和经典物理中速度的区别（通过动能公式 $E = \frac{1}{2}mv^2$ 得到）：
$$
\begin{equation*}
	v_\text{classical} = \sqrt{\frac{2E}{m}} = 2v_\text{quantum}
\end{equation*} \tag{2.44}
$$
这个差异究竟是为什么？此外，如果我们尝试对自由粒子的波函数归一化，会发现无法完成：
$$
\begin{equation*}
	\int_{-\infty}^\infty \Psi_k^*\Psi_k\,dx = |A|^2\int_{-\infty}^\infty\,dx = +\infty
\end{equation*} \tag{2.45}
$$
对此的解释是，$\Psi_k$ 无法代表自由粒子的一个合理状态：这和无限方井是不同的。我们需要将所有 $k$ 的情形都考虑在内才能得到合理的状态。由于 $k$ 不是离散的，我们需要求积分而非求和：
$$
\begin{equation*}
	\Psi(x, t) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^{\infty}\phi(k)e^{i(kx - \frac{\hbar k^2}{2m}t)}\,dk
\end{equation*} \tag{2.46} \label{wave-function-of-free-particle}
$$
上式中凭空出现的 $\frac{1}{\sqrt{2\pi}}\phi(k)$ 是用来对这个积分归一化的参数。为了算出这个 $\phi(k)$，我们只需要尝试将 $\Psi(x, 0)$ 归一化。根据 **普朗歇尔定理（Plancherel's Theorem）**，有：
$$
\begin{equation*}
	f(x) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^\infty F(k)e^{ikx}\,dk \Longleftrightarrow F(k) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^\infty f(x)e^{-ikx}\,dx
\end{equation*} \tag{2.47} \label{plancherel's-theorem}
$$
这里 $F(k)$ 是 $f(x)$ 的 **傅立叶变换（Fourier Transform）**，$f(x)$ 是 $F(k)$ 的 **逆傅立叶变换（Inverse Fourier Tranform）**。这样我们就得到了 $\phi(k)$ 的公式：
$$
\begin{equation*}
	\phi(k) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^\infty \Psi(x, 0)e^{-ikx}\,dx
\end{equation*} \tag{2.48}
$$

现在让我们尝试解释此前速度的悖论：为什么 $\Psi_k$ 传播的速度和波不同呢？实际上，$\Psi_k$ 本身就是一个“捏造”的状态，我们已经提到过它并不构成自由粒子系统中的合法状态，因此它的速度并不重要。现实中，一些波的轮廓会以 **波包（Wave Packet）** 的形式出现，它也像波一样传播，其速度被称为 **群速度（Group Velocity）**；而 $\Psi_k$ 本身，也就是组成波包的“小波”的速度称为 **相速度（Phase Velocity）**。下面这张图形象地展示了两者的构成：

<img src="graphs/qm1_2-5.png" alt="Wave packet" style="zoom:67%;" />

下面我们来证明，群速度正是之前通过动能公式计算出的，两倍于组速度的结果。我们首先将自由粒子的波函数通解写成一般的形式：
$$
\begin{equation*}
	\Psi(x, t) = \frac{1}{2\pi}\int_{-\infty}^\infty \phi(k)e^{i(kx - \omega t)}
\end{equation*} \tag{2.49}
$$
其中的 $\omega$ 代表波的频率，和 $k$ 的关系（称为 **色散关系（Dispersion Relation）**）是：
$$
\begin{equation*}
	\omega = \frac{\hbar k^2}{2m}
\end{equation*} \tag{2.50}
$$
不过下面的推导和这个特定的色散关系无关；我们的逻辑适用于所有的波包。现在假设 $\phi(k)$ 在 $k_0$ 处达到峰值，从上图中不难发现积分在除了 $k_0$ 附近的地方都可以被忽略。同时我们将 $\omega$ 写成泰勒展开的形式（并忽略第三项及以后的无穷小量）：
$$
\begin{equation*}
	\omega(k) = \omega_0 + \omega_0'(k - k_0)
\end{equation*} \tag{2.51}
$$
其中 $\omega_0 = \omega(k_0)$。最后让我们对原积分进行换元：$s = k - k_0$（这样我们就可以在 $k_0$ 周围进行积分了）：
$$
\begin{align*}
	\Psi(x, t) &\approx \frac{1}{\sqrt{2\pi}}\int_{-\infty}^\infty \phi(s + k_0)e^{i[(s + k_0)x - (\omega_0 + \omega_0's)t]}\,ds \\
	&= \frac{1}{\sqrt{2\pi}}e^{i(k_0x - \omega_0t)}\int_{-\infty}^\infty \phi(s + k_0)e^{is(x - \omega_0't)}\,ds
\end{align*} \tag{2.52}
$$
可以看出我们从积分中提出了一个波，这就是波包中小波的近似形式，其速度是：
$$
\begin{equation*}
	v_p = \frac{\omega}{k}
\end{equation*} \tag{2.53}
$$
相应地，波包整体的传递性质由积分内的关于 $x - \omega_0't$ 的函数描述，因此组速度是：
$$
\begin{equation*}
	v_g = \frac{d\omega}{dk}
\end{equation*} \tag{2.54}
$$
对于自由粒子，其相速度和组速度分别是：
$$
\begin{align*}
	v_p = \frac{\omega}{k} = \frac{\hbar k}{2m} \qquad 
	v_g = \frac{d\omega}{dk} = \frac{\hbar k}{m}
\end{align*} \tag{2.55}
$$
显然有：
$$
\begin{equation*}
	v_g = 2v_p
\end{equation*} \tag{2.56}
$$

### 有限方井

#### $E < 0$  的情况

 一个类似无限方井的设定是 **有限方井（Finite Square Well）**，其势能分布如下：
$$
\begin{align*}
	V(x) = 
	\begin{cases}
		-V_0 & -a \le x \le a \\
		0 & |x| > a
	\end{cases}
\end{align*} \tag{2.57}
$$
此处 $V_0$ 是某个常数。对于 $E < 0$ 或 $E > 0$，我们发现结果有显著的区别。我们先研究前一种情形。当 $x < -a$ 时，$V(x) = 0$，此时有：
$$
\begin{equation*}
	-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} = E\psi \implies \frac{d^2\psi}{dx^2} = \kappa^2\psi \quad \text{其中 $\kappa = \frac{\sqrt{-2mE}}{\hbar}$}
\end{equation*} \tag{2.58}
$$
它的通解是：
$$
\begin{equation*}
	\psi(x) = Ae^{-\kappa x} + Be^{\kappa x}
\end{equation*} \tag{2.59}
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
\begin{equation*}
	-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} = (V_0 + E)\psi \implies \frac{d^2\psi}{dx^2} = -l^2\psi \quad \text{其中 $l = \frac{\sqrt{2m(E + V_0)}}{\hbar}$}
\end{equation*} \tag{2.60}
$$
它的通解是：
$$
\begin{equation*}
	\psi(x) = D\sin(lx) + F\cos(lx)
\end{equation*} \tag{2.61}
$$
考虑 $V$ 的对称性，为了让 $\psi$ 和 $\frac{d\psi}{dx}$ 在 $\pm a$ 处连续，我们最终得到的应该是类似于下面的结果（注意 $x$ 的范围和前面使用的不太一样）：
$$
\psi(x) = 
\begin{cases}
	Ce^{-\kappa x} & x > a \\
	F\cos(lx) & 0 < x < a \\
	\psi(-x) & x < 0
\end{cases} \tag{2.62} \label{2.62}
$$
其中第二项取余弦而非正弦是因为它在 $x = 0$ 处的梯度是连续的。将边界条件代入，有：
$$
\begin{align*}
\begin{cases}
	Ce^{-\kappa a} = F\cos(la)  \\
	-\kappa Ce^{-\kappa a} = -lF\sin(la)
\end{cases}
\implies
\kappa = l\tan(la)
\end{align*} \tag{2.63}
$$
回忆 $l$ 是一个和 $E$ 相关的变量，因此这个公式给出了所有可能的 $E$。为了得到更好的形式，记 $z = la$，$z_0 = \frac{a}{\hbar}\sqrt{2mV_0}$。由我们对 $\kappa$ 和 $l$ 的定义，有 $\kappa^2 + l^2 = \frac{2mV_0}{\hbar^2}$，也即 $\kappa^2 + l^2 = (\frac{z_0}{a})^2$，将 $l = \frac{z}{a}$ 代入后得到：
$$
\begin{equation*}
	\tan{z} = \sqrt{\left(\frac{z_0}{z}\right)^2 - 1}
\end{equation*} \tag{2.64} \label{2.64}
$$
再次回顾上式中变量的含义：$z_0$ 是和问题设置有关的常量，$z$ 是和 $E$ 相关的量。下面是当 $z_0 = 8$ 时的示意图：

<img src="graphs/qm1_2-2.png" alt="qm1_2-2" style="zoom:80%;" />

可以看到，只有 $\tan{z} = \sqrt{(z_0/z)^2-1}$ 的地方，$z$ 对应的能量才是允许取的值（恕我们无法写出具体的解析解）。让我们针对 $z_0$ 的两种极端情形讨论有限方井的性质：

- 深且宽的方井：当 $z_0$ 很大时，上图中的 $\sqrt{(z_0/z)^2-1}$ 图像会向上移动到较高的地方，此时它与 $z$ 轴的交点也会大幅向右移动。观察 $\tan{z}$ 的图像，可以发现它和 $\sqrt{(z_0/z)^2 - 1}$ 的交点会在大约 $z_n = n\pi/2$ 的位置，此时对应的能量是：
  $$
  \begin{equation*}
  	E_n + V_0 \approx \frac{n^2\pi^2\hbar^2}{2m(2a)^2} \quad \text{其中 $n$ 是奇数}
  \end{equation*} \tag{2.65} \label{2.65}
  $$
  等式右侧这个形式我们应该比较熟悉：它完美对应了宽度为 $2a$ 的无限方井中允许取的能量里一半的情形。这也符合我们的预期：当方井深度无限加大时（$V_0 \to \infty$），它就类似于一个无限方井。

- 浅且窄的方井：当 $z_0$ 变小时，能取得的状态会越来越少；当 $z_0 < \frac{\pi}{2}$ 时只有一个。有趣的是，无论 $z_0$ 多么小，始终至少有一个合法的状态。

接下来，我们尝试将波函数 $(\ref{2.62})$ 归一化，即：
$$
\begin{align*}
	\int |\psi|^2\,dx 
		&= 2\left(\int_0^a|\psi|^2\,dx + \int_a^\infty |\psi|\,dx\right) \\
		&= 2\left(|F|^2\int_0^a \cos^2(lx)\,dx + |C|^2\int_a^\infty e^{-2\kappa x}\,dx \right)  \\
		&= 2\left[|F|^2\left(\left.\frac{x}{2} + \frac{\sin(2lx)}{4l}\right)\right|_0^a + |C|^2\left.\frac{e^{-2\kappa x}}{-2\kappa}\right|_a^\infty\right] \\
		&= 2|F|^2\left(\frac{a}{2} + \frac{\sin(2la)}{4l}\right) + 2|C|^2\frac{e^{-2\kappa a}}{2\kappa}
\end{align*}
$$
根据波函数在 $x = a$ 点的连续性，有：
$$
Ce^{-\kappa a} = F\cos(la) \implies C = F e^{\kappa a}\cos(la)
$$
代入之前没有完成的归一化算式：
$$
\begin{align*}
	\int |\psi|^2\,dx
		&= 2|F|^2\left(\frac{a}{2} + \frac{\sin(2la)}{4l}\right) + 2|F|^2e^{2\kappa a}\cos^2(la)\frac{e^{-2\kappa a}}{2\kappa} \\
		&= |F|^2\left(a + \frac{\sin(2la)}{2l} + \frac{\cos^2(la)}{\kappa}\right) \\
		&= |F|^2\left(a + \frac{2\sin(la)\cos(la)}{2l} + \frac{\cos^2(la)}{l\tan(la)}\right) \\
		&= |F|^2\left(a + \frac{\sin^2(la) + \cos^2(la)}{l\tan(la)}\right) \\
		&= |F|^2\left(a + \frac{1}{\kappa}\right) \\
		&= 1
\end{align*}
$$
因此我们得到：
$$
\begin{align*}
	F = \frac{1}{\sqrt{a + \frac{1}{\kappa}}} \qquad C = \frac{e^{\kappa a}\cos(la)}{\sqrt{a + \frac{1}{\kappa}}}
\end{align*} \tag{2.66}
$$
因此，有限方井的单个解便是：
$$
\begin{equation*}
	\Psi(x, t) = \frac{1}{\sqrt{a + \frac{1}{\kappa}}}e^{-iEt}
	\begin{cases}
		e^{\kappa(a - x)}\cos(la) & |x| > a \\
		\cos(lx) & |x| < a
	\end{cases}
\end{equation*} \tag{2.67} \label{wave-function-of-finite-square-well}
$$
这里的 $\kappa$ 和 $l$ 都通过该状态下的能量决定：
$$
\begin{equation*}
	\kappa = -\frac{\sqrt{-2mE}}{\hbar} \qquad l = \frac{\sqrt{2m(E + V_0)}}{\hbar}
\end{equation*} \tag{2.68}
$$
而有效的状态可以从之前我们设的 $z$ 得到（回忆 $z$ 是方程 $(\ref{2.64})$ 的解）：
$$
\begin{equation*}
	E = \frac{z^2\hbar^2}{2ma^2} - V_0
\end{equation*} \tag{2.69}
$$
此前已经讨论过 $z_0$ 很大时它的近似结果 $(\ref{2.65})$。有限方井的通解需要将所有可能的状态加起来，这里不赘述了。

#### $E > 0$ 的情况

现在研究 $E > 0$ 的有限方井。此时对于 $x < -a$ 时，情况和自由粒子是完全一致的：
$$
\begin{equation*}
	\psi(x) = Ae^{ikx} + Be^{-ikx} \qquad \text{其中 $k = \frac{\sqrt{2mE}}{\hbar}$}
\end{equation*} \tag{2.70}
$$

对于 $-a < x < a$ 则有（下面的 $l$ 和我们此前的定义一致）：
$$
\begin{equation*}
	\psi(x) = C\sin(lx) + D\cos(lx)
\end{equation*} \tag{2.71}
$$
而 $x > a$ 时有和 $x < - a$ 类似的形式（下面的 $k$ 定义相同）：
$$
\psi(x) = Fe^{ikx} + Ge^{-ikx} \tag{2.72}
$$
我们可以仿照自由粒子的处理，将其写成波方程的解：
$$
\begin{equation*}
	\Psi(x, t) = Ae^{ik(x - \frac{\hbar k}{2m}t)} + Be^{-ik(x + \frac{\hbar k}{2m}t)}
\end{equation*} \tag{2.73}
$$
它可以视为两个相同频率的波的线性叠加，其中 $A$ 系数项描述了一个向 $\unit{x}$ 方向移动的 **入射波（Incident Wave）**，而 $B$ 系数项描述的是一个向 $-\unit{x}$ 方向移动的 **反射波（Reflected Wave）**。$A$ 和 $B$ 分别是它们的振幅，波的两个速度和自由粒子的情形一致。同样地，$x > a$ 时我们有：
$$
\begin{equation*}
	\Psi(x, t) = Fe^{ik(x - \frac{\hbar k}{2m}t)} + Ge^{-ik(x + \frac{\hbar k}{2m}t)}
\end{equation*} \tag{2.74}
$$
这里我们进行一个微妙的处理，让 $G = 0$，此时 $y$ 轴右侧仅有一个 $\unit{x}$ 方向的 **透射波（Transmitted Wave）**，振幅为 $F$。可以看到我们将有限方井在 $E > 0$ 时处理为波在介质间的传播现象：一束波从 $y$ 轴左侧向右方前进，然后一部分在方井边缘处反射回来，一部分透射过去。下面让我们根据边界条件，计算 $A$、$B$、$C$、$F$ 之间的关系。

首先波函数在 $x = \pm a$ 处连续：
$$
\begin{cases}
	Ae^{-ika} + Be^{ika} &= -C\sin(la) + D\cos(la) \\
	Fe^{ika} &= C\sin(la) + D\cos(la)
\end{cases}
$$
然后波函数的梯度 $\frac{d\psi}{dx}$ 也应该在 $\pm a$ 处连续：
$$
\begin{cases}
	ik(Ae^{-ika} - Be^{ika}) &= l[C\cos(la) + D\sin(la)] \\
	ikFe^{ika} &= l[C\cos(la) - D\sin(la)]
\end{cases}
$$
我们可以将 $A$、$B$、$C$、$D$ 都表示成 $F$ 的形式，其中：
$$
\begin{align*}
	A &= \left[\cos(2la) - i\frac{k^2 + l^2}{2kl}\sin(2la)\right]e^{2ika}F \\
	B &= i\frac{\sin(2la)}{2kl}(l^2 - k^2)F
\end{align*} \tag{2.75}
$$
我们可以由此计算出 **反射系数（Reflection Coefficient）** 和 **透射系数（Transmission Coefficient）**：
$$
\begin{align*}
	T^{-1} &= \frac{|A|^2}{|F|^2} = 1 + \frac{V_0}{4E(E + V_0)}\sin^2\left[\frac{2a}{\hbar}\sqrt{2m(E + V_0)}\right]  \\
	R &= \frac{|B|^2}{|A|^2} = 1 - T
\end{align*} \tag{2.76}
$$
上面我们将所有 $l$、$k$ 都替换成了原始的变量 $E$、$V_0$ 等。虽然结果依然很复杂，我们可以发现由于正弦函数的周期性，透射系数 $T$ 会在特定时候取最大值 $1$，此时即：
$$
\begin{equation*}
	\frac{2a}{\hbar}\sqrt{2m(E + V_0)} = n\pi \implies E_n = \frac{n^2\pi^2\hbar^2}{2m(2a)^2} - V_0
\end{equation*} \tag{2.77}
$$
这里的能量再次和无线方井中的一致（不过需要注意，$E_n$ 仅仅是使得波完美透射的能量，事实上任何 $E > 0$ 都是合法的）。下面是一个透射系数和能量 $E$ 的关系图：

<img src="graphs/qm1_2-4.png" alt="Transmission coefficient and energy" style="zoom:67%;" />

本章的最后，我们会再次回顾类似这样的两种不同情况，并探究其物理含义。



### 简谐振子

回忆在经典力学中，我们将满足力与位移成线性关系的运动称为简谐运动，可以用 **胡克定律（Hooke's Law）** 来描述：
$$
F = -kx = m\frac{d^2x}{dt} \tag{2.78}
$$
它的解是三角函数的线性组合：
$$
\begin{equation*}
	x(t) = A\sin(\omega t) + B\cos(\omega t)
\end{equation*} \tag{2.79}
$$
其中角频率 $\omega$ 定义为：
$$
\begin{equation*}
	\omega = \sqrt{\frac{k}{m}}
\end{equation*} \tag{2.80} \label{2.80}
$$
势能和位移的平方成正比：
$$
V(x) = \frac{1}{2}kx^2 \tag{2.81}
$$
实际上，对于大多数运动，如果将其势能通过幂级数展开，在局部的一个最小值 $x = x_0$ 处有：
$$
V(x) = V(x_0) + V'(x_0)(x - x_0) + \frac{1}{2}V''(x_0)(x - x_0)^2 + \dots \tag{2.82}
$$
由于势能的参照点是任选的，我们可以令 $V(x_0) = 0$。同时由于 $V'(x_0) = 0$，我们只需要考虑第三项，即：
$$
V(x) \approx \frac{1}{2}V''(x_0)(x - x_0)^2 \tag{2.83}
$$
此处只需设 $k = V''(x_0) \ge 0$，就可以得到一个近似的简谐振子了。这在经典力学中是非常常用的近似。量子力学中，我们也有类似的假设；当势能满足下面的等式时，其描述的就是一个简谐振子（不难发现其满足色散关系 $(\ref{2.80})$）：
$$
\begin{equation*}
	V(x) = \frac{1}{2}m\omega^2x^2
\end{equation*} \tag{2.84}
$$
此时薛定谔方程变为：
$$
\begin{equation*}
	-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} + \frac{1}{2}m\omega^2x^2\psi = E\psi
\end{equation*} \tag{2.85} \label{schrodinger-euqation-of-harmonic-oscillator}
$$
其中的势能项让这个常微分方程变成非齐次方程，因此我们需要做一次换元：
$$
\xi = \sqrt{\frac{m\omega}{\hbar}}x \tag{2.86}
$$
这样就将原方程转化为：
$$
\frac{d^2\psi}{d\xi^2} = \left(\xi^2 - \frac{2E}{\hbar\omega}\right)\psi = (\xi^2 - K)\psi \qquad \text{其中 $K = \frac{2E}{\hbar\omega}$} \tag{2.87}
$$
注意到当 $\xi$ 非常大（也即 $x$ 非常大）时，等式右侧约等于 $\xi^2\psi$，这就得到了一个近似的方程解：
$$
\begin{equation*}
	\psi(\xi) \approx Ae^{-\xi^2/2} + Be^{\xi^2/2}
\end{equation*} \tag{2.88}
$$
在 $\xi$ 很大时第二项显然会炸掉，因此有 $B = 0$，此外，我们希望能用一个相对简单的表达式 $h(\xi)$ 作为系数，得到一个精准的解：
$$
\begin{equation*}
	\psi(\xi) = h(\xi)e^{-\xi^2/2}
\end{equation*} \tag{2.89} \label{2.89}
$$
我知道这一步看起来很迷惑，但简单概括就是对于非齐次的常微分方程，我们通常可以通过一些模板得到其精确的级数解。这里我们需要找到满足方程 $\psi'' = \xi^2\psi$ 的特殊解，其一般形式就是 $(\ref{2.89})$ 这样的。为了求得 $h(\xi)$，让我们将 $\psi$ 的导数和二阶导数代换为 $h$ 的表达式：
$$
\frac{d\psi}{d\xi} = \left(\frac{dh}{d\xi} - \xi h\right)e^{-\xi^2/2} \\
\frac{d^2\psi}{d\xi^2} = \left(\frac{d^2h}{d\xi^2} - 2\xi\frac{dh}{d\xi} + (\xi^2 - 1)h\right)e^{-\xi^2/2}
$$
因此薛定谔方程 $(\ref{schrodinger-euqation-of-harmonic-oscillator})$ 就可以改写为：
$$
\begin{equation*}
	\frac{d^2h}{d\xi^2} - 2\xi\frac{dh}{d\xi} + (K - 1)h = 0
\end{equation*} \tag{2.90}
$$
可以看到我们成功将非齐次的微分方程变为齐次的了。接下来，我们假定 $h(\xi)$ 的形式是多项式：
$$
\begin{equation*}
	h(\xi) = a_0 + a_1\xi + a_2\xi^2 + \dots = \sum_{j=0}^\infty a_j\xi^j 
\end{equation*} \tag{2.91} \label{2.91}
$$
它的导数非常好求：
$$
\frac{dh}{d\xi} = a_1 + 2a_2\xi + 3a_3\xi^2 + \dots = \sum_{j=0}^\infty ja_j\xi^{j-1} \\
\frac{d^h}{d\xi^2} = 2a^2 + 2\cdot 3a_3\xi + 3\cdot 4a_4\xi^2 + \dots = \sum_{j=0}^\infty(j+1)(j+2)a_{j+2}\xi^j
$$
现在将上面全部代入原方程中，得到：
$$
\sum_{j=0}^\infty [(j+1)(j+2)a_{j+2} - 2ja_j + (K - 1)a_j]\xi^j = 0 \tag{2.92}
$$
为了保证对于任意 $\xi$ 都成立，级数中的系数一定是零，我们得到下面的递归关系：
$$
\begin{equation*}
	a_{j+2} = \frac{(2j + 1 - K)}{(j+1)(j+2)}a_j
\end{equation*} \tag{2.93}
$$
这样我们就可以通过两个常数 $a_0$ 和 $a_1$ 来推出所有其它系数。下面罗列了一些具体的递归关系：
$$
\begin{align*}
	a_2 &= \frac{1 - K}{2}a_0 & a_4 &= \frac{5 - K}{12}a_2 = \frac{(5 - K)(1 - K)}{24}a_0 & \dots \\
	a_3 &= \frac{3 - K}{6}a_1 & a_5 &= \frac{7 - K}{20}a_3 = \frac{(7 - K)(3 - K)}{120}a_1 & \dots
\end{align*}
$$
当 $j$ 很大时，上面这个关系约为：
$$
a_{j+2} \approx \frac{2}{j}a_j \implies a_j \approx \frac{C}{(j/2)!} \tag{2.94}
$$
需要注意的是，如果我们接纳所有 $j$，其结果是不可归一化的：
$$
h(\xi) \approx C\sum_j^\infty \frac{1}{(j/2)!}\xi^j \approx \sum_j^\infty \frac{1}{j!}\xi^{2j} \approx Ce^{\xi^2} \tag{2.95}
$$
因此我们需要 $a_j$ 在某一项之后“消失”，设此时 $j = n$，则从递推公式中不难推出：
$$
K = 2n + 1
$$
如果 $n$ 是偶数，我们还得要求 $a_1 = 0$ （这样才能使得奇数项的不会溢出）；否则需要 $a_0 = 0$。最后，根据 $K$ 的定义我们可以得到不同 $n$ 下的能量：
$$
\begin{equation*}
	E_n = \left(n + \frac{1}{2}\right)\hbar\omega
\end{equation*} \tag{2.96} \label{energy-of-harmonic-oscillator}
$$
至此，我们惊奇地发现，简谐振子可能的能量是离散的；虽然薛定谔方程 $(\ref{schrodinger-euqation-of-harmonic-oscillator})$ 中看起来 $E$ 在任意情况下都有解，但是它们中的大多数都会导致波函数无法归一化。这听起来似乎非常精妙，我们可以观察两个和上面的能量稍微不同时波函数的图像来领会这一点：

<img src="graphs/qm1_2-6.png" alt="image-20220510141445901" style="zoom:70%;" />

上面是 $E = 0.49\hbar \omega$ 的情形，可以看到当 $|x|$ 变大时，$\psi(x)$ 也趋于无穷大。

<img src="graphs/qm1_2-7.png" alt="image-20220510141632732" style="zoom:70%;" />

上面则是 $E = 0.51\hbar \omega$ 的情形，当 $|x|$ 变大时，$\psi(x)$ 趋于负无穷大。与之相对地，当 $E$ 满足 $(\ref{energy-of-harmonic-oscillator})$ 时，波函数的图像是下面这样的：

<img src="graphs/qm1_2-8.png" alt="image-20220510142204035" style="zoom:67%;" />

可以看到 $|\psi_n(x)|^2$ 被精准地控制到了可以归一化的程度。

下面让我们完成前面没有做完的事情，将简谐振子的波函数求出来。首先可以将 $a_j$ 的递推公式变为 $n$ 相关的式子：
$$
a_{j+2} = -\frac{2(n - j)}{(j+1)(j+2)}a_j
$$
当 $n = 0$ 时，唯一存在的系数是 $a_0$，此时根据 $(\ref{2.91})$ 有：
$$
h_0(\xi) = a_0
$$
因此波函数是：
$$
\psi_0(\xi) = a_0e^{-\xi^2/2}
$$
让我们尝试对其归一化：
$$
\begin{align*}
	\int |\psi_0(\xi)|^2\,dx
		&= |a_0|^2 \int e^{-\xi^2}\sqrt{\frac{\hbar}{m\omega}}\,d\xi \\
		&= \sqrt{\frac{\hbar}{m\omega}}|a_0|^2\int_{-\infty}^\infty e^{-\xi^2}\,d\xi \\
		&= \sqrt{\frac{\hbar\pi}{m\omega}}|a_0|^2 \\
		&= 1
\end{align*}
$$
因此有：
$$
\begin{equation*}
	a_0 = \left(\frac{m\omega}{\hbar \pi}\right)^{1/4}
\end{equation*}
$$
所以基态的简谐振子，其波函数是：
$$
\begin{equation*}
	\psi_0(x) = \left(\frac{m\omega}{\hbar\pi}\right)^{1/4}e^{-\tfrac{m\omega}{2\hbar}x^2}
\end{equation*}
$$
当 $n = 1$ 时，唯一存在的系数是 $a_1$，我们和上面类似可以得到：
$$
h_1(\xi) = a_1\xi
$$
此时波函数就是：
$$
\begin{equation*}
	\psi_1(\xi) = a_1\xi e^{-\xi^2/2}
\end{equation*}
$$
对这个函数的归一化要复杂一些：
$$
\begin{align*}
	\int |\psi_1(\xi)|^2\,dx
		&= |a_1|^2\int \xi^2e^{-\xi^2}\sqrt{\frac{\hbar}{m\omega}}\,d\xi \\
		&= \sqrt{\frac{\hbar}{m\omega}}|a_1|^2 \int \xi^2e^{-\xi^2}\,d\xi \\
		&= \sqrt{\frac{\hbar}{m\omega}}|a_1|^2 \left(-\left.\frac{1}{2}\xi e^{-\xi^2}\right|_{-\infty}^\infty + \int_{-\infty}^\infty \frac{1}{2}e^{-\xi^2}\,d\xi\right) \\
		&= \frac{1}{2}\sqrt{\frac{\hbar}{m\omega}}|a_1|^2 \int_{-\infty}^\infty e^{-\xi^2}\,d\xi \\
		&= \frac{1}{2}\sqrt{\frac{\hbar\pi}{m\omega}}|a_1|^2 \\
		&= 1
\end{align*}
$$
因此有：
$$
\begin{equation*}
	a_1 = \frac{1}{\sqrt{2}}\left(\frac{m\omega}{\hbar\pi}\right)^{1/4}
\end{equation*}
$$
于是我们得到了第一激发态的简谐振子的波函数：
$$
\begin{align*}
	\psi_1(x) &= \frac{1}{\sqrt{2}}\left(\frac{m\omega}{\hbar\pi}\right)^{1/4}\sqrt{\frac{m\omega}{\hbar}}xe^{-\tfrac{m\omega}{2\hbar}x^2}
\end{align*}
$$
后面的每一项我们都可以像上面这样一个个算出来，但这里我就不展开说明其通用的推导方法了，这里直接给出简谐振子静止状态的通解：
$$
\psi_n(x) = \left(\frac{m\omega}{\pi\hbar}\right)^{1/4}\frac{1}{\sqrt{2^nn!}}H_n(\xi)e^{-\xi^2/2}
$$
其中的 $H_n(\xi)$ 称为 **厄米特多项式（Hermite Polynomial）**，它可以通过 **罗德里格斯公式（Roderigues Formula）** 计算得到：
$$
H_n(\xi) = (-1)^ne^{\xi^2}\left(\frac{d}{d\xi}\right)^ne^{-\xi^2}
$$

其前几项如下列出：
$$
\begin{align*}
	H_0(\xi) &= 1 \\
	H_1(\xi) &= 2\xi \\
	H_2(\xi) &= 4\xi^2 - 2 \\
	H_3(\xi) &= 8\xi^3 - 12\xi \\
	H_4(\xi) &= 16\xi^4 - 48\xi^2 + 12 \\
	H_5(\xi) &= 32\xi^5 - 160\xi^3 + 120\xi
\end{align*}
$$


### Delta 函数势能

截至此，我们已经遇到两大类薛定谔方程（根据不同势能分布），其一拥有离散的、可归一化的解，它们的线性组合是方程的通解（如无限方井、有限方井 $E < 0$ 的情形、简谐振子）；另一个拥有连续的、不可归一化的解（如自由粒子、有限方井 $E > 0$ 的情形），它们在全空间中的积分是方程的通解。这两者的区别有什么物理意义呢？

在经典力学中，考虑一维的情形，我们可以通过小车和斜坡的模型构建简单的势能模型。当总能量超过两边的势能时，小车会向无穷远处运动；当总能量小于两边的势能时，小车会在两个 **转向点（Turning Point）** 中移动；当总能量大于一边的势能时，小车可能会经过更高势能一边的转向点，并向另一边的无穷远处移动。

在量子力学中也有类似的结论。不过由于此时“小车”不再拥有确定的位置，它可以出现在任何能量大于势能的地方。因此，小车完全可能“穿过”斜坡到山坡另一侧的地方，我们称其为 **隧穿（Tunnelling）**。至于小车是否会跑到无穷远处，可以通过比较 $E$ 和 $V(\pm \infty)$ 确定。如果 $E$ 比两边都小，那么系统就处于 **束缚态（Bound State）**，否则处于 **散射态（Scattering State）**。我们不难验证前面介绍过的这些情况都对应了束缚态和散射态的一种。

定义 **狄拉克 Delta 函数（Dirac Delta Function）** 为在原点处无穷大，其余点均为零的函数，即：
$$
\delta(x) = 
\begin{cases}
	0 & x \ne 0 \\
	\infty & x = 0
\end{cases}
$$
此外，它还要求：
$$
\int_{-\infty}^\infty\delta(x)\,dx = 1
$$
这个函数在理论物理中非常受欢迎，比如质点在空间中的密度分布，或点电荷在空间中的电荷密度分布都只能用 $\delta$ 函数描述。严格意义上来讲它是一个分布，而非常规的函数。至于对其积分的要求，下面这个定义或许更好理解：
$$
\delta(x) = \lim_{a\to0}
\begin{cases}
	\dfrac{1}{a} & |x| < \dfrac{a}{2} \\
	0 & |x| \ge \dfrac{a}{2}
\end{cases}
$$
$\delta$ 函数有一个非常重要的性质：
$$
f(x)\delta(x - a) = f(a)\delta(x - a)
$$
这点并不难理解，因为除了 $x = a$ 外的点都被 $\delta$ 函数置零了；这个性质的直接结论是：
$$
\int_{-\infty}^\infty f(x)\delta(x - a)\,dx = f(a)
$$
因此，$\delta$ 函数常用于从函数中“取出”特定点上的值。

现在，考虑势能分布如下的系统：
$$
V(x) = -\alpha \delta(x)
$$
其中 $\alpha$ 是一个正实数。此时，时间无关的薛定谔方程变为：
$$
-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} - \alpha\delta(x)\psi = E\psi
$$
对于束缚态（$E < 0$）的情形，在 $x \ne 0$ 的区域，设：
$$
\frac{d^2\psi}{dx^2} = -\frac{2mE}{\hbar^2}\psi = \kappa^2\psi \qquad \text{其中 $\kappa = \frac{\sqrt{-2mE}}{\hbar}$}
$$
上面这个方程的通解是（对于 $x <0$ 或 $x > 0$）：
$$
\psi(x) = Ae^{-\kappa x} + Be^{\kappa x}
$$
之后的过程和我们在[有限方井](#有限方井)中的非常相似，由于波函数的连续性和这个系统的对称性，不难得到：
$$
\psi(x) = Be^{-\kappa|x|}
$$
呃，我们似乎还没有利用 $\delta$ 函数这个条件；显然我们可以消除上面这个方程中的 $B$ 或者 $\kappa$。因此我们需要应用另一个波函数的性质：在 $V \ne \infty$ 的地方 $d\psi/dx$ 连续。为了更好处理 $\delta$ 函数，让我们首先考虑一个区间 $[-\epsilon, \epsilon]$。在此区间中对时间无关的薛定谔方程积分：
$$
-\frac{\hbar^2}{2m}\int_{-\epsilon}^\epsilon \frac{d^2\psi}{dx^2}\,dx + \int_{-\epsilon}^\epsilon V(x)\psi(x)\,dx = E\int_{-\epsilon}^\epsilon \psi(x)\,dx 
$$
第一个积分得到的就是 $d\psi/dx$，最后一个积分则应该得到 $0$。至于第二个积分，我们可以通过 $\delta$ 函数的性质得到 $\alpha\psi(0)$，综合起来就是：
$$
\lim_{\epsilon\to 0}\left(\left.\frac{\partial \psi}{\partial x}\right|_{+\epsilon} - \left.\frac{\partial \psi}{\partial x}\right|_{-\epsilon}\right) = -\frac{2m\alpha}{\hbar^2}\psi(0)
$$
将我们之前得到的解代入，就得到：
$$
B = \psi(0) \qquad \kappa = \frac{m\alpha}{\hbar^2}
$$
其对应的能量是：
$$
E = -\frac{m\alpha^2}{2\hbar^2}
$$
将波函数归一化：
$$
\int_{-\infty}^\infty |\psi(x)|^2\,dx = 2|B|^2\int_0^\infty e^{-2\kappa x}\,dx = \frac{|B|^2}{\kappa} = 1 \implies B = \sqrt{\kappa}
$$
最终得到波函数：
$$
\psi(x) = \frac{\sqrt{m\alpha}}{\hbar}e^{-m\alpha|x|/\hbar^2}
$$
对于散射态（$E > 0$）的情形，设（$x \ne 0$）：
$$
\frac{d^2\psi}{dx^2} = -\frac{2mE}{\hbar^2}\psi = -k^2\psi \qquad \text{其中 $k = \frac{\sqrt{2mE}}{\hbar}$}
$$
这个方程的通解是：
$$
\psi(x) = 
\begin{cases}
	Ae^{ikx} + Be^{-ikx} & x < 0 \\
	Fe^{ikx} + Ge^{-ikx} & x > 0
\end{cases}
$$
这里我们把他们分开是有特殊用处的。为了在 $x = 0$ 处连续，我们需要满足 $F + G = A + B$。至于 $d\psi/dx$，我们需要满足和束缚态类似的要求：
$$
\left.\frac{d\psi}{dx}\right|_{x=0^-} - \left.\frac{d\psi}{dx}\right|_{x=0^+} = -\frac{2m\alpha}{\hbar^2}(A + B) \\
\implies F - G= A(1 + 2i\beta) - B(1 - 2i\beta) \qquad \text{其中 $\beta = \frac{m\alpha}{\hbar^2k}$}
$$
如果将波函数的每个指数项都理解为一个波，则 $ikx$ 可以理解为向右的波，$-ikx$ 则是向左的波；$A$、$B$、$F$、$G$ 则是这些波的幅度。下图是一个直观的展示：

<img src="graphs/qm1_2-3.png" alt="qm1_2-3" style="zoom:80%;" />

通常在散射实验中，粒子会从左侧射出，因此设 $G = 0$。这时我们就构建了一个类似于波经过不同介质边界的模型。将 $Ae^{ikx}$ 称为 **入射波（Incident Wave）**，$Be^{-ikx}$ 称为 **反射波（Reflected Wave）**，$Fe^{ikx}$ 称为 **透射波（Transmitted Wave）**，我们可以得到：
$$
B = \frac{i\beta}{1 - i\beta} A \qquad F = \frac{1}{1 - i\beta}A
$$
虽然我们得到的解并不能归一化，但是它依然能给出和概率相关的结论。考虑反射波和入射波的能量比：
$$
R = \frac{|B|^2}{|A|^2} = \frac{\beta^2}{1 + \beta^2}
$$
这反映了粒子被“反射”回来的概率。类似地，透射波和入射波的能量比：
$$
T = \frac{|F|^2}{|A|^2} = \frac{1}{1 + \beta^2}
$$
观察到 $R + T = 1$，这也是理所当然的。将 $\beta$ 用已知量代换，就得到：
$$
\begin{align*}
	R = \frac{1}{1 + 2\hbar^2E/(m\alpha^2)} \\
	T = \frac{1}{1 + m\alpha^2/(2\hbar^2E)}
\end{align*}
$$
因此能量 $E$ 越大，粒子越倾向于透射过去。



## 数学形式

前面两个章节中我们得到了一些在简单的量子系统中的有趣结论。其中有些可能比较巧合（比如简谐振子的能量均匀分布），但其它似乎可以一般化（比如测不准原理，还有静止状态的正交性）本章中我们会利用数学工具将这些结论拓展到更一般的形式。

### 希尔伯特空间

#### 矢量的记号和性质

量子理论中的两个核心概念是波函数和算符。系统的 **状态（State）**由波函数描述，而 **可观测量（Observable）**由算符描述。数学上，我们可以将波函数通过 **矢量（Vector）**表示，算符则可以抽象为 **线性变换（Linear Transformation）**。因此，**线性代数（Linear Algebra）**是量子力学的“自然语言”。

不过量子力学有它自己的个性，比起我们已经熟悉的矢量形式，它选择使用下面这种记号：
$$
\ket{\alpha} = \mathbf{a} = 
\begin{pmatrix}
	a_1 \\ a_2 \\ \vdots \\ a_N
\end{pmatrix}
$$
其表示一个 $N$ 维空间的矢量，其中 $a_i \in \mathbb{C}$。矢量之间的 **内积（Inner Product）**定义为：
$$
\langle \alpha | \beta \rangle = a_1^*b_1 + a_2^*b_2 + \dots + a_N^*b_N
$$
内积有一些重要的性质如下：

- $\langle \alpha | \beta \rangle^* = \langle \beta | \alpha \rangle$。
- $\langle \alpha | \alpha \rangle \ge 0$，且 $\langle \alpha | \alpha \rangle = 0 \Leftrightarrow |\alpha \rangle = 0$。
- $\langle \alpha | (b | \beta \rangle + c | \gamma \rangle \rangle = b \langle \alpha | \beta \rangle + c \langle \alpha | \gamma \rangle$。

矢量的 **范数（Norm）** 定义为它和自己的内积的平方根：
$$
\begin{equation*}
	\norm{|\alpha\rangle} = \sqrt{\langle \alpha | \alpha \rangle}
\end{equation*}
$$
不难发现它等同于向量的“长度” $|\mathbf{a}| = \sqrt{\sum a_i^2}$。范数为 $1$ 的矢量被称为 **归一化的（Normalized）** 矢量。

若两个矢量的内积是零，就称他们相互 **正交（Orthogonal）**。一集两两相互正交的归一化矢量称为 **标准正交集（Orthonormal Set）**；如果它们构成矢量空间的基，就称其为 **标准正交基（Orthonormal Basis）**。作为例子，二维空间中如果设 $|x\rangle = (1, 0)$ 和 $|y\rangle = (0, 1)$，不难发现它们构成 $\mathbb{R}^2$ 的一组标准正交基。

我们知道，矢量空间中任意变量 $\mathbf{a}$ 可以表示为一个标准正交基的线性组合：
$$
\begin{equation*}
	\mathbf{a} = \sum_{i = 1}^n a_i\mathbf{b}_i \qquad \text{其中 $\mathbf{b}_i$ 组成矢量空间的标准正交基 $\mathbf{b}$}
\end{equation*}
$$
此时 $\mathbf{a}$ 在每一个基矢量 $b_i$ 上的投影便是 $a_i$。我们不难发现：
$$
\begin{equation*}
	\mathbf{a}\cdot\mathbf{b}_i = \sum_{j = 1}^n a_i(\mathbf{b_i}\cdot\mathbf{b}_j) = a_i
\end{equation*}
$$
因此原向量可以写成下面的形式：
$$
\begin{equation*}
	\mathbf{a} = \sum_{i = 1}^n (\mathbf{b}_i\cdot\mathbf{a})\mathbf{b}_i
\end{equation*}
$$
对于新记号 $|\alpha \rangle$，我们也有类似的式子（假设标准正交基是 $|e_i\rangle$）：
$$
\begin{equation*}
	|\alpha \rangle = \sum_{i = 1}^n \langle e_i | \alpha \rangle|e_i\rangle
\end{equation*}
$$
其范数的平方即可以表示为：
$$
\begin{equation*}
	\langle \alpha | \alpha \rangle = \sum_{i = 1}^n |\langle e_i | \alpha \rangle|^2
\end{equation*}
$$
一个重要的不等式是 **施瓦尔兹不等式（Schwarz Inequality）**：
$$
\begin{equation*}
	|\langle \alpha | \beta \rangle|^2 \le \langle \alpha | \alpha \rangle \langle \beta | \beta \rangle
\end{equation*}
$$
为了证明这个公式，记：
$$
\begin{equation*}
	|\gamma\rangle = |\beta\rangle - \frac{\langle \alpha | \beta \rangle}{\langle \alpha | \alpha \rangle}|\alpha \rangle
\end{equation*}
$$
现在考虑 $\langle \gamma | \gamma \rangle$：
$$
\begin{align*}
	\langle \gamma | \gamma \rangle
		&= \sum_{i = 1}^n |\langle e_i | \gamma \rangle|^2 \\
		&= \sum_{i = 1}^n |\langle e_i | \beta \rangle - \frac{\langle \alpha | \beta \rangle}{\langle \alpha | \alpha \rangle}\langle e_i | \alpha \rangle|^2 \\
		&= \sum_{i = 1}^n|\langle e_i | \beta \rangle|^2 + \sum_{i = 1}^n\frac{|\langle \alpha | \beta \rangle|^2}{|\langle \alpha | \alpha \rangle|^2}|\langle e_i | \alpha \rangle |^2 - \sum_{i = 1}^n\langle e_i | \beta \rangle\frac{\langle \alpha | \beta \rangle}{\langle \alpha | \alpha \rangle}\langle e_i | \alpha \rangle - \sum_{i = 1}^n \frac{\langle \alpha | \beta \rangle}{\langle \alpha | \alpha \rangle}\langle e_i | \alpha \rangle\langle e_i | \beta \rangle \\
		&= \langle \beta | \beta \rangle + \frac{\langle \alpha | \beta \rangle^2}{\langle \alpha | \alpha \rangle^2}\langle \alpha | \alpha \rangle - \frac{\langle \alpha | \beta \rangle}{\langle \alpha | \alpha \rangle}\langle \beta | \beta \rangle \\
		&= \lackproof
\end{align*}
$$


敏锐的同学可能会发现其和矢量分析中下面公式的相似性：
$$
\begin{equation*}
	\cos\theta = \frac{\mathbf{a}\cdot\mathbf{b}}{|\mathbf{a}||\mathbf{b}|} \implies |\mathbf{a}\cdot\mathbf{b}| \le |\mathbf{a}||\mathbf{b}|
\end{equation*}
$$
实际上，我们可以因此定义 $|\alpha\rangle$ 和 $|\beta \rangle$ 的夹角：
$$
\begin{equation*}
	\cos\theta = \frac{\sqrt{|\langle \alpha | \beta \rangle|^2}}{\norm{| \alpha \rangle}\norm{|\beta \rangle}} \sqrt{\frac{|\langle \alpha | \beta \rangle|^2}{\langle \alpha | \alpha \rangle\langle \beta | \beta \rangle}}
\end{equation*}
$$
其与实数域矢量的区别来源于 $\langle \alpha | \beta \rangle$ 可能含有复数。当 $\langle \alpha | \beta \rangle \in \mathbb{R}$ 时，上式会化为此前我们熟悉的形式。

#### 矩阵的记号和性质

我们知道，矩阵等价于矢量空间中的线性变换，对于任一个 $n\times n$ 的矩阵 $\hat{T}$（这里我使用了和算符一样的上标，因为我们很快就会看到，算符正是希尔伯特空间中的矢量）：
$$
\begin{equation*}
	\hat{T}(a|\alpha \rangle + b|\beta \rangle) = a\left(\hat{T}|\alpha \rangle\right) + b\left(\hat{T}|\beta\rangle\right)
\end{equation*}
$$
这也正是线性变换的定义。根据矩阵乘法的性质，我们知道：
$$
\begin{equation*}
	\hat{T}|e_j\rangle = \sum_{i = 1}^n T_{ij}|e_i\rangle
\end{equation*}
$$
因此对于任意的矢量，有：
$$
\begin{align*}
	\hat{T}|\alpha \rangle
		&= \sum_{j = 1}^n a_j\left(\hat{T}|e_j\rangle\right) \\
		&= \sum_{j = 1}^n\sum_{i = 1}^n a_jT_{ij}|e_i\rangle \\
		&= \sum_{i = 1}^n\left(\sum_{j = 1}^nT_{ij}a_j\right)|e_i\rangle
\end{align*}
$$


矢量空间中的线性变换则（如前两章介绍过的）采用下面的记号：
$$
\ket{\beta} = \hat{T}\ket{\alpha} \Leftrightarrow \mathbf{b} = T\mathbf{a} =
\begin{pmatrix}
	t_{11} & t_{12} & \dots & t_{1N} \\
	t_{21} & t_{22} & \dots & t_{2N} \\
	\vdots & \vdots & \ddots & \vdots \\
	t_{N1} & t_{N2} & \dots & t_{NN}
\end{pmatrix}
\begin{pmatrix}
	a_1 \\ a_2 \\ \vdots \\ a_N
\end{pmatrix}
$$
不过，在无限维度矢量空间中，这种表示方式似乎有局限性，尤其是矢量的内积（无限和对应了积分）有可能不收敛；因此我们需要特别注意一些情况。

#### 波函数所在的矢量空间

理论上，所有关于 $x$ 的函数构成了一个矢量空间，但我们在量子力学中需要的仅仅是可以被归一化的函数空间，即：
$$
\left\{\Psi \mid \int|\Psi|^2\,dx = 1\right\}
$$
此外，所有在特定区间内 **平方可积（Square-Integrable）**的函数构成了一个更小的矢量空间：
$$
\left\{f(x) \mid\int_a^b |f(x)|^2\,dx < \infty \right\}
$$
我们将其称为 **希尔伯特空间（Hilbert Space）**，数学上记为 $L^2(a, b)$。所有的波函数都属于希尔伯特空间。两个函数的内积定义为：
$$
\begin{equation*}
	\langle f|g\rangle = \int_a^b f^*(x)g(x)\,dx
\end{equation*}
$$
通过 **施瓦尔兹不等式（Schwarz Inequality）**我们可以得到：
$$
\left|\int_a^b f^*(x)g(x)\,dx\right| \le \sqrt{\int_a^b |f(x)|^2\,dx \int_a^b|g(x)|^2\,dx}
$$
由于右式有限，我们可以保证希尔伯特空间中两个函数的内积一定存在。此外，不难观察到：
$$
\langle g|f \rangle = \langle f|g \rangle^*
$$
因此内积运算不可交换。函数和自己本身的内积是：
$$
\langle f|f \rangle = \int_a^b |f(x)|^2\,dx
$$
从物理角度来看，这个结果当且仅当 $f(x) = 0$ 时才为零（数学上我们可以构建仅在一系列特定点上不为零的病态函数，其积分也为零）。特别地，规定 $f = g$ 当且仅当 $\langle f - g|f - g\rangle = 0$，我们可以通过简单的代数证明这一点：
$$
\begin{align*}
	\langle f - g|f - g \rangle
		&= \int_a^b (f - g)^*(f - g)\,dx \\
		&= \int_a^b (f^* - g^*)(f - g)\,dx \\
		&= \int_a^b f^*f\,dx 
\end{align*}
$$


如果一个函数和自身的内积为 $1$，我们就称其是 **归一化的（Normalized）**（这完全符合我们最开始的定义）；如果两个函数的内积为 $0$，我们就称其 **正交（Orthogonal）**；一集归一化函数 $\{f_n\}$ 如果两两正交，则称它们是 **标准正交的（Orthonormal）**，也即下面等式成立：
$$
\begin{equation*}
	\langle f_m|f_n \rangle = \delta_{mn}
\end{equation*}
$$
对于一集标准正交函数，如果其线性组合能够得到指定的希尔伯特空间中的所有函数，就称其在该空间中 **完备（Complete）**，数学表示如下：
$$
\begin{equation*}
	f(x) = \sum_{n=1}^\infty c_nf_n(x)
\end{equation*}
$$
其中系数 $c_n$ 可以通过下面得到：
$$
\begin{equation*}
	c_n = \langle f_n|f \rangle
\end{equation*}
$$
至此，我们将[无限方井](#无限方井)小节中引入的概念拓展到了希尔伯特空间。可以看到，在无限方井和简谐振子模型中得到的波函数解分别是 $(0, a)$ 区间和 $(-\infty, \infty)$ 区间的标准正交函数集。

### 可观测量

回忆我们在第一章引入算符时给出的公式，对于任何可观测物理量 $Q(x, p)$，其期望值都可以通过下面公式来计算：
$$
\begin{equation*}
	\langle Q \rangle = \int\Psi^*\hat{Q}\Psi\,dx
\end{equation*}
$$
现在可以写成量子力学的数学形式：
$$
\begin{equation*}
	\langle Q \rangle = \langle \Psi|\hat{Q}\Psi\rangle
\end{equation*}
$$
考虑到期望值一定是实数，我们不难得到：
$$
\langle \Psi|\hat{Q}\Psi\rangle = \langle Q\rangle = \langle Q\rangle^* = \langle \hat{Q}\Psi|\Psi\rangle
$$
我们将这样的算符称为 **厄米特算符（Hermitian Operator）**，后文中我们会简称为算符。量子力学中所有的可观测量都对应了一个算符。在有限维度矢量空间中，我们用 **厄米特矩阵（Hermitian Matrix）**来表示一个算符，其满足下面的特性：
$$
T = T^\dagger \equiv \tilde{T}^*
$$
更一般地，我们可以将算符 $\hat{Q}$ 的 **厄米特共轭（Hermitian Conjugate）**定义为满足下面要求的 $\unit{Q}^\dagger$：
$$
\begin{equation*}
	\langle f|\unit{Q}g\rangle = \langle \hat{Q}^\dagger f|g\rangle
\end{equation*}
$$
其中 $f$、$g$ 是希尔伯特空间中任意的函数。此时依然有：
$$
\hat{Q} = \hat{Q}^\dagger
$$

### 特征函数

在量子系统中，对于完全相同的设定和状态 $\Psi$，每次观测某个可观测量 $Q$ 很可能得到不同的结果，这是由波函数的性质（概率分布）特性导致的。但是在特殊的设定下，我们可以保证每次观测都得到相同的结果，我们称此为 $Q$ 的 **确定状态（Determinate State）**，此时 $Q$ 的标准差应该是零，即：
$$
\sigma_Q^2 = \left\langle(Q - \langle Q\rangle)^2\right\rangle = \left\langle\Psi\mid (\hat{Q} - \langle Q\rangle)^2\Psi\right\rangle = \left\langle(\hat{Q} - \langle Q\rangle)\Psi\mid(\hat{Q} - \langle Q\rangle)\Psi\right\rangle = 0
$$
此时一定有：
$$
\begin{equation*}
	\hat{Q}\Psi = \langle Q\rangle \Psi
\end{equation*}
$$
这个等式被称为算符 $\hat{Q}$ 的 **特征值方程（Eigenvalue Equation）**，其中 $\Psi$ 是 $\hat{Q}$ 的 **特征函数（Eigenfunction）**，而 $\langle Q\rangle$ 是对应的 **特征值（Eigenvalue）**。量子力学中每个可观测量的确定状态都对应了其算符的特征函数。

从上面的方程来看，特征函数的任意常数倍数依然是特征函数；$0$ 是平凡的特征值，也是平凡的特征函数，但我们决定将排除后者（否则在这种情况下任意实数都是特征值）。

给定一个算符，其所有特征值的集合被称为 **光谱（Spectrum）**。如果多个线性无关的特征函数有相同的特征值，我们称这个光谱是 **退化的（Degenerate）**。通常我们可以将算符的光谱分为两种：离散或连续。前一种情况的特征函数一定在希尔伯特空间中且拥有实际的物理意义；后一种情况下，特征函数并不是可归一的，此时需要至少选取一段区间后才能得到可归一的函数。一些算符只有离散的光谱（比如简谐振子的哈密顿算符），一些只有连续的光谱（比如自由粒子的哈密顿算符），也有两者都有的（比如有限方井的哈密顿算符）。

#### 离散光谱

哈密顿算符的可归一的特征函数有两个重要的性质：

> **定理**：它们对应的特征值是实数。

> **证明**：假设 $\hat{Q}f = qf$，则：
> $$
> \langle f|\hat{Q}f\rangle = \langle \hat{Q}f|f\rangle
> $$
> 将系数 $q$ 提出来，得到：
> $$
> q\langle f|f\rangle = q^*\langle f|f\rangle
> $$
> 由于 $f$ 是可归一的，因此他和自己的内积不可能为 $0$，这就证明了 $q$ 是一个实数。

这个性质意味着每次测量确定状态的例子时，总能得到一个实数。

> **定理**：不同特征值对应的特征函数是正交的。

> **证明**：假设 $\hat{Q}f = qf$，且存在 $q' \ne q$ 使得 $\hat{Q}g = q'g$。此时：
> $$
> \langle f|\hat{Q}g\rangle = \langle\hat{Q}f|g\rangle
> $$
> 将 $q$ 和 $q'$ 提出来，得到：
> $$
> q'\langle f|g\rangle = q^*\langle f|g\rangle
> $$
> 根据给定条件必有 $\langle f|g\rangle = 0$，因此两者正交。

这个性质总结了此前我们在无限方井和简谐振子中得到的函数正交性。

在退化的光谱中，即定理二中 $q = q'$ 时，我们可以用 **格拉姆-施密特正交化过程（Gram-Schmidt Orthogonalization Procedure）**来得到新的正交特征函数。因此即使在特征函数中存在退化现象，我们可以通过这个算法得到等价的标准正交函数集。所以我们总是可以假设这个过程已经完成了。这一点为我们此前“心安理得”使用函数正交性提供了更多保障。

最后，让我们给出一个公理：

> **公理**：可观测算符的特征函数集是完备的。即，希尔伯特空间中的任意函数都能表示成它们的线性组合。

在一些情况下（比如静止状态），这个公理可以被证明出来。不过因为没有通用的证明方法（尤其是对于连续光谱），我们还是将它视作公理。

#### 连续光谱

如果哈密顿算符的光谱是连续的，其特征函数不能够归一化。此时离散情况下两个定理的证明就不成立了。不过我们依然希望它在某种角度来看是成立的，下面通过一个例子来说明。

假设我们希望找到 $(-\infty, \infty)$ 区间动量算符的特征函数和特征值，设特征函数为 $f_p(x)$，$p$ 是特征值，则：
$$
-i\hbar\frac{d}{dx}f_p(x) = pf_p(x)
$$
其通解是：
$$
f_p(x) = Ae^{ipx/\hbar}
$$
显然对于任意 $p \in \mathbb{C}$ 它都不是可归一化的。不过我们可以探索稍差一些的结论：如果令 $p \in \mathbb{R}$，此时有：
$$
\langle f_{p'}|f_p\rangle = \int_{-\infty}^\infty f_{p'}^*(x)f_p(x)\,dx = |A|^2\int_{-\infty}^\infty e^{i(p - p')x/\hbar}\,dx = |A|2\pi\hbar\delta(p - p')
$$
取 $A = 1/\sqrt{2\pi \hbar}$，则：
$$
\begin{equation*}
	f_p(x) = \frac{1}{\sqrt{2\pi\hbar}}e^{ipx/\hbar}
\end{equation*}
$$
注意到这是一个周期函数，波长是：
$$
\lambda = \frac{2\pi\hbar}{p}
$$
这正是德布罗意公式！我们马上会对此做出解释，在此之前先注意下面的等式：
$$
\langle f_{p'}|f_p\rangle = \delta(p - p')
$$
这和我们前文中给出的标准正交性非常相似，我们可以称其为 **狄拉克标准正交性（Dirac Orthonormality）**。此时我们可以得到一种略有不同的完备性：任何平方可积的函数都能表示为上述特征函数的“线性积分”（把线性组合中的求和换为积分）：
$$
f(x) = \int_{-\infty}^\infty c(p)f_p(x)\,dp = \frac{1}{\sqrt{2\pi \hbar}}\int_{-\infty}^\infty c(p)e^{ipx/\hbar}\,dp
$$
我们使用与此前类似的方式计算和 $p$ 相关的常数 $c(p)$：
$$
\langle f_{p'}|f\rangle = \int_{-\infty}^\infty c(p)\langle f_{p'}|f_p\rangle\,dp = \int_{-\infty}^\infty c(p)\delta(p - p')\,dp = c(p')
$$
当然，敏锐的同学可能已经发现前面的式子就是一个逆傅里叶变换，因此只需要用一次傅里叶变换就可以变成 $c(p)$ 相关的函数：
$$
c(p) = \frac{1}{\sqrt{2\pi\hbar}}\int_{-\infty}^\infty e^{-ipx/\hbar}\,dx
$$
现在让我们回到德布罗意公式，现在可以知道，其描述的粒子（拥有确定动量的粒子）实际上不存在，但我们总可以选取一段范围内的动量形成的可归一化的波包来描述，这也是它们存在波长的原因。

作为总结，动量算符的特征函数不在希尔伯特空间中，但它们的集合（一个区间）拥有某种角度而言的可归一化性质。事实上，所有连续的哈密顿算子（即拥有连续光谱），其对应的特征函数集都是狄拉克标准正交，且在积分意义上完备的。

### 通用的统计解释

经过了前面诸多铺垫之后，我们终于可以给出一个对量子力学的完整统计解释。

> **量子力学的通用统计解释**：在测量一个状态为 $\Psi(x, t)$ 的粒子的某个可观测量 $Q(x, p)$ 时，一定会得到哈密顿算符
> $$
> \hat{Q} = \left(x, -i\hbar\frac{d}{dx}\right)
> $$
> 的*一个*特征值。如果 $\hat{Q}$ 的光谱，即特征值分布是离散的，则得到和标准正交特征函数 $f_n(x)$ 对应的某个特征值 $q_n$ 的概率是：
> $$
> |c_n|^2 \qquad \text{其中}\ c_n = \langle f_n|\Psi \rangle
> $$
> 相反，如果 $\hat{Q}$ 的光谱是连续的，则在区间 $dz$ 中得到和狄拉克标准正交的特征函数 $f_z(x)$ 对应的某个特征值 $q(z)$ 的概率是：
> $$
> |c(z)|^2\,dz \qquad \text{其中}\ c(z) = \langle f_z|\Psi\rangle
> $$
> 在观测发生时，波函数会坍缩为对应的特征状态。对于连续光谱，则根据测量仪器的精度坍缩为一段极小区间的叠加状态。

在离散光谱中，我们不难验证概率的归一性：
$$
\begin{align*}
	\sum_n |c_n|^2 
	&= \sum_n c_n^*c_n \\
	&= \sum_{m}\sum_nc_{m}^*c_n\delta_{mn} \\
	&= \sum_{m}\sum_nc_{m}^*c_n\langle f_{m}|f_{n}\rangle \\
	&= \left\langle\left.\left(\sum_mc_mf_m\right)\right|\left(\sum_nc_nf_n\right)\right\rangle 
	= \langle\Psi|\Psi\rangle = 1
\end{align*}
$$
此外，可观测量的期望值是：
$$
\begin{align*}
	\langle Q \rangle = \langle \Psi|\hat{Q}\Psi\rangle
	&= \left\langle\left.\left(\sum_mc_mf_m\right)\right|\hat{Q}\left(\sum_nc_nf_n\right)\right\rangle \\
	&= \sum_m\sum_nc_m^*c_n\langle f_m|\hat{Q}f_n\rangle \\ 
	&= \sum_m\sum_nc_m^*c_nq_n\langle f_m|f_n\rangle \\
	&= \sum_m\sum_nc_m^*c_nq_n\delta_{mn} \\
	&= \sum_nq_n|c_n|^2
\end{align*}
$$
至于连续光谱，我们可以特别地对 $x$ 和 $p$ 进行验证。在上一小节中我们已经认识到：
$$
\begin{align*}
	c(x) &= \langle g_x|\Psi\rangle = \int_{-\infty}^\infty \delta(y - x)\Psi(y, t)\,dy = \Psi(x, t) \\
	c(p) &= \langle f_p|\Psi\rangle = \frac{1}{\sqrt{2\pi\hbar}}\int_{-\infty}^\infty e^{-ipx/\hbar}\Psi(x, t)\,dx
\end{align*}
$$
我们将第二个式子称为 **动量空间波函数（Momentum Space Wave Function）**，其正好对应了“位置空间”波函数，也就是 $\Psi(x, t)$；两者可以通过傅里叶变换和逆傅里叶变换相互转化：
$$
\begin{align*}
	\Phi(p, t) = \frac{1}{\sqrt{2\pi\hbar}}\int_{-\infty}^\infty e^{-ipx/\hbar}\Psi(x, t)\,dx \\
	\Psi(x, t) = \frac{1}{\sqrt{2\pi\hbar}}\int_{-\infty}^\infty e^{-ipx/\hbar}\Phi(x, t)\,dp
\end{align*}
$$

### 矢量和算符

本节将沿着上一节得到的结论，探索不同空间下波函数的数学本质，并最终得到在不同空间之间相互转化的方式。

#### 希尔伯特空间的基矢量

在矢量分析中，我们学过将空间中的矢量拆分为某个基上基矢量的加权和。比如在三维空间中，任意矢量 $\mathbf{A}$ 都可以写作：
$$
\mathbf{A} = A_x\unit{x} + A_y\unit{y} + A_z\unit{z} = (\mathbf{A}\cdot\unit{x})\unit{x} + (\mathbf{A}\cdot\unit{y})\unit{y} + (\mathbf{A}\cdot\unit{z})\unit{z}
$$
（这里的 $\unit{x}$ 符号代表的是单位矢量）在量子力学中，我们有类似于 $\mathbf{A}\cdot\unit{x}$ 的写法：
$$
\Psi(x, t) = \langle x|\mathcal{S}(t)\rangle
$$
其中 $|\mathcal{S}(t)\rangle$ 表示希尔伯特空间中的一个矢量，而 $\Psi(x, t)$ 表示了其 $x$ 方向的分量。$|x\rangle$ 表示特征值为 $x$ 的位置特征函数。类似地，同一个矢量的特征值为 $p$ 的动量特征函数是：
$$
\Phi(p, t) = \langle p|\mathcal{S}(t)\rangle
$$
如果以能量特征函数为基，根据不同的 $n$ 可以得到：
$$
c_n(t) = \langle n|\mathcal{S}(t)\rangle
$$
需要意识到的是，上面这三种不同的分量表示方法描述的其实都是同一个矢量：
$$
|\mathcal{S}(t)\rangle = \int \Psi(y, t)\delta(x - y)\,dy = \int \Phi(p, t)\frac{1}{\sqrt{2\pi\hbar}}e^{ipx/\hbar}\,dp = \sum_n^\infty c_ne^{-iE_nt/\hbar}\psi_n(x)
$$
算符是希尔伯特空间中的线性变换，它将一个矢量变换为另一个矢量：
$$
|\beta\rangle = \hat{Q}|\alpha\rangle
$$
如果设 $|\alpha\rangle = \sum a_n|e_n\rangle$，$|\beta\rangle = \sum b_n|e_n\rangle$，其分量为：
$$
a_n = \langle e_n|\alpha \rangle \qquad b_n = \langle e_n|\beta\rangle
$$
此时我们可以将前面的式子写成：
$$
\sum_n b_n|e_n\rangle = \sum_n a_n\hat{Q}|e_n\rangle
$$
我们可以将算符 $\hat{Q}$ 表示为：
$$
Q_{mn} = \left\langle e_m\bigg|\ \hat{Q}\ \bigg|\ e_n\right\rangle
$$
这样就有：
$$
\sum_n b_n\langle e_m|e_n\rangle = \sum_na_n\left\langle e_m\bigg|\ \hat{Q}\ \bigg|\ e_n\right\rangle
$$
注意到 $\langle e_m|e_n \rangle = \delta_{mn}$，因此就有：
$$
b_m = \sum_nQ_{mn}a_n
$$
就好像矢量和矩阵在不同基中的形式不同一样，算符在不同分量表示中的形式也是不同的。回忆我们此前对 $\Phi(x, t)$ 和 $\Phi(p, t)$ 的称呼（位置空间波函数和动量动量空间波函数，我们可以将某个基扩张成的空间这样称呼），在这两种情况下的位置算符和动量算符是不一样的：
$$
\begin{align*}
	\hat{x} = \begin{cases} x & \text{（位置空间）} \\ i\hbar\partial_p & \text{（动量空间）} \end{cases} \qquad
	\hat{p} = \begin{cases} -i\hbar\partial_x & \text{（位置空间）} \\ p & \text{（动量空间）} \end{cases}
\end{align*}
$$
因此当提到某个算符时，最严谨的说法是“在某个空间”中的算符，不过由于我们主要只关注位置空间的波函数，也即 $\Psi(x, t)$，我们默认情况下讨论的均为位置空间。

最后值得提一下算符的运算。由于它们对应着矢量空间的矩阵，其运算规律和矩阵完全一致：
$$
\left(\hat{Q} + \hat{R}\right)|\alpha\rangle = \hat{Q}|\alpha\rangle + \hat{R}|\alpha\rangle \\
\left(\hat{Q}\hat{R}\right)|\alpha\rangle = \hat{Q}\left(\hat{R}|\alpha\rangle\right)
$$
有时我们会将函数作用于算符上，物理中这个等价于用其幂级数展开：
$$
e^{\hat{Q}} = \hat{I} + \hat{Q} + \frac{1}{2}\hat{Q}^2 + \frac{1}{3!}\hat{Q}^3 + \dots \\
\frac{1}{1 - \hat{Q}} = \hat{I} + \hat{Q} + \hat{Q}^2 + \hat{Q}^3 + \dots \\
\ln\left(1 + \hat{Q}\right) = \hat{Q} - \frac{1}{2}\hat{Q}^2 + \frac{1}{3}\hat{Q}^3 - \frac{1}{4}\hat{Q}^4 + \dots
$$



#### 一个二维系统

在一些简单的系统中，$n$ 能取的值是有限的，此时 $|S(t)\rangle$ 就是一个 $N$ 维矢量，$\hat{Q}$ 对应的就是一个 $N\times N$ 的矢量。最简单的情形是下面这个只有两个线性无关状态的系统：
$$
|0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix} \qquad |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}
$$
此时任意的状态 $|S\rangle$ 都可以表示为：
$$
|S\rangle = a|0\rangle + b|1\rangle = \begin{pmatrix} a \\ b \end{pmatrix}
$$
其中需要 $|a|^2 + |b|^2 = 1$，$a, b \in \mathbb{C}$。假设该系统中的哈密顿算符可以写成：
$$
\hat{H} = \begin{pmatrix} h & g \\ g & h \end{pmatrix}
$$
其中 $g, h \in \mathbb{R}$，此时它显然是一个厄米特矩阵。回忆时间相关的薛定谔方程：
$$
i\hbar\frac{d}{dt}|\mathcal{S}(t)\rangle = \hat{H}|\mathcal{S}(t)\rangle
$$
和此前使用的方法一样，我们先求解时间无关的薛定谔方程：
$$
\hat{H}|s\rangle = E|s\rangle
$$
这一步实际上在求解 $\hat{H}$ 的特征矢量和特征值：
$$
\det(h - EI) = 
\det\begin{pmatrix}
	h - E & g \\ g & h - E
\end{pmatrix}
= (h - E)^2 - g^2 = 0
$$
我们可以得到两个特征值：$E_\pm = h \pm g$。同时也就得到两个特征矢量（归一化后）：
$$
|s_\pm\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 \\ \pm 1 \end{pmatrix}
$$
现在，我们可以将初始状态表示为这两个特征矢量的线性组合：
$$
|\mathcal{S}(0)\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix} = \frac{1}{\sqrt{2}}(|s_+\rangle + |s_-\rangle)
$$
对于任意时刻 $t$，我们只需乘上一个 $\exp(-iE_n t/\hbar)$：
$$
\begin{align*}
	|\mathcal{S}(t)\rangle
	&= \frac{1}{\sqrt{2}}\left[e^{-i(h+g)t/\hbar}|s_+\rangle + e^{-i(h-g)t/\hbar}|s_-\rangle\right] \\
	&= \frac{1}{2}e^{-iht/\hbar}\left[e^{-igt/\hbar}\begin{pmatrix} 1 \\ 1 \end{pmatrix} + e^{igh/\hbar}\begin{pmatrix} 1 \\ -1 \end{pmatrix}\right] \\
	&= \frac{1}{2}e^{-iht/\hbar}\begin{pmatrix} e^{-igh/\hbar} + e^{igh\hbar} \\ e^{-igh/\hbar} - e^{igt/\hbar} \end{pmatrix} \\
	&= e^{-iht/\hbar}\begin{pmatrix} \cos(gt/\hbar) \\ -i\sin(gt/\hbar) \end{pmatrix}
\end{align*}
$$

#### 狄拉克符号

**狄拉克符号（Dirac Notation）** 是将内积符号 $\langle \alpha | \beta \rangle$ 拆开后的记法，即 **左矢（Bra）** $\langle \alpha|$ 和 **右矢（Ket）** $|\beta\rangle$。我们已经知道右矢是一个矢量：
$$
|\beta\rangle = \begin{pmatrix} b_1 \\ b_2 \\ \vdots \\ b_n \end{pmatrix}
$$
我们可以适当地定义左矢，使得它们的内积满足 $\langle \alpha |\beta\rangle = \langle \alpha| \cdot |\beta \rangle$：
$$
\langle\alpha| = \begin{pmatrix}
	a_1^* & a_2^* & \dots & a_n^*
\end{pmatrix}
$$
通过观察不难发现左矢的集合也能形成一个矢量空间，我们称其为 **对偶空间（Dual Space）**。当左矢和右矢的维度相同时，我们可以将它们交换顺序利用矩阵乘法得到一个方阵，特别地，我们将一个归一化的右矢乘其对应的左矢称为 **投影算符（Projection Operator）**：
$$
\hat{P} = |\alpha\rangle\langle \alpha|
$$
观察下面的式子：
$$
\hat{P}|\beta\rangle = (|\alpha\rangle\langle \alpha|)|\beta\rangle = \langle \alpha|\beta\rangle|\alpha\rangle
$$
类比矢量分析中的 $(\mathbf{a}\cdot\mathbf{b})\mathbf{a}$，不难看出这是矢量 $|\beta\rangle$ 在 $|\alpha\rangle$ 方向上的投影，这也是 $\hat{P}$ 名称的由来。特别地，对于标准正交基 $\{|e_n\rangle\}$，有：
$$
\sum_n|e_n\rangle\langle e_n| = \hat{I}
$$
此处 $\hat{I}$ 是单位算符。为了认识到这一点，可以考虑一个任意的矢量 $|\alpha\rangle = \sum a_n|e_n\rangle$：
$$
\begin{align*}
	\left(\sum_n|e_n\rangle\langle e_n|\right)|\alpha\rangle
	&= \sum_m\sum_n a_m\langle e_n|e_m\rangle|e_n\rangle \\
	&= \sum_m\sum_n \delta_{mn}a_m|e_n\rangle \\
	&= \sum_na_n|e_n\rangle \\
	&= |\alpha\rangle
\end{align*}
$$
对狄拉克标准正交基也有类似的结论，对于 $\{|e_z\rangle\}$ 有：
$$
\int|e_z\rangle\langle e_z|\,dz = \hat{I}
$$
最后我们需要重新审视这个记号系统。$|\alpha\rangle$ 中的 $\alpha$ 只是一个名字，只有在利用狄拉克符号的时候才变成一个矢量。因此如果回顾此前给出的等式：
$$
\left\langle f\left|\hat{Q}f\right.\right\rangle = \left\langle\left. \hat{Q}f\right|f\right\rangle
$$
其中的 $\hat{Q}f$ 似乎没有任何意义，因为 $f$ 本身没有任何意义。我们可以将等式左侧写成熟悉的形式：
$$
\left\langle f\left|\hat{Q}f\right.\right\rangle \equiv \Big\langle f\ \Big| \hat{Q} \Big| f \Big\rangle
$$
右式则没有更好的简化形式；其含义是 $\hat{Q}|f\rangle$ 对应的左矢，我们之后就保持这样的写法。

#### 基变换

狄拉克符号的好处在于，其中不包含任何特定的基，且使我们能轻松地在不同基间切换。回忆我们对投影算符的定义，下面我们会着重考察位置特征状态、动量特征状态和能量特征状态间的变换。首先让我们回顾状态矢量 $|\mathcal{S}(t)\rangle$ 在这些空间中的表示：
$$
\begin{align*}
	|\mathcal{S}(t)\rangle 
	&= \int \Psi(x, t)|x\rangle\,dx = \int \langle x|\mathcal{S}(t)\rangle|x\rangle\,dx \\
	&= \int \Phi(p, t)|p\rangle\,dp = \int \langle p|\mathcal{S}(t)\rangle|p\rangle\,dp \\
	&= \sum_nc_n(t)|n\rangle = \sum_n\langle n|\mathcal{S}(t)\rangle|n\rangle
\end{align*}
$$
在不同空间中，我们只需要取相应的算符即可。比如在位置空间和动量空间中的位置算符：
$$
\big\langle x \big|\hat{x}\big|\mathcal{S}(t)\big\rangle = x\Psi(x, t) \\
\big\langle p \big|\hat{x}\big|\mathcal{S}(t)\big\rangle = i\hbar\frac{\partial \Psi(p, t)}{\partial t}
$$
动量算符则是：
$$
\begin{align*}
	\big\langle p \big|\hat{x}\big|\mathcal{S}(t)\big\rangle 
	&= \left\langle p \left|\hat{x}\int\,dx\right|x\right\rangle\langle x||\mathcal{S}(t)\rangle \\
	&= \int\langle p|x|x \rangle\langle x|\mathcal{S}(t)\rangle\,dx \\
	\big\langle p\big|\hat{x}\big|\mathcal{S}(t)\big\rangle
    &= \int x\langle p|x\rangle \Psi(x, t)\,dx \\
    &= \int x\frac{e^{-ipx/\hbar}}{\sqrt{2\pi\hbar}} \Psi(x, t)\,dx \\
    &= i\hbar\frac{\partial}{\partial p}\int \frac{e^{-ipx/\hbar}}{\sqrt{2\pi\hbar}}\Psi(x, t)\,dx
\end{align*}
$$
回忆此前我们给出的位置空间中的动量波函数，可以看到是一致的。

#### 厄米特算符的矩阵形式

假设一个厄米特算符 $\hat{A}$ 的特征值为 $a_n$，且对于任意 $n \ne m$ 都有 $a_n \ne a_m$。此时考虑 $\hat{A}$ 在 $|a_n\rangle$ 上的分量：
$$
\langle a_m|\hat{A}|a_n\rangle = \langle a_m|a_n|a_n\rangle = a_n\delta_{mn}
$$
这意味着 $\hat{A}$ 的矩阵形式是：
$$
\hat{A} =
\begin{pmatrix}
	a_1 & 0 & \dots \\
	0 & a_2 & \dots \\
	\vdots & \vdots & \ddots
\end{pmatrix}
$$
回忆投影算符的定义，可以将这个矩阵构建出来：
$$
\hat{A} = \sum_{j}a_j|a_j\rangle\langle a_j|
$$
我们可以将其代回去验证：
$$
\left(\sum_j a_j|a_j\rangle\langle a_j|\right)|a_n\rangle = \sum_j a_j |a_j\rangle\langle a_j|a_n\rangle = \sum_j a_j|a_j\rangle\delta_{jn} = a_n|a_n\rangle
$$
根据厄米特算符特征状态的正交性，我们知道前者等价于 $\hat{A}$ 本身。



### 测不准原理

#### 可对易性

算符之间有一个重要的关系，即 **可对易性（Commutability）**。首先，让我们定义 **对易子（Commutator）**：
$$
[\hat{A}, \hat{B}] = \hat{A}\hat{B} - \hat{B}\hat{A}
$$
当对易子为零时，我们就称这两个算符 **互易（Commutable）**。对易的算符之间有很好的性质，我们在下一节中就会讲到。现在，让我们给出一些对易子之间的运算规则：

- $[\hat{A}, \hat{A}] = 0$：可对易的自反性。
- $[\hat{A}, \hat{B}] = -[\hat{B}, \hat{A}]$：反交换律。
- $[\hat{A} + \hat{B}, \hat{C}] = [\hat{A}, \hat{C}] + [\hat{B}, \hat{C}]$：对加法的分配律。
- $[\hat{A}\hat{B}, \hat{C}] = \hat{A}[\hat{B}, \hat{C}] + [\hat{A}, \hat{C}]\hat{B}$：对乘法的分配律。
- $[\lambda\hat{A}, \hat{B}] = \lambda[\hat{A}, \hat{B}]$。
- $[\hat{A}, [\hat{B}, \hat{C}]] + [\hat{B}, [\hat{C}, \hat{A}]] + [\hat{C}, [\hat{A}, \hat{B}]] = 0$：雅可比等式。
- $[\hat{A}, f(\hat{A})] = 0$。

此外，一些常见的算符之间满足下面的对易关系，也被称为 **标准对易关系（Canonical Commutation Relation）**：

- $[\hat{r}_i, \hat{p}_j] = i\hbar\delta_{ij}$。
- $[\hat{r}_i, \hat{r}_j] = [\hat{p}_i, \hat{p}_j] = 0$。

我们可以对一些简单的情形进行证明，比如一维空间中 $[x, p] = i\hbar$，证明如下：
$$
[\hat{x}, \hat{p}]f(x) = [x, -i\hbar\partial_x]f(x) = -xi\hbar\frac{\partial f}{\partial x} + i\hbar\left(\frac{\partial x}{\partial x}f + x\frac{\partial f}{\partial x} \right) = i\hbar f(x)
$$
可以看到我们引入了一个辅助函数 $f(x)$ 用来推导算符的等式，这颇有助于避免出错。值得强调的是，对易子的值与使用的空间无关。我们上面的推导使用的是位置空间，下面将要使用的动量空间中可以得到同样的结果：
$$
[\hat{x}, \hat{p}]f(p) = [i\hbar\partial_p, p]f(p) = i\hbar\left(\frac{\partial p}{\partial p}f + p\frac{\partial f}{\partial p}\right) - i\hbar p\frac{\partial f}{\partial p} = i\hbar f(p)
$$

#### Baker–Campbell–Hausdorff 公式

此前我们曾提到，可以将算符运用于函数中。但是由于它的特殊性质，我们可能会得到意料之外的结果。考虑下面的等式：
$$
e^{\hat{A}}e^{\hat{B}} = e^{\hat{A} + \hat{B}}
$$
它并不一定成立！其原因不难看出：
$$
\begin{align*}
    e^\hat{A}e^\hat{B}
    	&= (1 + \hat{A} + \hat{A}^2 + \dots)(1 + \hat{B} + \hat{B}^2 + \dots) \\
    	&= 1 + \hat{A} + \hat{A}^2 + \dots + \hat{B} + \hat{A}\hat{B} \dots + \hat{B}^2 \\
    e^{\hat{A} + \hat{B}}
    	&= (1 + (\hat{A} + \hat{B}) + (\hat{A} + \hat{B})^2 + \dots) \\
    	&= 1 + \hat{A} + \hat{B} + \hat{A}^2 + \hat{B}\hat{A} + \hat{A}\hat{B} + \hat{B}^2 + \dots
\end{align*}
$$
下面，我们假设对于任两个算符，它们自身和它们的对易子互易：
$$
[\hat{A}, [\hat{A}, \hat{B}]] = [\hat{B}, [\hat{A}, \hat{B}]] = 0
$$
如果将上面的等式展开，就能得到：
$$
\hat{A}^2\hat{B} + \hat{B}^2\hat{A} = 2\hat{A}\hat{B}\hat{A} \qquad \hat{A}\hat{B}^2 + \hat{B}\hat{A}^2 = 2\hat{B}\hat{A}\hat{B}
$$
现在考虑 $[\hat{A}, \hat{B}^n]$，我们有：
$$
\begin{align*}
	[\hat{A}, \hat{B}^n] = [\hat{A}, \hat{B}]\hat{B}^{n-1} + \hat{B}[\hat{A}, \hat{B}^{n-1}]
\end{align*}
$$
发现它可以写为递归的形式，因此或许有更加简洁的通项公式。现在用数学归纳法证明 $[\hat{A}, \hat{B}^n] = n\hat{B}^{n-1}[\hat{A}, \hat{B}]$。对于 $n = 0$ 的情形，这是显然的。现在假设对于 $n = 0, 1, 2, \dots, k$ 都成立，则在 $n = k + 1$ 时：
$$
\begin{align*}
	[\hat{A}，\hat{B}^{k+1}] 
		&= [\hat{A}, \hat{B}^k]\hat{B} + \hat{B}^{k}[\hat{A}, \hat{B}] \\
		&= k\hat{B}^{k-1}[\hat{A}, \hat{B}]\hat{B} + \hat{B}^{k}[\hat{A}, \hat{B}] \\
		&= (k + 1)\hat{B}^k[\hat{A}, \hat{B}]
\end{align*}
$$
这样就证明了上面的等式。随后考虑一个存在泰勒级数的函数 $F$ 以及 $[\hat{A}, F(\hat{B})]$。如果将函数用泰勒级数展开为：
$$
F(\hat{B}) = F(0) + F'(0)\hat{B} + \frac{1}{2}F''(0)\hat{B}^2 + \frac{1}{6}F'''(0)\hat{B}^3 + \dots
$$
根据对易子对加法的分配律，我们有：
$$
\begin{align*}
	[\hat{A}, F(\hat{B})]
    	&= F'(0)[\hat{A}, \hat{B}] + \frac{1}{2}F''(0)[\hat{A}, \hat{B}^2] + \frac{1}{6}F'''(0)[\hat{A}, \hat{B}^3] + \dots \\
    	&= F'(0)[\hat{A}, \hat{B}] + \frac{1}{2}F''(0)\cdot 2\hat{B}[\hat{A}, \hat{B}] + \frac{1}{6}F'''(0)\cdot 3\hat{B}^2[\hat{A}, \hat{B}] + \dots \\
    	&= \frac{d}{d\hat{B}}(F'(0)\hat{B} + \frac{1}{2}F''(0)\hat{B}^2 + \frac{1}{6}F'''(0)\hat{B}^3 + \dots)[\hat{A}, \hat{B}] \\
    	&= \frac{dF}{d\hat{B}}[\hat{A}, \hat{B}]
\end{align*}
$$
特别地，对于 $F(x) = e^x$，我们有 $[\hat{A}, e^{\hat{B}}] = e^\hat{B}[\hat{A}, \hat{B}]$。

现在考虑函数 $f(\lambda) = e^{\lambda\hat{A}}e^{\lambda\hat{B}}$，其关于 $\lambda$ 的导数为：
$$
\frac{df}{d\lambda} = \hat{A}e^{\lambda\hat{A}}e^{\lambda\hat{B}} + e^{\lambda\hat{A}}\hat{B}e^{\lambda\hat{B}}
$$
我们将其中的 $e^{\lambda\hat{A}}\hat{B}$ 代换为 $-[\hat{B}, e^{\lambda\hat{A}}] + Be^{\lambda\hat{A}}$，随后有：
$$
e^{\lambda\hat{A}}\hat{B} = -\lambda e^{\lambda\hat{A}}[\hat{B}, \hat{A}] + Be^{\lambda\hat{A}} = \lambda e^{\lambda\hat{A}}[\hat{A}, \hat{B}] + \hat{B}e^{\lambda\hat{A}}
$$
因此：
$$
\begin{align*}
	\frac{df}{d\lambda} 
		&= \hat{A}e^{\lambda\hat{A}}{e^{\lambda\hat{B}}} + \lambda e^{\lambda\hat{A}}[\hat{A}, \hat{B}]e^{\lambda\hat{B}} + \hat{B}e^{\lambda\hat{A}}e^{\lambda\hat{B}} \\
		&= (\hat{A} + \hat{B} + \lambda[\hat{A}, \hat{B}])e^{\lambda\hat{A}}e^{\lambda\hat{B}} \\
		&= (\hat{A} + \hat{B} + \lambda[\hat{A}, \hat{B}])f
\end{align*}
$$
其中我们用到了 $e^{\lambda\hat{A}}$ 与 $[\hat{A}, \hat{B}]$ 相易的事实，这点可以通过将前者泰勒级数展开，然后再利用 $[\hat{A}, [\hat{A}, \hat{B}]] = 0$ 得到。最后，我们需要求出这个微分方程的解。不难验证 $e^{\lambda(\hat{A} + \hat{B})}e^{\lambda^2[\hat{A}, \hat{B}]/2}$ 是一个解：
$$
\begin{align*}
	\frac{df}{d\lambda}
		&= (\hat{A} + \hat{B})e^{\lambda(\hat{A} + \hat{B})}e^{\lambda^2[\hat{A}, \hat{B}]/2} + e^{\lambda(\hat{A} + \hat{B})}\lambda e^{\lambda^2[\hat{A}, \hat{B}]/2} \\
		&= (\hat{A} + \hat{B} + \lambda[\hat{A}, \hat{B}])e^{\lambda(\hat{A} + \hat{B})}e^{\lambda^2[\hat{A}, \hat{B}]/2}
\end{align*}
$$
 取 $\lambda = 1$ 时，我们就得到了 **Baker-Campbell-Hausdorff 公式**：
$$
e^{\hat{A}}e^{\hat{B}} = e^{\hat{A} + \hat{B}}e^{[\hat{A}, \hat{B}]/2}
$$


#### 广义测不准原理

我们在第一章中提到过位置和动量的测不准原理，现在让我们尝试证明它。有了上一节完整的统计解释后，我们可以看向更一般的情形：对于任意可观测量 $A$，其方差为：
$$
\sigma_A^2 = \left\langle\left.\left(\hat{A} - \langle A\rangle\right)\Psi\right|\left(\hat{A} - \langle A\rangle\right)\Psi\right\rangle = \langle f|f\rangle
$$
其中记 $f \equiv \left(\hat{A} - \langle A\rangle\right)\Psi$。 类似地，对于另一个可观测量 $B$，以及 $g \equiv \left(\hat{B} - \langle B\rangle\right)\Psi$，我们有：
$$
\sigma_B = \langle g|g\rangle
$$
此时根据施瓦尔兹不等式：
$$
\sigma_A^2\sigma_B^2 = \langle f|f\rangle\langle g|g\rangle \ge |\langle f|g\rangle|^2
$$
对任意复数 $z$，我们都有：
$$
|z|^2 = \Re^2z + \Im^2z \ge \Im^2z = \left[\frac{1}{2i}(z - z^*)\right]^2
$$
因此对于 $\langle f|g\rangle$：
$$
\sigma_A^2\sigma_B^2 \ge \left[\frac{1}{2i}(\langle f|g\rangle - \langle g|f\rangle)\right]^2
$$
同时：
$$
\begin{align*}
	\langle f|g\rangle
	&= \left\langle\left.\left(\hat{A} - \langle A\rangle\right)\Psi\right|\left(\hat{B} - \langle B\rangle\right)\Psi\right\rangle \\
	&= \left\langle\Psi\left|\left(\hat{A} - \langle A\rangle\right)\left(\hat{B} - \langle B\rangle\right)\Psi\right.\right\rangle \\
	&= \left\langle \Psi\left|\hat{A}\hat{B}\Psi\right.\right\rangle - \left\langle\Psi\left|\hat{A}\Psi\right.\right\rangle \langle B\rangle - \langle A\rangle \left\langle \Psi\left|\hat{B}\Psi\right.\right\rangle + \langle A\rangle\langle B\rangle \Big\langle \Psi\Big|\Psi\Big\rangle \\
	&= \langle \hat{A}\hat{B}\rangle - \langle A\rangle\langle B\rangle - \langle A\rangle\langle B\rangle + \langle A\rangle\langle B\rangle \\
	&= \langle\hat{A}\hat{B}\rangle - \langle A\rangle\langle B\rangle
\end{align*}
$$
类似地有：
$$
\langle g|f\rangle = \langle \hat{B}\hat{A}\rangle - \langle A\rangle\langle B\rangle
$$
因此两者的差是：
$$
\langle f|g\rangle - \langle g|f\rangle = \langle{\hat{A}\hat{B}}\rangle -  \langle\hat{B}\hat{A}\rangle = \langle\hat{A}\hat{B} - \hat{B}\hat{A}\rangle = \langle[\hat{A}, \hat{B}]\rangle
$$
回忆其中的方括号是我们上节中引入的对易子记号。这样我们就得到了最终的结论：
$$
\sigma_A^2\sigma_B^2 \ge \left(\frac{1}{2i}\left\langle\left[\hat{A}, \hat{B}\right]\right\rangle\right)^2
$$
这也被称为一般化的 **测不准原理**。特别地，取 $A = x$，$B = p$ 时我们有：
$$
\sigma_x^2\sigma_p^2 \ge \left(\frac{1}{2i}[\hat{x}, \hat{p}]\right)^2 = \left(\frac{1}{2i}i\hbar\right)^2 = \frac{\hbar^2}{4}
$$
这也就是我们在第一章得到的结论：
$$
\sigma_x\sigma_p \ge \frac{\hbar}{2}
$$
不难发现在一般化的测不准原理中，右式在两个算符互易时是 $0$，此时我们称这两个可观测量是 **兼容的（Compatible）**。换句话说，所有 **不兼容的（Incompatible）**算符之间都存在测不准的现象。比如氢原子中的哈密顿量、角动量大小和角动量的 $z$ 方向分量是相互兼容的，因此它们三个拥有相同的特征状态（也即令这几个量都确定的特征状态）。

测不准原理是量子力学的统计解释推出的结论，它对实验中可观测量之间的不可兼容性做出了数学上的解释。每次观测都会导致波函数坍缩，但是只有两次测量的量可兼容时，它们才能对应一致的结果。作为例子，我们观测粒子位置时，波函数会坍缩为一个尖峰（其对应的波长有无数种，动量同理）；随后如果我们观测粒子动量，波函数会坍缩为一个正弦曲线，此时波长（以及对应的动量）是确定的，但粒子的位置不再是我们此前测量得到的了。相反，如果观测的两个量拥有相同的特征状态，第二次观测时波函数会坍缩成与之前完全相同的形状，因此上一次测量的结果依然有效。

#### 最小不确定性波包

我们此前已经遇到过波函数正好位于测不准原理的边界的情形（简谐振子），我们对于这种情况的普遍情况更加感兴趣：什么条件下这种 **最小不确定性波包（Minimum-Uncertainty Wave Packet）** 会成立？观察我们上一节对广义测不准原理的推导，会发现其中的不等号在两个式子中出现，其一是施瓦尔兹不等式，另外一个是将复数化为虚部。如果想要这两步的不等号写成等号，函数 $f$ 和 $g$ 需要满足：
$$
g(x) = iaf(x)
$$
其中 $a \in \mathbb{R}$。令 $f = (\hat{p} - \langle p\rangle)$ 及 $g = (\hat{x} - \langle x\rangle)$，有：
$$
\left(-i\hbar\frac{d}{dx} - \langle p\rangle\right)\Psi = ia(x - \langle x\rangle)\Psi
$$
它的通解是：
$$
\Psi(x) = Ae^{-a(x - \langle x\rangle)^2/(2\hbar)}e^{i\langle p\rangle x/\hbar}
$$
可以看出这个波包是一个高斯函数，我们此前在简谐振子处得到的结果也是一个高斯函数。

#### 能量-时间测不准原理

我们已经熟悉的位置-动量测不准原理实际上对于同等系统重复测量的描述有些不严谨。通常它会跟着另一个测不准原理一起出现，即 **能量-时间测不准原理（Energy-Time Uncertainty Principle）**：
$$
\Delta t\Delta E \ge \frac{\hbar}{2}
$$
从狭义相对论角度来看，这两个测不准原理是高度一致的，因为位置和时间（$\mathbf{x}$ 和 $ct$）形成了位置-时间四元矢量；动量和能量（$\mathbf{p}$ 和 $E/c$）则形成了动量-能量四元矢量。不过本篇笔记并不涉及相对论（注意到薛定谔方程中 $x$ 和 $t$ 的是不平权的），因此这两个式子不能混为一谈。事实上，我们接下来要尝试推导能量-时间测不准原理，从中会发现它们是非常不同的。

首先我们注意到，在非相对论的语境中，$t$ 和 $x$、$p$、$E$ 是完全不同的；我们可以测量某一时刻的其它物理量，但时间没有办法测量：它是一个独立存在的量。我们没有办法测量多次时间，然后取其标准差；反过来说，正是因为时间的变化，我们才能测量其它物理量并计算得到它们的标准差。

为了测量一个系统变化的速率，我们可以取某个物理量 $Q$ 的期望值的平均值：
$$
\begin{align*}
	\frac{d}{dt}\langle Q\rangle 
	= \frac{d}{dt}\left\langle \Psi\left|\hat{Q}\Psi\right.\right\rangle 
	&= \left\langle\left.\frac{\partial \Psi}{\partial t}\right|\hat{Q}\Psi\right\rangle +
	\left\langle\Psi\left|\frac{\partial \hat{Q}}{\partial t}\Psi\right.\right\rangle + 
	\left\langle\Psi\left|\hat{Q}\frac{\partial \Psi}{\partial t}\right.\right\rangle \\
	&= -\frac{1}{i\hbar}\left\langle \hat{H}\Psi\left|\hat{Q}\Psi\right.\right\rangle +
	\left\langle\frac{\partial \hat{Q}}{\partial t}\right\rangle + 
	\frac{1}{i\hbar}\left\langle\Psi\left|\hat{Q}\hat{H}\Psi\right.\right\rangle \\
	&= \frac{i}{\hbar}\left\langle\Psi\left|(\hat{H}\hat{Q} - \hat{Q}\hat{H})\Psi\right.\right\rangle + 
	\left\langle\frac{\partial \hat{Q}}{\partial t}\right\rangle \\&
	= \frac{i}{\hbar}\left\langle\left[\hat{H}, \hat{Q}\right]\right\rangle + \left\langle\frac{\partial \hat{Q}}{\partial t}\right\rangle
\end{align*}
$$
我们将其称为 **广义埃伦费斯特定理（Generalized Ehrenfest Theorem）**。当算符与时间无关时（我们通常假设这一点成立），其对应的物理量期望值的变化率只和该算符与哈密顿算符的交换子有关；特别地，当 $\hat{Q}$ 和 $\hat{H}$ 兼容时，$Q$ 是一个保守的物理量。如果我们将上面的公式代入广义测不准原理，有：
$$
\sigma_H^2\sigma_Q^2 \ge \left(\frac{1}{2i}\left\langle\left[\hat{H}, \hat{Q}\right]\right\rangle\right)^2 = \frac{\hbar^2}{4}\left(\frac{d\langle Q\rangle}{dt}\right)^2
\implies \sigma_H\sigma_Q \ge \frac{\hbar}{2}\left|\frac{d\langle Q\rangle}{dt}\right|
$$
现在记 $\Delta E = \sigma_H$，且如果设：
$$
\Delta t = \frac{\sigma_Q}{|d\langle Q\rangle/dt|}
$$
此时就有：
$$
\Delta E\Delta t \ge \frac{\hbar}{2}
$$
可以看到，我们的 $\Delta t$，其物理意义是 $\langle Q\rangle$ 变化一个标准差 $\sigma_Q$ 所需的时间，因此它依赖于 $Q$ ，对于不同的可观测量会有不同的大小。当 $\Delta E$ 非常大的时候，说明 $Q$ 的变化非常迅速，也就是所需时间很小；否则 $Q$ 就是缓慢变化，其对应的时间就很长。

广义埃伦菲斯特定理可以给出一些有趣的结论，让我们以 $Q = 1$、$Q = H$、$Q = x$ 和 $Q = p$ 为例，探索这个等式蕴含的道理：
$$
\begin{align*}
	(Q = 1) \qquad &\left\{
	\begin{split}
		&
        \begin{split}
            \frac{d}{dt}\langle Q \rangle
                &= \frac{d}{dt} \int \Psi^*(x, t)(1)\Psi(x, t)\,dx \\
                &= \frac{d}{dt} \int |\Psi(x, t)|^2\,dx
        \end{split} \\
        &
        \begin{split}
            \frac{i}{\hbar}\left\langle\left[\hat{H}, \hat{Q}\right]\right\rangle + \left\langle \frac{\partial \hat{Q}}{\partial t}\right\rangle 
				&= \frac{i}{\hbar}\int\Psi^*(x, t)[\hat{H}, 1]\Psi(x, t)\,dx + \int \Psi^*(x, t)\frac{\partial (1)}{\partial t}\Psi(x, t)\,dx \\
				&= \frac{i}{\hbar}\int \Psi^*(x, t)(\hat{H} - \hat{H})\Psi(x, t)\,dx \\
				&= 0
        \end{split}
	\end{split}
	\right. \\
	& \implies \frac{d}{dt} \int |\Psi(x, t)|^2\,dx = 0 \tag{*} \\
	(Q = H) \qquad &\left\{
	\begin{split}
		&
		\begin{split}
            \frac{i}{\hbar}\left\langle\left[\hat{H}, \hat{Q}\right]\right\rangle + \left\langle \frac{\partial \hat{Q}}{\partial t} \right\rangle
            	&= \frac{i}{\hbar}\int \Psi^*(x, t)[\hat{H}, \hat{H}]\Psi(x, t)\,dx + \int\Psi^*(x, t)\frac{\partial \hat{H}}{\partial t}\Psi(x, t)\,dx \\
            	&= \int\Psi^*(x, t)\frac{\partial}{\partial t}\left(\frac{\hat{p}^2}{2m} + V(x)\right)\,\Psi(x, t)\,dx \\
            	&= 0
		\end{split}
	\end{split}
	\right. \\
	& \implies \frac{d}{dt}\langle H \rangle = 0 \tag{**} \\
	(Q = x) \qquad &\left\{
	\begin{split}
		&
		\begin{split}
			\frac{i}{\hbar}\left\langle\left[\hat{H}, \hat{Q}\right]\right\rangle + \left\langle \frac{\partial \hat{Q}}{\partial t} \right\rangle 
				&= \frac{i}{\hbar}\int \Psi^*(x, t)[\hat{H}, \hat{x}]\Psi(x, t)\,dx + \int \Psi^*(x, t)\frac{\partial \hat{x}}{\partial t}\Psi(x, t)\,dx \\
				&= \frac{i}{\hbar}\int \Psi^*(x, t)\left(\hat{H}x - x\hat{H}\right) \\
				&= \frac{i}{\hbar}\int \Psi^*(x, t)\left[-\frac{1}{2m}(\hat{p}^2x - x\hat{p}^2) + Vx - xV\right]\Psi(x, t)\,dx \\
				&= -\frac{i}{2m\hbar}\int \Psi^*(x, t)\left[\frac{\hbar}{i}\frac{\partial^2}{\partial x^2}(x\Psi(x, t)) - x\frac{\hbar}{i}\frac{\partial^2}{\partial x^2}\Psi(x, t)\right]\,dx \\
			&= \frac{}{}
		\end{split}
	\end{split}
	\right.
\end{align*}
$$



### 简谐振子与算符

这一节让我们再次回顾简谐振子，不过巧妙利用算符求解。回忆简谐振子的薛定谔方程：
$$
\begin{equation*}
	-\frac{\hbar^2}{2m}\frac{d^2\psi}{dx^2} + \frac{1}{2}m\omega^2x^2 = E\psi
\end{equation*}
$$
如果将其中的 $\frac{d}{dx}$ 用算符 $\hat{p}$ 代换，我们就得到：
$$
\frac{1}{2m}\left[\hat{p}^2 + (m\omega x)^2\right] = E\psi
$$
左边也应该是哈密顿算符，因此：
$$
\begin{equation*}
	\hat{H} = \frac{1}{2m}\left[\hat{p}^2 + (m\omega x)^2\right]
\end{equation*}
$$
为了进一步简化这个式子，我们可以尝试将等式右侧因式分解。参考复数域中 $a^2 + b^2 = (iu + v)(-iu + v)$，我们可以设出下面的两个算符：
$$
\begin{equation*}
	\hat{a}_\pm = \frac{1}{\sqrt{2\hbar m\omega}}(\mp i\hat{p} + m\omega x)
\end{equation*}
$$
其统称为 **阶梯算符（Ladder Operator）**，分别称为 **上升算符（Raising Operator）** 和 **下降算符（Lowering Operator）**。其命名的深意我们很快就会看到。

细心的同学可能已经发现，$\hat{H} \ne \hat{a}_+\hat{a}_-$。这是因为 $\hat{p}$ 和 $\hat{x}$ 之间并不互易。特别地，有：
$$
\begin{align*}
	\hat{a}_-\hat{a}_+ 
		&= \frac{1}{2\hbar m\omega}(i\hat{p} + m\omega x)(-i\hat{p} + m\omega x) \\
		&= \frac{1}{2\hbar m\omega}\left[\hat{p}^2 + (m\omega x)^2 - im\omega(x\hat{p} - \hat{p}x)\right] \\
		&= \frac{1}{2\hbar m\omega}\left[\hat{p}^2 + (m\omega x)^2 - im\omega(i\hbar)\right] \\
		&= \frac{1}{\hbar \omega}\hat{H} + \frac{1}{2}
\end{align*}
$$
类似地我们可以得到：
$$
\begin{equation*}
	\hat{a}_+\hat{a}_- = \frac{1}{\hbar\omega}\hat{H} - \frac{1}{2}
\end{equation*}
$$
因此阶梯算符之间也不互易：
$$
\begin{equation*}
	[\hat{a}_+, \hat{a}_-] = 1
\end{equation*}
$$




## 三维中的薛定谔方程

### 薛定谔方程

#### 笛卡尔坐标

我们此前研究的都是一维空间；如果在三维空间中，我们需要对动量做一些调整。回忆经典力学中的哈密顿量：
$$
H = \frac{1}{2}mv^2 + V = \frac{1}{2m}\left(p_x^2 + p_y^2 + p_z^2\right) + V
$$
我们同时考虑三个方向上的动量，因此三维中的动量算符是：
$$
\hat{\mathbf{p}} = -ih\unit{x}\frac{\partial}{\partial x} -ih\unit{y}\frac{\partial}{\partial y} -ih\unit{z}\frac{\partial}{\partial z} = -ih\nabla
$$
同时，薛定谔方程变为下面的形式：
$$
i\hbar\frac{\partial \Psi}{\partial t} = \hat{H}\Psi = -\frac{\hbar}{2m}\nabla^2\Psi + V\Psi
$$
三维中的归一化也要确保三个方向都顾及到：
$$
\int|\Psi|^2\,d^3\mathbf{r} = 1
$$
其中记号 $d^3\mathbf{r} = dx\,dy\,dz$ 代表一个体积微元。和此前类似地，我们可以通过分离变量法，将时间无关的薛定谔方程从原方程中提取出来：
$$
-\frac{\hbar^2}{2m}\nabla^2\psi + V\psi = E\psi
$$
这个方程的解 $\psi_n(\mathbf{r})$ 可以建立薛定谔方程的解：
$$
\Psi_n(\mathbf{r}, t) = \psi_n(\mathbf{r})e^{-iE_nt/\hbar}
$$
三维中薛定谔方程的通解是：
$$
\Psi(\mathbf{r}, t) = \sum_n c_n\psi_n(\mathbf{r})e^{-iE_nt/\hbar}
$$

#### 球坐标

球坐标中的拉普拉斯算子简直是梦魇：
$$
\nabla^2 = \frac{1}{r^2}\frac{\partial}{\partial r}\left(r^2\frac{\partial}{\partial r}\right) + \frac{1}{r^2\sin\theta}\frac{\partial}{\partial \theta}\left(\sin\theta\frac{\partial}{\partial \theta}\right) + \frac{1}{r^2\sin^2\theta}\left(\frac{\partial^2}{\partial \phi^2}\right)
$$
但我们依然可以通过分离变量法解决这个麻烦。假设所有时间无关解都可以分为两个部分：
$$
\psi(r, \theta, \phi) = R(r)Y(\theta, \phi)
$$
经过一些计算，我们可以得到一个等式：
$$
\frac{1}{R}\frac{d}{dr}\left(r^2\frac{dR}{dr}\right) - \frac{2mr^2}{\hbar^2}[V(r) - E] 
= -\frac{1}{Y}\left[\frac{1}{\sin\theta}\frac{\partial}{\partial \theta}\left(\sin\theta\frac{\partial Y}{\partial \theta}\right) + \frac{1}{\sin^2\theta}\frac{\partial^2 Y}{\partial \phi^2}\right]
= \ell(\ell + 1)
$$
这里的 $\ell \in \mathbb{C}$ 是任取的。下面我们将求解这两个常微分方程。

首先是角方程 $Y(\theta, \phi)$：
$$
\sin\theta\frac{\partial}{\partial \theta}\left(\sin\theta\frac{\partial Y}{\partial \theta}\right) + \frac{\partial^2 Y}{\partial \phi^2} = -\ell(\ell + 1)\sin^2\theta Y
$$
我们依然用分离变量法，设：
$$
Y(\theta, \phi) = \Theta(\theta)\Phi(\phi)
$$
再次经过一些计算，我们得到：
$$
\frac{1}{\Theta}\left[
	\sin\theta\frac{d}{d\theta}
	\left(
		\sin\theta\frac{d\Theta}{d\theta}
	\right)
\right]
+ \ell(\ell + 1)\sin^2\theta = -\frac{1}{\Phi}\frac{d^2\Phi}{d\phi^2} = m^2
$$
这里的 $m\in \mathbb{C}$ 是任取的。这里的 $\Phi$ 可以轻松送求解出来：
$$
\Psi(\phi) = e^{im\phi}
$$
这里其实还有另外一项 $e^{-im\phi}$，但只需让 $m$ 可以取负，两者就是等价的；常系数则可以让 $\Theta$ 吸收掉。在电动力学中我们求解电势的在球坐标系中的泊松方程时同样使用了分离变量法，不过彼时我们将角方程的解写成三角函数的形式，那是因为电动力学中不存在复数值。

由于 $\phi$ 的周期性，$\Phi$ 理应是一个周期函数，即：
$$
\Phi(\phi + 2\pi) = \Phi(\phi) \implies e^{im(\phi + 2\pi)} = e^{im\phi} \implies e^{2m\pi i} = 1
$$
所以 $m$ 必须是一个整数。另一边，$\Theta$ 的方程：
$$
\sin\theta\frac{d}{d\theta}\left(\sin\theta\frac{d\Theta}{d\theta}\right) + \left[\ell(\ell + 1) - m^2\right]\Theta = 0
$$
其解如下：
$$
\Theta(\theta) = AP_\ell^m(\cos\theta)
$$
其中 $P_\ell^m$ 是 **关联勒让德函数（Associated Legendre Function）**，$\ell \in \mathbb{Z}^+\cup\{0\}$，其定义如下：
$$
P_\ell^m(x) = (-1)^m(1 - x^2)^{m/2}\left(\frac{d}{dx}\right)^mP_\ell(x)
$$
其中 $P_\ell(x)$ 被称为 **勒让德多项式（Legendre Polynomial）**，我们可以通过 **罗德里格斯公式（Rodrigues Formula）** 计算得到它的定义：
$$
P_\ell(x) = \frac{1}{2^\ell \ell!}\left(\frac{d}{dx}\right)^\ell(x^2 - 1)^\ell
$$
下面罗列了 $\ell = 0$ 到 $\ell = 5$ 时勒让德多项式的定义：
$$
\begin{align*}
	P_0 &= 1 \\
	P_1 &= x \\
	P_2 &= \frac{1}{2}(3x^2 - 1) \\
	P_3 &= \frac{1}{3}(5x^3 - 3x) \\
	P_4 &= \frac{1}{8}(35x^4 - 30x^2 + 3) \\
	P_5 &= \frac{1}{8}(63x^5 - 70x^3 + 15x)
\end{align*}
$$
截取 $[-1, 1]$ 区间后，他们的图像如下：

<img src="graphs/qm1_4-1.png" alt="qm1_4-1" style="zoom:50%;" />

至于关联勒让德函数，其形式不再是一个多项式，我们可以以 $\ell = 2$ 为例：
$$
\begin{align*}
	P_2^0(x) &= \frac{1}{2}(3x^2 - 1) \\
	P_2^1(x) &= -3x\sqrt{1 - x^2} \\
	P_2^2(x) &= 3(1 - x^2) 
\end{align*}
$$
在 $\Theta$ 的解中，我们需要对 $\cos\theta$ 调用这个函数，此时 $\sqrt{1 - \cos\theta}$ 会得到 $\sin\theta$。因此，$\Theta(\theta)$ 是一系列 $\sin\theta$ 和 $\cos\theta$ 组成的“多项式”。下面是更多的例子：
$$
\begin{align*}
	P_0^0 &= 1 & P_1^0 &= \cos\theta & P_3^0 &= \frac{1}{2}(5\cos^2\theta - 3\cos\theta) \\
	P_1^1 &= -\sin\theta & P_3^1 &= \frac{3}{2}\sin\theta(5\cos^2\theta - 1) & P_3^2 &= 15\sin^2\theta\cos\theta \\
	P_3^3 &= -15\sin\theta(1 - \cos\theta)
\end{align*}
$$
从定义中不难看出 $m > \ell$ 时 $P_\ell^m = 0$，因此对于任何 $\ell$，有效的 $m$ 只在下面的整数中取：
$$
m = 0, \pm 1, \pm 2, \dots, \pm (\ell - 1), \pm \ell
$$
需要注意，由于 $\Theta(\theta)$ 是一个二次常微分方程的解，因此理论上我们应该还有一个解；事实证明另一个解不满足我们需要的物理性质（它会在 $\theta = 0$ 或 $\theta = \pi$ 处无意义），因此我们只研究关联勒让德函数。

最后让我们确保这个解是归一化的，也就是确定系数 $A$ 的值：
$$
\int|\psi|^2r^2\sin\theta\,dr\,d\theta\,d\phi = 1 \implies \int|R|^2r\,dr\int|Y|^2\sin\theta\,d\theta\,d\phi = 1
$$
一个简单的处理方法是将两个积分都设为 $1$。我们最终得到的解是：
$$
Y_\ell^m(\theta, \phi) = \sqrt{\frac{(2\ell + 1)(\ell - m)!}{4\pi(\ell + m)!}}e^{im\phi}P_\ell^m(\cos\theta)
$$
它被称为 **球谐函数（Spherical Harmonics）**。我们随后会认识到，这个函数是正交的：
$$
\int_0^{2\pi}\int_0^\pi [Y_\ell^m(\theta, \phi)]^*[Y_{\ell'}^{m'}(\theta, \phi)]\sin\theta\,d\theta\,d\phi = \delta_{\ell\ell'}\delta_{mm'}
$$
当势能 $V$ 在球坐标中对称（角度上）时，它只会对 $R$ 的方程产生影响，此时我们的方程是：
$$
\frac{d}{dr}\left(r^2\frac{dR}{dr}\right) - \frac{2mr^2}{\hbar^2}(V(r) - E)R = \ell(\ell + 1)R
$$
这里令 $u(r) = rR(r)$，就可以将上面的方程变为等价的 **径向方程（Radical Equation）**：
$$
-\frac{\hbar^2}{2m}\frac{d^2u}{dr^2} + \left[V + \frac{\hbar^2}{2m}\frac{\ell(\ell + 1)}{r^2}\right]u = Eu
$$
可以看到它和时间无关的薛定谔方程几乎完全一致，只不过这里的势能是 **有效势能（Effective Potential）**：
$$
V_\text{eff} = V + \frac{\hbar^2}{2m}\frac{\ell(\ell + 1)}{r^2}
$$
这里的第二项称为 **离心项（Centrifugal Term）**，其可以类比经典力学中的离心力，它是一个伪力，让粒子倾向于远离原点。我们依然需要归一化成立：
$$
\int_0^\infty |u|^2\,dr = 1
$$

### 角动量

在经典力学中，角动量通过下面的公式得到：
$$
\mathbf{L} = \mathbf{r}\times\mathbf{p}
$$
我们可以将其写成等价的分量形式：
$$
L_x = yp_z - zp_y \qquad L_y = zp_x - xp_z \qquad L_z = xp_y - yp_x
$$
这样做的好处是我们可以将其直接理解为算符，比如：
$$
\hat{L}_x = \hat{y}\hat{p}_z - \hat{z}\hat{p}_y = -i\hbar\hat{y}\frac{\partial}{\partial z} - i\hbar\hat{z}\frac{\partial}{\partial y}
$$
本节中为了防止将算符（如 $\hat{x}$）和单位矢量（如 $\unit{x}$）弄混，我们将不再为算符加上头标（事实上，不少物理学者从来不会加这个符号）。下面，我们将探索角动量空间的特征值和特征矢量。

#### 特征值

注意到角动量的不同分量之间不互易：
$$
\begin{align*}
	[L_x, L_y] 
	&= [yp_z - zp_y, zp_x - xp_z] \\
	&= [yp_z, zp_x] - [yp_z, xp_z] - [zp_y, zp_x] + [zp_y, xp_z]
\end{align*}
$$
回忆在[前面章节](#可对易性)中介绍过的标准对易关系，上面所有方向不同的位置与动量都互易，因此剩下的是：
$$
[L_x, L_y] = yp_x[p_z, z] + xp_y[z, p_z] = i\hbar(xp_y - yp_x) = i\hbar L_z
$$
根据对称性，我们得到了下面这样简介的结论：
$$
[L_x, L_y] = i\hbar L_z \qquad [L_y, L_z] = i\hbar L_x \qquad [L_z, L_x] = i\hbar L_y
$$
这就是三维空间中角动量的基本对易关系；和 $\mathbf{r}$ 与 $\mathbf{p}$ 不同的是，它的不同分量之间并不对易。根据广义测不准原理，我们有下面的结论（以 $x$、$y$ 方向为例）：
$$
\sigma_{L_x}\sigma_{L_y} \ge \frac{\hbar}{2}|\langle L_z\rangle|
$$
同时也就不存在同时为不同方向上角动量的特征函数。不过，这些分量的平方和，$L^2 = L_x^2 + L_y^2 + L_z^2$ 和每个分量一一对易（以 $x$ 方向为例）：
$$
\begin{align*}
	[L^2, L_x]
	&= [L_x^2, L_x] + [L_y^2, L_x] + [L_z^2, L_x] \\
	&= L_y[L_y, L_x], [L_y, L_x]L_y + L_z[L_z, L_x] + [L_z, L_x]L_z \\
	&= L_y(-i\hbar L_z) + (-i\hbar L_z)L_y + L_z(i\hbar L_y) + (i\hbar L_y)L_z \\
	&= 0
\end{align*}
$$
这进一步告诉我们：
$$
[L^2, \mathbf{L}] = 0
$$
此外，可能存在同时为 $L^2$ 和 $L_z$ 的特征函数 $f$，即：
$$
L^2f = \lambda f \qquad L_zf = \mu f
$$
注意到 $L_z^2f = \mu^2f$，因此：
$$
(L_x^2 + L_y^2)f = (\lambda - \mu^2)f
$$
左侧是两个算符的平方和；为了便于分析，我们引入两个共轭的算符，从初等代数的角度来看其积为 $L_x^2 + L_y^2$（但作为算符时其积要加上和交换子相关的项，我们后面会再次提到）：
$$
L_\pm = L_x \pm iL_y
$$
我们称其为 **阶梯算符（Ladder Operator）**。它们和 $L_z$ 与 $L^2$ 的对易关系不难计算：
$$
\begin{align*}
	[L_z, L_\pm] 
	&= [L_z, L_x] \pm i[L_z, L_y] \\
	&= i\hbar(L_y \mp i\hbar L_x) \\
	&= \pm \hbar L_\pm \\
	[L^2, L_\pm]
	&= [L^2, L_x] \pm i[L^2, L_y] \\
	&= 0
\end{align*}
$$
观察到 $L_\pm f$ 函数满足：
$$
\begin{align*}
	L^2(L_\pm f) &= L_\pm (L^2f) \\
	&= L_\pm(\lambda f) \\
	&= \lambda (L_\pm f) \\
	L_z(L_\pm f) &= [L_z, L_\pm]f + L_\pm L_zf \\
	&= \pm \hbar L_\pm f + L_\pm (\mu f) \\
	&= (\mu\pm \hbar)L_\pm f
\end{align*}
$$
因此 $L_\pm f$ 也是 $L^2$ 和 $L_z$ 的特征函数，其对应的特征值分别为 $\lambda$（不变）和 $\mu \pm \hbar$（相差一个常数）。这个“增减”的特性让我们也将 $L_+$ 和 $L_-$ 分别称为 **上升算符（Raising Operator）** 和 **下降算符（Lowering Operator）**。对于任何已知的特征值 $\mu$，我们都可以得到一系列特征函数和特征值。需要注意的是，为了保证 $\lambda \ge \mu^2$，一定存在一个最大的特征值，设其为 $\ell\hbar$，对应的特征函数为 $f_t$，则有：
$$
L^2f_t = \lambda f_t \qquad L_zf_t = \hbar\ell f_t
$$
现在让我们分析两个阶梯函数的积：
$$
\begin{align*}
	L_\pm L_\mp
	&= (L_x \pm iL_y)(L_x \mp iL_y) \\
	&= L_x^2 + L_y^2 \mp i(L_xL_y - L_yL_x) \\
	&= L^2 - L_z^2 \pm \hbar L_z
\end{align*}
$$
因此我们可以将 $L^2$ 表示为阶梯函数的形式：
$$
L^2 = L_\pm L_\mp + L_z^2 \mp \hbar L_z
$$
对于 $f_t$，应有：
$$
\begin{align*}
	L^2f_t
	&= (L_-L_+ + L_z^2 + \hbar L_z)f_t \\
	&= L_-(L_+f_t) + \hbar^2\ell^2 f_t + \hbar^2\ell f_t \\
	&= \hbar^2\ell(\ell + 1)f_t
\end{align*}
$$
这里我们设 $L_+f_t = 0$（这只是一个假设，也可能有 $L_+f_t = \infty$，不过那是另一种情况了）。因此我们得到：
$$
\lambda = \hbar^2\ell(\ell+1)
$$
类似地（为了保证 $\lambda \ge \mu^2$），一定也存在一个最小的特征值 $\bar{\ell}\hbar$，对应了特征函数 $f_b$，则有：
$$
L^2f_b = \lambda f_b \qquad L_zf_b = \hbar\bar{\ell}f_b
$$
同样将 $L^2$ 替换为阶梯函数：
$$
\begin{align*}
	L^2f_b &= (L_+L_- + L_z^2 - \hbar L_z)f_b \\
	&= L_+(L_-f_b) + \hbar^2\bar{\ell}^2f_b - \hbar^2\bar{\ell}f_b \\
	&= \hbar^2\bar{\ell}(\ell - 1)f_b
\end{align*}
$$
因此：
$$
\lambda = \hbar\bar{\ell}(\bar{\ell} - 1)
$$
对比之前得到的等式，可以解得 $\bar{\ell} = -\ell$。至此，我们得到了一个高度对称的结果，即 $L_z$ 的特征值是从 $-\hbar\ell$ 到 $\hbar\ell$ 的一系列数。我们将其记为下面的形式：
$$
L^2f_\ell^m = \hbar^2\ell(\ell + 1)f_\ell^m \qquad L_zf_\ell^m = \hbar mf_\ell^m
$$
其中 $\ell$ 是一系列半正整数，即 $0, 1/2, 1, 3/2, 2, \dots$，而 $m$ 是夹在 $\pm \ell$ 中的半整数，即 $0, \pm 1, \dots, \pm (\ell -1), \pm \ell$。

听起来有些神奇，因为我们在对特征函数一无所知的情况下就解出了特征值；这也是阶梯算符的一个独特之处。事实上，我们下一节中将要求解的角动量的特征函数正是前面我们接触过的球谐函数 $Y_\ell^m$（实际上从上下标的使用就可以隐约猜到它们的联系）。

#### 特征函数

本节中我们将以解析形式构建上节中的算符。首先，球坐标系中的角动量是：
$$
\mathbf{L} = -i\hbar(\mathbf{r}\times\nabla)
$$
将球坐标系上的 Del 算符代入：
$$
\nabla = \unit{r}\frac{\partial}{\partial r} + \unit{\theta}\frac{1}{r}\frac{\partial}{\partial \theta} + \unit{\phi}\frac{1}{r\sin\theta}\frac{\partial}{\partial \phi}
$$
得到：
$$
\begin{align*}
	\mathbf{L}
	&= -i\hbar\left[r(\unit{r}\times\unit{r})\frac{\partial}{\partial r} + (\unit{r}\times\unit{\theta})\frac{\partial}{\partial \theta} + (\unit{r}\times\unit{\phi})\frac{\partial}{\partial \phi}\right] \\
	&= -i\hbar\left(\unit{\phi}\frac{\partial}{\partial \theta} - \unit{\theta}\frac{1}{\sin\theta}\frac{\partial}{\partial \phi}\right) \\
	&= -i\hbar\left[
		(-\unit{x}\sin\phi + \unit{y}\cos\phi)\frac{\partial}{\partial \theta} - (\unit{x}\cos\theta\cos\phi + \unit{y}\cos\theta\sin\phi - \unit{z}\sin\theta)\frac{1}{\sin\theta}\frac{\partial}{\partial\phi}
	\right] \\
	& \implies
	\begin{cases}
		L_x &= -i\hbar\left(-\sin\phi\dfrac{\partial}{\partial \theta} - \cos\phi\cot\theta\dfrac{\partial}{\partial\phi}\right) \\
		L_y &= -i\hbar\left(+\cos\phi\dfrac{\partial}{\partial \theta} - \sin\phi\cot\theta\dfrac{\partial}{\partial \phi}\right) \\
		L_z &= -i\hbar\dfrac{\partial}{\partial \phi}
	\end{cases}
\end{align*}
$$
随即就能得到阶梯算符的解析形式：
$$
\begin{align*}
	L_\pm
    &= -i\hbar\left[(-\sin\phi \pm i\cos\phi)\frac{\partial}{\partial} - (\cos\phi \pm i\sin\phi)\cot\theta\frac{\partial}{\partial \phi}\right] \\
	&= -\hbar e^{\pm i\phi}\left(\frac{\partial}{\partial \theta} \pm i\cot\theta\frac{\partial }{\partial \phi}\right)
\end{align*}
$$
它们的积是：
$$
L_+L_- = -\hbar^2\left(\frac{\partial^2}{\partial \theta^2} + \cot\theta\frac{\partial}{\partial \theta} + \cot^2\theta\frac{\partial^2}{\partial\phi^2} + i\frac{\partial}{\partial \phi}\right)
$$

之后我们可以得到 $L^2$ 的解析形式：
$$
L^2 = -\hbar^2\left[\frac{1}{\sin\theta}\frac{\partial}{\partial\theta}\left(\sin\theta\frac{\partial}{\partial \theta}\right) + \frac{1}{\sin^2\theta}\frac{\partial^2}{\partial \phi^2}\right]
$$

根据上一节中得到的 $L^2$ 的特征值 $\hbar^2\ell(\ell + 1)$，因此：
$$
-\hbar^2\left[\frac{1}{\sin\theta}\frac{\partial}{\partial\theta}\left(\sin\theta\frac{\partial}{\partial \theta}\right) + \frac{1}{\sin^2\theta}\frac{\partial^2}{\partial \phi^2}\right]f_\ell^m = \hbar^2\ell(\ell + 1)f_\ell^m
$$

我们惊奇地发现，这个方程和此前的角方程 $Y(\theta, \phi)$ 完全一致；同时由于 $f_\ell^m$ 也是 $L_z$ 的特征函数，就有：
$$
-i\hbar\frac{\partial f_\ell^m}{\partial \phi} = \hbar mf_\ell^m
$$

这个形式和此前遇到的方位角方程 $\Phi(\phi)$ 有一样的形式。所以，角动量的特征函数就是我们已经解决了的球谐函数。
作为总结，球谐函数是 $L^2$ 和 $L_z$ 的特征函数；在求解 $Y_\ell^m(\theta, \phi)$ 的同时，我们同时得到了下面三个方程的特征函数：
$$
H\psi = E\psi \quad L^2\psi = \hbar\ell(\ell + 1)\psi \qquad L_z\psi = \hbar m\psi
$$

我们实际上可以利用 $L^2$ 来重写球坐标中的薛定谔方程：
$$
\frac{1}{2mr^2} \left[ -\hbar^2\frac{\partial}{\partial r} \left( r^2\frac{\partial}{\partial r} \right) + L^2 \right]\psi + V\psi = E\psi
$$

最后值得一提的是，球谐函数中我们要求 $m$ 是一个整数，但角动量特征函数中的 $m$ 可以一个半整数，因此半整数理应被舍弃才对。这个差别比较微妙，我们在下一节中会单独讲半整数情况下的重要意义。

### 自旋
在经典力学中，刚体只有两种角动量：质心运动或其绕着质心旋转。前者称为 **轨道的（Orbital）** 通过 $\mathbf{L} = \mathbf{r}\times\mathbf{p}$ 得到，后者则被称为 **自旋（Spin）**，通过 $\mathbf{S} = I\boldsymbol{\omega}$ 得到。比如地球沿轨道绕太阳做公转产生一年四季，绕质心自转则产生白昼黑夜。通常我们不认为两者有本质的不同，因为自转其实只是地球上的物质绕着轴旋转；但量子力学中完全类似的情形没法做这样的理解：比如电子绕中心轴旋转时，因为它没法进一步分解，我们只能将其理解为不同类型的自旋。因此，基本粒子的自旋展示出一种 **内在的（Intrinsic）**角动量，即自旋；反之，绕轨道的旋转是 **外在的（Extrinsic）**角动量，这也就是我们上一节中研究的 $\mathbf{L}$。
作为自旋的公理，下面的这些基本对易关系完全照搬自角动量：
$$
[S_x, S_y] = i\hbar S_z \qquad [S_y, S_z] = i\hbar S_x \qquad [S_z, S_x] = i\hbar S_y
$$

通过和此前类似的推导过程，我们可以得到：
$$
S^2|s\ m\rangle = \hbar^2s(s + 1)|s\ m\rangle
$$

这里我们使用狄拉克符号来表示自旋的特征状态，因为它不是一个函数。此外，定义 $S_\pm = S_x \pm iS_y$，可以得到和角动量几乎一致的结论：
$$
S^2|s\ m\rangle = \hbar s(s + 1)|s\ m\rangle \qquad S_z|s\ m\rangle = \hbar m|s\ m\rangle
$$

其中的 $s = 0, 1/2, 1, 3/2, \dots$ 而 $m = -s, -s + 1, \dots, s - 1, s$。此时由于特征状态不再是球谐函数，我们没有理由舍弃半整数解。
现实世界中，我们发现每个基本粒子都有唯一的自旋数 $s$。比如 $\pi$ 介子的自旋是 $0$、电子的自旋是 $1/2$、光子的自旋是 $1$、$\Delta$ 重子的自旋是 $3/2$、引力子的自旋是 $2$。相比之下，轨道角动量的量子数 $\ell$ 可以取任意的整数，且在同一个系统中会因为微扰发生改变。因此，自旋是一个相对简单的话题，这对我们来说是个好消息。
#### 1/2 自旋
最重要的自旋数是 s = 1/2，它是所有构成物质的常见粒子的自旋，如质子、中子和电子（还有所有的夸克和轻子）。在 1/2 自旋的系统中，只可能有两种状态，$|\frac{1}{2}\  \frac{1}{2}\rangle$ 和 $|\frac{1}{2}\ \text{\emph{-}}\frac{1}{2}\rangle$，分别称为 **上自旋（Spin Up）** 和 下自旋（Spin Down）。它们的矢量定义是：
$$
\chi_+ = \begin{pmatrix} 1 \\ 0 \end{pmatrix} \qquad \chi_- = \begin{pmatrix} 0 \\ 1 \end{pmatrix}
$$

它们两个扩张成的空间中任意的矢量以下的形式（称为 **旋量（Spinor）**）：
$$
\chi = \begin{pmatrix} a \\ b \end{pmatrix} = a\chi_+ + b\chi_-
$$

这个空间中的算符是一系列 $2\times 2$ 矩阵。下面让我们尝试得到 $S^2$ 和 $S_z$ 的矩阵形式。由特征方程，我们有：
$$
S_{\chi_+}^2 = \frac{3}{4}\hbar^2\chi_+ \qquad S_{\chi_-}^2 = \frac{3}{4}\hbar^2\chi_-
$$

现在设 $S^2$ 为任意的 $2\times 2$ 矩阵：
$$
S^2 = \begin{pmatrix} c & d \\ e & f \end{pmatrix}
$$

代入前面的两个式子有：
$$
\begin{align*} \begin{pmatrix} c & d \\ e & f \end{pmatrix} \begin{pmatrix} 1 \\ 0 \end{pmatrix} = \frac{3}{4}\hbar^2 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \implies \begin{pmatrix} c \\ e \end{pmatrix} = \begin{pmatrix} \frac{3}{4}\hbar^2 \\ 0 \end{pmatrix} \\ \begin{pmatrix} c & d \\ e & f \end{pmatrix} \begin{pmatrix} 0 \\ 1 \end{pmatrix} = \frac{3}{4}\hbar^2 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \implies \begin{pmatrix} c \\ e \end{pmatrix} = \begin{pmatrix} 0 \\ \frac{3}{4}\hbar^2 \end{pmatrix} \end{align*}
$$

因此：
$$
S^2 = \frac{3}{4}\hbar^2 \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}
$$

类似地由 $S_z$ 的特征方程：
$$
S_z\chi_+ = \frac{\hbar}{2}\chi_+ \qquad S_z\chi_- = \frac{\hbar}{2}\chi_-
$$

我们可以得到：
$$
S_z = \frac{\hbar}{2} \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$

最后由 $S_\pm$ 的特征方程：
$$
S_+\chi_- = \hbar\chi_+ \qquad S_-\chi_+ = \hbar\chi_- \qquad S_+\chi_+ = 0 \qquad S_-\chi_- = 0
$$
得到：
$$
S_+ = \hbar \begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix} \qquad S_- = \hbar \begin{pmatrix} 0 & 0 \\ 1 & 0 \end{pmatrix}
$$

根据 $S_\pm = S_x \pm iS_y$，我们就得到了另外两个方向上自旋算符的矩阵形式：
$$
S_x = \frac{\hbar}{2} \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} \qquad S_y = \frac{\hbar}{2} \begin{pmatrix} 0 & -i \\ i & \phantom{x}0 \end{pmatrix}
$$

发现三个方向的自旋矩阵有相同的系数，因此一个习惯的写法是令 $S = (\hbar/2)\sigma$，其中 $\sigma$ 的分量定义如下：
$$
\sigma_x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} \qquad \sigma_y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} \qquad \sigma_z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$

我们称它们为 **泡利自旋矩阵（Pauli Spin Matrices）**。和预想中一致地，$S_x$、$S_y$、$S_z$ 以及 $S^2$ 都是厄米特矩阵（因为它们都是可观测量），而 $S_\pm$ 并非厄米特矩阵。
如果我们要观测 $S_z$，因为它的特征旋量是 $\chi_+$ 和 $\chi_-$（对应特征值 $\frac{\hbar}{2}$ 和 $-\frac{\hbar}{2}$），所以我们分别有 $|a|^2$ 的概率和 $|b|^2$ 的概率得到 $\frac{\hbar}{2}$ 或 $-\frac{\hbar}{2}$。因为这是唯二的情形，我们有：
$$
|a|^2 + |b|^2 = 1
$$

这也暗示了旋量本身是归一化的，即 $\chi_\pm^\dagger\chi_\pm = 1$。
如果我们要观测 $S_x$，可以先计算它可能的特征值；不出意外地，我们得到的依然是 $\pm \frac{\hbar}{2}$：
$$
\begin{vmatrix} -\lambda & \hbar/2 \\ \hbar/2 & -\lambda \end{vmatrix} = 0 \implies \lambda^2 = \left(\frac{\hbar}{2}\right)^2 \implies \lambda = \pm \frac{\hbar}{2}
$$

然后，设特征旋量为 $(\alpha\ \ \beta)^T$，则：
$$
\frac{\hbar}{2} \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} \begin{pmatrix} \alpha \\ \beta \end{pmatrix} = \pm\frac{\hbar}{2} \begin{pmatrix} \alpha \\ \beta \end{pmatrix} \implies \begin{pmatrix} \beta \\ \alpha \end{pmatrix} = \pm \begin{pmatrix} \alpha \\ \beta \end{pmatrix}
$$

不难解得特征旋量为：
$$
\chi_+^x = \begin{pmatrix} 1/\sqrt{2} \\ 1/\sqrt{2} \end{pmatrix} \qquad \chi_-^x = \begin{pmatrix} 1/\sqrt{2} \\ -1/\sqrt{2} \end{pmatrix} 
$$

因此在 $S_x$ 空间中的旋量都可以写成：
$$
\chi = \frac{a + b}{\sqrt{2}}\chi_+^x + \frac{a - b}{\sqrt{2}}\chi_-^x
$$

测量得到 $\chi_+^x$ 和 $\chi_-^x$ 的概率分别为 $|a+b|^2/2$ 和 $|a-b|^2/2$（使用绝对值是因为这里的 $a$, $b$ 均为复数）。不难发现它们的和依然是 1，符合我们的认知。类似地，$S_y $的特征旋量是：
$$
\chi_+^y = \begin{pmatrix} 1/\sqrt{2} \\ i/\sqrt{2} \end{pmatrix} \qquad \chi_-^y = \begin{pmatrix} 1/\sqrt{2} \\ -i/\sqrt{2} \end{pmatrix}
$$

在 $S_y$ 空间的旋量都可以写为：
$$
\chi = \frac{a + ib}{\sqrt{2}}\chi_+^y + \frac{a - ib}{\sqrt{2}}\chi_-^y
$$

#### 磁场中的电子
带电粒子的自旋会产生 **磁偶极矩（Magnetic Dipole Moment）**：
$$
\boldsymbol{\mu} = \gamma\mathbf{S}
$$

其中 $\gamma$ 是 **旋磁比（Gyromagnetic Ratio）**。我们可以通过将传统的磁偶极矩（绕轴运动的电荷）的半径逼近为零来计算这个值：
$$
\boldsymbol{\mu} = I\mathbf{a} = \frac{qv}{2\pi r}\pi r^2\unit{a} = \frac{q}{2m}\mathbf{r}\times m\mathbf{v} = \frac{q}{2m}\mathbf{L}
$$

当 $r\to 0$ 时，我们认为上式依然满足，此时 $\mathbf{L}$ 变为 $\mathbf{S}$，因此 $\gamma = q/2m$。注意这只在非相对论情形下成立。
磁场中的磁偶极矩会受到一个力矩 $\boldsymbol{\mu}\times\mathbf{B}$，令其最终和磁场方向相同（我们在电动力学的第一篇笔记中讨论了这个情形）。这个力矩中的能量是（假设粒子静止不动，否则我们需要考虑动能，以及洛伦兹力的影响）：
$$
H = -\boldsymbol{\mu}\cdot\mathbf{B} = -\gamma \mathbf{B}\cdot\mathbf{S}
$$

因此这也是磁场中带电自旋粒子的哈密顿矩阵。$\mathbf{S}$ 则对应了 $S_x$、$S_y$、$S_z$ 三个方向上自旋中的一个。
下面我们重点讨论一个匀强磁场 $\mathbf{B}$ 中 1/2 自旋的粒子。假设磁场的方向是 $\unit{z}$：
$$
\mathbf{B} = B_0\unit{z}
$$

因此这个系统的哈密顿量是：
$$
H = -\gamma B_0S_z = -\frac{\gamma B_0\hbar}{2} \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$

其特征状态和 $S_z$ 完全一样（即 $\chi_\pm$），只不过对应的特征值是 $\mp (\gamma B_0 \hbar)/2$。当磁偶极矩和磁场平行时，达到最小能量。现在考虑时间相关的薛定谔方程：
$$
i\hbar \frac{\partial \chi}{\partial t} = H\chi
$$

其解为：
$$
\chi(t) = a\chi_+e^{-iE_+t/\hbar} + b\chi_-e^{-iE_-t/\hbar} = \begin{pmatrix} ae^{i\gamma B_0t/2} \\ be^{-i\gamma B_0t/2} \end{pmatrix}
$$

这里的 $a$、$b$ 需要满足 $|a|^2 + |b|^2 = 1$。因此我们实际上可以将它们简化为一个变量；设 $a = \cos(\alpha /2)$，则有：
$$
\chi(t) = \begin{pmatrix} \cos\frac{\alpha}{2}e^{i\gamma B_0t/2} \\ \sin\frac{\alpha}{2}e^{i\gamma B_0t/2} \end{pmatrix}
$$

如果尝试计算自旋三个分量的期望值：
$$
\begin{align*} \langle S_x\rangle &= \chi^\dagger(t)S_x\chi(t) = \phantom{x}\frac{\hbar}{2}\sin\alpha\cos(\gamma B_0 t) \\ \langle S_y\rangle &= \chi^\dagger(t)S_y\chi(t) = -\frac{\hbar}{2}\sin\alpha\sin(\gamma B_0 t) \\ \langle S_z\rangle &= \chi^\dagger(t)S_z\chi(t) = \phantom{x|}\frac{\hbar}{2}\cos\alpha \end{align*}
$$

可以猜测到 $\langle \mathbf{S}\rangle$ 和 z 轴倾斜 $\alpha$ 度角，且以一定频率进行进动，我们将其称为 **拉莫尔频率（Larmor Frequency）**：
$$
\omega = \gamma B_0
$$

这和经典力学中的行为是一致的。

### 氢原子

本节中我们将分析量子力学中一个典型的研究对象，氢原子。它包含一个相对静止，带有正电 $e$ 的原子核（我们将其置于原点）和一个轻得多的，带有负电 $-e$ 的电子围绕它运动。根据经典电磁学的库仑定律，电子的势能是：
$$
V(r) = -\frac{e^2}{4\pi\epsilon_0}\frac{1}{r}
$$
所以它的径向方程是：
$$
\begin{equation*}
	-\frac{\hbar^2}{2m_e}\frac{d^2u}{dr^2} + \left[-\frac{e^2}{4\pi\epsilon_0}\frac{1}{r} + \frac{\hbar^2}{2m_e}\frac{\ell(\ell + 1)}{r^2}\right]u = Eu
\end{equation*}
$$
我们将尝试用简谐振子的解析解法攻克这个方程，不过由于这种情况的复杂性，过程会比较曲折。

首先设：
$$
\kappa = \frac{\sqrt{-2m_eE}}{\hbar}
$$
其中 $E < 0$，因此可以看出我们采用的是离散的束缚态。此时径向方程变为：
$$
\frac{1}{\kappa^2}\frac{d^2u}{dr^2} = \left[1 - \frac{m_ee^2}{2\pi\epsilon_0\hbar^2\kappa}\frac{1}{\kappa r} + \frac{\ell(\ell + 1)}{(\kappa r)^2}\right]u
$$
这里设：
$$
\rho = \kappa r \qquad \rho_0 = \frac{m_ee^2}{2\pi\epsilon_0\hbar^2\kappa}
$$
就可以进一步简化为：
$$
\begin{equation*}
	\frac{d^2u}{d\rho^2} = \left[1 - \frac{\rho_0}{\rho} + \frac{\ell(\ell + 1)}{\rho^2}\right]u
\end{equation*}
$$
接下来，让我们猜测一下解析解的近似形式。由于当 $\rho \to \infty$ 时，上面方程右侧趋近于 $u$，因此我们可以假设通解在 $\rho$ 很大时满足（另一项 $e^\rho$ 会炸掉，因此系数取零）： 
$$
u(\rho) \sim Ae^{-\rho}
$$
当 $\rho \to 0$ 时，反比平方的项占主导，因此通解满足（另一项 $e^{-\ell}$ 会炸掉，因此系数取零）：
$$
u(\rho) \sim B\rho^{\ell + 1}
$$
我们接下来的任务就是将这两个近似糅合在一起。引入新的函数 $v(\rho)$：
$$
v(\rho) = \frac{1}{\rho^{\ell + 1}e^{-\rho}}u(\rho)
$$
此时 $u$ 的导数可以表示为 $v$ 的形式：
$$
\frac{du}{d\rho} = \rho^{\ell}e^{-\rho}\left[(\ell + 1 - \rho)v + \rho\frac{dv}{d\rho}\right] \\
\frac{d^2u}{d\rho^2} = \rho^{\ell}e^{-\rho}\left\{\left[-2(\ell + 1) + \rho + \frac{\ell(\ell + 1)}{\rho}\right]v + 2(\ell + 1 - \rho)\frac{dv}{d\rho} + \rho\frac{d^2v}{d\rho^2}\right\}
$$
将其代入径向方程：
$$
\begin{equation*}
	\rho\frac{d^2v}{d\rho^2} + 2(\ell + 1 - \rho)\frac{dv}{d\rho} + [\rho_0 - 2(\ell + 1)]v = 0
\end{equation*}
$$
现在，让我们假设其通解可以写为幂级数的形式：
$$
v(\rho) = \sum_{j=0}^\infty c_j\rho^j
$$
其导数为：
$$
\frac{dv}{d\rho} = \sum_{j=0}^\infty (j + 1)c_{j + 1}\rho^j \\
\frac{d^2v}{d\rho^2} = \sum_{j=0}^\infty j(j + 1)c_{j + 1}\rho^{j - 1}
$$
代到径向方程后让合并的幂级数系数为零，最终可以得到递推公式：
$$
c_{j+1} = \frac{2(j + \ell + 1) - \rho_0}{(j + 1)(j + 2\ell + 2)}c_j
$$
当 $\rho$ 很大时，$c_j$ 中 $j$ 更大的占主导，此时有：
$$
c_{j + 1} \approx \frac{2j}{j(j + 1)}c_j = \frac{2}{j + 1}c_j
$$
因此需要满足：
$$
c_j \approx \frac{2^j}{j!}c_0
$$
如果假设这就是准确的递推公式，则：
$$
v(\rho) = c_0\sum_{j=0}^\infty \frac{2^j}{j!}\rho^j = c_0e^{2\rho}
$$
推出原本 $u$ 的解：
$$
u(\rho) = c_0\rho^{\ell + 1}e^\rho
$$
呃，有点尴尬，这在 $\rho \to \infty$（我们这几步的前提）时直接炸掉了。因此 $c_j$ 必须在某个地方开始归零（和简谐振子的情况颇似）。假设 $c_N$ 是第一个归零的系数，观察递推公式不难得到：
$$
2(N + \ell) = \rho_0
$$
如果设 $n = N + \ell$，就可以通过 $n$ 来表示 $\rho_0$ 以及与其相联系的 $E$ 了：
$$
E = -\frac{\hbar^2\kappa^2}{2m_e} = -\frac{m_ee^4}{8\pi^2\epsilon_0^2\hbar^2\rho_0^2}
$$
根据 $n$ 的取值，我们得到离散的能量特征值：
$$
\begin{equation*}
	E_n = -\left[\frac{m_e}{2\hbar^2}\left(\frac{e^2}{4\pi\epsilon}\right)\right]\frac{1}{n^2} = \frac{E_1}{n^2} \qquad \text{其中 $n = 1, 2, 3, \dots$}
\end{equation*}
$$
这个公式被称为 **波尔公式（Bohr Formula）**，它是量子力学中最重要的结果。二十世纪初，波尔通过经典物理和尚不成熟的量子理论（当时还没有薛定谔方程！）推出了同样的公式。

我们记 **波尔半径（Bohr Radius）** 为：
$$
a = \frac{4\pi\epsilon_0\hbar^2}{m_ee^2} \approx 0.529 \times 10^{-10}\ \text{m}
$$
它和 $\rho$ 的关系是：
$$
\rho = \frac{r}{an}
$$
最终，我们得到的解（将球谐函数考虑进来）是：
$$
\begin{align*}
	\psi_{n\ell m}(r, \theta, \phi) 
		&= R_{n\ell}(r)Y_\ell^m(\theta, \phi) \\
		&= \frac{1}{r}\rho^{\ell + 1}e^{-\rho}v(\rho)Y_\ell^m(\theta, \phi) \\
		&= \frac{1}{r}\rho^{\ell + 1}e^{-\rho}\left(\sum_{j=0}^{n - \ell - 1} c_j\rho^j\right)Y_\ell^m(\theta, \phi)
\end{align*}
$$
这里面的三个参数 $n$、$\ell$、$m$ 分别被称为 **主量子数（Principal Quantum Number）**、**方位角量子数（Azimuthal Quantum Number）** 和 **磁量子数（Magnetic Quantum Number）**。第一个数和电子的能量有关（正如我们前面给出的能量特征值所述）；如果你足够敏锐，会发现后面这两个和电子的角动量有关（我们采用了完全相同的符号）。

电子的基态能量位于 $n = 1$ 的情形：
$$
E_1 = -\left[\frac{m_e}{2\hbar^2}\left(\frac{e^2}{4\pi\epsilon_0}\right)^2\right] = -13.6\ \text{eV}
$$
因此，氢原子的 **结合能（Binding Energy）**，也即将其电子从基态脱出的能量是 $13.6\ \text{eV}$。此时将波函数归一化后可以得到：
$$
\psi_{100}(r, \theta, \phi) = \frac{1}{\sqrt{\pi a^3}}e^{-r/a}
$$
电子的第一个激发态位于 $n = 2$ 的情形：
$$
E_2 = -3.4\ \text{eV}
$$
回忆球谐函数中的 $\ell$ 和 $m$ 的范围，此时 $\ell$ 可以是 $0$ 或 $1$，相对应地 $m$ 可以是 $0$ 或 $0$、$\pm 1$。事实上给定某个主量子数 $n$，对于每个可能的方位角量子数 $\ell = 0, 1, 2, \dots, n - 1$，都存在 $2\ell + 1$ 个可能的 $m$，其总共的退化数量是：
$$
d(n) = \sum_{\ell = 0}^{n - 1}(2\ell + 1) = n^2
$$
可以看到和无线方井相比，库仑势能会带有更多的退化情形。

下面让我们尝试给出 $v(\rho)$ 的非求和形式。事实上，数学中有简洁表示它的函数：
$$
v(\rho) = L_{n - \ell - 1}^{2\ell + 1}(2\rho)
$$
我们称 $L_q^p$ 为 **关联拉盖尔多项式（Associated Laguerre Polynomial）**，定义为：
$$
\begin{equation*}
	L_q^p(x) = (-1)^p\left(\frac{d}{dx}\right)^pL_{p+q}(x)
\end{equation*}
$$
这里的 $L_q$ 则是 **拉盖尔多项式（Laguerre Polynomial）**：
$$
\begin{equation*}
	L_q(x) = \frac{e^x}{q!}\left(\frac{d}{dx}\right)(e^{-x}x^q)
\end{equation*}
$$
下面罗列了 $q = 0$ 到 $q = 5$ 时拉盖尔多项式的值：
$$
\begin{align*}
	L_0(x) &= 1 \\
	L_1(x) &= -x + 1 \\
	L_2(x) &= \frac{1}{2}x^2 - 2x + 1 \\
	L_3(x) &= -\frac{1}{6}x^3 + \frac{3}{2}x^2 - 3x + 1 \\
	L_4(x) &= \frac{1}{24}x^4 - \frac{2}{3}x^3 + 3x^2 - 4x + 1 \\
	L_5(x) &= -\frac{1}{120}x^5 + \frac{5}{24}x^4 - \frac{5}{3}x^3 + 5x^2 - 5x + 1
\end{align*}
$$
下面是一张示意图：

<img src="graphs/qm1_4-2.png" alt="image-20220410114841258" style="zoom:50%;" />

最终，我们得到了下面（归一化后）的波函数：
$$
\begin{equation*}
	\psi_{n\ell m} = \sqrt{\left(\frac{2}{na}\right)^3\frac{(n - \ell - 1)!}{2n(n + \ell)!}}\ e^{-r/na}\left(\frac{2r}{na}\right)^\ell L_{n - \ell - 1}^{2\ell + 1}\left(\frac{2r}{na}\right)Y_\ell^m(\theta, \phi)
\end{equation*}
$$
虽然说实话，这个结果相当丑陋，但是这是现实世界中为数不多的可以完全描述系统的方程解。波函数中关于三个量子数都是正交的：
$$
\iint\psi_{n\ell m}^*\psi_{n'\ell'm'}r^2\,dr\,d\Omega = \delta_{nn'}\delta_{\ell\ell'}\delta_{mm'}
$$
通常，一个氢原子会固定在一个静止状态 $\Psi_{n\ell m}$ 中，但如果让它产生能量交换，比如和其它原子进行碰撞时，就会发生 **量子跃迁（Quantum Jump）**，从当前的状态转移到另一个状态。在这个过程中，放出的能量是通过 **光子（Photon）** 传递的。根据 **普朗克公式（Planck Formula）**，光子的能量和它的频率有关：
$$
E_\gamma = h\nu
$$
因此光子的波长 $\lambda$ 可以通过下面的式子得出：
$$
\frac{1}{\lambda} = \mathcal{R}\left(\frac{1}{n_f^2} - \frac{1}{n_i^2}\right)
$$
其中 $\mathcal{R}$ 是 **Rydberg 常数（Rydberg Constant）**，满足下面的 **Rydberg 公式（Rydberg Formula）**：
$$
\mathcal{R} = \frac{m_e}{4\pi c\hbar^3}\left(\frac{e^2}{4\pi\epsilon_0}\right)^2 = 1.097\times 10^7\ \text{m}^{-1}
$$
这个常数的值在 19 世纪就通过实验得到了，但是上面这个公式是波尔通过量子力学得到的。这某种程度上验证了量子力学的正确性。





## 相同粒子的系统

### 两个粒子的系统

此前我们研究的都是单独粒子的系统，其（在位置空间中）用波函数 $\Psi(\mathbf{r}, t)$ 描述。本章开始我们将探索一些多粒子的系统，其中最简单的便是双粒子系统，我们可以用两个位矢和一个时间参数来构建其波函数：
$$
\Psi(\mathbf{r}_1, \mathbf{r}_2, t)
$$
它依然满足时间相关的薛定谔方程：
$$
i\hbar\frac{\partial \Psi}{\partial t} = \hat{H}\Psi
$$
其中的哈密顿量 $\hat{H}$ 需要定义为整个系统的能量：
$$
\hat{H} = -\frac{\hbar^2}{2m_1}\nabla_1^2 - \frac{\hbar^2}{2m}\nabla_2^2 + V(\mathbf{r}_1, \mathbf{r}_2, t)
$$
这里我们用 $\nabla_1$ 和 $\nabla_2$ 分别表示两个粒子所在坐标系中的微分算符。此时，在某一微元体积 $d^3\mathbf{r}_1$ 中找到第一个粒子，在微元体积 $d^2\mathbf{r}_2$ 中找到第二个粒子的概率是：
$$
|\Psi(\mathbf{r}_1, \mathbf{r}_2, t)|^2\,d^3\mathbf{r}_1\,d^2\mathbf{r}_2
$$
为了保证归一化，需要有：
$$
\iint|\Psi(\mathbf{r}_1, \mathbf{r}_2, t)|^2\,d\mathbf{r}_1\,d\mathbf{r}_2 = 1
$$
如果已经得到了时间无关的解 $\psi(\mathbf{r}_1, \mathbf{r}_2)$，我们可以通过和此前一样的方式得到时间相关的解：
$$
\Psi(\mathbf{r}_1, \mathbf{r}_2, t) = \psi(\mathbf{r}_1, \mathbf{r}_2)e^{-iEt/h}
$$
这里的 $\psi$ 是下面时间无关方程的解：
$$
\frac{\hbar^2}{2m_1}\nabla_1^2\psi - \frac{\hbar^2}{2m_2}\nabla_2^2\psi + V\psi = E\psi
$$
其中 $E$ 是系统的总能量。这个方程很难解，不过我们可以处理一些特殊情况：

- 两个粒子之间没有相互作用：假设两个粒子没有相互作用，但它们都受到了外部力。比如它们可能各自连接到一根弹簧上，此时总势能为两者势能之和：
  $$
  V(\mathbf{r}_1, \mathbf{r}_2) = V_1(\mathbf{r}_1) + V_2(\mathbf{r}_2)
  $$
  我们依然用分离变量法解方程：
  $$
  \psi(\mathbf{r}_1, \mathbf{r}_2) = \psi_a(\mathbf{r}_1)\psi(\mathbf{r}_2)
  $$
  插入时间无关的薛定谔方程，得到一个方程组：
  $$
  -\frac{\hbar^2}{2m_1}\nabla_1^2 \psi_a(\mathbf{r}_1) + V_1(\mathbf{r}_1)\psi_a(\mathbf{r}_1) = E_a\psi_a(\mathbf{r}_1) \\
  -\frac{\hbar^2}{2m_2}\nabla_2^2 \psi_b(\mathbf{r}_2) + V_2(\mathbf{r}_2)\psi_b(\mathbf{r}_2) = E_b\psi_b(\mathbf{r}_2)
  $$
  根据 $E = E_a + E_b$，时间相关的解就是两个粒子各自时间相关解的积：
  $$
  \Psi(\mathbf{r}_1, \mathbf{r}_2, t) = \psi_a(\mathbf{r}_1)\psi_b(\mathbf{r}_2)e^{-i(E_a + E_b)/\hbar} = \left(\psi_a(\mathbf{r}_1)e^{-iE_at/\hbar}\right)\left(\psi_b(\mathbf{r}_2)e^{-iE_bt/\hbar}\right) = \Psi_a(\mathbf{r}, t)\Psi_b(\mathbf{r}_2, t)
  $$
  此时我们可以说，第一个粒子处于 $a$ 状态，且第二个粒子处于 $b$ 状态中。通解应该是所有这样的乘积只和，比如可能是：
  $$
  \Psi(\mathbf{r}_1, \mathbf{r}_2, t) = \frac{3}{5}\Psi_a(\mathbf{r}_1, t)\Psi_b(\mathbf{r}_1, t) + \frac{4}{5}\Psi_c(\mathbf{r}_1, t)\Psi_d(\mathbf{r}_2, t)
  $$
  这个波函数说明有 $9/25$ 的情况下，第一个粒子处于 $\Psi_a$ 且第二个粒子处于 $\Psi_b$；$16/25$ 的情况下，第一个粒子处于 $\Psi_c$ 且第二个粒子处于 $\Psi_d$。我们将这种相互关联的现象称为 **纠缠（Entanglement）**。处于纠缠态的两个粒子没有办法写成两个单粒子状态的积。一个经典的例子是两个 1/2 自旋粒子组成的系统。

- 有心势能。假设两个粒子只在相互之间存在作用，此时势能只和两者之间的径矢有关：
  $$
  V(\mathbf{r}_1, \mathbf{r}_2) = V(|\mathbf{r}_1 - \mathbf{r}_2|)
  $$
  氢原子就是一个例子（此时我们将质子的运动也考虑进来了）。此时我们可以用经典力学中类似的方法，将其变为一个等价的单体问题。

不过一般情况下，两个粒子之间以及外部都存在力，因此我们的分析会复杂很多。比如考虑一个氦原子，其中有两个电子，它们之间会相互排斥；同时它们都受到原子核的吸引：
$$
V(\mathbf{r}_1, \mathbf{r}_2) = \frac{1}{4\pi\epsilon_0}\left(-\frac{2e^2}{|\mathbf{r}_1|} - \frac{2e^2}{|\mathbf{r}_2|} + \frac{e^2}{|\mathbf{r}_1 - \mathbf{r}_2|}\right)
$$
我们会在本章后续解决这个问题。

#### 玻色子和费米子

假设有两个没有相互作用的粒子，一个处于 $\psi_a(\mathbf{r}_1)$ 状态，另一个处于 $\psi_b(\mathbf{r}_2)$ 状态。和经典力学不同的是，任意时刻看到这两个状态时，我们无法确定是哪个粒子处于其中的某个状态，我们只能说这两个粒子分别处于其中的一个状态。因此下面这个公式描述了所有可能的情形：
$$
\psi_\pm(\mathbf{r}_1, \mathbf{r}_2) = A[\psi_a(\mathbf{r}_1)\psi_b(\mathbf{r}_2) \pm \psi_b(\mathbf{r}_1)\psi_a(\mathbf{r}_2)]
$$
这实际上说明了有两种类型的粒子对：**玻色子（Boson）** 和 **费米子（Fermion）**，前者取加号，后者取减号。玻色子的特点是两个粒子是 **对称（Symmetric）** 的，即 $\psi_+(\mathbf{r}_2, \mathbf{r}_1) = \psi_+(\mathbf{r}_1, \mathbf{r}_2)$。相反地，费米子的两个粒子是 **反对陈（Antisymmetric）** 的，即 $\psi_-(\mathbf{r}_2, \mathbf{r}_1) = -\psi_-(\mathbf{r}_1, \mathbf{r}_2)$。

统计上我们发现，玻色子是所有拥有整数自旋的粒子，而费米子是所有拥有半整数自旋的粒子。这个 **自旋与统计的联系（Connection between Spin and Statistics）** 在考虑相对论效应的量子力学中可以证明出来，这里我们不深入说明，仅将其视作公理。玻色子和费米子的统计性质有很大的不同，其中一个重要的结论是，两个相同的费米子（比如电子）不能同时拥有相同的状态。这一点实际上很好验证，因为假如 $\psi_a = \psi_b$，则：
$$
\psi_-(\mathbf{r}_1, \mathbf{r}_2) = A[\psi_a(\mathbf{r}_1)\psi_a(\mathbf{r}_2) - \psi_a(\mathbf{r}_1)\psi_a(\mathbf{r}_2)] = 0
$$
此时就不存在任何波函数了。我们将这个效应称为 **泡利不相容原理（Pauli Exclusion Principle）**。

#### 粒子间的距离平方期望

本节让我们根据已经介绍过的三种情况（可区分的粒子、玻色子和费米子）计算 $\langle (x_1 - x_2)^2 \rangle$。显然它可以拆为：
$$
\langle (x_1 - x_2)^2 \rangle = \langle x_1^2 \rangle + \langle x_2^2 \rangle - 2\langle x_1x_2 \rangle
$$
 对于**可区分的粒子**，此时情况非常简单，因为我们可以将它们的波函数分为两个部分：
$$
\langle x_1^2 \rangle = \int x_1^2 |\psi_a(x_1)|^2\,dx_1\int |\psi_b(x_2)|^2\,dx_2 = \langle x^2 \rangle_a
$$
这里用到了单个粒子的期望和波函数的归一性。同样地，另一个粒子的距离平方期望是：
$$
\langle x_2^2 \rangle = \int |\psi_a(x_1)|^2\,dx_1 \int x_2^2 |\psi_b(x_2)|^2\,dx_2 = \langle x^2 \rangle_b
$$
同时：
$$
\langle x_1x_2 \rangle = \int x_1 |\psi_a(x_1)|^2\,dx_1 \int x_2 |\psi_b(x_2)|^2\,dx_2 = \langle x \rangle_a\langle x \rangle_b
$$
因此我们得到结果：
$$
\langle (x_1 - x_2)^2 \rangle = \langle x^2 \rangle_a + \langle x^2 \rangle_b - 2\langle x \rangle_a\langle x \rangle_b
$$
这个答案完全符合我们的期望，如果 $x_1$ 在 $\psi_b$ 状态，$x_2$ 在 $\psi_a$ 状态中，结果是完全一样的。

对于**不可区分的粒子**，即玻色子或费米子，由于它们的波函数只相差一个正负号，我们下面一同计算：
$$
\begin{align*}
	\langle x_1^2 \rangle 
		&= \frac{1}{2}\bigg[
			\int x_1^2 |\psi_a(x_1)|^2\,dx_1 \int |\psi_b(x_2)|^2\,dx_2 \\
			&\phantom{xxx}+ \int |\psi_a(x_1)|^2\,dx_1 \int x_2^2 |\psi_b(x_2)|^2\,dx_2 \\
			&\phantom{xxx}+ \int x_1^2 \psi_a^*(x_1)\psi_b(x_2)\,dx_1 \int \psi_b^*(x_2)\psi_a(x_1)\,dx_2 \\
			&\phantom{xxx}+ \int x_1^2 \psi_b^*(x_1)\psi_a(x_1)\,dx_1 \int \psi_a^*(x_1)\psi_b(x_2)\,dx_2
		\bigg] \\
		&= \frac{1}{2}\left(\langle x^2 \rangle_a + \langle x^2 \rangle_b\right)
\end{align*}
$$
这里用到了波函数的狄拉克标准正交性。对于另一个平方期望，其结果是一致的：
$$
\langle x_2^2 \rangle = \frac{1}{2}\left(\langle x^2 \rangle_a + \langle x^2 \rangle_b\right)
$$
两个位置乘积的期望要相对复杂一些：
$$
\begin{align*}
	\langle x_1x_2 \rangle
    	&= \frac{1}{2}\bigg[
            \int x_1 |\psi_a(x_1)|^2\,dx_1 \int x_2 |\psi_b(x_2)|^2\,dx_2 \\
            &\phantom{xxx}+ \int x_1 |\psi_b(x_1)|^2\,dx_1 \int x_2 |\psi_a(x_2)|^2\,dx_2 \\
            &\phantom{xxx}\pm \int x_1 \psi_a^*(x_1)\psi_b(x_1)\,dx_1 \int x_2 \psi_b^*(x_2)\psi_a(x_2)\,dx_2 \\
            &\phantom{xxx}\pm \int x_1 \psi_b^*(x_1)\psi_a(x_1)\,dx_1 \int x_2 \psi_a^*(x_2)\psi_b(x_2)\,dx_2
		\bigg] \\
		&= \frac{1}{2}\left(\langle x \rangle_a\langle x \rangle_b + \langle x \rangle_b\langle x \rangle_a \pm \langle x \rangle_{ab}\langle x \rangle_{ba} \pm \langle x \rangle_{ba} \langle x \rangle_{ab} \right) \\
		&= \langle x \rangle_a \langle x \rangle_b + |\langle x \rangle_{ab}|^2
\end{align*}
$$
其中 $\langle x \rangle_{ab}$ 是积分 $\int x \psi_a^*(x)\psi_b(x)\,dx$ 的简记。最后我们得到：
$$
\langle (x_1 - x_2)^2 \rangle_\pm = \langle x^2 \rangle_a + \langle x^2 \rangle_b - 2\langle x \rangle_a\langle x \rangle_b \mp 2|\langle x \rangle_{ab}|^2
$$
对比可区分粒子的情形相差了一个额外的项 $\mp 2|\langle x \rangle_{ab}|^2$。由此我们可以得出结论：相比可区分的两个粒子，玻色子之间的距离更小，而费米子之间的距离更大。仿佛有一种未知的力（我们称其为 **交换力（Exchange Force）**）让两个粒子相互吸引或排斥；不过这种力并不真实存在，它只是为了满足对称性而产生的一种几何现象。

#### 自旋

现在让我们考虑两个粒子系统中的自旋。假设两者分别处于 $|s_1\ m_1 \rangle$ 和 $|s_2\ m_2\rangle$ 状态，记整个系统的状态为 $|s_1 \ s_2\ m_1\ m_2\rangle$，此时有（回忆我们上一章的结论）：
$$
\begin{align*}
	(S^1)^2 |s_1\ s_2\ m_1\ m_2 \rangle &= \hbar^2 s_1(s_1 + 1) |s_1\ s_2\ m_1\ m_2 \rangle \\
	(S^2)^2 |s_1\ s_2\ m_1\ m_2 \rangle &= \hbar^2 s_2(s_2 + 1) |s_1\ s_2\ m_1\ m_2 \rangle \\
	S_z^1 |s_1\ s_2\ m_1\ m_2 \rangle &= \hbar m_1 |s_1\ s_2\ m_1\ m_2 \rangle \\
	S_z^2 |s_1\ s_2\ m_1\ m_2 \rangle &= \hbar m_2 |s_1\ s_2\ m_1\ m_2 \rangle
\end{align*}
$$
考虑整个系统的角动量，有：
$$
\begin{align*}
	S_z |s_1\ s_2\ m_1\ m_2 \rangle 
		&= \hbar m |s_1\ s_2\ m_1\ m_2 \rangle \\
		&= (S_z^1 + S_z^2) |s_1\ s_2\ m_1\ m_2 \rangle \\
		&= \hbar(m_1 + m_2)
\end{align*}
$$
因此有 $m = m_1 + m_2$。相比之下，$s$ 的取值会复杂很多，让我们先从一个 $1/2$ 自旋的双粒子系统开始，我们可以轻易地列出下面的四种情况：
$$
\begin{align*}
	& |\uparrow \uparrow\,\rangle = \left|\frac{1}{2}\ \frac{1}{2}\ \frac{1}{2}\ \frac{1}{2}\right\rangle \qquad && m = 1 \\
	& |\uparrow \downarrow\,\rangle = \left|\frac{1}{2}\ \frac{1}{2}\ \frac{1}{2}\ \frac{-1}{2}\right\rangle && m = 0 \\
	& |\downarrow \uparrow\,\rangle = \left|\frac{1}{2}\ \frac{1}{2}\ \frac{-1}{2}\ \frac{1}{2}\right\rangle && m = 0 \\
	& |\downarrow \downarrow\,\rangle = \left|\frac{1}{2}\ \frac{1}{2}\ \frac{-1}{2}\ \frac{-1}{2}\right\rangle && m = -1
\end{align*}
$$
我们此前讨论过，$m$ 的取值应该是介于 $\pm s$ 之间的整数，每个值对应了一种状态，但这里有两种 $m = 0$ 的情况，应该如何考虑呢？我们可以尝试对 $|\uparrow\uparrow\rangle$ 使用下降算符：
$$
\begin{align*}
	S_- |\uparrow \uparrow\,\rangle
		&= \left(S_-^1|\uparrow\,\rangle\right)|\uparrow\,\rangle + |\uparrow\,\rangle\left(S_-^2|\uparrow\,\rangle\right) \\
		&= \hbar|\downarrow\,\rangle |\uparrow\,\rangle + |\uparrow\,\rangle \hbar|\downarrow\,\rangle \\
		&= \hbar(|\uparrow\downarrow\,\rangle + |\downarrow\uparrow\,\rangle)
\end{align*}
$$
可见当 $s = 1$ 时，我们可以得到下面的三个状态：
$$
\left\{
\begin{split}
	&|1\ 1\rangle & &= |\uparrow\uparrow\,\rangle \\
	&|1\ 0\rangle & &= \frac{1}{\sqrt{2}}(|\uparrow\downarrow\,\rangle + |\downarrow\uparrow\,\rangle) \\
	&|1\ -1\rangle & &= |\downarrow\downarrow\,\rangle
\end{split}
\right\} \qquad s = 1
$$
对于 $s = 0$ 的情形，我们有：
$$
|0\ 0\rangle = \frac{1}{\sqrt{2}}(|\uparrow\downarrow\,\rangle - |\downarrow\uparrow\,\rangle) \qquad s = 0
$$
不难验证 $S_-|0\ 0\rangle = S_+|0\ 0\rangle = 0$。至此我们得到了 $1/2$ 自旋的双粒子系统总自旋可能是 $1$ 或 $0$，其由两个粒子所处的状态决定。敏锐的同学可能已经发现，$s = 1$ 的系统是对称的（即，交换两个粒子的自旋方向不改变结果），而 $s = 0$ 的系统是反对称的；然而考虑到系统整体的状态应该通过空间（借用波函数 $\psi$ 的记号）和方向（借用旋量 $\chi$ 的记号）同时描述，即：
$$
\psi(\mathbf{r}_1, \mathbf{r}_2)\chi(1, 2)
$$
且显然这个量应该是反对称的：
$$
\psi(\mathbf{r}_1, \mathbf{r}_2)\chi(1, 2) = -\psi(\mathbf{r}_2, \mathbf{r}_1)\chi(2, 1)
$$
因此 $s = 1$ 时的空间函数是反对称的，而 $s = 0$ 时的空间函数是对称的。
