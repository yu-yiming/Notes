# Linear Programming

本笔记是对应 *UIUC MATH 482 Linear Programming* 的学习笔记，其中包含了线性优化相关的算法设计与证明。参考资料主要是讲师 *[Mikhail Lavrov](https://faculty.math.illinois.edu/~mlavrov/)* 提供的讲座笔记，大部分图形和例子都来自于该笔记（可以从[这里](https://faculty.math.illinois.edu/~mlavrov/courses/482-spring-2020.html)找到原笔记）。本课的前置课程是线性代数。

[TOC]

## 线性优化概述

**线性优化（Linear Programming）** 指的是让计算机对一个线性的系统进行优化求值，下面是一个 **线性优化问题（Linear Program）**的例子：
$$
\begin{align*}
	\underset{x, y \in \R}{\text{maximize}} \quad & x - y \\
	\text{subject to} \quad & y \le 3\\
	& y \ge 2x - 5 \\
	& x + y \ge 1
\end{align*}
$$
上面的问题中，出现了两个重要的部分。其一是 **目标函数（Objective Function）**，即我们需要优化的线性多项式，它的值称为 **目标值（Objective Value）**；另一个是 **约束（Constraint）**，即下面列出的多个不等式（约束也可以包含等式）。经过重排列，我们可以将上面的约束记为矩阵的形式：
$$
A\mathbf{x} \le \mathbf{b} \\
\text{其中}\quad A = \begin{bmatrix} 0 & 1 \\ 2 & -1 \\ -1 & -1 \end{bmatrix}, 
\mathbf{x} = \begin{bmatrix} x \\ y \end{bmatrix}, \nonumber
\mathbf{b} = \begin{bmatrix} 3 \\ 5 \\ 1 \end{bmatrix}
$$
我们将所有满足上面式子的点集 $\{ \mathbf{x} \mid A\mathbf{x} \le \mathbf{b}\}$ 称为 **可行域（Feasible Region）**。可行域中每一个点都被称为 **可行解（Feasible Solution）**。如果将目标函数也改写成矩阵的形式，我们就得到了一个比较通用的线性优化问题：
$$
\begin{align*}
\underset{x, y \in \R}{\text{maximize}}& \quad \mathbf{c}^\text{T}\mathbf{x} \\
\text{subject to}& \quad A\mathbf{x} \le \mathbf{b} \\\\
\text{其中} \quad &\mathbf{c} = \begin{bmatrix} 1 \\ -1 \end{bmatrix}
\end{align*}
$$
如果我们找到了这个最大值（或最小值）$\mathbf{x}$，我们称其为这个线性优化问题的 **最优解（Optimized Solution）**。在上面的例子中，最优解是 $(x, y) = (2, -1)$。我们暂时不给出它的证明。需要注意的是，*不是所有* 线性优化问题都有唯一的最优解。在一些情况中，我们可能会遇到比较特殊的解。

### 异常的线性优化问题

**异常的线性优化问题（Misbehaving Linear Program）** 存在下面几种情况：

1. 最优解并非唯一解
2. 最优解不存在，因为目标函数可以是任意大（任意小）的
3. 最优解不存在，因为可行域为空

下面给出例子：
$$
\begin{align*}
	\underset{x, y \in \R}{\text{maximize}} \quad & y \\
	\text{subject to} \quad & y \le 3\\
	& y \ge 2x - 5 \\
	& x + y \ge 1
\end{align*}
$$
此时不难发现， $y$ 只要等于 $3$，就能够得到最优解，$x$ 可以是 $[-2, 4]$ 中的任意实数。此时有无穷多的最优解。
$$
\begin{align*}
	\underset{x, y \in \R}{\text{maximize}} \quad & x - y \\
	\text{subject to} \quad & y \le 3\\
	& x + y \ge 1
\end{align*}
$$
此时甚至不存在最优解，因为 $x$ 可以任意大，导致目标函数上不有界。
$$
\begin{align*}
	\underset{x, y \in \R}{\text{maximize}} \quad & x - y \\
	\text{subject to} \quad & y \le -3\\
	& y \ge 2x - 5 \\
	& x + y \ge 1
\end{align*}
$$
此时找不到任何满足约束的点，因此可行域为空，不存在最优解。当一个线性优化问题出现上面这些异常时，我们称其为 **不一致的（Inconsistent）** 的线性优化问题。

### 简单的线性优化算法

让我们介绍一个初等的线性优化问题解法。注意到所有的图像都对应了一条直线（两个变量时）和一个区域。因此我们可以通过函数图像来得到线性优化问题的可行域。对于开头给出的：
$$
\begin{align*}
	\underset{x, y \in \R}{\text{maximize}} \quad & x - y \\
	\text{subject to} \quad & y \le 3\\
	& y \ge 2x - 5 \\
	& x + y \ge 1
\end{align*}
$$
我们可以得到以下的图像：

<img src="graphs/lp1.png" alt="lp1" style="zoom:30%;" />

这三个点包围的区域就是我们要求的可行域了。如果把目标函数也视作直线，那就是 $x - y = C$ 这样的无数条直线的集，它和上面给出的可行域需要拥有交集且满足 $C$ 最大。这里我们直接给出要点：线性优化问题的最优解一定出现在 **顶点（Vertex）** 集当中，即所有直线交集的集合。因此我们只需要将这些交点代入目标函数就能得到最优解了。上例中 $2 - (-1) = 3$ 是最大值，因此 $(2, -1)$ 是最优解。

对于多个变量多个方程的线性优化问题，我们都可以通过这个方式得到最优解。但是之所以称其为 **简单的（Naive）** 线性优化算法，是因为它很好操作，却过于低效。考虑一个拥有 50 个变量、100 个不等式组成的约束，我们需要计算 $C_{50}^{100}\approx 10^{29}$ 次目标函数才能比较出最优解，这个数量级甚至不能被超级计算机承受，因此现实中我们不会用这种方法考虑较为复杂的线性优化问题。本篇的重点也即是，如何用更高效的方法解决线性优化问题。

### 线性优化问题的诸多公式化表示

在深入开始讨论线性优化问题之前，让我们先明确一些共识：

#### 非负数的约束变量

我们几乎总是默认所有的约束变量（约束中出现的变量）都是非负的，也即 $\mathbf{x} \ge 0$。其原因主要是：

- 现实生活中很多约束变量本来就不会出现负数情形。
- 在数学上非负数一些美好的性质（比如其拥有下确界 $0$）。
- 当所有约束变量都为正数时，我们可以保证所有最优解都出现在顶点中（这一点我们不作更多解释）。

因此，我们在此可以给出线性优化问题的 **标准形式（Canonical Form）**：
$$
\begin{align*}
\underset{\mathbf{x} \in \R^n}{\text{maximize}} \quad & \mathbf{c}^\text{T}\mathbf{x} \\
	\text{subject to} \quad & A\mathbf{x} \le \mathbf{b} \\
	& \mathbf{x} \ge 0
\end{align*} \tag{1}
$$
这里虽然最优解要求目标函数取最大值，但其实对于要求最小值的情形，只需令 $\mathbf{c}' = -\mathbf{c}$，就可以将问题转换为求最大值的线性优化问题。

如果题目没有给出标准形式，比如并不要求约束变量都为非负数，我们可以对每一个约束变量 $x_i$ 引入变量 $x_i^+, x_i^- \ge 0$ 使得 $x = x_i^+ - x_i^-$，然后将问题转换为：
$$
\begin{align*}
\underset{\mathbf{x}' \in \R^{2n}}{\text{maximize}} \quad & \mathbf{c}^\text{T}\mathbf{x}' \\
	\text{subject to} \quad & A\mathbf{x}' \le \mathbf{b} \\
	& \mathbf{x}' \ge 0
\end{align*} \tag{2}
$$
此时可能会出现有无数组最优解的情形，但我们依然能够从中得到原问题的最优解。

举个例子：
$$
\begin{align*}
	\underset{x, y \in \R}{\text{maximize}} \quad & x - y \\
	\text{subject to} \quad & y \le 3\\
	& y \ge 2x - 5 \\
	& x + y \ge 1
\end{align*}
$$
我们可以引入 $x^+, x^-, y^+, y^-$ 四个变量，然后将问题转换为：
$$
\begin{align*}
	\underset{x^+, x^-, y^+, y^- \in \R}{\text{maximize}} \quad & x^+ - x^- - y^+ + y^- \\
	\text{subject to} \quad & y^+ - y^- \le 3\\
	& 2x^+ - 2x^- - y^+ + y^- \le 5 \\
	& -x^+ + x^- - y^+ + y^- \le -1 \\
	& x^+, x^-, y^+, y^- \ge 0
\end{align*}
$$


#### 线性优化问题的等式形式

不等式问题解决起来并不是那么方便。实际上，我们可以通过引入 **松弛变量（Slack Variable）** 来将约束中的不等式变为等式，这是解决线性优化问题时极为常用的工具：
$$
\begin{align*}
\underset{\mathbf{x} \in \R^n}{\text{maximize}} \quad & \mathbf{c}^\text{T}\mathbf{x} \\
	\text{subject to} \quad & A\mathbf{x} = \mathbf{b} \\
	& \mathbf{x} \ge 0 \\
	\text{其中} \quad &\mathbf{x}^\text{T} = \begin{bmatrix} x_1,...,x_n,s_1,...s_m \end{bmatrix}
\end{align*} \tag{3}
$$
还是前面那个例子，我们可以引入松弛变量 $s_1, s_2, s_3$ 使其变为：
$$
\begin{align*}
	\underset{x, y \in \R}{\text{maximize}} \quad & x^+ - x^- - y^+ + y^- \\
	\text{subject to} \quad & y^+ - y^- + s_1 = 3\\
	& 2x^+ - 2x^- - y^+ + y^- + s_2 = 5 \\
	& -x^+ + x^- - y^+ + y^-  + s_3 = -1 \\
	& x^+, x^-, y^+, y^- \ge 0
\end{align*}
$$
这也是我们下文马上要介绍的 **简单形法（Simplex Method）** 采用的线性优化问题的最初形式。



## 简单形法

**简单形法（Simplex Method）** 可以描述为：从线性优化问题的一个可行解线性变换为另一个可行解，每次都使其得到优化（或至少不恶化），直到达到最优解。这个变换过程遵循了中矩阵的 **行运算（Row Operation）**以及 **高斯消元法（Gaussian Elimination）**，比如下面的方程：
$$
\begin{cases}
3x + y = 6 \\
x - y = -2
\end{cases}\nonumber
$$
可以写成矩阵的形式：
$$
A\mathbf{x} = \mathbf{b} \\
A = \begin{bmatrix} 3 & 1 \\ 1 & -1 \end{bmatrix}, \
\mathbf{x} = \begin{bmatrix} x \\ y \end{bmatrix}, \
\mathbf{b} = \begin{bmatrix} 6 \\ -2 \end{bmatrix}\nonumber
$$
我们将 $A$ 和 $\mathbf{b}$ 合并为一个 **增广矩阵（Augmented Matrix）**，并对其进行行运算：
$$
\begin{bmatrix}
	3 & 1 & 6 \\
	1 & -1 & -2
\end{bmatrix} \leadsto
\begin{bmatrix}
	1 & 0 & 1 \\
	0 & 1 & 3
\end{bmatrix}\nonumber
$$
当增广矩阵变为右侧这样每一行都有特殊的一项值为 $1$，且其所在列的所有其它元素都为 $0$ 的形式时，我们不难发现其给出了方程的一组解：$x = 1, y = 3$。在线性优化问题中，我们通常不能得到所有约束变量的解。考虑下面这个方程组：
$$
\begin{cases}
	3x + y + 5z = 6 \\
	x - y + 3z = -2
\end{cases}\nonumber
$$
我们同样通过行运算得到：
$$
\begin{bmatrix}
	1 & 0 & 2 & 1 \\
	0 & 1 & -1 & 3
\end{bmatrix}\nonumber
$$
这个方程没有唯一解，因为其中 $z$ 的值可能是任意的，而 $x, y$ 的值依赖于 $z$。我们可以将上面的矩阵写成方程的形式：
$$
\begin{cases}
	x = -2z + 1 \\
	y = -z + 3
\end{cases} \nonumber
$$
此时 $x, y$ 仿佛得到了唯一解，我们称它们为 **基本变量（Basic Variable）**，而 $z$ 被称为 **非基本变量（Non-Basic Variable）**。显然，如果设 $z = 0$，就能得到一组解 $x = 1, y = 3$，我们它们为方程的  **基本解（Basic Solution）**。当我们选取不同基本变量时，可以得到不同的基本解。比如上面的方程中将 $y, z$ 作为基本变量时，我们可以得到基本解 $y = \frac{7}{2}, z = \frac{1}{2}$。

### 规则简述

简单形法就是从一组基本变量开始，每次尝试改变基本变量以提升基本解，直到达到最优解。这个方法给予下面两个规则：

- 可行域的顶点均为线性方程的基本解，在线性优化问题中也被称为 **基本可行解（Basic Feasible Solution）**。
- 可行域的顶点中一定存在线性优化问题的一个最优解。

整体来看，简单形法的步骤包括下面几步：

- 从一个基本可行解开始。
- 让一个非基本变量进入 **基（Basis）**，即成为基本变量。这个变量的选择基于某种 **基准规则（Pivoting Rule）**。
- 让一个基本变量离开基，其选择保证 $\text{初始值}/\text{减少率}$ 在所有可选项中最小且为正数。如果有多个最小比值，则可能需要基准规则来指定让某个候选的变量离开。

这里讲的东西让人有点云里雾里。马上我们会用例子来展示简单形法的使用方法，并在之后解释这个方法的原理。

### 简单形表和规则阐述

在对简单形法举例说明之前，我们先引入 **简单形表（Simplex Tableau）** 这个方便于记录方程组、基本变量以及目标函数信息的工具。比如下面这个已经得到一组基本可行解的线性优化问题：
$$
\begin{align*}
\underset{x \in \R^5}{\text{maximize}} \quad & x_1 + x_2 + x_3 + x_4 + x_5 \\
\text{subject to} \quad & x_1 + 2x_4 + x+5 = 1 \\
& x_2 - 3x_4 + x_5 = 2 \\
& x_3 + x_4 - 3x_5 = 3 \\
& x_1, x_2, x_3, x_4, x_5 \ge 0
\end{align*}
$$
我们可以将其等价写成以下的简单形表：
$$
\begin{array}{cccccc|c}
	 & x_1 & x_2 & x_3 & x_4 & x_5 & \\\hline
	 x_1 & 1 & 0 & 0 & 2 & 1 & 1 \\
	 x_2 & 0 & 1 & 0 & -3 & 1 & 2 \\
	 x_3 & 0 & 0 & 1 & 1 & -3 & 3 \\\hline
	 -z & 0 & 0 & 0 & 1 & 2 & -6
\end{array} \nonumber
$$
可以看到，这个表格中中间的五列给出了首行中每个变量对应的系数，最左侧一列则是给出了每个方程中的基本变量。最右侧一列（除去最后一行）则是基本变量的取值，它们总是一个正数。最下面一行是方程的目标函数，满足 $-z +x_4 + 2x_5 = -6$。这点可以通过设 $z = x_1 + x_2 + x_3 + x_4 + x_5$ 并进行简单的行变换得到。不难发现令非基本变量为 $0$ 后就能得到这个表对应的基本可行解 $6$，因此每个简单形表右下角取负即是一个基本可行解对应的目标值，我们的目标就是每次进行行变换都让这个数变得更小，以达到最大值。

不过我们看到 $z = 6 + x_4 + 2x_5$ 时，显然 $z$ 不应该止步于 $6$。当 $x_4, x_5$ 取某个整数时，都能得到更大的值。因此我们需要进行行变换，直到 $-z$ 列中 $x_1, x_2, x_3, x_4, x_5$ 列中不存在任何正数，此时 $z = n - ax_i - bx_j$。由于 $x_i, x_j \ge 0$，我们就得到了最大值 $n$。

此时我们先不管基准规则，假设我们需要让 $x_4$ 进入基，也就是让 $x_4$ 这一列中除了某一行为 $1$，其它都需要变为 $0$。接下来需要让 $x_1, x_2, x_3$ 中一个变量离开基。对比发现 $x_1$ 行相比 $x_3$ 行有 $1/2 < 3/1$，因此应该让 $x_1$ 离开（$x_2$ 不适用，因为它在 $x_4$ 这一列的系数为负）。这样我们就能得到一个新的简单形表：
$$
\begin{array}{cccccc|c}
	 & x_1 & x_2 & x_3 & x_4 & x_5 & \\\hline
	 x_4 & 1/2 & 0 & 0 & 1 & 1/2 & 1/2 \\
	 x_2 & 3/2 & 1 & 0 & 0 & 5/2 & 7/2 \\
	 x_3 & -1/2 & 0 & 1 & 0 & -7/2 & 5/2 \\\hline
	 -z & -1/2 & 0 & 0 & 0 & 3/2 & -13/2
\end{array} \nonumber
$$
可以看到我们确实让目标函数的值更加优化了，不过它还可以更好。让 $x_5$ 进入基，然后让 $x_4$ 离开：
$$
\begin{array}{cccccc|c}
	 & x_1 & x_2 & x_3 & x_4 & x_5 & \\\hline
	 x_5 & 1 & 0 & 0 & 2 & 1 & 1 \\
	 x_2 & -1 & 1 & 0 & -5 & 0 & 1 \\
	 x_3 & 3 & 0 & 1 & 7 & 0 & 6 \\\hline
	 -z & -2 & 0 & 0 & -3 & 0 & -8
\end{array} \nonumber
$$
这样我们就得到了目标函数的最大值 $z = 8$，因为此时目标函数中所有非基本变量的系数都是负数，我们不可能进一步优化这个表了。此时我们知道最优解为：$x_1 = 0, x_2 = 1, x_3 = 6, x_4 = 0, x_5 = 1$。这样我们就完成了简单形法。

等下，我们似乎还没有解释为什么每次都让比值最低且为正的基本变量离开基。这其实一切都是为了让表中右下角的值越小越好（为了达到最优解）。当基本变量 $x_i$ 在准备进入基的 $x_j$ 这列的系数为负数时，我们需要加上 $-z$ 行的值（注意这行我们选的总是正数）才能让这一项变为 $0$；因为 $x_i$ 的基本解总是正数，我们会对右下角这一项加上一个正数，也就是让基本可行解变得更小了，这是违背我们的期望的。当 $x_i$ 在 $x_j$ 这列的系数是正数时，我们希望 $b_i/x_i$ 的比值更低，这是因为为了保证满足给定的约束，我们不能让 $x_i$ 从基中退出（变为 $0$）时，$x_j$ 的基本解让其它的基本变量小于零。比如上面这个例子在最开始的状态，我们有（设 $x_5 = 0$ 因为它是和这次变换无关的非基本变量）：
$$
\begin{cases}
	x_1 = 1 - 2x_4 \\
	x_2 = 2 + 3x_4 \\
	x_3 = 3 - x_4
\end{cases} \nonumber
$$
抛开 $x_2$ 不谈（因为它在表中 $x_4$ 这一列的系数是负数），如果我们选择比值更大的 $x_3$ 离开基，其值将变为 $0$。此时 $x_4 = 3$，而代入第一个等式我们得到 $x_1 = -5$，这违反了所有约束变量都是非负数的规则。因此我们始终需要让比值最小的基本变量（此处为 $x_1$）离开基，这样其它基本变量的取值不会出现问题。

上面的例子是求最大值。如果求最小值，则需要让 $-z$ 行中所有正数列都进入基，最后得到 $z = n + \sum a_ix_i$ 的形式。因为 $x_i \ge 0$，所以我们得到最小值 $n$。

### 更多的例子

让我们再演示一个需要引入松弛变量的例子：
$$
\begin{align*}
\underset{x, y \in \R}{\text{maximize}} \quad & 2x + 3y \\
\text{subject to} \quad & -x + y \le 3 \\
&  x - 2y \le 2 \\
& x + y \le 7 \\
& x, y \ge 0
\end{align*}
$$
之前我们介绍了松弛变量将不等式组变为等式方程组，因此我们可以将线性优化问题变为下面的形式：
$$
\begin{align*}
\underset{x, y \in \R}{\text{maximize}} \quad & 2x + 3y \\
\text{subject to} \quad & -x + y + s_1 = 3 \\
&  x - 2y + s_2 =  2 \\
& x + y + s_3 =  7 \\
& x, y, s_1, s_2, s_3 \ge 0
\end{align*}
$$
它可以立即转换为简单形表：
$$
\begin{array}{cccccc|c}
	& x & y & s_1 & s_2 & s_3 & \\\hline
	s_1 & -1 & 1 & 1 & 0 & 0 & 3 \\
	s_2 & 1 & -2 & 0 & 1 & 0 & 2 \\
	s_3 & 1 & 1 & 0 & 0 & 1 & 7 \\\hline
	-z & 2 & 3 & 0 & 0 & 0 & 0 
\end{array}\nonumber
$$
可以看到，引入松弛变量的一个好处在于，方程一上来就能得到基本可行解 $(3, 2, 7)$，不过显然这并不是优解，因为 $-z$ 行中还有正数。首先可以引入 $y$ 到基中，然后令 $s_1$ 离开：
$$
\begin{array}{cccccc|c}
	& x & y & s_1 & s_2 & s_3 & \\\hline
	y & -1 & 1 & 1 & 0 & 0 & 3 \\
	s_2 & -1 & 0 & 2 & 1 & 0 & 8 \\
	s_3 & 2 & 0 & -1 & 0 & 1 & 4 \\\hline
	-z & 5 & 0 & -3 & 0 & 0 & -9 
\end{array}\nonumber
$$
接下来将 $x$ 引入并让 $s_3$ 离开：
$$
\begin{array}{cccccc|c}
	& x & y & s_1 & s_2 & s_3 & \\\hline
	y & 0 & 1 & 1/2 & 0 & 1/2 & 5 \\
	s_2 & 0 & 0 & 3/2 & 1 & 1/2 & 10 \\
	x & 1 & 0 & -1/2 & 0 & 1/2 & 2 \\\hline
	-z & 0 & 0 & -1/2 & 0 & -5/2 & -19 
\end{array}\nonumber
$$
这样我们就得到了最优解 $x = 2, y  = 5$，此时目标函数为 $19$。

### 简单形法的几何意义

现在让我们将上面这个例子的几何图形展示出来。最一开始时，我们的基本可行解是 $x = 0, y = 0$，也即原点：

<img src="graphs/lp2.png" alt="lp2" style="zoom:50%;" />

当我们决定将 $y$ 变为基本变量时，我们相当于沿着 $y$ 轴向上出发，直到 $s_1 = 0$（图中并没有显示出 $s_1, s_2, s_3$ 的数轴，这是因为我们没有办法画出五维的图形；但是它们取 $0$ 的区域在图中可行域的边界展示出来了）：

<img src="graphs/lp3.png" alt="lp3" style="zoom:50%;" />

此时经过检查发现目标函数并不是最大值，因此我们决定将 $x$ 引入基，并让 $s_3$ 离开。沿着可行域的边界向右上走，直到 $s_3 = 0$：

<img src="graphs/lp4.png" alt="lp4" style="zoom:50%;" />

这里我们就得到了最优解 $(2, 5)$。至此可以看到，简单形法实际上就是从可行域的一个顶点出发，经历边到达一个又一个其它的顶点，直到得到最优解。因此，每次选取的方向比较重要：比如本例中，如果一开始我们决定引入 $x$ 而不是 $y$，就不得不经历三次简单形表的变换才能到达最优解 $(2, 5)$。有没有什么方法能够提前知道最好的选取方式，防止走远路呢？

坏消息是，没有。更坏的消息是，在一些例子中，可能会出现原地打转的情况（多次变换之后回到了遍历过的某个可行解）。唯一的好消息是我们有方法避免原地打转，这个方法就是之前提到过的 **基准规则**。我们将在之后详细介绍它。

#### 无界的线性优化问题

上面的例子中，如果我们去除最后一个约束，也即将问题变为如下：
$$
\begin{align*}
\underset{x, y \in \R}{\text{maximize}} \quad & 2x + 3y \\
\text{subject to} \quad & -x + y \le 3 \\
&  x - 2y \le 2 \\
& x, y \ge 0
\end{align*}
$$
我们在引入 $y$ 后，简单形表会变为下面的样子：
$$
\begin{array}{ccccc|c}
	& x & y & s_1 & s_2 & \\\hline
	y & -1 & 1 & 1 & 0 & 3 \\
	s_2 & -1 & 0 & 2 & 1 & 8 \\\hline
	-z & 5 & 0 & -3 & 0 & -9
\end{array}\nonumber
$$
此时毫无疑问没有达到最优解，但是理应作为下一个引入基的变量 $x$ 这列的所有系数都是负数。根据我们之前的分析，此时的 $x$ 是不受控制的，目标函数可以变得任意大。下面是这个情况对应的几何图形：

<img src="graphs/lp5.png" alt="lp5" style="zoom:50%;" />

可以看到在 $(0, 3)$ 处我们下一步的路径是无界的。沿着这条线我们可以得到任意大的 $x, y$。

#### 基准退化现象

让我们再来看一个异常的优化问题。考虑下面这个线性优化问题：
$$
\begin{align*}
\underset{x, y \in \R}{\text{maximize}} \quad & 2x + 3y \\
\text{subject to} \quad & -x + y \le 3 \\
&  x - 2y \le 2 \\
& x + 2y \le 6 \\
& x, y \ge 0
\end{align*}
$$
我们依旧引入松弛变量，并将其化为简单形表：
$$
\begin{array}{cccccc|c}
	& x & y & s_1 & s_2 & s_3 & \\\hline
	s_1 & -1 & 1 & 1 & 0 & 0 & 3 \\
	s_2 & 1 & -2 & 0 & 1 & 0 & 2 \\
	s_3 & 1 & 2 & 0 & 0 & 1 & 6 \\\hline
	-z & 2 & 3 & 0 & 0 & 0 & 0
\end{array}\nonumber
$$
我们依旧选择 $y$ 进入基，不过这个时候我们发现 $s_1, s_2$ 的比值是相同的，因此它们中任一个都可以离开。假设让 $s_1$ 离开，我们就得到新的表：
$$
\begin{array}{cccccc|c}
	& x & y & s_1 & s_2 & s_3 & \\\hline
	y & -1 & 1 & 1 & 0 & 0 & 3 \\
	s_2 & -1 & 0 & 2 & 1 & 0 & 8 \\
	s_3 & 3 & 0 & -2 & 0 & 1 & 0 \\\hline
	-z & 5 & 0 & -3 & 0 & 0 & -9
\end{array}\nonumber
$$
此时出现了一件奇异的事情。按照规则我们应该让 $x$ 进入，$s_3$ 离开。但此时 $s_3$ 的基本解为 $0$，我们在这一步根本不能够提升目标函数。事实上如果求得下一张表：
$$
\begin{array}{cccccc|c}
	& x & y & s_1 & s_2 & s_3 & \\\hline
	y & 0 & 1 & 1/3 & 0 & 1/3 & 3 \\
	s_2 & 0 & 0 & 4/3 & 1 & 1/3 & 8 \\
	s_3 & 1 & 0 & -2/3 & 0 & 1/3 & 0 \\\hline
	-z & 0 & 0 & -1/3 & 0 & -5/3 & -9
\end{array}\nonumber
$$
目标函数的值确实没有任何进步。我们可以通过几何图形来解释这种情况：

<img src="/Users/yimingyu/Documents/GitHub/Notes/math/algorithm/graphs/ip6.png" alt="ip6" style="zoom:50%;" />

可以看到，$(0, 3)$ 是三条直线的交点。斜着的那条直线是我们引入 $x$ 时的路线，但是显然它和可行域的交集只有 $(0, 3)$ 这一点。所以我们在下一步得到的可行解依然是 $(0, 3)$，而它实际上就是最优解。我们把这种情况称为 **基准退化（Degenerate Pivoting）**。出现基准退化时，我们有可能在同一个顶点上不必要地变换很多次，而实际上没有提升目标函数。我们急需一个方法避免这种情形。没错，依然是 **基准规则**，我们很快就会提到这个知识点。

### 两阶段的简单形法

在前面的论述中，我们总是从一个基本可行解开始的。但是线性优化问题可能根本没有可行域，此时我们甚至没法进行简单形法。为了判断一个线性优化问题是否有基本可行解，我们可以设置一个辅助问题，其中引入了 **虚构变量（Artificial Variable）** 并假定它们拥有可行解；之后将它们的和优化为最小值，如果这个最小值是 $0$，就说明原线性优化问题存在可行域。让我们用一个例子说明：
$$
\begin{align*}
\underset{\mathbf{x} \in \R^4}{\text{minimize}} \quad & x_1 + x_2 - x_3 - x_4 \\
\text{subject to} \quad & -3x_1 + x_2 + x_3 + x_4 = 7 \\
& -2x_1 + x_2 + x_3 + 3x_4 = 1 \\
& x_1, x_2, x_3, x_4 \ge 0
\end{align*}
$$
引入虚构变量 $x_1^\alpha, x_2^\alpha$ 并将原问题化为：
$$
\begin{align*}
\underset{\mathbf{x} \in \R^4, \mathbf{x}^\alpha \in \R^2}{\text{minimize}} \quad & x_1^\alpha + x_2^\alpha \\
\text{subject to} \quad & -3x_1 + x_2 + x_3 + x_4 + x_1^\alpha = 7 \\
& -2x_1 + x_2 + x_3 + 3x_4 + x_2^\alpha = 1 \\
& x_1, x_2, x_3, x_4, x_1^\alpha, x_2^\alpha \ge 0
\end{align*}
$$
为了解决这个问题，我们依然使用简单形表只不过加了新的一行 $-z^\alpha$ 用来存储辅助问题的目标函数信息：
$$
\begin{array}{ccccccc|c}
	& x_1 & x_2 & x_3 & x_4 & x_1^\alpha & x_2^\alpha & \\\hline
	x_1^\alpha & -3 & 2 & 1 & 1 & 1 & 0 & 7 \\
	x_2^\alpha & -2 & 1 & 1 & 3 & 0 & 1 & 1 \\\hline
	-z & 1 & 1 & -1 & -1 & 0 & 0 & 0 \\\hline
	-z^\alpha & 5 & -3 & -2 & -4 & 0 & 0 & -8
\end{array}\nonumber
$$
注意这里目标函数在填表前就进行了行运算，使得 $x_1^\alpha, x_2^\alpha$ 两列为 $0$。接下来就是枯燥无味的表变换。顺次引入 $x_2, x_1$ 后我们得到：
$$
\begin{array}{ccccccc|c}
	& x_1 & x_2 & x_3 & x_4 & x_1^\alpha & x_2^\alpha & \\\hline
	x_1 & 1 & 0 & -1 & -5 & 1 & -2 & 5 \\
	x_2 & 0 & 1 & -1 & -7 & 2 & -3 & 11 \\\hline
	-z & 0 & 0 & 1 & 16 & -3 & 5 & -16 \\\hline
	-z^\alpha & 0 & 0 & 0 & 0 & 1 & 1 & 0
\end{array}\nonumber
$$
至此我们就得到了辅助问题的最优解。此时目标函数为 $0$，正合期望。所以原问题存在可行域，我们可以令 $x_1^\alpha = x_2^\alpha = 0$，然后就得到：
$$
\begin{array}{ccccc|c}
	& x_1 & x_2 & x_3 & x_4 & \\\hline
	x_1 & 1 & 0 & -1 & -5 & 5 \\
	x_2 & 0 & 1 & -1 & -7 & 11 \\\hline
	-z & 0 & 0 & 1 & 16 & -16
\end{array}\nonumber
$$
凑巧，我们也得到了原问题的最优解 $(5, 11, 0, 0)$，此时目标函数有最小值 $16$。

需要注意的是，我们得到辅助问题的最优解时，基本变量有可能包含虚构变量。比如下面这种情形：
$$
\begin{array}{ccccccc|c}
	& x_1 & x_2 & x_3 & x_4 & x_1^\alpha & x_2^\alpha & \\\hline
	x_1^\alpha & 1 & 0 & -1 & -5 & 1 & -2 & 0 \\
	x_2 & 0 & 1 & -1 & -7 & 2 & -3 & 1 \\\hline
	-z & 0 & 0 & 1 & 16 & -3 & 5 & -1 \\\hline
	-z^\alpha & 0 & 0 & 0 & 0 & 1 & 1 & 0
\end{array}\nonumber
$$
消除所有虚构变量我们得到：
$$
\begin{array}{ccccc|c}
	& x_1 & x_2 & x_3 & x_4 & \\\hline
	 & 1 & 0 & -1 & -5 & 0 \\
	x_2 & 0 & 1 & -1 & -7 & 1 \\\hline
	-z & 0 & 0 & 1 & 16 & -1
\end{array}\nonumber
$$
呃，第一行是什么东西呢？一个简单的解决方法是让出了 $x_2$ 外任一个变量成为基本变量，比如 $x_3$，然后进行行变换得到：
$$
\begin{array}{ccccc|c}
	& x_1 & x_2 & x_3 & x_4 & \\\hline
	x_3 & -1 & 0 & 1 & 5 & 0 \\
	x_2 & -1 & 1 & 0 & -2 & 1 \\\hline
	-z & 1 & 0 & 0 & 11 & -1
\end{array}\nonumber
$$
这样我们就得到了最优解 $(0, 1, 0, 0)$，对应目标函数最小值 $1$。

### 简单形表退化现象

如果一个简单形表中的简单可行解存在 $0$，我们就成这个简单形表是 **退化的（Degenerate）**。比如下面的例子：
$$
\begin{align*}
\underset{x_1, x_2 \in \R}{\text{maximize}} \quad & x_1 + 4x_2 \\
\text{subject to} \quad & x_1 + x_2 \le 0 \\
& x_1 - 3x_2 \le 0 \\
& -2x_1 + x_2 \le 0 \\
& x_1, x_2 \ge 0
\end{align*}
$$
一般来说，形如 $A\mathbf{x} \le \mathbf{0}$ 或 $A\mathbf{x} \ge \mathbf{0}$ 的线性优化问题或者最优解在 $(0, 0)$ ，或者其可行域无界因而不存在最优解。我们引入松弛变量并将方程变为简单形表：
$$
\begin{array}{cccccc|c}
	& x_1 & x_2 & s_1 & s_2 & s_3 & \\\hline
	s_1 & 1 & 1 & 1 & 0 & 0 & 0 \\
	s_2 & 1 & -3 & 0 & 1 & 0 & 0 \\
	s_3 & -2 & 1 & 0 & 0 & 1 & 0 \\\hline
	-z & 1 & 4 & 0 & 0 & 0 & 0
\end{array}\nonumber
$$
现在我们可以按照简单形法继续进行化简，但由于所有基本可行解都是 $0$，我们每次引入新的基本变量时无法通过右下角得知是否对目标函数进行了优化，只能不断尝试直到 $-z$ 这行都变为负数。这个过程我们并不知道是否出现原地打转的情况。因此再一次地，我们需要一个 **基准规则** 来保证我们一定能到达最优解。

### 基准规则

正如简单形法的 [规则简述](# 规则简述) 中提到的，在选择进入基的变量，以及出现比值相同的离开基的候选变量时，我们有选择的空间。为了防止我们的选择导致无限循环，需要特定的 **基准规则（Pivoting Rule）** 保证不会出现无限循环：

- Bland 基准规则：给所有约束变量（$x_1,...,x_n,s_1,...,s_m$）一个序号。在每次选择时，优先选择序号低的。
- 随机基准规则：每次选择时随机进行。
- 词典基准规则：为每个方程设置 $\epsilon_1,...,\epsilon_m$ 使得 $1 \gg \epsilon_1 \gg \epsilon_2 \gg ... \gg \epsilon_m > 0$，然后将 $\mathbf{b}$ 全体加上 $[\epsilon_1,....,\epsilon_m]^\text{T}$，再继续进行简单形法。此时可以保证不会因为简单形表的退化而出现循环。

我们将详细描述 **词典基准规则（Lexicographic Pivoting Rule）**，其基于一个简单的思考：简单形表变换中出现循环的原因通常是多个基本可行解描述的是同一个点。如果我们对约束右侧加上一系列很小且不同的值，那样就不会出现多条线重合的情形了。让我们以上一节中遇到的问题为例，展示词典基准规则的使用：
$$
\begin{align*}
\underset{x_1, x_2 \in \R}{\text{maximize}} \quad & x_1 + 4x_2 \\
\text{subject to} \quad & x_1 + x_2 \le \epsilon_1 \\
& x_1 - 3x_2 \le \epsilon_2 \\
& -2x_1 + x_2 \le \epsilon_3 \\
& x_1, x_2 \ge 0
\end{align*}
$$
其可以写为以下的简单形表：
$$
\begin{array}{cccccc|c|cccc}
	& x_1 & x_2 & s_1 & s_2 & s_3 & & 1 & \epsilon_1 & \epsilon_2 & \epsilon_3 \\\hline
	s_1 & 1 & 1 & 1 & 0 & 0 & \epsilon_1 & 0 & 1 & 0 & 0 \\
	s_2 & 1 & -3 & 0 & 1 & 0 & \epsilon_2 & 0 & 0 & 1 & 0 \\
	s_3 & -2 & 1 & 0 & 0 & 1 & \epsilon_3 & 0 & 0 & 0 & 1 \\\hline
	-z & 1 & 4 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0
\end{array}\nonumber
$$
这里可以选择有最大 **差额成本（Reduced Cost）** 的 $x_2$ 进入基，然后由于 $\epsilon_3 < \epsilon_1$，我们选择 $s_3$ 退出：
$$
\begin{array}{cccccc|c|cccc}
	& x_1 & x_2 & s_1 & s_2 & s_3 & & 1 & \epsilon_1 & \epsilon_2 & \epsilon_3 \\\hline
	s_1 & 3 & 0 & 1 & 0 & -1 & \epsilon_1 - \epsilon_3 & 0 & 1 & 0 & -1 \\
	s_2 & -5 & 0 & 0 & 1 & 3 & \epsilon_2 + 3\epsilon_3 & 0 & 0 & 1 & 3 \\
	x_2 & -2 & 1 & 0 & 0 & 1 & \epsilon_3 & 0 & 0 & 0 & 1 \\\hline
	-z & 9 & 0 & 0 & 0 & -4 & -4\epsilon_3 & 0 & 0 & 0 & -4
\end{array}\nonumber
$$
引入 $x_1$ 并让 $s_1$ 退出：
$$
\begin{array}{cccccc|c|cccc}
	& x_1 & x_2 & s_1 & s_2 & s_3 & & 1 & \epsilon_1 & \epsilon_2 & \epsilon_3 \\\hline
	x_1 & 1 & 0 & 1/3 & 0 & -1/3 & \epsilon_1/3 - \epsilon_3/3 & 0 & 1/3 & 0 & -1/3 \\
	s_2 & 0 & 0 & 5/3 & 1 & 4/3 & 5\epsilon_2/3 + \epsilon_2 + 4\epsilon_3/3 & 0 & 5/3 & 1 & 4/3 \\
	x_2 & 0 & 1 & 2/3 & 0 & 1/3 & 2\epsilon_1/3  + \epsilon_3/3 & 0 & 2/3 & 0 & 1/3 \\\hline
	-z & 0 & 0 & -3 & 0 & -1 & -3\epsilon_1 - \epsilon_3 & 0 & -3 & 0 & -1
\end{array}\nonumber
$$
至此所有的差额成本都为非正数了，我们得到了最优解 $(0, 0, 0)$，此时目标函数为 $0$。

细心的同学可能会发现，每次操作时，$\epsilon_i$ 的系数总是和 $s_i$ 的系数一致，因此存在松弛变量的情况下我们只需要按照它们的系数来判断即可。即使是没有松弛变量的方程，我们也可以对一开始的基本变量所在列作出标记 $C_1,...,C_k$，然后每次判断离开的基本变量出现选择时，选择 $C_1$ 列最小的，不然顺次比较 $C_2$、$C_3$…… 这和使用 $\epsilon_i$ 是完全等价的。

## 矩阵形式

### 基本可行解

现在让我们用矩阵构建更为严格的线性优化问题算法。考虑一个等式形式的线性优化问题（引入松弛变量后一定是下这种形式）：
$$
\begin{align}
\begin{split}
	\underset{\mathbf{x} \in \R^n}{\text{maximize}} \quad & \mathbf{c}^\text{T}\mathbf{x} \\
	\text{subject to} \quad & A\mathbf{x} = \mathbf{b} \\
	& \mathbf{x} \ge \mathbf{0}
\end{split}
\end{align} \tag{4} \label{4}
$$
此处 $A$ 是一个 $m\times n$ 的矩阵，$\mathbf{c} \in \R^n, \mathbf{b} \in \R^m$（这个设定将贯穿下面全文；当没有特别给出这些矩阵和向量的类型时，都以此处的声明为准）。为了方便讨论，我们假设 $A$ 中的各行是线性无关的。下面让我们来正式地定义 **基本可行解** ：从 $A$ 中选取不重复且按顺序排列的某 $m$ 列序号记为 $\mathcal{B}$，其代表我们将选取的基本变量的序号。其余的 $n - m$ 个序号按顺序排列记为 $\mathcal{N}$。定义 $\mathbf{x}_\mathcal{B} = (x_{k_1}, x_{k_2},..., x_{k_m})$，其中 $k_1, k_2,..., k_m \in \mathcal{B}$ 且 $k_1 < k_2 <. ..< k_m$。类似地我们可以定义 $\mathbf{x}_\mathcal{N}$。这样我们就可以将目标函数写为：
$$
\begin{align*}
	\mathbf{c}^\text{T}\mathbf{x} = \mathbf{c}_\mathcal{B}^\text{T}\mathbf{x}_\mathcal{B} + \mathbf{c}_\mathcal{N}^\text{T}\mathbf{x}_\mathcal{N}
\end{align*} \tag{5}
$$
而约束条件也可以写为：
$$
\begin{align*}
	A_\mathcal{B}\mathbf{x}_\mathcal{B} + A_\mathcal{N}\mathbf{x}_\mathcal{N} = \mathbf{b}
\end{align*} \tag{6}
$$
为了得到基本可行解，我们需要 $A_\mathcal{B}$ 是可逆的，此时设 $\mathbf{x}_\mathcal{N} = 0$，我们就有 $\mathbf{x}_\mathcal{B} = A_\mathcal{B}^{-1}\mathbf{b}$。如果此时同时有 $\mathbf{x}_\mathcal{B} \ge \mathbf{0}$，我们就称其是一个基本可行解。

此外，让我们再定义 $\R^n$ 子集的 **顶点（Vertex）** 与 **极值点（Extreme Point）** 的概念。对于 $S \subseteq \R^n$，顶点是指令某个线性函数 $\boldsymbol{\alpha}^\text{T}\mathbf{x}$ 严达到格最小值的点 $\mathbf{y} \in S$。极值点则是指在不在任意两个点 $\mathbf{x}, \mathbf{y} \in S$ 之间的点 $\mathbf{z} \in S$。最后，定义 **可行域** $F = \{x \in \R^n \mid A\mathbf{x} = \mathbf{b}\}$。现在我们要证明在可行域中，基本可行解、顶点与极值点是等价的：

> **命题**：任何基本可行解都是可行域的一个顶点。

> **证明**：首先任意选择一个基本可行解及其对应的 $\mathcal{B}, \mathcal{N}$。设 $\alpha_i = \begin{cases}1\quad i \in \mathcal{N} \\ 0 \quad i \in \mathcal{B}\end{cases}$，则 $\boldsymbol{\alpha}^\text{T}\mathbf{x}$ 就是所有非基本变量的和。显然当且仅当 $\mathbf{x}_\mathcal{N} = \mathbf{0}$ 时才有最小值 $\mathbf{0}$，因此基本可行解对应了可行域的一个顶点。

> **命题**：$S \subseteq \R^n$ 的顶点同时也是它的极值点。

> **证明**：设 $\mathbf{x} \in S$ 是 $S$ 的一个顶点，$\boldsymbol{\alpha}$ 是令 $\boldsymbol{\alpha}^\text{T}\mathbf{x} < \boldsymbol{\alpha}^\text{T}\mathbf{y}$ 对任意 $\mathbf{y} \ne \mathbf{x} \in S$ 成立的向量。假设 $\mathbf{x}$ 并不是极值点，则存在 $\mathbf{y}, \mathbf{y}'$ 使得 $\mathbf{x} = t\mathbf{y} + (1-t)\mathbf{y}'$。此时有：
> $$
> \begin{equation*}
> 	\boldsymbol{\alpha}^\text{T}\mathbf{x} = t\boldsymbol{\alpha}^\text{T}\mathbf{y} + (1-t)\boldsymbol{\alpha}^\text{T}\mathbf{y}' < t\boldsymbol{\alpha}^\text{T}\mathbf{x} + (1-t)\boldsymbol{\alpha}^\text{T}\mathbf{x} = \boldsymbol{\alpha}^\text{T}\mathbf{x}
> \end{equation*}
> $$
> 矛盾！因此 $\mathbf{x}$ 必然是一个极值点。

> **命题**：任何可行域的极值点都是它的一个基本可行解。

> **证明**：暂略。

### 行运算

我们在介绍简单形法时没有介绍行运算，这里详细讲一下。行运算包括下面三种：

- 将某一行乘以任一个非零的数。
- 将某一行乘以任一个数并加到另一行上。
- 交换两行。

矩阵的行运算不会改变其对应的线性方程组的解，因此非常有用。实际上，我们可以将行运算视为一个 **线性变换（Linear Transformation）**，对 $A\mathbf{x} = \mathbf{b}$ 的行变换可以写为 $(EA)\mathbf{x} = E\mathbf{b}$ 的形式，其中 $E$ 是一个 **基本矩阵（Elementary Matrix）**，即和单位矩阵只差一次行运算的矩阵。作为举例，下面的基本矩阵 $E$ 将第一行乘上 $-2$ 后加到第三行中：
$$
E = \begin{bmatrix}
	1 & 0 & 0 \\
	0 & 1 & 0 \\
	-2 & 0 & 1
\end{bmatrix}, A = 
\begin{bmatrix}
	1 & 4 \\
	2 & 5 \\
	3 & 6
\end{bmatrix}, \mathbf{b} =
\begin{bmatrix}
	7 \\ 8 \\ 9
\end{bmatrix}, \\ A\mathbf{x} = \mathbf{b} \quad\leadsto\quad
(EA)\mathbf{x} = Eb \Leftrightarrow 
\begin{bmatrix}
	1 & 4 \\
	2 & 5 \\
	1 & -2 
\end{bmatrix}
\begin{bmatrix}
	x_1 \\ x_2 \\ x_3
\end{bmatrix}
= \begin{bmatrix}
	7 \\ 8 \\ -5
\end{bmatrix}
\nonumber
$$
这样，我们就可以将经过一系列行运算的等式写作 $MA\mathbf{x} = M\mathbf{b}$ 的形式。实际上 $M = A_\mathcal{B}^{-1}$，这是因为 $(MA)_\mathcal{B} = I$。因此对于给定的基 $\mathcal{B}$，我们可以找到基本解：
$$
\begin{align*}
	\mathbf{x}_\mathcal{B} = A_\mathcal{B}^{-1}\mathbf{b} - A_\mathcal{B}^{-1} A_\mathcal{N} \mathbf{x}_\mathcal{N}
\end{align*} \tag{7}
$$
当令 $\mathbf{x}_\mathcal{N} = \mathbf{0}$ 时我们就得到了 $\mathbf{x}_\mathcal{B} = A_\mathcal{B}^{-1}\mathbf{b}$。这个结论在线性代数中应该已经讨论过，这里只是进行重温。

### 简单形表的公式

现在让我们把上面的一些结论代入到简单形表中。对于 $(\ref{4})$ 描述的线性优化问题，我们在简单形法中每次选择 $\mathcal{B}$ 时要确保：

- $A_\mathcal{B}$ 是可逆的，此时令 $\mathbf{x}_\mathcal{N} = 0$，我们就得到了一个基本解 $\mathbf{x}_\mathcal{B} = A_\mathcal{B}^{-1}\mathbf{b}$。
- $\mathbf{x}_\mathcal{B} \ge \mathbf{0}$，此时这个基本解就是可行的。

简单形表的模式如下：
$$
\begin{array}{cccccc|c}
	& & \mathbf{x}_\mathcal{B} & & \mathbf{x}_\mathcal{N} & & \\\hline
	\\
	\mathbf{x}_\mathcal{B} & & I & & Q & & \mathbf{p} \\
	\\\hline
	-z & & \mathbf{0}^\text{T} & & \mathbf{r}^\text{T} & & -z_0
\end{array} \tag{8}
$$
这里 $Q$ 是一个矩阵，而 $\mathbf{p}, \mathbf{r}$ 是两个向量。需要注意的是我们要求表的左上角是单位矩阵 $I$，因此可能需要列之间的交换。下面举一个例子说明：
$$
\begin{align*}
\underset{x, y \in \R}{\text{maximize}} \quad & 2x + 3y \\
\text{subject to} \quad & -x + y + s_1 = 3 \\
& x - 2y + s_2 = 2 \\
& x + y + s_3 = 7 \\
& x, y, s_1, s_2, s_3 \ge 0
\end{align*}
$$
让我们把它写成 (8) 式的形式（这是我们期望的最终成果）：
$$
\begin{array}{cccccc|c}
	& y & s_2 & x & s_1 & s_3 & \\\hline
	y & 1 & 0 & 0 & 1/2 & 1/2 & 5 \\
	s_2 & 0 & 1 & 0 & 3/2 & 1/2 & 10 \\
	x & 0 & 0 & 1 & -1/2 & 1/2 & 2 \\\hline
	-z & 0 & 0 & 0 & -1/2 & -5/2 & -19
\end{array}\nonumber
$$
如果使用上表的顺序重写最一开始的方程，应该是下面这样的：
$$
\begin{array}{cccccc|c}
	& y & s_2 & x & s_1 & s_3 & \\\hline
	s_1 & 1 & 0 & -1 & 1 & 0 & 3 \\
	s_2 & -2 & 1 & 1 & 0 & 0 & 2 \\
	s_3 & 1 & 0 & 1 & 0 & 1 & 7 \\\hline
	-z & 3 & 0 & 2 & 0 & 0 & 0
\end{array}\nonumber
$$
我们的目标是通过这个表尝试得出基为 $(y, s_2, x)$ 的新表中的 $\mathbf{p}, Q, \mathbf{r}, z_0$ 这些新数据。目前已知的是我们选取的基 $\mathcal{B}$ 满足：
$$
A_\mathcal{B} = \begin{bmatrix}
	1 & 0 & -1 \\
	-2 & 1 & 1 \\
	1 & 0 & 1
\end{bmatrix}\nonumber
$$
首先让我们看看 $\mathbf{p}$。它存储的是经过行运算后得到的新基本可行解。因此不难猜测有：
$$
A_\mathcal{B}^{-1}\mathbf{b} = \begin{bmatrix}
	1 & 0 & -1 \\
	-2 & 1 & 1 \\
	1 & 0 & 1
\end{bmatrix} \begin{bmatrix}
	3 \\ 2 \\ 7
\end{bmatrix}
= \begin{bmatrix}
	5 \\ 10 \\ 2
\end{bmatrix}
= \mathbf{p}
$$
然后 $A_\mathcal{N}$ 在等式左侧其实经历了和 $\mathbf{b}$ 相同的运算过程，因此也有：
$$
A_\mathcal{B}^{-1}A_\mathcal{N} = \begin{bmatrix}
	1 & 0 & -1 \\
	-2 & 1 & 1 \\
	1 & 0 & 1
\end{bmatrix} \begin{bmatrix}
	1 & 0 \\
	0 & 0 \\
	0 & 1
\end{bmatrix}
= \begin{bmatrix}
	1/2 & 1/2 \\
	3/2 & 1/2 \\
	-1/2 & 1/2
\end{bmatrix}
= Q
$$
目标函数 $z_0$ 也比较直白，正是 $z = \mathbf{c}^\text{T}\mathbf{x} = \mathbf{c}_\mathcal{B}^\text{T}\mathbf{x}_\mathcal{B} + \mathbf{c}_\mathcal{N}^\text{T}\mathbf{x}_\mathcal{N}$，且此时 $\mathbf{x}_\mathcal{B} = A_\mathcal{B}^{-1}\mathbf{b}$ 。最后让我们尝试得到 $\mathbf{r}$。实际上我们可以完全让 $-z$ 行保持不变，然后得到 $I$ 与 $Q$ 后（它们合起来即是 $A_\mathcal{B}^{-1}A$），为了让基本变量所在的列能够被清空，我们需要从 $\mathbf{c}^\text{T}$ 中减去 $\mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}A$，其过程如下：
$$
\mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}A = 
\begin{bmatrix}
	3 & 0 & 2
\end{bmatrix}
\begin{bmatrix}
	1 & 0 & 0 & 1/2 & 1/2 \\
	0 & 1 & 0 & 3/2 & 1/2 \\
	0 & 0 & 1 & -1/2 & 1/2
\end{bmatrix}
= \begin{bmatrix}
	3 & 0 & 2 & 1/2 & 5/2
\end{bmatrix} \\
\mathbf{c}^\text{T} - \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}A =
\begin{bmatrix}
	0 & 0 & 0 & -1/2 & -5/2
