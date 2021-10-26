# Intro to Real Analysis

本篇是 *UIUC MATH 447 Real Variables* 的学习笔记，其中包含了实数、实数列、度量空间、以及微积分等知识点；和此前可能接触过的微积分不同，实数分析的数学理论更加严格，包含大量证明。

[TOC]

## 绪论

**实数分析（Real Analysis）**，或实分析是和研究实数以及实变量函数相关的理论，它为 **微积分（Calculus）** 提供了数学理论基础。微积分是现实世界中工程问题必备的数学工具之一，因此为其提供可靠的理论基础非常重要。在一切开始之前，首先要正式地定义 **实数（Real Number）** 的概念。中学阶段我们可能已经学过，实数是数轴上的所有数组成的集合。不过这作为实分析的定义不算合格。为了能够严格地定义实数集 $\mathbb{R}$，我们需要首先从自然数集 $\mathbb{N}$ 开始，然后再建立有理数集 $\mathbb{Q}$，最后再定义实数集。

### 自然数集

我们将自然数集定义为所有正整数的集合，即 $\{1, 2, 3,...\}$。这个集合中的元素是显然的，且它们满足下面列出的性质：

- **N1**：$1 \in \mathbb{N}$。
- **N2**：如果 $n \in \mathbb{N}$，则它的 **后继（Successor）** $n + 1 \in \mathbb{N}$。
- **N3**：$1$ 不是任何自然数的后继。
- **N4**：如果 $n, m \in \mathbb{N}$ 拥有相同的后继，则 $n = m$。
- **N5**：一个 $\mathbb{N}$ 的子集 $N$ 如果包括 $1$ ，且对任何 $n \in N$，都有 $n + 1 \in N$，此时我们可以断定 $N = \mathbb{N}$。

上面这些被称为 **皮亚诺公理（Peano Axiom）**。绝大多数自然数集上的性质都可以通过这些公理得到。它们每一个都是不可缺少的，如果我们假设任意一个不成立，都会出现反例。比如，假设 N5 是错误的，那么我们能够找到某个包含 $1$ 的集合 $N \subset \mathbb{N}$ 使得任意 $n \in N$ 都有 $n + 1 \in N$。假设 $n_0$ 是 $\N$ 中最小的不在 $N$ 中的元素，显然 $n_0 \ne 1$，那么 $n_0$ 一定是某个元素的后继，即 $n_0 - 1$。此时我们有 $n_0 - 1 \in N$。然而，根据 $N$ 的定义，$n_0 - 1 + 1 = n_0 \in N$，这就导致了矛盾。

上面的公理并不是唯一的定义自然数的方式。下面的两种公理体系也可以定义等价的自然数集：

- **A1**： $\mathbb{N}$ 是一个无限集。
- **A2**：$1 \in \mathbb{N}$。
- **A3**：如果 $n \in \mathbb{N}$，则它的后继 $n + 1 \in \mathbb{N}$。
- **A4**：一个 $\mathbb{N}$ 的子集 $N$ 如果包括 $1$ ，且对任何 $n \in N$，都有 $n + 1 \in N$，此时我们可以断定 $N = \mathbb{N}$。

以及：

- **B0**：存在一个函数 $S: \N \to \N$。
- **B1**：$\operatorname{Im}(S) = \N\backslash\{1\}$。
- **B2**：$S$ 是一个单射。
- **B3**：一个 $\N$ 的子集 $N$ 如果包含 $1$，且 $S(N) \subset N$，此时我们可以断定 $N = \N$。

不难证明上面的两套说法和皮亚诺公理是等价的。我们可以从任一套公理推出任一条其它的公理。我们接下来主要将 **B** 作为自然数集的公理。

### 关系

本节回忆在离散数学中学习的内容。一个集合 **X** 上的 **关系（Relation）** $R$ 是 $X\times X$ 的一个子集。如果两个元素 $a, b \in X$ 满足这个关系，可以写成 $(a, b) \in R$ 或 $a \sim b$。我们对 **等价关系（Equivalence Relation）** 比较感兴趣，它是指满足下面三种条件的关系：

- **自反（Reflexive）**： 即对于任意 $x \in X$ 都有 $x \sim x$。
- **对称（Symmetric）**：即若 $x \sim y$ 则 $y \sim x$。
- **传递（Transitive）**：即若 $x \sim y$ 且 $y \sim z$，则 $x \sim z$。

对于等价关系 $R$，我们定义如下记号：

- 函数 $[]: X \to \mathbb{P}(X)$，记 $[](x)$ 为 $[x]$。
- $[x]$ 定义为 $\{y \mid x \sim y\}$。它是一个集合，我们将其称为 $x$ 的等价类。
- $X/\sim = \operatorname{Im}([]) \subseteq \mathbb{P}(X)$。这借用了商群的记号。