\end{bmatrix}
$$
只要取它序数为 $\mathcal{N}$ 中的项，就得到了 $\mathbf{r}^\text{T}$。

最后我们总结如下：

- $\mathbf{p} = A_\mathcal{B}^{-1}\mathbf{b}$
- $Q = A_\mathcal{B}^{-1}A_\mathcal{N}$
- $\mathbf{r}^\text{T} = \mathbf{c}_\mathcal{N}^\text{T} - \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}A_\mathcal{N}$
- $z_0 = \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}\mathbf{b}$ 

### 修改后的简单形法

通过上面这些公式的启发，我们意识到并不需要计算矩阵中的每一项，而只需每次计算关键的数据即可推进。理论上，我们只需要一个 $A_\mathcal{B}$，因为它在每个公式都出现了，且决定了基本变量的取舍。不过，$\mathbf{p}$ 也同样重要，因为我们需要随时更新基本变量当前的解。

下面是修改的简单形法的全部步骤：

- **计价（Pricing）**：决定新的基 $\mathcal{B}$。此前使用简单形表时，这步只需要选择一个差额成本符号正确（求最大值时寻找正数，反之找负数）的基本变量即可。现在我们并不打算将整个差额成本向量 $\mathbf{r}^\text{T}$ 算出来，只需对 $\mathcal{N}$ 中每个变量 $x_j$，计算 $r_j = c_i - \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}A_j$，只要其满足符号要求，就选择其为新的基本变量。这里我们使用 Bland 基准规则，因此 $j$ 将从 $\mathcal{N}$ 中最小的开始直到最大。$r_j$ 的计算细节也值得讨论。由于 $\mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}$ 和 $j$ 无关且可能多次重复计算，我们应该首先计算这个乘积。这个乘积在目标函数 $z_0$ 的公式中也出现了，我们记其为 $\mathbf{u}^\text{T}$。

- 列生成：为了找到离开的变量，我们应该计算 $Q_j$。它可以通过公式 $Q_j = (A_\mathcal{B}^{-1}A_\mathcal{N})_j = A_\mathcal{B}^{-1}A_j$ 得到。

- 寻找基准：我们应该对 $Q_j$ 和 $\mathbf{p}$ 中的每项（要求 $Q_j$ 的该项为正数）计算 $p_i/Q_{ij}$。使得这个比值最小的 $i$ 将成为离开的变量。

- 更新 $A_\mathcal{B}^{-1}$ 和 $\mathbf{p}$：通过前面的步骤我们知道如何从 $\mathcal{B}$ 得到 $\mathcal{B}'$，该过程实际上就是寻找一个进入基和一个离开基的变量。现在我们尝试从 $A_\mathcal{B}^{-1}$ 得到 $A_{\mathcal{B}'}^{-1}$，并将 $\mathbf{p}$ 变为 $A_{\mathcal{B}'}^{-1}\mathbf{b}$。之前我们提到过，$A_\mathcal{B}^{-1}$ 的实质就是对使 $A$ 变为 $[I|Q]$ 的基本矩阵之积。因此：
  $$
  A\mathbf{x} = \mathbf{b} \quad\leadsto\quad A_\mathcal{B}^{-1}A\mathbf{x} = A_\mathcal{B}^{-1}\mathbf{b}
  $$
  现在假设我们还需要一系列行变换，$M$ 来从 $\mathcal{B}$ 切换到 $\mathcal{B}'$，则有：
  $$
  A_\mathcal{B}^{-1}A\mathbf{x} = A_\mathcal{B}^{-1}\mathbf{b} \quad\leadsto\quad MA_\mathcal{B}^{-1}A\mathbf{x} = MA_\mathcal{B}^{-1}\mathbf{b}
  $$
  另一方面，如果一开始就从将基设为 $\mathcal{B}'$，则有：
  $$
  A\mathbf{x} = \mathbf{b} \quad\leadsto\quad A_{\mathcal{B}'}^{-1}A\mathbf{x} = A_{\mathcal{B}'}^{-1}\mathbf{x}
  $$
  经过对比我们有：
  $$
  A_{\mathcal{B}'} = MA_\mathcal{B}^{-1}
  $$
  因此，每次进行额外的行变换时，只需要将其乘上一步的基变量的逆矩阵 $A_\mathcal{B}^{-1}$ 就可以得到新的逆矩阵。为了方便计算，我们可以借用一个迷你的简单形表：
  $$
  \begin{array}{|c|c|c|}
  	A_\mathcal{B}^{-1} & Q_j &\mathbf{p}
  \end{array}
  \nonumber
  $$
  当我们将 $Q_j$ 中的第 $i$ 行（离开的基本变量）为基准，将该列其它项都清为 $0$ 时（且该项变为 $1$ 时），$A_\mathcal{B}^{-1}$ 便自然地变成 $A_{\mathcal{B}'}$，且 $\mathbf{p}$ 变为 $\mathbf{p}'$ 了。

现举例说明：
$$
\begin{align*}
	\underset{x_1, x_2, x_3, x_4 \in \R}{\text{maximize}}\quad & x_1 + x_2 + x_3 + 2x_4 \\
	\text{subject to} \quad & 3x_1 - x_2 + 4x_3 - x_4 \le 4 \\
	& 2x_2 + x_4 \le 5 \\
	& x_1, x_2, x_3, x_4 \ge 0
\end{align*}
$$
引入松弛变量后，我们可以写出如下的简单形表：
$$
\begin{array}{ccccccc|c}
	& x_1 & x_2 & x_3 & x_4 & s_1 & s_2 & \\\hline
	s_1 & 3 & -1 & 4 & -1 & 1 & 0 & 4 \\
	s_2 & 0 & 1 & 0 & 1 & 0 & 1 & 5 \\\hline
	-z & 1 & 1 & 1 & 2 & 0 & 0 & 0
\end{array}\nonumber
$$
此时有 $\mathcal{B} = (s_1, s_2)$。易知 $A_\mathcal{B}^{-1} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$，$\mathbf{p} = \begin{bmatrix} 4 \\ 5 \end{bmatrix}$。依此我们可以得到 $\mathbf{u}^\text{T} = \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1} = \begin{bmatrix} 0 & 0 \end{bmatrix}$。简单形表的核心信息如下：
$$
\begin{array}{ccc|c}
	\mathcal{B} & A_\mathcal{B}^{-1} & x_1 & \mathbf{p} \\\hline
	s_1 & 1\quad 0 & 3 & 4 \\
	s_2 & 0\quad 1 & 0 & 5
\end{array}\nonumber
\quad\leadsto\quad
\begin{array}{ccc|c}
	\mathcal{B} & A_\mathcal{B}^{-1} & x_1 & \mathbf{p} \\\hline
	x_1 & 1/3\quad 0 & 1 & 4/3 \\
	s_2 & 0\quad 1 & 0 & 5
\end{array}\nonumber
$$
我们在上面的变换中选择让 $x_3$ 进入基，$s_1$ 离开。随后我们得到 $\mathbf{u}^\text{T} = \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1} = \begin{bmatrix} 1 & 0 \end{bmatrix} \begin{bmatrix} 1/3 & 0 \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} 1/3 & 0 \end{bmatrix}$。计算得到 $x_2$ 的差额成本为：$1 - \begin{bmatrix} 1/3 & 0 \end{bmatrix}\begin{bmatrix} -1 \\ 2 \end{bmatrix} = \frac{4}{3} > 0$。因此 $x_2$ 可以进入基。经过计算可以得到 $Q_{x_2} = A_\mathcal{B}^{-1}A_{x_2} = \begin{bmatrix} 1/3 & 0 \\ 0 & 1 \end{bmatrix}\begin{bmatrix} -1 \\ 2 \end{bmatrix} = \begin{bmatrix} -1/3 \\ 2 \end{bmatrix}$。现在简单形表为：
$$
\begin{array}{ccc|c}
	\mathcal{B} & A_\mathcal{B}^{-1} & x_2 & \mathbf{p} \\\hline
	x_1 & 1/3\quad 0 & -1/3 & 4/3 \\
	s_2 & 0\quad 1 & 2 & 5
\end{array}\nonumber
\quad\leadsto\quad
\begin{array}{ccc|c}
	\mathcal{B} & A_\mathcal{B}^{-1} & x_2 & \mathbf{p} \\\hline
	x_1 & 1/3\quad 1/6 & 0 & 13/6 \\
	x_2 & 0\quad 1/2 & 1 & 5/2
\end{array}
$$
在此计算 $\mathbf{u}^\text{T} = \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1} = \begin{bmatrix} 1 & 1 \end{bmatrix}\begin{bmatrix} 1/3 & 1/6 \\ 0 & 1/2 \end{bmatrix} = \begin{bmatrix} 1/3 & 2/3 \end{bmatrix}$。接下来通过 $r_j = c_j - \mathbf{u}^\text{T}A_j$ 来寻找合适的新的基本变量。$x_3$ 的差额成本为：$1 - \begin{bmatrix} 1/3 & 2/3 \end{bmatrix}\begin{bmatrix} 4 \\ 0 \end{bmatrix} = -\frac{1}{3} < 0$，因此并不合适。$x_4$ 的差额成本为：$2 - \begin{bmatrix} 1/3 & 2/3 \end{bmatrix}\begin{bmatrix} -1 \\ 1 \end{bmatrix} = \frac{5}{3} > 0$，因此 $x_4$ 可以进入基。计算得到 $Q_{x_4} = A_\mathcal{B}^{-1}A_{x_4} = \begin{bmatrix} 1/3 & 1/6 \\ 0 & 1/2 \end{bmatrix}\begin{bmatrix} -1 \\ 1 \end{bmatrix} = \begin{bmatrix} -1/6 \\ 1/2 \end{bmatrix}$。简单形表如下：
$$
\begin{array}{ccc|c}
	\mathcal{B} & A_\mathcal{B}^{-1} & x_2 & \mathbf{p} \\\hline
	x_1 & 1/3\quad 1/6 & -1/6 & 13/6 \\
	x_2 & 0\quad 1/2 & 1/2 & 5/2
\end{array}
\quad\leadsto\quad
\begin{array}{ccc|c}
	\mathcal{B} & A_\mathcal{B}^{-1} & x_2 & \mathbf{p} \\\hline
	x_1 & 1/3\quad 1/3 & 0 & 3 \\
	x_4 & 0\quad 1 & 1 & 5
\end{array}\nonumber
$$
$\mathbf{u}^\text{T} = \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1} = \begin{bmatrix} 1 & 2 \end{bmatrix} \begin{bmatrix} 1/3 & 1/3 \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} 1/3 & 7/3 \end{bmatrix}$。似乎我们已经走完了所有变量，检查一下差额成本：
$$
\begin{align*}
\mathbf{r}^\text{T} &= \mathbf{c}_\mathcal{N}^\text{T} - \mathbf{u}^\text{T}A_\mathcal{N} \\
&= \begin{bmatrix} 1 & 1 & 0 & 0 \end{bmatrix} - 
\begin{bmatrix} 1/3 & 7/3 \end{bmatrix}
\begin{bmatrix} -1 & 4 & 1 & 0 \\ 2 & 0 & 0 & 1 \end{bmatrix} \\
&= \begin{bmatrix} -10/3 & -1/3 & -1/3 & -7/3 \end{bmatrix}
\end{align*}
$$
差额成本都是负数，因此我们已经得到了最优解 $(3, 0, 0, 5)$。

### 最差情形

现在让我们尝试得到上面介绍的简单形法的最差时间复杂度。考虑一个有 $d$ 个变量和 $2d$ 个约束的线性优化问题。此时我们最多经过 $\begin{pmatrix} 2d \\ d \end{pmatrix} < 4^d$ 次变化才能得到最优解。考虑下面这个问题：
$$
\begin{align*}
\underset{x \in \R^d}{\text{maximize}} \quad & x_d \\
\text{subject to} \quad & 0 \le x_1 \le 1 \\
& 0 \le x_2 \le 1 \\
&... \\
& 0 \le x_d \le 1
\end{align*} \tag{9}
$$
最优解显然是 $(0, ..., 1)$，不过根据 Bland 基准规则，我们需要遍历所有可能后才能达到这个最优解。我们将 $d=3$ 的情形用下图展示出来：

<img src="graphs/lp7.png" alt="lp7" style="zoom:40%;" />

我们将这样的路径称为 **可怕轨迹（Terrible Trajectory）**（这并不是一个正式的术语）。其特征在于，除了最后一段轨迹，其它的所有变化都不会提升目标函数的值。如果我们能够设计一种方式，保证每次转换时都能提升目标函数，这样我们至少不会失去方向。

#### 欺骗 Bland 规则

一种方式是将约束条件改为等价的形式：
$$
\begin{align*}
\underset{x\in\R^d}{\text{maximize}}\quad &x_d \\
\text{subject to} \quad &\epsilon \le x_1 \le 1 + \epsilon \\
&\epsilon x_1 \le x_2 \le 1 + \epsilon x_1 \\
&... \\
& \epsilon x_{d-1} \le x_d \le 1 + \epsilon x_{d-1}
\end{align*}
$$
这里 $\epsilon$ 是任取的一个很小的正数。只要我们使 $\epsilon \to 0$，上面的线性优化问题的解就无限趋近于原问题。正是这个 $\epsilon$ 让我们的求解出现了一些决定性的不同，我们依然来看 $d=3$ 的示意图，假设 $\epsilon=0.1$：

<img src="graphs/lp8.png" alt="lp8" style="zoom:40%;" />

可以看到我们的目标函数经历了 $0.001 \to 0.009 \to 0.091 \to ... \to 0.999$ 这样逐渐优化的变化。不过，走的还是可怕轨迹。

#### Klee-Monty 正方体

前面我们展示的例子展现了 Bland 基准规则的弱点。但其它规则，比如选取最大差额成本的基准规则依然有致命的弱点。考虑下面的线性优化问题：
$$
\begin{align*}
\underset{x\in\R^d}{\text{maximize}}\quad & 2^{d-1}x_1 + 2^{d-2}x_2 + ... + x_d \\
\text{subject to}\quad & 5 \ge x_1 \\
& 25 \ge 4x_1 + x_2 \\
& ... \\
& 5^d \ge 2^dx_1 + 2^{d-1}x_2 + ... + x_d \\
& x_1, ..., x_d \ge 0
\end{align*}
$$
我们依然可以将其图形化。考虑 $d=3$，此时可以画出下面的（非正常比例）图形：

<img src="graphs/lp9.png" alt="lp9" style="zoom:40%;" />

Klee-Monty 问题成功让我们堕入陷阱，又走了一遍可怕轨迹。

#### 其它的基准规则下的最差情形

还有一些其它的基准规则，但是即使上面提到的优化问题不会产生可怕轨迹，也可能存在其它的构造使得其复杂度呈指数。一些聪明（但复杂很多）的基准规则可以达到更好的最差复杂度，$C^{\sqrt{d}\log{n}}$，这里 $C$ 是一个常数。但我们的理想状态是能够有一个多项式时间（由 $n$ 和 $d$ 组成的多项式）的算法。这个理想状态至今还没有找到答案。



## 对偶性

接下来让我们先不考虑如何解一个线性优化问题，而是缩小它目标函数的上下界。比如下面的优化问题：
$$
\begin{align*}
\underset{x_1, x_2, x_3 \in \R}{\text{maximize}} \quad &x_1 + x_2 \\
\text{subject to} \quad & 2x_1 + x_2 + 4x_3 \le 3 \\
& x_1 + x_2 - 3x_3 \le 1 \\
& x_1, x_2, x_3 \ge 0
\end{align*}
$$
首先寻找它的下界：