关于等价类有一个重要的结论：$[x] = [y]$ 或 $[x] \cap [y] = \emptyset$。这是因为如果存在 $x_0 \in [x], x_1 \in [y], x_2 \in [x] \cap [y]$，我们有 $x_0 \sim x_2, x_1 \sim x_2 \implies x_0 \sim x_1$，故 $[x] = [y]$。因此，$X/\sim$ 是由一系列不相容的集合组成的，且：
$$
X = \bigcup_{Y \in X/\sim} Y \nonumber
$$
作为等价关系的例子，考虑一个关系，定义为 $a \sim b$ 当且仅当 $q \mid a - b$。此时 $[k] = \{k, q+k, 2q + k, ...\}$ 就是一个等价类。

### 有理数集

为了引入有理数集，我们需要介绍 **半群（Semigroup）**。一个交换半群 $(S, +)$ 需要满足二元运算 $+$ 的结合律和交换律。下面我们证明半群 $(\N, +)$ 满足消去律，即对于任意 $a, b, c \in S$：
$$
a + c = b + c \implies a = b \nonumber
$$

> **证**：通过数学归纳法证明（对 $c$ 进行归纳） $A(m)$：对任意 $k, l \in \N$ 都有 $k + m = l + m \implies k = l$。
>
> - $A(1)$：显然成立，因为 $ k+ 1 = l + 1 \implies k = l$ 即 $S(k) = S(l) \implies k = l$ 源自公理 **B1**。
> - $A(m)$：假设 $A(m)$ 成立，即 $k + m = l + m \implies k = l$ 成立。
> - $A(m + 1)$：已知 $k + m + 1 = l + m + 1$ 此时有 $S(k + m) = S(l + m)$，根据公理 **B1** 有 $k + m = l + m$，根据 $A(m)$ 则有 $k = l$，得证。

定义满足消去律的非空交换半群 $S$ 的 **Grothendieck 群（Grothendieck Group）**为最小的包含 $S$ 的交换群。一个交换群 $(S, +)$ 需要满足二元运算 $+$ 的结合律、交换律，并有唯一的单位元 $0$ 使得对任意 $a \in S$ 都有 $e + a = a + e = a$，并存在任意 $a \in S$ 的唯一逆元 $b$ 使得 $a + b = b + a = e$。

下面证明 Grothendieck 群的存在性：

> **证**：定义 $S \times S$ 上的二元关系 $\sim$，使对任意 $a, b, c, d \in S$，当 $a + d = b + c$ 时有 $(a, b) \sim (c, d)$。我们声明这是一个等价关系。自反性和对称性是显然的。传递性可以参考下面的论述，假设 $(a, b) \sim (c, d) \sim (e, f)$：
> $$
> \begin{align*}
> 	(a, b) \sim (c, d) \implies a + d = b + c && (1)\\
> 	(c, d) \sim (e, f) \implies c + f = d + e && (2)\\
> 	(1) + (2) \implies a + d + c + f = b + c + d + e
> \end{align*}
> $$
> 根据消去律我们可以得到 $a + f = b + e$，即 $(a, b) \sim (e, f)$。这样我们就证明了这是一个等价关系。
>
> **定义**：**Grothendieck 群** $G = (S\times S)/\sim$。下面我们将证明它是最小的包含 $S$ 的交换群。
>
> 首先，定义 $G$ 上的二元运算 $+$ 为 $[(a, b)] + [(c, d)] = [(a + c, b + d)]$。显然这个运算满足结合律和交换律。随后，定义单位元 $[(a, a)] = 0$，这里的 $a \in S$ 是任取的，因为任意 $a, b \in S$ 都有 $(a, a) \sim (b, b)$，同时 $[(a, a)] + [(b, c)] = [(a + b, a + c)] = [(b, c)]$。最后定义 $[(a, b)]$ 的逆元 $[(b, a)]$，因为 $[(a + b, b + a)] = 0$。这样我们就得到 $G$ 是一个交换群。
>
> 为了证明 $S \subset G$，只需考虑 $\{[(0, a)]\mid a \in S\}$ ，显然它和 $S$ 能够形成一个双射 $(0, a) \mapsto a$。假设存在 $H$ 使得 $S \subset H$，$\notprovedyet$。

我们对 Grothendieck 的一个重要的应用是将自然数集扩充为整数集：$(\N, +)$  的 Grothendieck 群满足下面条件时，就等价于 $\Z$：
$$
(a, b) =
\left.\begin{cases}
	((b - a), +) & \text{若 $a < b$} \\
	((a - b), -) & \text{若 $a > b$} \\
	0 & \text{若 $a = b$}
\end{cases} \right\} \in (\N, \{+\})\cup (\N, \{-\})\cup \{0\}\nonumber 
$$
注意其中的 $0$ 是 Grothendieck 群中的单位元。

现在我们考察 $(\N, \cdot)$ 的 Grothendieck 群。我们将其中的元素记为下面的形式：
$$
\frac{p}{q} = [(p, q)] = \{(pr, qr) \mid r \in \N\}, p, q \in \N
$$
为了标记方便，我们以后将半群 $S$ 的 Grothendieck 群写成 $G(S)$。