- 如果令 $x_1 = x_2 = x_3 = 0$，那么显然约束都是符合的，此时目标函数是 $0$。这是最宽泛的下界。
- 假设 $x_1 = 1$ 且 $x_2 = x_3 = 0$，此时目标函数是 $1$。
- 所有的可行解都可以给出一个下界，不过这就得开始解问题了，因此我们在此打住。

随后寻找它的上界：

- 显然目标函数 $x_1 + x_2 \le 2x_1 + x_2 + 4x_3$，根据第一个约束条件我们有 $x_1 + x_2 \le 3$，也即上界为 $3$。
- 对两个约束求和并取均值后，我们得到 $\frac{3}{2}x_1 + x_2 + \frac{1}{2}x_3 \le 2$，由于 $x_1 + x_2 \le \frac{3}{2}x_1 + x_2 + \frac{1}{2}x_3$，此时得到了更好的上界 $2$。

受上面寻找上界的启发，我们发现可以尝试引入 $u_1, u_2$ 得到：$u_1(2x_1 + x_2 + 4x_3) + u_2(x_1 + x_2 - 3x_3) \le 3u_1 + u_2$。经过重排列得到：$(2u_1 + u_2)x_1 + (u_1 + u_2)x_2 + (4u_1 - 3u_2)x_3 \le 3u_1 + u_2$。为了让 $3u_1 + u_1$ 成为 $x_1 + x_2$ 的上界，它需要满足：
$$
2u_1 + u_2 \ge 1 \qquad u_1 + u_2 \ge 1
$$
所以不经意间，我们构造了一个新的线性优化问题：
$$
\begin{align*}
\underset{u_1, u_2 \in \R}{\text{minimize}}\quad & 3u_1 + u_2 \\
\text{subject to}\quad & 2u_1 + u_2 \ge 1 \\
& u_1 + u_2 \ge 1 \\
& 4u_1 - 3u_2 \ge 0 \\
& u_1, u_2 \ge 0
\end{align*}
$$
找到这个问题的解，我们就能找到原题目标函数的最小上界。

### 弱与强对偶性

现在让我们考虑通用的情况，一个如下的线性优化问题（称为 **原始线性优化问题（Primal Linear Program）**）：
$$
\begin{align*}
(\mathbf{P}) \quad
\begin{cases}
\underset{\mathbf{x} \in \R^n}{\text{maximize}}\quad & \mathbf{c}^\text{T}\mathbf{x} \\
\text{subject to}\quad & A\mathbf{x} \le \mathbf{b} \\
& \mathbf{x} \ge \mathbf{0}
\end{cases}
\end{align*} \tag{10} \label{primal-program}
$$
它存在一个 **对偶线性优化问题（Dual Linear Program）** 如下：
$$
\begin{align*}
(\mathbf{D}) \quad
\begin{cases}
\underset{\mathbf{x} \in \R^m}{\text{minimize}} \quad & \mathbf{u}^\text{T}\mathbf{b} \\
\text{subject to}\quad & \mathbf{u}^\text{T}A \ge \mathbf{c}^\text{T} \\
& \mathbf{u} \ge \mathbf{0}
\end{cases}
\end{align*} \tag{11}
$$
该问题和下面的等价，我们更通常采取下面的形式：
$$
\begin{align*}
(\mathbf{D}) \quad
\begin{cases}
\underset{\mathbf{x} \in \R^m}{\text{minimize}}\quad & \mathbf{b}^\text{T}\mathbf{u} \\
\text{subject to} \quad & A^\text{T}\mathbf{u} \ge \mathbf{c} \\
& \mathbf{u} \ge 0
\end{cases}
\end{align*} \tag{12} \label{dual-program}
$$
下面给出两个定理：

> **线性优化问题的弱对偶性（Weak Duality of Linear Program)**：对于原始问题的任何可行解 $\mathbf{x}$ 和该原始问题对应的对偶问题的任何可行解 $\mathbf{u}$ 都有 $\mathbf{c}^\text{T}\mathbf{x} \le \mathbf{b}^\text{T}\mathbf{u}$。特别地，对偶问题最优解下的目标值是原始问题最优解下的目标值的上界。
>
> **线性优化问题的强对偶性（Strong Duality of Linear Program）**：如果原始问题和对偶问题都有最优解，那么它们最优解时的目标值是一样的。

我们暂时不对强对偶性进行证明。但通过这个结论，我们知道寻找线性优化问题的上界是一个有意义的操作，这个上界的最小值就是原始问题的目标函数最大值。不过，原始问题中的一些细节可能和我们这里设置的不太一样：

- 比如此前的例子中，我们不再要求 $x_2 \ge 0$。此时我们必须令 $x_2$ 的系数，即 $u_1 + u_2$ 等于 $1$，否则任何其它的系数都无法保证是 $u_1 + u_2$ 的上界。比如 $u_1 + 2u_2 + u_3$ 并非 $u_1 + u_2$ 的上界，因为 $u_2$ 可以是任意小的数。
- 比如此前的例子中，我们将第一个约束的两侧交换（即将符号取反），得到 $2x_1 + x_2 + 4x_3 \ge 3$。此时为了保持符号一致，我们应该为两侧都乘上 $-1$。此时对应的对偶变量应该是非正数。
- 比如我们需要 *最小化* 而不是最大化目标函数。此时所有过程都反过来了。我们需要找到目标函数的最小下界，因此在对偶问题中我们应该尝试得到最大值。

不难理解，所谓的原始问题和对偶问题是高度对称的。原始问题本身就是对偶问题的对偶问题。其中一者的变量对应着另一者的约束，其中的最大值问题的目标值对应着（另外那个）最小值问题的目标值。因此，我们没必要细究原始问题和对偶问题哪一个是最大值问题（及哪一个是最小值问题）。下表是一个简单的对应：

|  最大值问题  |  最小值问题  |
| :----------: | :----------: |
|  $\le$ 约束  | 变量 $\ge 0$ |
|   $=$ 约束   |  变量无限制  |
|  $\ge$ 约束  | 变量 $\le 0$ |
| 变量 $\ge 0$ |  $\ge$ 约束  |
|  变量无限制  |   $=$ 约束   |
| 变量 $\le 0$ |  $\le$ 约束  |

下文中，除了矩阵 $A$ 的列 $A_1, ..., A_n$，我们还会用到它的行 $\mathbf{a}_1^\text{T}, ..., \mathbf{a}_m^\text{T}$。

### 互补松弛性

现在让我们进一步探索对偶性相关的一些性质。根据线性代数中的一些运算性质，我们在原问题中通过 $A \mathbf{x} \le \mathbf{b}$ 及 $\mathbf{u}^\text{T} \ge \mathbf{0}^\text{T}$ 可以得到 $\mathbf{u}^\text{T}A\mathbf{x} \le \mathbf{u}^\text{T}\mathbf{b}$；然后在对偶问题中通过 $\mathbf{u}^\text{T}A \ge \mathbf{c}^\text{T}$ 和 $\mathbf{x} \ge 0$ 得到 $\mathbf{u}^\text{T}A\mathbf{x} \ge \mathbf{c}^\text{T}\mathbf{x}$。根据强对偶性，我们有：
$$
\mathbf{u}^\text{T}\mathbf{b} = \mathbf{u}^\text{T}A\mathbf{x} = \mathbf{c}^\text{T} \mathbf{x} \tag{13}
$$
从这个连等式我们可以得到：
$$
\begin{align*}
	\sum_{i=1}^n(\mathbf{u}^\text{T}A_i - c_i)x_i = 0 &&
	\sum_{i=1}^mu_i(b_i - \mathbf{a}_i^\text{T}\mathbf{x}) = 0
\end{align*} \tag{14} \label{complementary-slackness}
$$
注意到我们在原始问题和对偶问题中的约束条件 $\mathbf{a}_i^\text{T}\mathbf{x} \le b_i$ 和 $\mathbf{u}^\text{T}A_i - c_i \ge 0$，因此上面的求和公式中，要么 $u_i = 0$，要么 $\mathbf{a}_i^\text{T} = b_i$；且要么 $\mathbf{u}^\text{T}A_i = c_i$，要么 $x_i = 0$。这就让我们得到了下面的定理：

> **互补松弛性（Complementary Slackness）**：假设 $\mathbf{x}$ 是原始问题的最优解，而 $\mathbf{u}$ 是对偶问题最优解。此时：
>
> - 对于任意 $i = 1, ..., m$，或者原始问题中第 $i$ 个约束条件中的等号成立，或者 $u_i = 0$。
> - 对于任意 $i = 1, ..., n$，或者对偶问题中第 $i$ 个约束条件中的的等号成立，或者 $x_i = 0$。

上面定理得名于这样的术语：一个 $\le$ 或 $\ge$ 约束在等号成立时被称为 **紧致的（Tight）**，否则就称为 **松弛的（Slack）**。由于对于每一对 $u_i$ 和 $\mathbf{a}_i^\text{T} - b_i$ 或 $\mathbf{u}^\text{T}A_i - c_i$ 和 $c_i$ 中必定至少有一个是松弛的，我们将其称为”互补” 松弛性。虽然这个结论只是通过上一节给出的两种情形下推出的，其它情况下可以得到相同的结论。下面有一个更强的结论：

> - 设 $\mathbf{x}$ 是原始问题的一个可行解，$\mathbf{u}$ 是对偶问题的一个可行解。如果 $\mathbf{x}$ 和 $\mathbf{u}$ 均满足互补松弛性，则它们都是最优解。

该结论是显然成立的，因为互补松弛性暗示了可行解与每个约束的乘积都是 $0$。进行逆推后很容易得到 (13) 式，这也就证明了可行解最优。

互补松弛性启示我们的是，$u_i$ 本身表示了原始问题中第 $i$ 个约束条件对最优解的意义大小。当 $\mathbf{a}_i^\text{T} - b_i \le 0$ （约束是松弛的）时，这个约束条件是没有意义的，因此 $u_i = 0$。反过来，当对偶问题中的约束是松弛的时候，它对应的 $x_i = 0$。

下面举一个例子进行说明：
$$
\begin{align*}
	\underset{x_1, x_2, x_3 \in \R}{\text{maximize}} \quad & 3x_1 + 2x_3 \\
	\text{subject to} \quad & x_1 + x_2 + x_3 \le 6 \\
	& 2x_1 - x_2 + x_3 \le 3 \\
	& 3x_1 + x_2 - x_3 \le 3 \\
	& x_1, x_2, x_3 \ge 0 
\end{align*}
$$
假设我们得到了这个线性优化问题的一个可行解 $(0, 1.5, 4.5)$，现在需要验证该可行解是一个最优解。我们首先寻找到它的对偶问题：
$$
\begin{align*}
	\underset{u_1, u_2, u_3 \in \R}{\text{minimize}}\quad & 6u_1 + 3u_2 + 3u_3 \\
	\text{subject to} \quad & u_1 + 2u_2 + 3u_3 \ge 3 \\
	& u_1 - u_2 + u_3 \ge 0 \\
	& u_1 + u_2 - u_3 \ge 2 \\
	& u_1, u_2, u_3 \ge 0 
\end{align*}
$$
将原始问题可行解代入原始问题的约束条件后，我们发现前两个都是紧致的，而第三个是松弛的。因此我们有 $u_3 = 0$。另一方面，由于 $x_2, x_3 > 0$，对偶问题中的第二和第三个约束条件必然不松弛，因此我们将对偶问题转化为：
$$
\begin{align*}
	u_1 - u_2 + u_3 = 0 \\
	u_1 + u_2 - u_3 = 2 \\
	u_3 = 0
\end{align*}
$$
这样我们就得到了解 $(1, 1, 0)$。经过检验发现这个解确实是可行解。此时根据互补松弛性的第三条，我们知道 $(0, 1.5, 4.5)$ 确实是原始问题的最优解。

### 对偶性与简单形表

假设我们有一个线性规划问题（以及其对偶问题）：
$$
(\mathbf{P})
\begin{cases}
\displaystyle
	\underset{x, y \in \R}{\text{maximize}} \quad & 2x + 3y \\
	\text{subject to} \quad & -x + y \le 3 \\
	& x - 2y \le 2 \\
	& x + y \le 7 \\
	& x, y \ge 0
\end{cases}
\quad
(\mathbf{D})
\begin{cases}
	\underset{u, v, w \in \R}{\text{minimize}} \quad & 3u + 2v + 7w \\
	\text{subject to} \quad & -u + v + w \ge 2 \\
	& u - 2v + w \ge 3 \\
	& u, v, w \ge 0
\end{cases}
\nonumber
$$
我们可以利用简单形表求解原始问题：
$$
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	s_1 & -1 & 1 & 1 & 0 & 0 & 3 \\
	s_2 & 1 & -2 & 0 & 1 & 0 & 2 \\
	s_3 & 1 & 1 & 0 & 0 & 1 & 7 \\\hline
	-z & 2 & 3 & 0 & 0 & 0 & 0
\end{array}
\quad\leadsto \quad
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	y & 0 & 1 & 1/2 & 0 & 1/2 & 5 \\
	s_2 & 0 & 0 & 3/2 & 1 & 1/2 & 10 \\
	x & 1 & 0 & -1/2 & 0 & 1/2 & 2 \\\hline
	-z & 0 & 0 & -1/2 & 0 & -5/2 & -19
\end{array}
\nonumber
$$
（一个值得注意的点：我们通过引入松弛变量将原始约束变成了一系列等式，这原本会让对偶变量 $u, v, w$ 变成无限制的变量。但恰好，引入的 $s_1, s_2, s_3$ 在对偶约束中对应的是 $u, v, w \ge 0$，因此没有任何影响）

我们之前已经多次求结果这样的问题，因此可以立刻知道它的（原始）最优解是 $(2, 5)$。不过其对应的对偶最优解是什么呢？首先请回忆[修改后的简单形法](# 修改后的简单形法)里我们给出的差额成本公式：
$$
r_j = c_j - \mathbf{u}^\text{T}A_j
$$
此处的 $\mathbf{u}^\text{T} = \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}$。给它起名为 $\mathbf{u}$ 并不是巧合，实际上它最终就是我们想要的对偶最优解。

> **引理**：如果 $\mathcal{B}$ 是原始问题最优解的基，则 $\mathbf{u}^\text{T} = \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}$ 是对偶问题的最优解。

> **证**：为了证明它是最优解，我们首先应该证明其是一个对偶可行解。我们已经知道在得到原始最优解时有 $r_j \le 0$，因而 $\mathbf{u}^\text{T}A_j \ge c_j$，这恰好是对偶约束，因此 $\mathbf{u}$ 确实是对偶可行的。然后，由于：
> $$
> \mathbf{u}^\text{T}\mathbf{b} = \mathbf{c}_\mathcal{B}^\text{T}A_\mathcal{B}^{-1}\mathbf{b} = \mathbf{c}_\mathcal{B}^\text{T}\mathbf{p} = \mathbf{c}^\text{T}\mathbf{x} = z_0 \nonumber
> $$
> 当对偶问题的目标函数等于原始问题的目标函数时，它就是最优解。

下面，尝试观察引入松弛变量的情形。此时有 $\mathbf{r}_\mathcal{S}^\text{T} = \mathbf{c}_\mathcal{S}^\text{T} - \mathbf{u}^\text{T}A_\mathcal{S}^\text{T}$，由于 $\mathcal{c}_\mathcal{S}^\text{T} = \mathbf{0}$ 且 $A_\mathcal{S}^\text{T} = I$，我们有：
$$
\mathbf{u}^\text{T} = -\mathbf{r}_\mathcal{S}^\text{T} \tag{15}
$$
这倒是一个意外收获。简单来说，当为线性优化问题引入松弛变量时，其差额成本所在行就是对偶问题的解的相反数。不过需要注意原始问题是最小化问题的情形，此时对于 $\sum a_ix_i \ge b$ 应该化为 $-\sum a_ix_i + s = -b$（使所有约束乘上 $-1$）。这样的变化会导致对偶问题的解是原来的负数，因此有 $\mathbf{u}^\text{T} = \mathbf{r}_\mathcal{S}^\text{T}$。

现在我们有了一个计算对偶最优解的公式，并发现它和原始最优解时的目标函数是一样的。这就是强对偶性的证明（这并不是一个严格的证明）。通过对偶性，我们对于任意线性优化问题，都有下面的结论：

- 原始问题的目标函数总是不大于对偶问题的目标函数（求最大值时），因此如果两者中任一个无界，另一个一定不可能无界。（弱对偶性）
- 原始问题和对偶问题中一旦一个有最优解，另一个也有最优解。（强对偶性）

下面是更加宽泛的结论：

- 只要原始问题有最优解，那么对偶问题也存在最优解。它们的目标函数相同。反之亦然。
- 原始问题和对偶问题中只可能有一个无界，此时另一个没有可行解。
- 原始问题和对偶问题可能同时没有可行解。

#### 对偶可行表

特别注意，前面我们给出的计算对偶最优解的公式，是建立在 $r_j \le 0$ 的基础上的（否则这个解甚至不是对偶可行的）。因此更宽泛地说，一个简单形表是 **对偶可行的（Dual Feasible）** 当且仅当其差额成本均为非正数（这是原始问题求最大值时，否则要求非负数）。但从这个判断标准来看，无论 $\mathbf{b}$ 的值是什么都不会影响对偶问题解的可行性（尽管当其中存在负数时，这就不对应原始可行解了），比如下面这个例子：
$$
\begin{aligned}
	\underset{x, y \in \R}{\text{minimize}} \quad & x + y \\
	\text{subject to} \quad & 2x + y \ge 6 \\
	& 3x + y \ge 7 \\
	& x + 2y \ge 9 \\
	& x, y \ge 0
\end{aligned} 
\qquad\leadsto\qquad
\begin{aligned}
	\underset{x, y, s_1, s_, s_3 \in \mathbb{R}}{\text{minimize}} \quad & x + y \\
	\text{subject to} \quad & -2x - y + s_1 = -6 \\
	& -3x - y + s_ = -7 \\
	& -x - 2y + s_3 = -9 \\
	& x, y, s_1, s_2, s_3 \ge 0
\end{aligned}
$$
写成 $\mathcal{B} = \{s_1, s_2, s_3 \}$ 的简单形表：
$$
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	s_1 & -2 & -1 & 1 & 0 & 0 & -6 \\
	s_2 & -3 & -1 & 0 & 1 & 0 & -7 \\
	s_3 & -1 & -2 & 0 & 0 & 1 & -9 \\\hline
	-z & 1 & 1 & 0 & 0 & 0 & 0
\end{array}
\nonumber
$$
由于 $s_1, s_2, s_3$ 都是负数，这不是一个原始可行解，然而 $(0, 0, 0)$ 确实是对偶可行解。这给我们一个启示：很多时候较难找到一个原始可行解，但可以轻松地找到一个对偶可行解（比如通过引入松弛变量）。此时我们可以采用两阶段的简单形法，逐步得到最优解。

#### 对偶简单形法

我们也可以干脆直接通过优化对偶问题的可行解来得到最优解，这被称为 **对偶简单形法（Dual Simplex Method）**。比如在上一节的例子中，我们可以通过行变换来将 $\mathbf{p}$ 的值全部变为正数，以得到原始可行解。就和简单形法中，从 $-z$ 行挑出一个非负数（最大值问题）类似，我们应该从 $\mathbf{p}$ 中挑出一个非正数，然后从它所在行中挑选一个负数，使得 $r_i/a_{ij}$ 最小。这样我们就可以让 $i$ 离开基，并引入 $j$ 了。这里需要让 $r_i/a_{ij}$ 最小的原因和简单形法中让 $r_i/a_{ij}$ 最小的原因类似，如果我们选择比值更大的，就无法让得到的解对偶可行（$-z$ 行会变成负数）。上一节中的例子可以由此化为下表：
$$
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	x & 1 & 1/2 & -1/2 & 0 & 0 & 3 \\
	s_2 & 0 & 1/2 & -3/2 & 1 & 0 & 2 \\
	s_3 & 0 & -3/2 & -1/2 & 0 & 1 & -6 \\\hline
	-z & 0 & 1/2 & 1/2 & 0 & 0 & -3
\end{array} \nonumber
$$
如果想要深究这样选择的原因，首先考虑用 $j$ 代替 $i$ 成为基础变量时，$i$ 行的系数会变为：
$$
\begin{bmatrix}
	\frac{a_1}{a_j} & ... & \frac{a_{j-1}}{a_{j-1}} & 1 & \frac{a_{j+1}}{a_{j+1}} & ... & \frac{a_k}{a_j}
\end{bmatrix}
\nonumber
$$
此时差额成本会变为：
$$
\begin{bmatrix}
	r_1 - r_j\cdot\frac{a_1}{a_j} & ... & r_{j-1} - r_j\cdot\frac{a_{j-1}}{a_j} & 0 & r_{j+1} - r_j\cdot\frac{a_{j+1}}{a_j} & ... & r_k - r_j \cdot \frac{a_k}{a_j} \nonumber 
\end{bmatrix}
$$
为了让这一串数和原来的符号相同，我们需要满足：
$$
|r_j| \ge \left|r_i\cdot\frac{a_j}{a_i}\right| \implies \left|\frac{r_j}{a_j}\right| \ge \left|\frac{r_i}{a_i}\right| \nonumber
$$
这也即是我们此前给出的结论。

现在重新回到前面的例子，我们可以进一步将 $s_3$ 移出基，并引入 $y$：
$$
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	x & 1 & 0 & -2/3 & 0 & 1/3 & 1 \\
	s_2 & 0 & 0 & -5/3 & 1 & 1/3 & 0 \\
	y & 0 & 1 & 1/3 & 0 & -2/3 & 4 \\\hline
	-z & 0 & 0 & 1/3 & 0 & 1/3 & -5
\end{array}
\nonumber
$$
现在，$\mathbf{b} \ge \mathbf{0}$，因此 $(1, 4)$ 是一个原始可行解，其目标函数达到最小值 $5$。

对偶简单形法的作用可以总结如下：

- 在由不等式组成的线性优化问题中，我们可以引入松弛变量，此时简单形表直接就是对偶可行的。对最小值问题，我们需要所有成本都是非负数；对最大值问题则需要所有成本都是非正数。

- 有时我们只需要找到一个可行解，这时可以通过设置虚构目标函数的方式得到答案，比如：

  - 最小化 $0$，这是对非负成本问题的最简单的目标函数。不过这可能会导致简单形表退化。
  - 最小化 $x_1 + ... + x_n$，这对于非负成本问题也是有效的。
  - 最小化一个我们认为可以得到不错的基本可行解的函数。比如，如果实际上的目标函数是最大化 $x - y$，我们可以尝试最小化 $y$。因为 $y$ 取最小值的时候 $x - y$ 很可能是更大的。

  全部的步骤是：找到一个虚构目标函数，用对偶简单形法最小化这个目标函数；最后从最后一个简单形表开始，用简单形法解原来的问题。

- 如果需要对一个对偶可行的简单形表，如果添加一个额外的约束，此时可以通过行变换以吻合当前的基，然后再通过对偶简单形法使其原始可行。

作为例子，让我们看看下面这个可行性问题：
$$
\begin{cases}
	x_1 + 3x_2 \le 5 \\
	x_1 - x_2 \ge 1 \\
	x_1, x_2 \ge 0
\end{cases}
\nonumber
$$
此时我们只需要随意设置一个目标函数，比如最小化 $x_1 + x_2$，这样就可以得到一个对偶可行表：
$$
\begin{array}{ccccc|c}
	 & x_1 & x_2 & s_1 & s_2 & \\\hline
	s_1 & 1 & 3 & 1 & 0 & 5 \\
	s_2 & -1 & 1 & 0 & 1 & -1 \\\hline
	-z^\alpha & 1 & 1 & 0 & 0 & 0
\end{array}
\nonumber
$$
我们实际上并不在意目标函数究竟如何，只需按照前面的步骤将其变为原始可行表即可。之后，如果原问题本身有一个目标函数，我们可以再继续求解。

不过，有时可行性问题中给出的是等式而非不等式，这种情况下我们没法立刻得到对偶可行解。因此我们只能先找到一组基本变量。比如下面这个线性方程组可以转换成右侧的形式：
$$
\begin{cases}
	x_1 + 2x_2 - 3x_3 + x_4 = 5 \\
	2x_1 + 4x_2 + x_3 - 2x_4 = 2 \\
	x_1, x_2, x_3, x_4 \ge 0
\end{cases}
\quad \leadsto \quad
\begin{cases}
	x_1 + 2x_2 - \frac{5}{7}x_4 = \frac{11}{7} \\
	x_3 - \frac{4}{7}x_4 = -\frac{8}{7} \\
	x_1, x_2, x_3, x_4 \ge 0
\end{cases}
\nonumber
$$
这下，$(x_1, x_3)$ 就成为了基本变量，我们可以通过设置虚构目标函数，最小化 $x_2 + x_4$ 的方式找到原始可行解。

最后，让我们举一个增加约束的例子：
$$
\begin{align*}
	\underset{x, y \in \R}{\text{minimize}} \quad & x + y \\
	\text{subject to} \quad & 2x + y \ge 6 \\
	& 3x + y \ge 7 \\
	& x + 2y \ge 9 \\
	& x, y \ge 0 
\end{align*}
$$
从它得到的简单形表是：
$$
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	x & 1 & 0 & -2/3 & 0 & 1/3 & 1 \\
	s_2 & 0 & 0 & -5/3 & 1 & 1/3 & 0 \\
	y & 0 & 1 & 1/3 & 0 & -2/3 & 4 \\\hline
	-z & 0 & 0 & 1/3 & 0 & 1/3 & -5
\end{array}
\nonumber
$$
现在我们想加入一个新的约束 $x \ge 2$，即 $-x + s_4 = -2$，此时可以将它直接塞到简单形表中：
$$
\begin{array}{ccccccc|c}
	 & x & y & s_1 & s_2 & s_3 & s_4 & \\\hline
	x & 1 & 0 & -2/3 & 0 & 1/3 & 0 & 1 \\
	s_2 & 0 & 0 & -5/3 & 1 & 1/3 & 0 & 0 \\
	y & 0 & 1 & 1/3 & 0 & -2/3 & 0 & 4 \\
	s_4 & -1 & 0 & 0 &0 & 0 & 1 & -2 \\\hline
	-z & 0 & 0 & 1/3 & 0 & 1/3 & 0 & -5
\end{array}
\nonumber
$$
将 $s_4$ 行适应已有的基 $(x, y, s_2, s_4)$，可以得到：
$$
\begin{array}{ccccccc|c}
	 & x & y & s_1 & s_2 & s_3 & s_4 & \\\hline
	x & 1 & 0 & -2/3 & 0 & 1/3 & 0 & 1 \\
	s_2 & 0 & 0 & -5/2 & 1 & 1/3 & 0 & 0 \\
	y & 0 & 1 & 1/3 & 0 & -2/3 & 0 & 4 \\
	s_4 & 0 & 0 & -2/3 & 0 & 1/3 & 1 & -1 \\\hline
	-z & 0 & 0 & 1/3 & 0 & 1/3 & 0 & -5
\end{array}
\nonumber
$$
接下来只需要用对偶简单形法将其变为原始可行的即可。

#### 敏感性分析

两阶段的简单形法

### 零和游戏

让我们考虑一个只有两个玩家（我们称他们为 Alice 和 Bob）的游戏。**零和游戏（Zero-Sum Game）** 是指，两个玩家中得胜者的获利正好来自于失败者的损失，也即他们的总收益为 $0$。因此我们只需要用一个变量来表示两人的盈亏情况。假设 $x$ 是 Alice 的收益，那么她的目标就是最大化 $x$，而 Bob 的目标是将其最小化。

接下来，让我们介绍一些零和游戏，并分析 Alice 和 Bob 分别的游戏策略。

#### 主导策略

有一个叫做“更大的数”的游戏，两位玩家举起 1、2 或 3根手指；举起较少手指的玩家需要给另一位玩家 $1。下面是 Alice 的 **盈亏表（Payoff Table）**：
$$
\begin{array}{c|ccc}
 & \text{Bob: 1} & \text{Bob: 2} & \text{Bob: 3} \\\hline
 \text{Alice: 1} & 0 & -1 & -1 \\
 \text{Alice: 2} & 1 & 0 & -1 \\
 \text{Alice: 3} & 1 & 1 & 0 
\end{array}\nonumber
$$
可以看到，无论 Bob 做出什么选择，Alice 的最优解都是展示最多数量的手指（反过来对 Bob 也是一样的）。此时我们称这种策略 **主导（Dominate）** 了其它的任何策略，因为不论另外的玩家作出的选择，该策略都能达到最优解。虽然零和游戏中不一定出现主导策略（主导所有其它策略），但如果一个策略能够主导另一个策略，后一种策略可以直接被我们排除；它不可能是最优解。通过这种方式我们可以简化盈亏表。

#### 鞍点

下面是另一个游戏（我们不用在意它宏观上的规则，只需观察它）的盈亏表：
$$
\begin{array}{c|ccc}
 & B_1 & B_2 & B_3 \\\hline
A_1 & 0 & -1 & 3 \\
A_2 & 1 & 3 & 2 \\ 
A_3 & -1 & 4 & 2
\end{array} \nonumber
$$
显然上面的游戏中不存在主导策略，但是 $A_2$ 是对 Alice 的一个最优的选择：无论 Bob 作何策略，Alice 都可以盈利。而 $B_1$ 对 Bob 是一个最优的选择，因为无论 Alice 作何策略，Bob 的亏损都不会超过 1。显然这个游戏对 Bob 并不公平，不过这并不是重点。我们将 $(A_2, B_1)$ 这样的在行中取最小值且在列中取最大值的点称为 **鞍点（Saddle Point）**。当盈亏表中存在鞍点时，双方选择这个策略总能保证其收益至少（或亏损至多）为鞍点指示的值。

#### 混合策略

考虑一个叫做“奇偶”的游戏。其中 Alice 和 Bob 要么举起一根手指或者两根手指。设两数之和为 $N$。如果 $N$ 是一个奇数，则 Bob 给 Alice $N，反之 Alice 给 Bob \$N。此时 Alice 的盈亏表为：
$$
\begin{array}{c|cc}
 & B_1 & B_2 \\\hline
A_1 & -2 & 3 \\
A_2 & 3 & -4
\end{array} \nonumber
$$
可以看到，这个游戏中既不存在主导策略，也不存在鞍点给予提示。如果 Alice 知道 Bob 选择什么，显然她能够立刻选择正确的对应策略，并稳赚 \$3，可惜她不能知道。假设 Alice 决定抛硬币决定自己的策略，也即 $A_1$ 和 $A_2$ 各占 $50\%$ 的可能性，此时盈亏表需要通过数学期望得到：
$$
\begin{array}{c|cc}
 & B_1 & B_2 \\\hline
A_? & 1/2 & -1/2
\end{array} \nonumber
$$
此时从统计上来看，Alice 能保证无论 Bob 做怎样的选择，都最多亏损 $1/2$；Bob 只需选择 $B_2$ 就能保证赚 $1/2$。我们将 Alice 用随机方式从 $a$ 种策略中选择一种的方式称为 **混合策略（Mixed Strategy）**。假设每种策略的概率是 $x_1,... x_a$，其满足 $x_1 + ... + x_a = 1$，且盈亏表可以用 $A$ 表示，则采用混合策略后的期望盈亏表是 $\mathbf{x}^\text{T} A$。类似地，如果 Bob 随机从 $b$ 种策略中选择一种方式的概率为 $y_1, ..., y_b$，则期望盈亏表是 $A\mathbf{y}$。当两者都采用混合策略时，最终得到的期望盈亏表则是：
$$
A' = \mathbf{x}^\text{T}A\mathbf{y}
$$

#### 最优最劣策略

Alice 当然希望能够选择 $\mathbf{x}$ 的最优解，但是由于她不清楚 $\mathbf{y}$ 具体是多少（这取决于 Bob），因此一个策略是考虑对 $\mathbf{x}$ 最差的情况下，能达到的最优解，我们将其称为 **最优最劣策略（Maxmin Strategy）**。显然很多情况下这并不是理想的选择，在一些非零和游戏中，这会导致严重的失误。比如考虑一个“赢、输或复读”游戏中，Bob 可以选择赢取 \$100 或输掉 $100，而 Alice 可以选择什么都不做或复读 Bob。根据最优最劣策略，Alice 应该每次都选择什么都不做，因为不理想的情况下，Bob 会选择输掉 \$100，而如果复读，Alice 也会输掉 \$100，此时选择什么都不做才是正确的选择。然而事实上这是愚蠢的策略，因为显然 Bob 每次都会选择赢取 \$100，而 Alice 通过复读可以保证每次也赢取 \$100。

#### 线性优化算法

我们这里直接给出结论：当 Alice 采用确定的混合策略 $\mathbf{x}$ 时，存在让 Bob 最受益的唯一策略。证明如下：

> **证**：当 Alice 采用混合策略 $\mathbf{x}$ 时，她的盈亏表就变为 $\mathbf{x}^\text{T}A$。
>
> - 如果 Bob 采用唯一的策略，比如每次都会选择 $B_i$，其中 $i \in \{1, ..., b\}$，Alice 的盈亏将是 $(\mathbf{x}^\text{T}A)_i$。
>
> - 如果 Bob 采用混合策略，此时盈亏表变为 $\mathbf{x}^\text{T}A\mathbf{y}$，其相当于：
>   $$
>   (\mathbf{x}^\text{T}A)_1y_1 + \dots + (\mathbf{x}^\text{T}A)_by_b\nonumber
>   $$
>   此时一定存在一个最小值 $(\mathbf{x}^\text{T}A)_i$ 作为 Alice 的最劣情形；如果进行加权，就等价于在一些情况下选择对 Bob 的最优解，而一些情况下选择其它的解。显然这比不上唯一策略。

在这个引理下，我们可以找出 Alice 进行时的最劣情形，即：
$$
\min{\{(\mathbf{x}^\text{T}A)_1,...,(\mathbf{x}^\text{T}A)_b \}} \nonumber
$$
为了让其适应线性优化问题，我们设 $u = \min\{(\mathbf{x}^\text{T}A)_1,...,(\mathbf{x}^\text{T}A)_b\}$，则下面的线性优化问题就是 Alice 采用最优最劣策略时寻找的最优情形（其中设 $\mathbf{1}$ 为每一项都是 $1$ 的 $b$ 维向量）：
$$
\begin{align*}
\underset{\mathbf{x} \in \R^a, u \in \R}{\text{maximize}} \quad & u \\
\text{subject to} \quad & u\mathbf{1}^\text{T} - \mathbf{x}^\text{T}A \le \mathbf{0}^\text{T} \\
& \mathbf{x}^\text{T}\mathbf{1} = 1 \\
& \mathbf{x} \ge \mathbf{0}
\end{align*}
$$
类似地，我们也可以找到 Bob 采用 **最劣最优策略（Minmax）** 时（即考虑 Alice 最优选择下的最差情形）的线性优化问题，此时假设 $v = \max\{(A\mathbf{y})_1, ..., (A\mathbf{y})_a\}$：
$$
\begin{align*}
\underset{\mathbf{y} \in \R^b, v \in \R}{\text{minimize}} \quad & v \\
\text{subject to} \quad & \mathbf{1}v - A\mathbf{y} \ge \mathbf{0} \\
& \mathbf{1}^\text{T}\mathbf{y} = 1 \\
& \mathbf{y} \ge \mathbf{0}
\end{align*}
$$
有趣地是，我们发现 Alice 和 Bob 这两个线性优化问题是相互对偶的。这会导致一个更加有趣的结果：Alice 和 Bob 所采用的策略会将他们引导到相同的最优解，Alice 会得到最佳收益为 $z^*$ 时，Bob 的最佳收益就是 $-z^*$。其中的逻辑和此前介绍的鞍点情形颇为类似：Alice 找不到其它的选项令其最小收益大于 $z^*$，而 Bob 找不到其它选项令其最大损失小于 $z^*$（也就是最小收益大于 $-z^*$），因此这就是最优解。

#### 举例

我们以此前介绍的“奇偶”游戏为例，我们可以给出 Alice 的线性优化问题：
$$
\begin{align*}
\underset{\mathbf{x} \in \R^2, u \in \R}{\text{maximize}} \quad & u \\
\text{subject to} \quad & u \le -2x_1 + 3x_2 \\
& u \le 3x_1 - 4x_2 \\
& x_1 + x_2 = 1 \\
& x_1, x_2 \ge 0 \\
& u \ \text{is unrestricted}
\end{align*}
$$
这个问题的最优解是 $(x_1, x_2, u) = (7/12, 5/12, 1/12)$。具体来说，Alice 只需在 $7/12$ 情况下举一根手指，在 $5/12$ 情况下举两根手指，就能保证至少 $1/12$ 的收益。

### Fourier-Motzkin 消元法

现在我们已经有足够的知识解决诸如给定矩阵 $A$ 和向量 $\mathbf{b}$，求集合 $\{\mathbf{x} \in \R^n \mid A\mathbf{x} \le \mathbf{b}\}$ 是否为空的问题，也即线性优化问题是否存在可行解。比如使用对偶简单形法：

- 将约束变为等式形式。
- 通过高斯消元法得到一个基本解（不一定是可行解）。
- 设一个目标函数使得其组成一个对偶可行解。
- 利用对偶简单形法让其产生原始可行解。

事实上，可行性问题和优化问题的难度是相当的。根据强对偶性，我们可以将任意优化问题：
$$
\max\{\mathbf{c}^\text{T}\mathbf{x} \mid A\mathbf{x} \le \mathbf{b}, \mathbf{x} \ge\mathbf{0}\} \nonumber
$$
转化为等价的可行性问题：
$$
\{(\mathbf{x}, \mathbf{u}) \in \R^{n+m}\mid \mathbf{c}^\text{T}\mathbf{x} = \mathbf{u}^\text{T}\mathbf{b}, A\mathbf{x} \le \mathbf{b}, \mathbf{u}^\text{T}A \ge \mathbf{c}, \mathbf{x} \ge \mathbf{0}, \mathbf{u} \ge \mathbf{0}\} \nonumber
$$
因此，当我们找到下面这个问题的解时，也就自然地得到了优化问题的解。

这里让我们介绍一个简单形法以外的，解决可行性问题的方法，即 **Fourier-Motzkin 消元法**。虽然在大型问题上会变得极其复杂，但是它的思路还是比较有趣的。其主要思想如下：

- 将一个有 $n$ 个变量的问题转化为等价的有 $n - 1$ 个变量的问题，直到最后只剩下一个变量。如果此时的上限和下限之间存在数字，我们可以为最后的变量设定一个值，然后代入上一个问题（本来有两个变量，现在也只剩下一个变量）。以此类推，最终我们就能找到原问题的可行解。

- 作为一个通用的例子，我们设其中一条约束为 $a_1x_1 + ... + a_nx_n \le b$，如果我们要消除 $x_n$，就可以将此不等式化为：
  $$
  x_n \le \frac{b}{a_n} - \frac{a_1}{a_n}x_1 - ... - \frac{a_{n-1}}{a_n}x_{n-1} \quad (a_n > 0) \nonumber
  $$
  或：
  $$
  x_n \ge \frac{b}{a_n} - \frac{a_1}{a_n}x_1 - ... - \frac{a_{n-1}}{a_n}x_{n-1} \quad (a_n < 0) \nonumber
  $$
  每一个约束都可以列出上面两个式子中的一个，这样我们就得到一系列 $x_n$ 的上限和下限。为了消除 $x_n$，我们只需将这些上下限一一组合并列出不等式。对于下限 $L_1,...,L_n$ 和上限 $U_1, ..., U_m$，我们能够列出下一步的不等式为：
  $$
  \begin{matrix}
  	L_1 \le U_1 & L_1 \le U_2 & \dots & L_1 \le U_m \\
  	L_2 \le U_1 & L_2 \le U_2 & \dots & L_2 \le U_m \\
  	\vdots & \vdots & \ddots & \vdots \\
  	L_n \le U_1 & L_n \le U_2 & \dots & L_n \le U_m
  \end{matrix} \nonumber
  $$

这似乎看起来就有很高的复杂度。最差情形下（每次的不等式中都有一半为下限，一半为上限），其复杂度为 $O(2^{2^{n-1} + 2})$。这比指数复杂度还要高上不知道多少数量级。



## 图论问题

本章中让我们讨论图中的线性优化问题。

### 二分图匹配

考虑下面的一些情景：

- 你有 $n$ 名员工和 $m$ 份工作，每位员工都能做其中的一些工作。
- 出租车公司有 $m$ 辆出租车，现在有 $n$ 位等待用车的顾客，每辆车只能为其中的一些顾客提供服务（考虑到时间和地点）。
- 云计算服务需要将 $n$ 项任务分配给 $m$ 台服务器，不过每项任务都只有一些服务器可以完成（这取决于服务器的特点，如内存、处理器性能等）。

上面这些问题都可以抽象为 **二分图匹配（Bipartite Matching）**问题。我们将其中的所有对象（如员工、顾客、服务器等）都抽象成 **顶点（Vertex）**。显然对于每张图，我们都可以将顶点分为两种类型，这也是为什么我们称其为 **二分图（Bipartite Graph）**。我们用 **边（Edge）**将这两个部分中各取的两个顶点连在一起，以表示这两者之间存在联系，如下图所示：

<img src="graphs/lp10.png" alt="lp10" style="zoom:50%;" />

我们将一个图中，不包括重复顶点的边组成的子集称为 **匹配（Match）**。联系我们给出的实际问题，我们需要做的是找出图中最大的匹配（即包含最多不相交边的匹配）。

上面这个情形很容易总结为线性优化问题。设图的顶点可以分为两个部分 $X = \{1, ..., n\}$ 和 $Y = \{n + 1, ..., n + m\}$。为了表示出图中边的选取情况，我们用 $x_{ij}$ 来表示 $i \in X$ 和 $j \in Y$ 这两个顶点连起来的边是否在匹配中。下面就是一个例子：

<img src="graphs/lp11.png" alt="lp11" style="zoom:50%;" />

根据图中可以选择的边，我们可以写出下面等价的线性优化问题：
$$
\begin{align*}
\underset{x_{ij} \in \R}{\text{maximize}} \quad & x_{14} + x_{16} + x_{25} + x_{27} + x_{34} + x_{35} \\
\text{subject to} \quad & x_{14} + x_{16} \le 1 \\
& x_{25} + x_{27} \le 1 \\
& x_{14} + x_{34} \le 1 \\
& x_{25} + x_{35} \le 1 \\
& x_{16} \le 1 \\
& x_{27} \le 1 \\
& x_{14}, x_{16}, x_{25}, x_{27}, x_{34}, x_{35} \ge 0
\end{align*}
$$
不过这回引起一个不得不重视的问题，这个线性优化问题的最优解如果存在分数怎么办？实际上，二分图匹配问题的最优解 *总是*一系列整数。我们下面来证实这一点。

#### 全幺模矩阵

对于基为 $\mathcal{B}$ 的线性方程 $A\mathbf{x} = \mathbf{b}$，我们得到的解是 $\mathbf{x}_\mathcal{B} = A_\mathcal{B}^{-1}\mathbf{b}$，且有 $\mathbf{x}_\mathcal{N} = \mathbf{0}$。为了让 $\mathbf{x}_\mathcal{B}$ 包含的都是整数，显然 $A_\mathcal{B}^{-1}$ 理应是一个全是整数的矩阵，$\mathbf{b}$ 也应该只包含整数。

> **引理**：方阵 $M$ 是一个整数矩阵（所有项都是整数），则当且仅当 $\det{M} = \pm 1$ 时 $M^{-1}$ 也是一个整数矩阵。

> **证**：当两者都是整数矩阵时，它们的行列式都应该是整数。由于 $\det{M} \cdot \det{M^{-1}} = 1$ 满足这个条件的 $\det{M}$ 和 $\det{M^{-1}}$ 只可能是 $\pm 1$。
>
> 反过来，我们可以将 $M^{-1}$ 写成伴随矩阵的形式：
> $$
> M^{-1} =
> \begin{bmatrix}
> 	a & b \\ c & d
> \end{bmatrix}
> = \frac{1}{\det{M}}
> \begin{bmatrix}
> 	d & -b \\ -c & a
> \end{bmatrix}
> $$
> 由于 $a, b, c, d$ 均为整数，由于 $\det{M}$ 是 $\pm 1$，我们得到的一定是整数矩阵。

在线性优化问题中，我们没法预先知道每次选择的基是什么，因此我们对初始矩阵 $A$ 的要求更为苛刻：

> **定理**：给定的可行域为 $\{\mathbf{x} \in \R^n\mid A\mathbf{x} \le \mathbf{b}, \mathbf{x} \ge \mathbf{0}\}$ 的线性优化问题，其中 $\mathbf{b}$ 由整数组成，且 $A$ 任选的一个子方阵（任选 $n$ 行和 $n$ 列）的行列式都是 $0, \pm 1$ 中的一个，我们称这个矩阵是 **全幺模矩阵（Totally Unimodular Matrix）**。此时该线性优化问题的基本可行解一定是整数。

> **证**：解决线性优化问题时，我们首先会添加松弛变量，将 $A\mathbf{x} \le \mathbf{b}$ 改写成 $A\mathbf{x} + I\mathbf{s} = \mathbf{b}$。对于某一个基 $\mathcal{B}$，我们得到的基本解是 $A_\mathcal{B}^{-1}\mathbf{b}$，由于 $\mathbf{s}$ 一定在基中，我们可以将 $A_\mathcal{B}$ 想像成矩阵 $\begin{bmatrix} A & I \end{bmatrix}$ 的一个 $n\times n$ 的子方阵。假设 $A_\mathcal{B}$ 从 $A$ 中提取了 $k$ 列，而从 $I$ 中提取了 $n - k $ 列。根据行列式的性质，我们可以将 $\det{A_\mathcal{B}}$ 最终写成一个 $k\times k$ 的 $A$ 的子方阵的行列式，它的值一定是 $0$ 或 $\pm 1$。第一种该情况下，$\mathcal{B}$ 是无效的基，不存在基本可行解；后面的两种情况都能得到整数的基本可行解。下面是一个将 $A_\mathcal{B}$ 变为 $A$ 的子方阵的演示：
> $$
> \det\begin{bmatrix}
> 	1 & 5 & 0 & 0 \\
> 	2 & 6 & 1 & 0 \\
> 	3 & 7 & 0 & 0 \\
> 	4 & 8 & 0 & 1
> \end{bmatrix} =
> -\det\begin{bmatrix}
> 	1 & 5 & 0 \\
> 	3 & 7 & 0 \\
> 	4 & 8 & 1
> \end{bmatrix} = 
> -\det\begin{bmatrix}
> 	1 & 5 \\
> 	3 & 7
> \end{bmatrix} \nonumber
> $$
> 上面的矩阵并不是全幺模矩阵，不过不反感展示行列式的计算原理。

现在我们需要证明二分图匹配问题中，其对应矩阵 $A$ 是全幺模矩阵。首先将其线性优化问题抽象出来：
$$
\begin{align}
\begin{split}
	\underset{\mathbf{x} \in \R^{|E|}}{\text{maximize}} \quad & \sum_{(i, j) \in E}x_{ij} \\
	\text{subject to} \quad & \sum_{j:(i, j) \in E}x_{ij} \le 1 \quad \text{for each $i \in X$} \\
	& \sum_{i:(i, j) \in E}x_{ij} \le 1 \quad \text{for each $j \in Y$} \\
	& \mathbf{x} \ge \mathbf{0}
\end{split}
\end{align}
$$
我们将 **关联矩阵（Incidence Matrix）** 定义为一个 $(|X| + |Y|)\times|E|$ 的矩阵，其中的项 $A_{ve}$ 定义为顶点 $v \in X\cup Y$ 是否为边 $e \in E$ 的一个端点。下面就是一个例子：

<img src="graphs/lp12.png" alt="lp12" style="zoom:50%;" />

对于关联矩阵，我们能够将约束条件写成 $A\mathbf{x} \le \mathbf{1}$ 的形式，这和前面给出的线性优化问题中的约束是等价的。

> **定理**：二分图的关联矩阵总是全幺模的。

> **证明**：让我们用对任意关联矩阵的 $k\times k$ 子矩阵中的 $k$ 进行数学归纳，证明其行列式为 $0, \pm 1$ 中的一个。
>
> - 当 $k = 1$ 时，这个矩阵或者是 $[0]$ 或者是 $[1]$，因此它显然满足要求。
> - 如果 $k \ge 2$ 且其中有一列全部是 $0$，则它的行列式是 $0$。
> - 如果 $k \ge 2$ 且其中有一列只有一个 $1$，则可以将其转化为 $(k - 1)\times(k - 1)$ 的行列式，根据归纳假设，它的行列式一定是 $0, \pm 1$ 中的一个。
> - 如果 $k \ge 2$ 且其中有一列中有两个 $1$。记这个子矩阵为 $B$。将 $\mathbf{u} \in \R^k$ 定义为 $B$ 中第 $i$ 行是否对应 $X$ 中的一个顶点（是则 $u_i = 1$，否则 $u_i = -1$）。这样，$\mathbf{u}^\text{T}B$ 就给出了 $u_i + u_j$ 组成的向量，其中 $(i, j)$ 是 $B$ 中出现的列。由于 $(i, j)$ 只有两个端点 $i \in X$ 和 $j \in Y$，有 $u_i + u_j = 1 + (-1) = 0$，故 $\mathbf{u}^\text{T}B = \mathbf{0}^\text{T}$。这意味着 $B$ 的行之间线性相关，由行列式的性质有 $\det{B} = 0$。
> - 不可能出现一列中存在 3 个及以上 $1$ 的情况，因为列对应的边有且只有两个端点。
>
> 至此，我们证明了二分图的关联矩阵一定是全幺模的。

#### 顶点覆盖

正如所有其它线性优化问题，二分图匹配问题也有对偶问题。接下来让我们尝试找到这个问题。由于原始问题比较复杂，下面对它进行一些分析：

- 若 $x_i$ 是一个原始变量，$u_j$ 是一个对偶变量，则 $u_j$ 在第 $i$ 个对偶约束的系数等于 $x_i$ 在第 $j$ 个原始约束的系数。
- 特别地，当且仅当 $u_j$ 出现在第 $i$ 个对偶约束时，$x_i$ 才会出现在第 $j$ 个原始约束中。

下面是这个问题的详细内容：
$$
\begin{align*}
\begin{split}
	\underset{\mathbf{u} \in \R^{n+m}}{\text{minimize}} \quad & \sum_{i \in X\cup Y}u_i \\
	\text{subject to} \quad & u_i + u_j \ge 1 \quad \text{for each $(i, j)\in E$} \\
    & \mathbf{u} \ge \mathbf{0}
\end{split}
\end{align*}
$$
我们可以将这里的 $\mathbf{u}$ 理解成一集顶点 $S$，当 $i \in S$ 时有 $u_i = 1$，否则 $u_i = 0$。目标函数是 $|S|$。它的约束则是要求对于每个边 $(i, j) \in E$，其中至少有一个端点在 $S$ 中。用更加通俗的话来解释，就是寻找图中，能够涉及所有边的最小顶点集。我们将这个集合称为 **顶点覆盖（Vertex Cover）**。

> **定理**：在任意二分图中，最大匹配和最小顶点覆盖相等。

这点根据强对偶性是显然的。不过还需要证明对偶问题的最优解也全部是整数。对偶问题中的约束矩阵正好是原始问题中矩阵的转置，因此其子矩阵也是原始问题中子矩阵的转置，而转置矩阵和原矩阵的行列式相同，因此对偶问题依然是全幺模矩阵。

作为一个简单的拓展，在非二分图中，匹配问题和顶点覆盖问题依然满足弱对偶关系，即最大匹配小于等于最小顶点覆盖，不过强对偶性经常不能成立。

### 网络流

#### 最大流问题

现在让我们考虑另一类问题：一些资源需要从一个点运输到另一点，其中的运输信息可以抽象为一个图 $(N, A)$，称为 **网络（Network）**：

- $N$ 是 **结点（Node）** 集，表示网络中的不同地点。
- $A$ 是 **弧（Arc）** 集，表示网络中地点之间的有向连线，其中从 $i$ 流向 $j$ 的弧可以表示为 $(i, j)$ 的形式，其中 $i, j \in N$。

不难通过类比发现网络本质上就是一个有向图，其中的结点和弧等价于图中的顶点和边。我们首先要解决的是最大流问题。假设每个弧都有一个运载容量 $c_{ij}$，它是一个非负实数。有两个特殊的节点：**源（Source）** $s$ 和 **汇（Sink）** $t$ 表示资源运输的起点和终点。我们的目标是找到一条 $s$ 到 $t$ 的路径使得能够运输的资源最大化。

为了构建线性优化问题，设 $x_{ij}$ 为在 $(i, j)$ 上通过货物的 **流量（Flow）**，其需要满足：

- 容量约束：对于任意 $(i, j) \in A$ 都有 $x_{ij} \le c_{ij}$。

- 非负约束：对于任意 $(i, j) \in A$ 都有 $x_{ij} \ge 0$。

- 流量守恒：对于任意节点 $k \in N$，流向它的流量等于从它流出的流量，即：
  $$
  \sum_{i:(i, k) \in A}x_{ik} = \sum_{j:(k, j)\in A}x_{kj} \nonumber
  $$
  源和汇所在点 *不需要* 流量守恒。实际上，流向汇的流量与流出汇的流量之差正是汇接收的资源数量，我们称其为流量 $\mathbf{x}$ 的 **值（Value）**。这也是我们想要最大化的目标。不难发现这应该等同于流出源和流入源的流量之差。

为了简化最大流问题，我们假设任意两结点之间都存在一对反向的弧，只不过我们不需要弧 $(i, j)$ 时可以令 $c_{ij} = 0$。同时，流向源和流出汇的弧始终容量为 $0$。此时，流量 $\mathbf{x}$ 就可以简单表示为：
$$
\sum_{j:(s, j) \in A} x_{sj} \quad \text{或} \quad \sum_{i:(i, t) \in A} x_{it}\nonumber
$$
有了这样的设置，我们就可以开始考虑如何最大化流量了。首先，最大值一定不会超过从源流出的最大流量。因此，如果我们能够找到一个符合图的运输方式，其流量的值等于从源流出的容量之和，这就是最大流量。比如参见左下图：

<img src="graphs/lp13.png" alt="lp13" style="zoom:50%;" />

不过多数情况下，我们无法让网络流的值达到流出源的容量，如右上图所示。此时，对结点进行分组是一个有效的做法。上面的例子中，如果我们将 $\{s, b\}$ 分为一组，$\{a, t\}$ 分到另一组，现在我们考察从第一组流向第二组的容量（$s\to a: 3$、$b\to t: 4$），正好是 $7$。此时如果将第一组结点看成是源，我们就能断定流的最大值不会超过 $7$。我们将 **分割（Cut）**定义为图中顶点集 $N$ 的两个不相交子集 $S, T$ 满足 $s \in S, t \in T$ 且 $S\cup T = N$。一个分割的容量是指从 $S$ 到 $T$ 能够移动的最大流量，即：
$$
\sum_{i\in S}\sum_{j\in T}c_{ij} \nonumber
$$
分割对我们寻找最大流有很重要的意义，因为它们限定了最大值的上限。

> **定理**：如果分割 $(S, T)$ 的容量是 $c(S, T)$，则不存在从 $S$ 到 $T$ 的流超过 $c(S, T)$。

> **证**：本定理的证明从流量守恒出发：
> $$
> v(\mathbf{x}) = \sum_{k\in S}\left(\sum_{j:(k, j)\in A}x_{kj} - \sum_{i:(i, k)\in A}x_{ik}\right) \nonumber
> $$
> 这个值由于对于除了 $s, t$ 两点外都为 $0$，因此其代表了整体的流量。现在让我们对它进行下面的恒等变换：
> $$
> \begin{align*}
> 	v(\mathbf{x}) &= \sum_{k\in S}\sum_{j:(k, j)\in A}x_{kj} - \sum_{k\in S}\sum_{i:(i, k) \in A}x_{ik} \\
> 	&= \left(\sum_{k\in S}\sum_{j\in S}x_{kj} + \sum_{k\in S}\sum_{j\in T}x_{kj}\right) - \left(\sum_{k\in S}\sum_{i \in S}x_{ik} + \sum_{k \in S}\sum_{i \in T}x_{ik}\right) \\
> 	&= \sum_{k\in S}\sum_{j \in T}x_{kj} - \sum_{k\in S}\sum_{i\in T}x_{ik} \\
> 	&\le \sum_{k\in S}\sum_{j \in T}c_{kj}
> \end{align*}
> $$
> 最后的式子就是任选分割 $(S, T)$ 的容量。

#### 最大流的对偶问题

现在让我们假设在网络 $(N, A)$ 中对于任意 $(i, j) \in A$ 都有容量 $c_{ij}$，且不存在流向源及流出汇的弧。此时，等价的线性优化问题是：
$$
\begin{align}
\begin{split}
	\underset{\mathbf{x} \in  \R^{|A|}}{\text{maximize}} \quad & \sum_{j:(s, j) \in A}{x_{sj}} \\
	\text{subject to} \quad & \sum_{i:(i, k) \in A}x_{ik} - \sum_{j:(k, j) \in A}x_{kj} = 0 & (k \in N, k \ne s, t) \\
	& x_{ij} \le c_{ij} & (i, j) \in A \\
	& \mathbf{x} \ge \mathbf{0}
\end{split}
\end{align}
$$
为了找到对偶问题，我们首先需要找到对偶变量。对于任意除源汇结点 $k \in N$，需要 $u_k$ 以对应 $k$ 处的流量守恒，且对于任意弧 $(i, j) \in A$ 都有 $y_{ij}$ 以对应 $(i, j)$ 的容量约束。

大多数原始变量 $x_{ij}$ 都会在三个约束中出现：一个是 $i$ 的流量守恒（从 $i$ 流出）、一个是 $j$ 的流量守恒（从 $j$ 流入），还有 $(i, j)$ 的容量约束。因此我们会在对偶问题中得到 $-u_i + u_j + y_{ij} \ge 0$。有两个例外，对于源来说，没有流入的流量，因此我们只有 $u_j + y_{sj}$；类似地，对于汇来说，我们只有 $-u_i + y_{it}$。

最后我们得到下列对偶问题：
$$
\begin{align}
\begin{split}
	\underset{\mathbf{u} \in \R^{|N|-2}, \mathbf{y} \in \R^{|A|}}{\text{minimize}} \quad & \sum_{(i, j) \in A}c_{ij}y_{ij} \\
	\text{subject to} \quad & u_j + y_{sj} \ge 1 & (x_{sj}) \\
	& -u_i + u_j + y_{ij} \ge 0 \quad & (x_{ij}, i\ne s, j \ne t) \\
	& -u_i + y_{it} \ge 0 & (x_{it}) \\
	& \mathbf{y} \ge \mathbf{0}, \mathbf{u}\ \text{unrestricted}
\end{split}
\end{align}
$$
不过这个对偶问题的意义有些不清晰，我们需要对它再进行一些变换。如果我们将每个约束中的 $u$ 都移动到不等式右侧，同时为了让 $s, t$ 也能拥有相同的形式，引入 $u_s = 1, u_t = 0$。这样我们就得到了等价的线性优化问题：
$$
\begin{align}
\begin{split}
	\underset{\mathbf{u} \in \R^{|N|}, \mathbf{y} \in \R^{|A|}}{\text{minimize}} \quad & \sum_{(i, j) \in A}c_{ij}y_{ij} \\
	\text{subject to} \quad & y_{ij} \ge u_i - u_j & \text{(for all $(i, j) \in A$)} \\
	& u_s = 1 \\
	& u_t = 0 \\
	& \mathbf{y} \ge \mathbf{0}, \mathbf{u}\ \text{unrestricted}
\end{split}
\end{align}
$$
对于第一个约束，我们甚至可以再进一步，将其直接合并到目标函数中。下面这个等价的问题不再是线性优化问题，但更加直观：
$$
\begin{align}
\begin{split}
	\underset{\mathbf{u}\in\R^{|N|}}{\text{minimize}} \quad & \sum_{(i, j)\in A}c_{ij}\max\{u_i - u_j, 0\} \\
	\text{subject to} \quad & u_s = 1 \\
	& u_t = 0
\end{split}
\end{align}
$$
这里，虽然 $u_i$ 依然是没有约束的，我们可以假定对于任意弧 $(k, j)$, $u_k$ 都不会比最小的 $u_j$ 小，同样对于任意弧 $(i, k)$，$u_k$ 都不会比最大的 $u_i$ 大。由于 $u_s, u_t$ 始终是 $1$ 和 $0$，我们得到 $0\le u_i \le 1$，因此它们只能是 $0$ 或 $1$。此时我们可以为它们赋予一定含义：令 $S = \{i \in N \mid u_i = 1\}$，$T = \{i \in N \mid u_i = 0\}$，此时不难发现 $S, T$ 满足了分割的定义，而目标函数即是让我们找到最小容量的分割。

#### 最大流最小分割定理

通过强对偶性，我们几乎已经证明了下面的定理：

> **最大流最小分割定理（Max-Flow Min-Cut Theorem）**：在任何网络中，最大流的值和最小分割的容量相等。

我们还需要证明最小分割问题的最优解情况下的目标值是一个整数。下面是证明：

> **证**：为了能将问题引到全幺模矩阵，我们要求 $\mathbf{u} \ge 0$（我们之前的假设实际上已经满足了这一点）。为了更好理解这个约束矩阵，让我们对它的列进行分析：
>
> - 每个 $u_k$ 列都在流向 $k$ 的边所在的行 $(i, k)$ 处是 $1$，而在流出 $k$ 的边所在的行 $(k, j)$ 处是 $-1$。
> - $u_s, u_t$ 列都在某一行是 $1$。
> - $y_{ij}$ 列几乎都是 $0$，仅在 $(i, j)$ 所在行是 $1$。
>
> 下面让我们考察约束矩阵的任意 $k\times k$ 子矩阵。和此前证明二分图的关联矩阵为全幺模矩阵类似，可以通过数学归纳法进行论述。除了 $k = 1$，其余情形下我们可以尝试寻找全是 $0$ 的列（此时行列式为 $0$），或只存在一个 $1$ 的列来归纳为 $(k - 1)\times (k - 1)$ 的情况。由于 $y_{ij}$ 只有在 $(i, j)$ 所在行是 $1$，我们一定能找到满足上面两种情况中一种的列。当 $k = 1$ 时，由约束矩阵的构成可知其行列式必为 $0, \pm 1$ 之一。
>
> 这样我们就得到，最小分割问题的约束矩阵是全幺模矩阵，最大流最小分割定理成立。

#### 贪婪增长流

对于最大流问题，有一些比通过线性优化算法更佳的手段。为了给予一些提示，我们先从一个看起来相当有效，实际上不能解决问题的考虑方式。比如下面这个图：

<img src="graphs/lp14.png" alt="lp14" style="zoom:50%;" />

我们很快就能找到一条从 $s$ 到 $t$ 的路径：$s\to a\to b\to t$。我们决定放入最大流量 $10$。类似地，另一条路径是 $s\to c\to d\to t$，我们放入最大流量 $4$。此时 $s\to c$ 和 $b\to t$ 似乎还有空余容量，我们可以再放入流量 $2$。最终的结果是

<img src="graphs/lp15.png" alt="lp15" style="zoom:50%;" />

我们每一步似乎都做出了最优的选择，但是最后得到的 *并不是* 最优解。所以需要一个更可靠的策略。

#### 增广路径与剩余图

现在，以之前得到的最后一张图为基础，我们可以寻找一个 **增广路径（Augmenting Path）**，其要求：

- 对于没有填满容量的路径，我们可以额外运输一些，只要流量不超过其容量。
- 对于满容量的路径，我们可以 *逆向* 运输一个流量（等于正向运输一个 *负数* 容量），只要不超过其容量。
- 整个路径依然保证流量守恒。

因此，我们可以将原图优化为下面的情形：

<img src="graphs/lp16.png" alt="lp16" style="zoom:50%;" />

不过，对于更复杂的情形，我们应该怎么找到这样一条增广路径呢？可以构建一个 **剩余图（Residual Graph）** 帮助寻找，其只需要对原始图进行一定变形：

- 将没有满容量的弧的容量变为其剩余容量，即对于 $(i, j)$，将其容量标记为 $c_{ij} - x_{ij}$。
- 对所有流量非零的弧增加一条逆向的等于其流量的弧，即对于 $(i, j)$，增加一条弧 $(j, i)$ 满足 $c_{ji} = x_{ij}$。

上一节未经增广路径优化的图，在构建剩余图后变成下面的样子：

<img src="graphs/lp17.png" alt="lp17" style="zoom:50%;" />

在这个图中，我们不难找到一条增广路径 $s\to c\to b\to a\to d\to t$，也即是前面我们提到的路径。

现在，我们可以将最近几节的内容总结为一个定理：

> **定理**：假设图中某个流量 $\mathbf{x}$ 对应的剩余图中不存在从 $s$ 到 $t$ 的路径，则：
>
> - $\mathbf{x}$ 是一个最大流。
> - 设 $S$ 是所有在剩余图中 $s$ 可以达到的结点集（包括 $s$ 自己），$T$ 是剩余的所有结点，则 $(S, T)$ 是一个最小分割，其容量等于流量 $\mathbf{x}$ 的值。

> **证**：此前我们证明图的最大流小于任一个分割的容量时，得到结论：
> $$
> \sum_{j : (s, j) \in A}x_{sj} - \sum_{i : (i, s) \in A}x_{is} = \sum_{i \in S}\sum_{j \in T}x_{ij} - \sum_{i \in T}\sum_{j \in S}x_{ij} \le \sum_{i \in S}\sum_{j \in T}c_{ij} \nonumber
> $$
> 也即流量的值等于从分割 $(S, T)$ 中从 $S$ 到 $T$ 的流量净值，其小于等于该分割的容量。现在，考虑 $S$ 是所有 $s$ 能够达到的结点集。此时我们可以断定，剩余图中不存在从 $S$ 到 $T$ 的弧。现在让我们对原图中的弧进行分类讨论：
>
> - 对于 $(i, j)$，有 $x_{ij} < c_{ij}$ 的。由于这会导致剩余图中依然存在这样的弧，因此对于任意 $i \in S$ 及 $j \in T$ 都有 $x_{ij} = c_{ij}$。
> - 对于 $(i, j)$，有 $x_{ij} > 0$ 的。这回导致剩余图中出现一条弧 $(j, i)$，因此对于任意 $i \in T$ 及 $j \in S$ 都有 $x_{ij} = 0$。
>
> 现在我们可以将前面列出的不等式变形为：
> $$
> \sum_{i \in S}\sum_{j \in T}c_{ij} - \sum_{i \in T}\sum_{j \in S} 0 \le \sum_{i \in S}\sum_{j \in T}c_{ij} \nonumber
> $$
> 显然，不等式此时取等号。我们也就得到了最优情形。

#### Ford-Fulkerson 算法

我们上面的发现使得不需要解线性优化问题就能得到最大流问题的答案。下面的一系列步骤被称为 **Ford-Fulkerson 算法**。首先，从一个可行流开始（比如流量为 $0$）：

- 得到 $\mathbf{x}$ 的剩余图。
- 尝试在剩余图中找到一条路径。
- 如果能够找到路径，说明我们成功将 $\mathbf{x}$ 优化了，跳回第一步。
- 如果找不到路径，上一节的结论告诉我们这就是一个最小切割，此时 $\mathbf{x}$ 就是最大流。

不过我们还是有些顾虑：尽管我们每次都保证提升了最大流，但就好像简单形法一样，我们怎么保证一定能得到最优解？不幸的是，我们在同城情况下没法保证。不过如果图中所有容量都是整数，那么通过每次增加一，我们可以预见其一定能达到最优解；容量是有理数时，则可以通过每次增加 $1/d$（$d$ 是所有容量的最大公约数）以逐步到达终点。不过即使如此，这个过程还是太慢了。在下图给出的极端情形中，需要优化 2000 次才能达到最优解：

<img src="graphs/lp18.png" alt="lp18" style="zoom:90%;" />

好消息是，通过使用 **Edmonds-Karp 算法**，即每次选取 *最短* 的增广路径，我们可以保证在 $nm$ 次优化后得到最优解（其中 $n$ 是结点个数，$m$ 是弧的个数）。

### 最大流问题拓展

这一节中我们会介绍其它的网络流问题。其中可能会出现 *多个源汇* 的情形。不过我们可以通过添加辅助弧，并设置两个辅助结点 $s, t$ 来将其转换为等效的单一源汇的问题，参考下面的示意图：

<img src="graphs/lp19.png" alt="lp19" style="zoom:100%;" />

在通过右图解决问题后，我们只需要删除一开始加入的弧和结点即可。

#### 带有供给和需求的结点

这个情形下，我们的结点中会出现有限供给或有限需求（对比源和汇的无限供给和无限需求）；此时我们可以从该点额外流出（或流入）特定的流量。对于每个结点 $k$，下列公式定义了该点的需求：
$$
d_k = \sum_{i : (i, k) \in A}x_{ik} - \sum_{j : (k, j) \in A}x_{kj}
$$
当需求为负数时，这个结点就是一个供给结点。此前我们分析的网络流都是由流量守恒，即需求为 $0$ 的结点组成的。不过，我们依然可以通过引入辅助弧和辅助结点来将其转化为一般的网络流：创建 $s, t$ 结点作为源和汇，并从 $s$ 指向对每个 $d_k < 0$ 的结点，且从每个 $d_k > 0$ 的结点指向 $t$。不难看出前后两个图是等价的。以下面示意图为例：

<img src="graphs/lp20.png" alt="lp20" style="zoom:100%;" />

需要说明的是，供给需求问题是一个 *可行性* 问题，而不是优化问题。所以我们需要找到能够让每一条流向 $t$ 的弧都能达到其容量（即满足其所有需求）。此时我们就可以删除 $s, t$ 及相关结点，并得到最终的解。

#### 可行环路问题

考虑寻找一个图中的可行环路。当然，为了避免平凡解（即流量处处为 $0$），我们需要为每条弧上的流量设置一个大于零的下界。这样，就等效于对任何 $(i, j) \in A$ 都有 $x_{ij} \in [a_{ij}, b_{ij}]$。我们可以将其转换为供给需求问题：由于每个结点都至少从流向它的结点处得到一定流量，且至少对每个它指向的其它结点贡献一定流量，这实际上就等价于供给/需求结点：
$$
d_k = \sum_{j : (k, j) \in A}x_{kj} - \sum_{i : (i, k) \in A}x_{ik} \nonumber
$$
如此设置后就能保证每条弧上的流量不低于其要求的下界。为了不超过上界，我们只需设 $c_{ij} = b_{ij} - a_{ij}$。现在这完全变成了一个供给需求问题。在得到该问题的可行解后，我们只需对原图计算 $x_{ij} + a_{ij}$ 就能得到原图的可行解。

#### 二分图匹配

没错，二分图匹配问题也可以转化为最大流问题。构造方式如下：

- 对任意边 $(i, j)$，都画一条弧并令 $x_{ij} = \infty$。
- 添加 $s, t$ 两个结点，让 $s$ 连向所有 $X$ 中的结点，让 $Y$ 中所有结点连向 $t$。这些新的弧容量均为 $1$。

示意图如下：

<img src="graphs/lp21.png" alt="lp21" style="zoom:100%;" />

#### 最小成本流问题

最小成本流问题描述的是一个网络 $(N, A)$ 中，每个结点 $k \in N$ 都对应一个需求 $d_k$，每条弧 $(i, j)$ 都对应一个 **成本（Cost）** $c_{ij}$。我们的目标是找到一个可行流使得总成本最小：
$$
\begin{align*}
	\underset{\mathbf{x} \in \mathbb{R}^{|N|}}{\text{minimize}} \quad & \sum_{(i, j) \in A}c_{ij}x_{ij} \\
	\text{subject to} \quad & \sum_{i: (i, k) \in A} x_{ik} - \sum_{j : (k, j)\in A}x_{kj} = d_k \\
	& \mathbf{x} \ge \mathbf{0}
\end{align*}
$$
为了简化讨论，我们假设这个网络是连通的，即任两个结点都能找到一条路径连接。对于不连通的情况我们可以将其拆分为多个最小成本流问题分别解决。

我们不希望通过简单形表解决这个问题（产生的矩阵太庞大了）。或许还是直接在图上进行操作更加简单直观。但在此之前让我们先分析一下这个线性优化问题对应的矩阵。

约束大体可以写成 $F\mathbf{x} = \mathbf{d}$ 的形式（我们暂时不考虑 $\mathbf{x} \ge \mathbf{0}$），其中 $F$ 是一个 $|N|$ 行 $|A|$ 列的矩阵。每一列都对应着一段弧 $(i, j)$，因此只在 $i$ 行有一个 $-1$，$j$ 行有一个 $1$。为了得到一个基本解，我们会选取某个基 $\mathcal{B}$，并计算 $\mathbf{x}_\mathcal{B} = F_\mathcal{B}^{-1}\mathbf{d}$。不过这里有个严重的问题，矩阵 $F$ 并不是线性无关的！这是因为每一行都对应一个结点，在将所有行相加后一定会得到 $0$（不然这个网络的需求和供给就不平衡了，此时不存在可行解）。因此我们需要去掉其中（任）一个约束。一个方便且等价的处理方式是只选择 $|N| - 1$ 个基本变量。

选取基本变量时，我们需要确保这些弧不会产生环路（即不考虑弧的方向时，存在两个结点，有多于一种路径将它们相连）。环路的问题在于它会导致无穷多个可行解，比如下面这种情形：

<img src="graphs/lp22.png" alt="lp22" style="zoom:50%;" />

假设我们找到了一组可行解，其中包括 $x_{12}, x_{23}, x_{16}, x_{63}$，则很可能存在 $\delta$ 使得 $x_{12} + \delta, x_{23} + \delta, x_{16} - \delta, x_{63} - \delta$ 也满足条件，这就造成了无穷多组解。

当选取的 $|N| - 1$ 条弧中不存在环路时，我们称这些弧是一个 **生成树（Spanning Tree）**。对于一个网络的生成树 $T$ 及需求向量 $\mathbf{d}$（其各项和应该为 $0$），我们 *总能* 找到 $x_{ij}$，对应所有弧 $(i, j)$，使得 $ F\mathbf{x} = \mathbf{d}$ 成立。方法如下：

- 寻找一个 $T$ 中只和一条我们没有确定解的弧相连的结点 $k$（无论是从 $k$ 出发还是到达 $k$），通过此处的需求得到对应的弧的唯一解。
- 重复上一步 $|N| - 1$ 次直到得到所有基本变量的解。

不过，这样得到的解并不一定是可行解，因为至此我们都没有强调 $\mathbf{x} \ge 0$ 这个约束。此时我们将这组解称为 **平衡流（Balanced Flow）**。我们会在稍后讨论如何满足这一条件。

接下来，我们将模拟简单形法中的基准步骤，向得到一组基本可行解的网络引入新的基本变量并移除旧的基本变量。这一步形象化的操作就是删除一条弧并引入一条新的弧。假设我们一开始的生成树如下图所示：

<img src="graphs/lp23.png" alt="lp23" style="zoom:50%;" />

我们可以选择一个可以加入弧，比如说 $(4, 5)$，加入后结果如下：

<img src="graphs/lp24.png" alt="lp24" style="zoom:50%;" />

此时为了保证各结点需求守恒，可以假设 $x_{45} = \delta$，然后和此前环路分析的结果类似，得到下面的”通解“：

<img src="graphs/lp25.png" alt="lp25" style="zoom:50%;" />

现在，我们一方面要避免环路，也要避免网络中任何一条弧的流量小于零。此时唯一可能的情况就是令 $\delta = 1$，此时 $x_{45} = 1, x_{56} = 4, x_{63} = 3, x_{43} = 0$。因此 $x_{43}$ 离开了基。

不过，为什么选择了 $(4, 5)$ 加入基呢？是否有什么基准规则帮助我们思考？和简单形法类似，我们需要差额成本小于 $0$。比如如果我们想要加入 $(4, 5)$，就应该计算：
$$
c_{45} + c_{56} + c_{63} - c_{43}
$$
这个结果小于零时，最终得到的目标值才能被优化（具体为 $\delta(c_{45} + c_{56} + c_{63} - c_{43})$）。当任何可能加入的弧的差额成本都为非负数时，我们就得到了最优解。

最后，让我们讨论令 $\mathbf{x} \ge \mathbf{0}$ 的方法。我们可以引入一个虚构结点 $a$，并将其和所有其它结点用虚构弧连接，具体如下：

- 对于任意结点 $k$，如果 $d_k < 0$，则添加一个虚拟弧 $(k, a)$ 并令 $c_{ka} = 1$。
- 对于任意结点 $k$，如果 $d_k > 0$，则添加一个虚拟弧 $(a, k)$ 并令 $c_{ak} = 1$。
- 所有不和 $a$ 相连的弧的成本都暂记为 $0$。这样可以保证我们逐步移除和 $a$ 相连的弧。

接下来就是利用基准方法将虚构弧统统移出基。这里有一个微妙的点，就是在引入虚构结点和虚构弧之后，网络中一共有 $|N| + 1$ 条弧，但我们最终只需要 $|N|$ 条，所以最后一定会剩下一条。但注意，当走到只剩下一条虚构弧的情况时，这条弧上的流量 *一定* 是零（因为需求是平衡的），因此此时我们直接删除这条弧即可。

这样，我们就得到了一个对应可行解的生成树。随后就可以接着像前文所述解决最小成本流问题了。



## 原始-对偶算法

**原始-对偶算法（The Primal-Dual Algorithm）** 是受 Ford-Fulkerson 算法启发的，用来解决线性优化问题的算法。该算法有点类似于在网络流中寻找增广路径，每次从一个可行解出发，寻找最可能提升目标值的方向。它的利弊如下：

- 每个“增广“步骤要比简单形法中的一步要更加复杂（耗时更长）。
- 一条增广路径可能达到等同于在简单形法中好几步的结果。

### 算法引入

首先，假设我们从一个等式形式的最小值原始问题和对应的对偶问题：
$$
(\mathbf{P}) \quad
\begin{cases}
	\underset{\mathbf{x} \in \mathbb{R}^n}{\text{minimize}} \quad & \mathbf{c}^\text{T}\mathbf{x} \\
	\text{subject to} \quad & A\mathbf{x} = \mathbf{b} \\
	& \mathbf{x} \ge \mathbf{0}
\end{cases}
\qquad
(\mathbf{D}) \quad
\begin{cases}
	\underset{\mathbf{u} \in \mathbb{R}^m}{\text{maximize}} \quad & \mathbf{u}^\text{T} \mathbf{b} \\
	\text{subject to} \quad & \mathbf{u}^\text{T}A \le \mathbf{c}^\text{T} \\
	& \mathbf{u}\ \text{unrestricted}
\end{cases}
$$
原始问题中我们假设 $\mathbf{b} \ge \mathbf{0}$。$(\mathbf{P})$ 这种形式总是可以达到，因此我们不用在意此前的过程（引入松弛变量，两边同乘 $-1 $ 等）。现实生活中可能对对偶问题更感兴趣，此时我们依然可以利用原始-对偶算法求解问题。

考虑下面的例子：
$$
(\mathbf{P}) \quad
\begin{cases}
	\underset{\mathbf{x} \in \mathbb{R}^3}{\text{minimize}} \quad & 2x_1 + 2x_2 + x_3 \\
	\text{subject to} \quad & 2x_1 + x_2 - 4x_3 = 3 \\
	& 4x_1 - x_2 + x_3 = 3 \\
	& x_1, x_2, x_3 \ge 0
\end{cases}
\qquad (\mathbf{D}) \quad
\begin{cases}
	\underset{\mathbf{u} \in \mathbb{R}^2}{\text{maximize}} \quad & 3u_1 + 3u_2 \\
	\text{subject to} \quad & 2u_1 + 4u_2 \ge 2 \\
	& u_1 - u_2 \le 2 \\
	& -4u_1 + u_2 \le 1
\end{cases}
$$
假设我们已经得到了一个对偶解 $(1, 0)$。此时应该如何找到一个最好的方向呢？我们可以假设这个方向是 $\mathbf{v} \in \mathbb{R}^2$。我们下一步得到的是 $\mathbf{u} + t\mathbf{v}$，其中 $t > 0$ 是能取得的满足约束的最大值。此时目标值变化了 $t(\mathbf{v}^\text{T}\mathbf{b})$。为了让这个增长最快，我们理应选择最大的 $\mathbf{v}^\text{T}\mathbf{b}$，也即前一次的目标值 $\mathbf{u}^\text{T}\mathbf{b}$。

现在，让我们尝试得到 $\mathbf{v}$ 的范围。将前一个对偶解带入对偶约束中，发现第一个是紧致的，第二个和第三个都是松弛的。因此为了保证将解优化到 $\mathbf{u} + t\mathbf{v}$ 后依然满足约束，显然有 $2v_1 + 4v_2 \le 0$。在非线性优化中，我们通常还会加上一个条件 $||\mathbf{v}|| = 1$，这里我们只需要求 $v_1, v_2 \le 1$。因此我们得到：
$$
\begin{align*}
	\underset{\mathbf{v} \in \mathbb{R}^2}{\text{maximize}} \quad & 3v_1 + 3v_2 \\
	\text{subject to} \quad & 2v_1 + 3v_2 \le 0 \\
	& v_1 \le 1 \\
	& v_2 \le 1
\end{align*}
$$
更通用的形式如下：
$$
\begin{align*}
	\underset{\mathbf{v} \in \mathbb{R}}{\text{maximize}} \quad & \mathbf{v}^\text{T}\mathbf{b} \\
	\text{subject to} \quad & \mathbf{v}^\text{T}A_J \le \mathbf{0}^\text{T} \\
	& v_1, ..., v_m \le 1
\end{align*}
$$
其中 $J$ 对应紧致的对偶约束。

在上面的例子中，我们可以得到 $\mathbf{v} = (1, -\frac{1}{2})$，下一步就是确定在这个方向前进的距离 $t$。把 $\mathbf{v}$ 代入 $\mathbf{u} + t\mathbf{v}$，再放到对偶约束中，我们得到：
$$
\begin{align*}
	(1 + t) - (-\frac{1}{2}t) \le 2 &\implies t \le \frac{2}{3} \\
    -4(1 + t) + (-\frac{1}{2}t) \le 1 & \implies t \ge -\frac{10}{9}
\end{align*}
$$
（这里没有放到第一个约束中，因为我们对 $\mathbf{v}$ 的限制保证了 $\mathbf{u} + t\mathbf{v}$ 一定可以满足这个约束）因此最大的 $t$ 就是 $\frac{2}{3}$。新的基本可行解是 $(\frac{5}{3}, -\frac{1}{3})$。之后，我们可以类似地再进行一次次优化。不过肉眼可见地，这种方式相当繁琐：我们在每一步都要额外引入一个线性优化问题，这太过低效。

对于上面这类问题，我们有一种特殊的变形，称为 **限定原始问题（Restricted Primal）**，而其对应的 **对偶限定原始问题（Dual Restricted Primal）** 恰好是我们前面得到的关于 $\mathbf{v}$ 的线性优化问题：
$$
(\mathbf{RP}) \quad
\begin{cases}
	\underset{\mathbf{x} \in \mathbb{R}^n, \mathbf{y} \in \mathbb{R}^m}{\text{minimize}} \quad & y_1 + ... + y_m \\
	\text{subject to} \quad & A_J\mathbf{x}_J + I\mathbf{y} = \mathbf{b} \\
	& \mathbf{x}, \mathbf{y} \ge \mathbf{0}
\end{cases}
\qquad (\mathbf{DRP}) \quad
\begin{cases}
	\underset{\mathbf{v} \in \mathbb{R}^m}{\text{maximize}} \quad & \mathbf{v}^\text{T}\mathbf{b} \\
	\text{subject to} \quad & \mathbf{v}^\text{T}A_J \le \mathbf{c}_J^\text{T} \\
	& v_1, ..., v_m \le 1
\end{cases}
$$

这里 $y_i$ 对应着对偶问题 $(\mathbf{D})$ 中的松弛约束。因此从此前的例子可以得到下面的限定原始问题：
$$
\begin{align*}
	\underset{\mathbf{x} \in \mathbb{R}, \mathbf{y} \in \mathbb{R}^2}{\text{minimize}} \quad & y_1 + y_2 \\
	\text{subject to} \quad & 2x_1 + y_1 = 3 \\
	& 4x_1 + y_2 = 3 \\
	& x_1, y_1, y_2 \ge 0
\end{align*}
$$
我们更倾向于使用限定原始问题，而非对偶限定原始问题来迭代，这是因为在对偶简单形法中已经见识到，对偶解最优解会在得到原始最优解时自然得到。此外，在 $(\mathbf{RP})$ 中的不同迭代不需要一个个独立求解。我们可以将一次迭代的最优解作为下一次迭代的起始点。

### 利用 $(\mathbf{DRP})$ 的原始-对偶算法

这就是我们此前介绍的低效方法，总结如下：

- 对 $(\mathbf{P})$ 和 $(\mathbf{D})$ 进行适当变形使得 $\mathbf{b} \ge \mathbf{0}$。找到 $(\mathbf{D})$ 的一个可行解 $\mathbf{u}$。
- 判断 $(\mathbf{D})$ 约束的紧致性，并用其构建 $(\mathbf{DRP})$。找到它的解 $\mathbf{v}$。这是我们提升 $\mathbf{u}$ 的最佳方向。
- 找到使 $\mathbf{u} + t\mathbf{v}$ 可行的最大值 $t$，并用 $\mathbf{u} + t\mathbf{v}$ 代替 $\mathbf{u}$。
- 重复前面两个步骤，直到不能再优化 $\mathbf{u}$ 为止。

### 利用 $(\mathbf{RP})$ 的原始-对偶算法

我们依然使用前面的例子，即：
$$
(\mathbf{P}) \quad
\begin{cases}
	\underset{\mathbf{x} \in \mathbb{R}^3}{\text{minimize}} \quad & 2x_1 + 2x_2 + x_3 \\
	\text{subject to} \quad & 2x_1 + x_2 - 4x_3 = 3 \\
	& 4x_1 - x_2 + x_3 = 3 \\
	& x_1, x_2, x_3 \ge 0
\end{cases}
\qquad (\mathbf{D}) \quad
\begin{cases}
	\underset{\mathbf{u} \in \mathbb{R}^2}{\text{maximize}} \quad & 3u_1 + 3u_2 \\
	\text{subject to} \quad & 2u_1 + 4u_2 \ge 2 \\
	& u_1 - u_2 \le 2 \\
	& -4u_1 + u_2 \le 1
\end{cases}
$$
从对偶解 $\mathbf{u} = (1, 0)$ 出发，我们可以得到对应的 $(\mathbf{RP})$ 和 $(\mathbf{DRP})$：
$$
(\mathbf{RP}) \quad
\begin{cases}
	\underset{\mathbf{x \in \mathbb{R}, \mathbf{y} \in \mathbb{R}^2}}{\text{minimize}} \quad & y_1 + y_2 \\
	\text{subject to} \quad & 2x_1 + y_1 = 3 \\
	& 4x_1 + y_2 = 3 \\
	& x_1, y_1, y_2 \ge 0
\end{cases}
\qquad (\mathbf{DRP}) \quad
\begin{cases}
	\underset{\mathbf{v} \in \mathbb{R}^2}{\text{maximize}} \quad & 3v_1 + 3v_2 \\
	\text{subject to} \quad & 2v_1 + 4v_2 \le 0 \\
	& v_1 \le 1 \\
	& v_2 \le 1
\end{cases}
$$
我们可以通过简单形法解 $(\mathbf{RP})$：
$$
\begin{array}{cccc|c}
	 & x_1 & y_1 & y_2 & \\\hline
	y_1  & 2 & 1 & 0 & 3 \\
	y_2 & 4 & 0 & 1 & 3 \\\hline
	-z_{rp} & -6 & 0 & 0 & -6
\end{array}
\quad\leadsto\quad
\begin{array}{cccc|c}
	 & x_1 & y_1 & y_2 & \\\hline
	y_1 & 0 & 1 & -1/2 & 3/2 \\
	x_1 & 1 & 0 & 1/4 & 3/4 \\\hline
	-z_{rp} & 0 & 0 & 3/2 & -3/2
\end{array}
$$
为了得到 $(\mathbf{DRP})$ 的解，回忆 $r_i = c_i - \mathbf{v}^\text{T}A_i$，因此对于 $y_1, ..., y_m$ 所在列 $\mathcal{Y}$，有：
$$
\mathbf{r}_\mathcal{Y}^\text{T} = \mathbf{c}_\mathcal{Y}^\text{T} - \mathbf{v}^\text{T}A_\mathcal{Y} = \mathbf{1}^\text{T} - \mathbf{v}^\text{T}
$$
因此我们得到 $\mathbf{v}^\text{T} = \begin{bmatrix} 1 & -1/2 \end{bmatrix}$。在令对偶约束成立的情况下，我们可以得到最大的 $t$ 是 $\frac{2}{3}$。此时新的解是 $\mathbf{u} = (\frac{5}{3}, -\frac{1}{3})$。此时 $(\mathbf{D})$ 的前两个约束都是紧致的，因此我们得到了新的 $(\mathbf{RP})$ 与 $(\mathbf{DRP})$：
$$
(\mathbf{RP}) \quad
\begin{cases}
	\underset{\mathbf{x} \in \mathbb{R}, \mathbf{y} \in \mathbb{R}^2}{\text{minimize}} \quad & y_1 + y_2 \\
	\text{subject to} \quad & 2x_1 + x_2 + y_1 = 3 \\
	& 4x_1 - x_2 + y_2 = 3 \\
	& x_1, x_2, y_1, y_2 \ge 0
\end{cases}
\qquad (\mathbf{DRP}) \quad
\begin{cases}
	\underset{\mathbf{v} \in \mathbb{R}^2}{\text{maximize}} \quad & 3v_1 + 3v_2 \\
	\text{subject to} \quad & 2v_1 + 4v_2 \le 0 \\
	& v_1 - v_2 \le 0 \\
	& v_1 \le 1 \\
	& v_2 \le 1
\end{cases}
$$
这里就显示出以 $(\mathbf{RP})$ 为核心的好处了。上一个迭代中 $(\mathbf{RP})$ 的解在这一次依然是可行解，只需令 $x_2 = 0$ 即可。不过这只是个例，是否会出现原来的解在新的 $(\mathbf{RP})$ 中消失的情况，若出现应该如何处理？下面我们做一些分析：

- 如果消失的变量为零，则不会产生任何影响，我们不必在意。
- *不可能* 出现正数变量消失的情况。具体来说，如果原来的 $(\mathbf{RP})$ 中某个变量为正数，根据互补松弛性，其对应的 $(\mathbf{DRP})$ 中的约束必然是紧致的（且等式右侧为 $0$）。回忆我们最开始对 $(\mathbf{DRP})$ 的构造，其约束中的等式对应的正是 $(\mathbf{D})$ 中的等式。现在对于新的解 $\mathbf{u} + t\mathbf{v}$，由于 $\mathbf{v} = 0$，我们得到的约束依然是紧致的。因此其对应的 $\mathbf{(P)}$ 中的变量依然会出现在 $(\mathbf{RP})$ 中。

直到 $\mathbf{u}$ 无法继续优化为止，我们不断迭代进行这个算法。

#### 冻结变量



## 整数线性优化

**整数线性优化问题（Integer Linear Program）**，或 **整数优化问题（Integer Program）** 是指限制一或多个变量需要是整数的线性优化问题。这在现实中相当常见，因为一些量在非整数时没有定义。比如此前讨论的二分图匹配以及最大流问题都是整数优化问题。不过当时我们通过证明其约束矩阵为全幺模矩阵，轻松地解决了解是否为整数的问题。但在一般的情形下，整数优化问题是非常复杂的一类问题：约束上的一点差别就可能导致完全不同的解。

下列是一个线性优化问题，在不加整数约束、为 $x$ 加上整数约束，以及为 $x, y$ 均加上整数约束时可行域的示意图：

<img src="graphs/lp26.png" alt="lp26" style="zoom:50%;" />

除了整数约束外，它们是完全相同的线性优化问题：
$$
\begin{align*}
	{\text{maximize}} \quad & x + y \\
	\text{subject to} \quad & 3x + 8y \le 24 \\
	& 3x - 4y \le 6 \\
	& x, y \ge 0
\end{align*}
$$
然而正如图中暗示的，前两种情况下，拥有相同的最优解 $(4, \frac{3}{2})$，而最后一种的最优解却是 $(2, 2)$ 或 $(3, 1)$，甚至不是唯一解。在一些极端情况下，比如一个可行域中不存在整数点时，整数优化问题是没有解的（而没有整数约束时有解）。我们需要解决这个问题。

### 逻辑约束

在正式讨论整数优化问题前，让我们首先介绍逻辑的一些基本概念。一个 **布尔变量（Boolean Variable）** 只可能有两种值：**TRUE** 和 **FALSE**。有三个主要的逻辑运算：

- **AND**：$X_1$ **AND** $X_2$ 当且仅当 $X_1, X_2$ 都为 **TRUE** 时才是 **TRUE**，否则为 **FALSE**。
- **OR**：$X_1$ **OR** $X_2$ 当且仅当 $X_1, X_2$ 都为 **FALSE** 时才是 **FALSE**，否则为 **TRUE**。
- **NOT**：**NOT** $X$ 在 $X$ 为 **TRUE** 时为 **FALSE**，否则为 **TRUE**。

布尔值满足性问题是指判断一个布尔值表达式是否可以得到 **TRUE**。比如下面这个表达式：
$$
(X_1 \ \text{OR} \ \text{NOT}(X_2))\ \text{AND}\ (X_2 \ \text{OR}\ \text{NOT}(X_3))\ \text{AND}\ (X_3 \ \text{OR}\ X_1)
$$
在 $X_1, X_2, X_3$ 均为 **TRUE** 时就是 **TRUE**。这个解并不一定是唯一的，但我们只关注这个解的 *存在性*。这个表达式的形式实际上是比较特殊的。定义 **合取范式（Conjunctive Normal Form, CNF）** 由以下成分组成：

- **字面量（Literal）**，即 $X_i$ 或 $\text{NOT}(X_i)$。
- 字面量组成的 **从句（Clause）**，即由 **OR** 连接的多个字面量。
- 从句之间通过 **AND** 连接，形成最终的合取范式。

所有的逻辑表达式都可以等价转换为合取范式。我们不在本篇中进行证明。

我们可以用只能取 $0$ 或 $1$ 的整数变量来表示布尔变量。此时，逻辑运算变为等价的下列运算：

- 对于任意 $X_i$，$\text{NOT}(X_i)$ 可以用 $1 - x_i$ 表示。
- 对于任意 $X_1, ..., X_k$，$X_1\ \text{OR}\ ...\ \text{OR}\ X_k$ 为 **TRUE** 可以用 $x_1 + ... + x_k \ge 1$ 表示。
- 至于合取范式中的 **AND**，我们不需要在意，因为已经要求其中的每一项 **OR** 都成立（即上一条）了。
- 最后，为了让 $x_i$ 满足我们一开始的定义，需要对于每个 $x_i$ 加上 $0 \le x_i \le 1$ 这条约束。

因此，只要我们找到这个问题的可行解，就知道其对应的逻辑表达式是否能被满足了。

#### 将逻辑约束和线性约束结合

在一些和逻辑判断相关的问题中，我们可以巧妙地引入逻辑约束。下面我们用一个例子展示。

- 一个香蕉公司每卖一只香蕉就可以赚 \$1，但可以选择是否花 \$1000 租一个仓库。此时我们可以引入变量 $w$ 表示是否租赁一间仓库，$1000w$ 就是对应的花销。
- 假设 $x_1, x_2, x_3$ 表明需要存在仓库中的产品。假设当且仅当租一间仓库时有 $x_1 + x_2 + x_3 \le 100$，否则 $x_1 = x_2 = x_3 = 0$。我们可以通过 $x_1 + x_2 + x_3 \le 100w$ 来表示这个约束。
- 同样的设置下，假设当且仅当不租仓库时有 $x + y \le 50$，此外 $x + y$ 没有限制。这时我们可以用 $x + y \le 50 + Nw$ 表示这个约束。这里 $N$ 是一个非常大的数，使得 $x + y$ 不可能超过这个界限。

### 分支定界法

考虑下面的整数优化问题：
$$
\begin{align*}
	\underset{x, y \in \mathbb{Z}}{\text{maximize}} \quad & 4x + 5y \\
	\text{subject to} \quad & x + 4y \le 10 \\
	\quad & 3x - 4y \le 6 \\
	& x, y \ge 0
\end{align*}
$$
这个问题的可行域如下（用黑点标记出来了）：

<img src="graphs/lp27.png" alt="lp27" style="zoom:50%;" />

我们可以不在意整数约束，求得阴影部分可行域下的最优解：
$$
\begin{array}{ccccc|c}
	 & x & y & s_1 & s_2 & \\\hline
	s_1 & 1 & 4 & 1 & 0 & 10 \\
	s_2 & 3 & -4 & 0 & 1 & 6 \\\hline
	-z & 4 & 5 & 0 & 0 & 0
\end{array}
\quad \leadsto \quad
\begin{array}{ccccc|c}
	 & x & y & s_1 & s_2 & \\\hline
	y & 1/4 & 1 & 1/4 & 0 & 5/2 \\
	s_2 & 4 & 0 & 1 & 1 & 16 \\\hline
	-z & 11/4 & 0 & -5/4 & 0 & -25/2
\end{array}
\quad \leadsto \quad
\begin{array}{ccccc|c}
	 & x & y & s_1 & s_2 & \\\hline
	 y & 0 & 1 & 3/16 & -1/16 & 3/2 \\
	 x & 1 & 0 & 1/4 & 1/4 & 4 \\\hline
	 -z & 0 & 0 & -31/16 & -11/16 & -47/2
\end{array}
$$
因此 $(4, 1.5)$ 是最优解，此时目标值为 $23.5$。不过显然这并不是整数优化问题的解，但我们至少能得出，$23.5$ 是我们要求的整数目标值的上界。如果需要更加严格的上界（或，最小值问题的下界），可以利用下面的公式：
$$
x_i \le \lfloor f \rfloor \qquad \text{或} \qquad x_i \ge \lceil f \rceil
$$
其中 $f$ 是线性优化问题最优解下的目标值。由于 $y = 1.5$ 不满足整数的条件，我们可以两边进行尝试，即分别设 $y \le 1$ 或 $y \ge 2$。让我们先考虑后面一种情况。我们可以在简单形表中加入一行 $s_3$，此时该表依然是对偶可行的：
$$
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	y & 0 & 1 & 3/16 & -1/16 & 0 & 3/2 \\
	x & 1 & 0 & 1/4 & 1/4 & 0 & 4 \\
	s_3 & 0 & -1 & 0 & 0 & 1 & -2 \\\hline
	-z & 0 & 0 & -31/16 & -11/16 & 0 & -47/2
\end{array}
$$
我们可以先将 $y$ 列的 $-1$ 清除，然后再用对偶简单形法得到新的最优解（用 $s_2$ 代替 $s_3$）：
$$
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	y & 0 & 1 & 3/16 & -1/16 & 0 & 3/2 \\
	x & 1 & 0 & 1/4 & 1/4 & 0 & 4 \\
	s_3 & 0 & 0 & 3/16 & -1/16 & 1 & -1/2 \\\hline
	-z & 0 & 0 & -31/16 & -11/16 & 0 & -47/2
\end{array}
\quad\leadsto\quad
\begin{array}{cccccc|c}
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	y & 0 & 1 & 0 & 0 & -1 & 2 \\
	x & 1 & 0 & 1 & 0 & 4 & 2 \\
	s_2 & 0 & 0 & -3 & 1 & -16 & 8 \\\hline
	-z & 0 & 0 & -4 & 0 & -11 & -18
\end{array}
$$

现在我们得到了一个最优解 $(2, 2)$ 以及对应的目标值 $18$。现在还有另一边（也即 $y \le 1$）要考虑。这种情况下最优解是 $(\frac{10}{3}, 1)$，对应的目标值是 $\frac{55}{3}$。这个结果颇为尴尬，因为它优于 $18$，但不满足整数约束。因此我们需要再次将情况分为 $x \le 3$ 和 $x \ge 4$ 两种情况。需要注意的是，我们应该将这两个条件加到已经加入 $y \le 1$ 的简单形表中。

经过不懈的努力，我们得到 $y\le 1$ 且 $x \le 3$ 时最优解是 $(3, 1)$，目标值为 $17$；$y \le 1$ 且 $x \ge 4$ 时不存在可行解。因此，我们得出结论，$(2, 2)$ 是最优解。

作为总结，对于求最大值的整数优化问题（对于最小值问题，则应该将下文中的上界和下界兑换，并使用 $+\infty$ 作为最差值），我们需要维护两组数据：

- 一列结点 $\mathcal{L}$，其中包含描述部分或全部可行域的线性优化问题。最开始时，$\mathcal{L}$ 只有一个元素，即没有整数约束的线性优化问题。
- 一个候选的最优解 $\mathbf{x}^*$ 和其对应的目标值 $z^*$。最开始时，不存在一个最优解，并设 $z^* = -\infty$。

算法中，每一步对 $\mathcal{L}$ 中的一个结点进行检查：解该线性优化问题，然后将其移出 $\mathcal{L}$。假设 $\mathbf{x}$ 是这里得到的最优解，$z$ 是目标值。接下来有三种情形：

- 如果 $z \le z^*$，这个结点不可能得到更好的结果了，因此我们不再对该结点感兴趣。此时称该结点被 *边界修剪了（pruned by bound）*。一种更加特殊的情况是没有找到最优解（等价于 $z = -\infty$），此时我们称该结点被 *不可行性修剪了 （pruned by infeasibility）*。
- 如果 $z > z^*$，且 $\mathbf{x}$ 是一个整数向量，则更新 $\mathbf{x}^* = \mathbf{x}$ 以及 $z^* = z$。我们同样也不需要对这个结点进行任何深入探索了，此时该结点被 *完整性修剪了（pruned by integrality）*。
- 如果 $z > z^*$，但 $\mathbf{x}$ 不是一个整数向量。此时我们需要找到某一个 $x_i = f \notin \mathbb{Z}$，并在 $\mathcal{L}$ 中增加两个结点，其中一个等于当前结点加上 $x_i \le \lfloor f \rfloor$ 约束，另一个等于当前结点加上 $x_i \ge \lceil f \rceil$ 约束。

我们不断进行上面这些步骤，直到 $\mathcal{L}$ 为空为止。此时 $\mathbf{x}^*$（如果存在）就是最优解，而 $z^*$ 是对应的目标值。

### 切割平面法

**切割平面法（The Cutting Plane Method）** 是解决整数优化问题的一系列策略。其构思于，整数优化算法中会包含一系列线性优化问题的构造，这些问题描述的是同一集整数点。如果从其中的一个线性优化问题中得到了分数的最优解，那就说明我们的构造不够好。我们应该尝试改善构造。

假设下面是我们需要解决的整数优化问题：
$$
\begin{align*}
	\underset{\mathbf{x} \in \mathbb{Z}^n}{\text{maximize}} \quad & \mathbf{c}^\text{T}\mathbf{x} \\
	\text{subject to} \quad & A\mathbf{x} \le \mathbf{b} \\
	& \mathbf{x} \ge \mathbf{0}
\end{align*}
$$
假设我们在此得到了一个分数最优解 $\mathbf{x}^*$，为了改善之前对线性优化问题的构造，假设引入了一个新的不等式 $\boldsymbol{\alpha}^\text{T}\mathbf{x} \le \beta$ 满足下面的要求：

- 其对于我们的整数优化问题有效。对于任意满足 $A\mathbf{x} \le \mathbf{b}$ 且 $\mathbf{x} > \mathbf{0}$ 的点 $\mathbf{x} \in \mathbb{Z}^n$ 都满足这个不等式。
- 分数最优解被切在新的可行域外面，即 $\boldsymbol{\alpha}^\text{T}\mathbf{x}^* > \beta$。不然新一轮得到的最优解还是 $\mathbf{x}^*$。

我们将这个不等式称为该整数优化问题的 **切割平面（Cutting Plane）**。只要能找到一个切割平面，我们就可以将其当作额外的约束，并继续求解优化问题。

有许多种寻找切割平面的方式，下面我们介绍其中一种。

#### Gomory 分数切割

**Gomory 分数切割（Gomory Fractional Cut）** 要求所有变量都是整数（不允许有些是实数范围的），且约束矩阵中全是整数（如果不是的话可以尝试乘以分母的最小公倍数）。比如下面这个整数优化问题：
$$
\begin{align*}
	\underset{x, y \in \mathbb{Z}}{\text{maximize}} \quad & 2x + 3y \\
	\text{subject to} \quad & x + 2y \le 3 \\
	& 4x + 5y \le 10 \\
	& x, y \ge 0
\end{align*}
$$
此处可以引入松弛变量 $s_1, s_2$。由于我们巧妙的设置，可以保证 $s_1, s_2$ 均为整数。随后进行简单形表的变化：
$$
\begin{array}{ccccc|c}
	 & x & y & s_1 & s_2 & \\\hline
	s_1 & 1 & 2 & 1 & 0 & 3 \\
	s_2 & 4 & 5 & 0 & 1 & 10 \\\hline
	-z & 2 & 3 & 0 & 0 & 0
\end{array}
\quad \leadsto \quad
\begin{array}{ccccc|c}
	 & x & y & s_1 & s_2 & \\\hline
	y & 1/2 & 1 & 1/2 & 0 & 3/2 \\
	s_2 & 3/2 & 0 & -5/2 & 1 & 5/2 \\\hline
	-z & 1/2 & 0 & -3/2 & 0 & -9/2
\end{array}
\quad \leadsto \quad
\begin{array}{ccccc|c}
	 & x & y & s_1 & s_2 & \\\hline
	y & 0 & 1 & 4/3 & -1/3 & 2/3 \\
	x & 1 & 0 & -5/3 & 2/3 & 5/3 \\\hline
	-z & 0 & 0 & -2/3 & -1/3 & -16/3
\end{array}
$$
现在我们可以将 $y$ 表示成：
$$
y + \frac{4}{3}s_1 - \frac{1}{3}s_2 = \frac{2}{3} \implies y + s_1 - s_2 + \frac{1}{3}s_1 + \frac{2}{3}s_2 = \frac{2}{3}
$$
由于 $y + s_1 - s_2$ 一定是整数，且 $\frac{1}{3}s_1 + \frac{2}{3}s_2$ 为非负数，我们可以进一步得到：
$$
y + s_1 - s_2 \le 0
$$
这便是 Gomory 分数切割的用法。除了这种构造方式，我们也可以：

- 由于 $y - s_1 - s_2$ 是整数，因此余下的 $\frac{1}{3}s_1 + \frac{2}{3}s_2$ 必定不小于等式右侧向下取整，也即：
  $$
  \frac{1}{3}s_1 + \frac{2}{3}s_2 \ge \frac{2}{3}
  $$

- 我们可以尝试将 $s_1, s_2$ 用 $x, y$ 表示后再代入前面第一种方法得到的结论。从最开始的设置中不难得到 $s_1 = 3 - x - 2y, s_2 = 10 - 4x - 5y$，因此：
  $$
  y + (3 - x - 2y) - (10 - 4x - 5y) \le 0 \implies 3x + 4y \le 7
  $$

这样就得到了下一个线性优化问题：
$$
\begin{align*}
	\underset{x, y \in \mathbb{Z}}{\text{maximize}} \quad & 2x + 3y \\
	\text{subject to} \quad & 2x + 3y \le 3 \\
	& 4x + 5y \le 10 \\
	& 3x + 4y \le 7
\end{align*}
$$
也可以通过前面得到的 $\frac{1}{3}s_1 + \frac{2}{3}s_2 \ge \frac{2}{3}$ 直接引入松弛变量。此时由于不包含任何 $x, y$，我们可以直接得到一个基本变量：
$$
-\frac{1}{3}s_1 - \frac{2}{3}s_2 + s_3 = -\frac{2}{3}
$$
因此得到新的简单形表：
$$
\begin{array}{cccccc|c} 
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	y & 0 & 1 & 4/3 & -1/3 & 0 & 2/3 \\
	x & 1 & 0 & -5/3 & 2/3 & 0 & 5/3 \\
	s_3 & 0 & 0 & -1/3 & -2/3 & 1 & -2/3 \\\hline
	-z & 0 & 0 & -2/3 & -1/3 & 0 & -16/3
\end{array}
$$
利用对偶简单形法可以进一步得到：
$$
\begin{array}{cccccc|c} 
	 & x & y & s_1 & s_2 & s_3 & \\\hline
	y & 0 & 1 & 3/2 & 0 & -1/2 & 1 \\
	x & 1 & 0 & -2 & 0 & 1 & 1 \\
	s_2 & 0 & 0 & 1/2 & 1 & -3/2 & 1 \\\hline
	-z & 0 & 0 & -1/2 & 0 & -1/2 & -5
\end{array}
$$
现在最优解是 $(1, 1)$，而这就是该整数优化问题的最优解。

总结来说，对于非负整数变量 $x_1, ..., x_n$ 及线性方程：
$$
a_1x_1 + ... + a_nx_n = b
$$
我们总能得到：
$$
\lfloor a_1 \rfloor x_1 + ... + \lfloor a_n \rfloor x_n \le \lfloor b\rfloor
$$
另一个常见的形式则是：
$$
(a_1 - \lfloor a_1 \rfloor)x_1 + ... + (a_n - \lfloor a_n \rfloor)x_n \ge b - \lfloor b \rfloor
$$

#### 延伸

我们可以将切割平面法和前面介绍过的分支定界法混合使用，称为分支切割法。对于一个有分数最优解的线性优化问题，我们可以从下面两个选项中执行一个：

- 选择一个分数解 $x_i$，然后分支出两个新的线性优化问题。
- 通过切割平面法将此线性优化问题替换为另一个。

这两者的选择是完全参照直觉的，并没有类似”基准规则“的东西。

### 旅行商问题

**旅行商问题（Traveling Salesman Problem, TSP）** 叙述如下：

> 有 $n$ 个城市，标记为 $1, ..., n$。且两两城市之间的交通有一定成本 $c_{ij}$，其中 $i, j$ 是两个城市。一名从城市 $1$ 出发的商人想要周游所有城市并最终返回到 $1$，我们称此为一个 $n$ 个城市的 *旅行（Tour）*。求最小成本的旅行路径。

由于 $n$ 个城市的排列方式多达 $n!$，我们不可能通过一个个遍历找到最短路径。这是一个比较复杂的问题。现实中有很多问题拥有类似的形式，如果它们能有高效的解决方式是再好不过了：

- 在城市中规划路径，比如垃圾车收拾垃圾、邮递员送信件等。
- 零部件加工，比如在电路板的何处打洞、在木板的何处剪裁等。

为了方便考虑，我们要求在旅途中不能重复访问一个城市。在一些情况下这是比较合理的，比如地图中，成本通常与距离有关，如果重复访问一个地点会导致环路，这一定不是最高效的。另外，现实生活中两点间可能存在多个通勤方式，我们一律将其中最便宜的作为旅行成本。

#### 构建初探

让我们尝试通过整数优化问题构建 **TSP**。对于任两个城市 $i, j$，设 $x_{ij}$ 为商人是否会在该两点间穿梭。也即 $x_{ij}$ 或者为 $0$（商人不采用这个路径），或者为 $1$（商人采用这个路径）。因此，我们的目标是：
$$
\text{minimize} \quad \sum_{i = 1}^n\sum_{j = 1}^n c_{ij}x_{ij}
$$
由于每个城市只会被访问一次，因此对于任何城市 $j$，一定存在 $i, k$ 使得 $x_{ij} = x_{jk} = 1$：
$$
\underset{i \ne j}{\sum_{1 \le i \le n}} x_{ij} = 1 \quad \underset{k \ne j}{\sum_{1 \le k \le n}}x_{jk} = 1 \quad \text{（其中 $j = 1, ..., n$）}
$$
这似乎完美地描述了 **TSP**，且我们可以惊喜地发现约束矩阵是一个全幺模矩阵，因此我们甚至不需要用任何整数优化问题的方法就能得到解……真的是这样么？

上面的构建方式有一个致命的缺陷，那就是一些情况下解描述的根本不是一个合法的旅行路径！比如城市可以被多个互不相连的环路连接，由于旅行商不能使用传送门，这种情况我们必须要避免。

#### 消除子旅程环路

第一个解决方法是加入大量 **子旅程消除约束（Subtour Elimination Constraint）**：
$$
\sum_{i \in S}\sum_{j \notin S} x_{ij} \ge1 \quad \text{（其中 $S$ 是城市集合的子集，且 $1 \le |S| \le n - 1$）}
$$

#### 时间约束

另一种方式是对于每个城市 $i$ 都定义一个时间系数 $t_i$ 表示商人到访这个城市为止已经使用的时间。其关键点在于对于$i, j \ne 1$，如果 $x_{ij} = 1$，则 $t_j \ge t_i + 1$。这里不考虑起始城市是因为它在开头和结尾都会出现。为了把上面的逻辑式表示出，可以使用下面的等价不等式：
$$
t_j \ge t_i + 1 - M(1 - x_{ij})
$$
其中 $M$ 是一个很大的数。如果 $x_{ij} = 0$，我们得到 $t_j \ge t_i + 1 - M$，右侧是一个负数因此没有意义。如果 $x_{ij} = 1$，我们就得到前面给出的 $t_j \ge t_i + 1$。事实上我们可以直接取 $M = n$，此时令 $t_1 = 1$ ，并对于任一个城市，其时间都等于此前拜访城市加一即可。

