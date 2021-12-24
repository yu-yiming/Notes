# Intro to Real Analysis

本篇是 *UIUC MATH 447 Real Variables* 的学习笔记，其中包含了实数、实数列、度量空间、以及微积分等知识点；和此前可能接触过的微积分不同，实数分析的数学理论更加严格，包含大量证明。参考教材是 *Elementary Analysis: Kenneth A. Ross*。

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

> **证明**：通过数学归纳法证明（对 $c$ 进行归纳） $A(m)$：对任意 $k, l \in \N$ 都有 $k + m = l + m \implies k = l$。
>
> - $A(1)$：显然成立，因为 $ k+ 1 = l + 1 \implies k = l$ 即 $S(k) = S(l) \implies k = l$ 源自公理 **B1**。
> - $A(m)$：假设 $A(m)$ 成立，即 $k + m = l + m \implies k = l$ 成立。
> - $A(m + 1)$：已知 $k + m + 1 = l + m + 1$ 此时有 $S(k + m) = S(l + m)$，根据公理 **B1** 有 $k + m = l + m$，根据 $A(m)$ 则有 $k = l$，得证。

定义满足消去律的非空交换半群 $S$ 的 **Grothendieck 群（Grothendieck Group）**为最小的包含 $S$ 的交换群，记为 $G(S)$。一个交换群 $(S, +)$ 需要满足二元运算 $+$ 的结合律、交换律，并有唯一的单位元 $0$ 使得对任意 $a \in S$ 都有 $e + a = a + e = a$，并存在任意 $a \in S$ 的唯一逆元 $b$ 使得 $a + b = b + a = e$。

下面证明 Grothendieck 群的存在性：

> **证明**：定义 $S \times S$ 上的二元关系 $\sim$，使对任意 $a, b, c, d \in S$，当 $a + d = b + c$ 时有 $(a, b) \sim (c, d)$。我们声明这是一个等价关系。自反性和对称性是显然的。传递性可以参考下面的论述，假设 $(a, b) \sim (c, d) \sim (e, f)$：
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
> 为了证明 Grothendieck 群是最小的包含 $S$ 的交换群，我们只需要尝试构造 $G$ 可能的子群，比如 $H = \{[(a, 2a)]\}$ 也包含 $S$（存在对应 $a \mapsto [(a, 2a)]$）且构成一个群（仍使用 $G$ 中的运算），且显然是 $G$ 的子集，因此我们有 $S \subseteq H \subseteq G$。下面我们证明 $H = G$：对任意的 $[(m, n)] \in G$，都有：
> $$
> [(m, n)] + [(2n, n)] + [(m, 2m)] = [(2m + 2n, 2m + 2n)]\nonumber
> $$
> 式中 $[(2n, n)] = -[(n, 2n)] \in H, [(m, 2m)] \in H, [(2m + 2n, 2m + 2n)] = 0 \in H$，因此 $[(m, n)] \in H$。这样就得到了 $G \subseteq H$，故 $G = H$。就此得证。

上面的定义也可以叙述为以下引理：

> **Grothendieck 引理**：如果交换半群 $(S, +)$ 满足消去律，则存在一个最小的包含 $S$ 的交换群 $G(S)$。

我们对 Grothendieck 的一个重要的应用是将自然数集扩充为整数集：$(\N, +)$  的 Grothendieck 群满足下面条件时，就等价于 $\Z$（后文中我们会数次用到下面的构造）：
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
这就是我们熟悉的有理数的分数形式。不过在研究它之前，我们似乎有一个重要的缺失步骤：证明 $(\N, \cdot)$ 满足消去律。为了证明这一点，我们首先引入几个记号：

> **定义**：对于 $a, b \in \N$，如果存在 $k \in \N$ 使得 $a + k = b$ 成立，我们记 $a < b$。

> **备注**：对于任意的 $a, b, \in \N$，要么 $a < b$，要么 $b < a$，要么 $a = b$。

下面我们尝试证明消去律的逆否命题，即对于 $m, n, n' \in \N$，若 $n \ne n'$，则 $nm \ne n'm$。

> **证**：不失一般性，我们假设 $n < n'$，此时让我们证明 $B(m): nm < n'm$。对 $m$ 使用数学归纳法：
>
> - $B(1)$：此时 $n < n'$，显然成立。
> - $B(m)$：假设 $B(m)$ 成立，即 $nm < n'm$ 成立。
> - $B(m + 1)$：此时有 $n(m + 1) = nm + n < n'm + n' = n'(m + 1)$，因此 $B(m + 1)$ 成立。
>
> 这样就证明了原命题的成立。

现在我们可以正式定义有理数集为：
$$
\Q^+ = G(\N, \cdot) \qquad \Q = G(\Z\backslash\{0\}, \cdot)
$$
$\Q$ 也可以定义为 $G(\Q^+, +)$，不过此时这里的 $+$ 要特别定义出来：
$$
\frac{p}{q} + \frac{p'}{q'} = \frac{pq' + qp'}{qq'}
$$
有理数集有一系列性质：

- $\Q$ 是一个有全序关系的 **域（Field）**，$\frac{p}{q} < \frac{p'}{q'}$ 等价于 $pq' < qp'$。其中域的公理如下：

  - $(\Q, +)$ 是一个交换群。
  - $(\Q\backslash \{0\}, \cdot)$ 是一个交换群。
  - $+, \cdot$ 满足分配律，即 $a\cdot(b + c) = a\cdot b + a\cdot c$。

- $\Q$ 在序有关的规律如下：

  - $\forall\ a, b, c \quad a < b \implies a + c < b + c$ 
  - $\forall\ a, b, c$ 且 $c > 0 \quad a < b \implies ac < bc$。
  - $\forall\ a < b$ 或 $b < a$ 或 $a = b$。

- $\Q$ 不是 **完备的（Complete）**。有序域的完备性定义如下：

  > **定义**： 设 $F$ 是一个有序域，且 $ b\in F$，$A \subseteq F$，则我们称 $b$ 是 $A$ 的 **上界（Upper Bound）** 当且仅当对任意 $a \in A$ 都有 $a \le b$，记作 $A \le b$。当 $b$ 是 $A$ 所有上界中最小的那个时（即 $A \le b$ 且 $A \le b' \implies b \le b'$），我们称 $b$ 是 $A$ 的上确界，记作 $b = \sup{A}$。
  >
  > 有更严格的方式描述 $\sup A$ 是 $A$ 的最小上界：对任意 $\epsilon > 0$，都存在 $a \in A$ 使得 $\sup A - a < \epsilon$。

  > **定义**： 一个 **完备的（Complete）** 有序域要求其任意有上界的子集（$\exists\ b', A \le b'$）都有上确界，即 $\exists\ b = \sup{A}$。
  
  作为例子，$\{x \in \Q\mid x^2 < 2\}$ 不存在上确界。

由于有理数集的不完备，我们不难发现有理数之间存在大量的“漏洞”。一个理应有解的方程在有理数中并不能找到解，事实上 **有理数零点定理** 叙述了以下的事实：

> **定理**：方程 $\sum_{n=0}^n c_ks^k = 0$ 有有理数解 $\frac{p}{q}$ 当且仅当 $q \mid c_0$ 且 $p \mid c_n$。

通过这个定理我们可以证明许多数是否是有理数，比如 $2 + \sqrt{17}$，设其为 $x$ 则可以得到式子 $(x - 2)^2 = 17 \implies x^2 - 4x - 13 = 0$。如果它是一个有理数，分母只可能是 $1$，分子可以是 $1$ 或 $13$。显然任一种情况均不满足，故 $2 + \sqrt{17}$ 不是有理数。

### 实数集

我们可以直接给出一个事实，实数集 $\R$ 是最小的包含 $\Q$ 的完备集。下面将一步步构建 $\R$。

> **定义**：**戴德金分割（Dedekind Cut）** 是有理数集 $\Q$ 的一个子集 $A$，使得：
>
> - $\forall\ a \in A, b < a \implies b \in A$。
> - $\forall\ a\in A, \exists\ a' \in A$ 使得 $a < a'$。

比如 $A_\sqrt{2} = \{x \in \Q\mid x^2 < 2\ \text{且}\ x \le 0\}$ 就是一个戴德金分割。下面是证明：

> **证明**：$x \le 0$ 以及第一个条件的情形是显然的，我们下面证明对于任意 $A_\sqrt{2}$ 中的 $\frac{p}{q} > 0$，都有 $x > \frac{p}{q}$ 使得 $x^2 < 2$ 成立。假设 $x = \frac{p}{q} + \varepsilon$，这里的 $\varepsilon > 0$ 是一个满足特定条件的小值令 $(\frac{p}{q} + \varepsilon)^2 < 2$。下面我们尝试找到 $\varepsilon$：
> $$
> \begin{align*}
> \left(\frac{p}{q} + \varepsilon\right)^2 &= \frac{p^2}{q^2} + \frac{2p}{q}\varepsilon + \varepsilon^2 \\
> &\le \frac{p^2}{q^2} + (\frac{2p}{q} + q)\varepsilon \\
> &< 2 \\
> \implies \varepsilon &\le \frac{2q^2 - p^2}{2pq + q^3} 
> \end{align*}
> $$
> 由于 $\frac{p}{q} < 2 \implies p^2 < 2q^2$，我们总能找到一个正有理数 $\varepsilon$ 满足上面的不等式。

我们将首先证明 $R = \{D \subseteq \Q\}$ （即所有戴德金分割的集合）是一个完备集。为了后文表述方便，记两个集合 $D_1 \le D_2$，如果 $D_1 \subseteq D_2$。

> **证明**：令 $S \subseteq R$，$D \in R$，且对于任意 $s \in S$ 均有 $s \le D$（即 $D$ 是所有 $s$ 的上界）。将 $S$ 的上确界定义为：
> $$
> \sup{S} = \bigcup_{s \in S} s \nonumber
> $$
> 不难发现  $\sup{S} \in R$ 且 $\sup{S} \le D$ 。此外作为验证，对 $S$ 的任意上界 $D' \in R$，若对于任何 $s \in S$ 均有 $s \le D'$，则有 $\sup{S} = \bigcup s \le D '$，因此 $\sup{S}$ 确实是上确界。综上，我们得到了 $R$ 是一个完备集的结论。

不过实数集并不等价于 $R$，因为对于 $\Q \in R$，其对应的是 $\R$ 中不存在的 $\infty$。因此实数集实际上的定义是：
$$
\R = R\backslash\Q = \{D \subset \Q\}
$$
如果我们借用 $R$ 完备的结论，实数集 $\R$ 的完备性是比较显然的，它常常被叙述为如下公理：

> **完备性公理（Completeness Axiom）**：实数集 $\mathbb{R}$ 的任意有上界子集 $S$ 都有最小上界，也即 $\sup S \in \mathbb{R}$。这个公理与其对称叙述等价，即实数集的任意有下界子集 $S$ 都有最大下界。

为了方便定义 $\R$ 上的运算，先定义 $\R^+ = \{D \in \R\mid (-\infty, 0] \subseteq D\}$，并定义 $\R^+$ 上的 $+$ 和 $\cdot$ 运算。对于 $D_1, D_2 \in \R^+$：
$$
\begin{align*}
D_1 + D_2 &= \{d_1 + d_2 \mid d_1 \in D_1, d_2 \in D_2\} \\
D_1 \cdot D_2 &= \{d_1 \cdot d_2 \mid d_1 \in D_1, d_2 \in D_2\}
\end{align*}
$$
它们遵循结合律、交换律和分配律：

- $D_1 + (D_2 + D_3) = (D_1 + D_2) + D_3$.
- $D_1\cdot(D_2\cdot D_3) = (D_1\cdot D_2) \cdot D_3$。
- $D_1 + D_2 = D_2 + D_1$。
- $D_1\cdot D_2 = D_2 \cdot D_1$。
- $D_1\cdot(D_2 + D_3) = D_1\cdot D_2 + D_1\cdot D_3$。

由此我们可以得到 $(\R^+, +)$ 和 $(\R^+, \cdot)$ 均为满足消去律的半群。最终，我们可以得到它的 Grothendick 群，而它实际上就是我们的实数集，即 $\R = G(\R^+)$。

> **性质**：实数集 $\mathbb{R}$ 是一个全序集。

> **证明**：对于任意两个戴德金分割 $D_1, D_2$，如果 $D_1 \subseteq D_2$ 则没有问题。若 $D_1 \nsubseteq D_2$，则一定存在 $d_1 \in D_1$ 使得 $d_1 \notin D_2$。此时显然有 $D_2 \le d_1$，因此 $D_2 \subseteq D_1$。

> **性质**：有理数在实数集中稠密。

> **证明**：由 $D_1 < D_2$ 可得 $D_1 \subset D_2$，故存在 $d \in D_2$ 满足 $d \notin D_1$。此时有 $D_1 \subset (-\infty, d) \subset D_2$。

> **定理**：实数集 $\mathbb{R}$ 不可数。

> **证明**：让我们先证明下面的引理：
>
> > **引理**：$\mathbb{P}(\mathbb{N})$ 是不可数集。
>
> > **证明**：假设 $\mathbb{P}(\mathbb{N})$ 可数，则一定存在双射 $\varphi : \mathbb{N} \to \mathbb{P}(\mathbb{N})$ 。考虑 $\mathbb{N}$ 的子集 $B = \{n \in \mathbb{N} \mid n \notin \phi(n)\}$。由于 $B \in \mathbb{P}(\mathbb{N})$，存在 $m$ 使得 $\varphi(m) = B$。由 $B$ 的定义可知 $m \notin B$，但此时又符合了 $B$ 中元素的定义，因此 $m \in B$ 构成矛盾。
>
> 下面为了证明 $\mathbb{R}$ 不可数，我们只需要构建一个单射 $\varphi : \mathbb{P}(\mathbb{N}) \to [0, 1]$ 即可。实际上可以定义：
> $$
> \varphi(A) = \sum_{k \in A}3^{-k} = \sup \left\{\sum_{k \le n} 3^{-k} \mid k \in A, n \in \mathbb{N}\right\}
> $$
> 我们还没有正规地引入级数地概念，但是上面第二个等式给出了其等价定义。这个函数是良定义的，因为对于任意 $A, n \in \mathbb{N}$ 都有：
> $$
> \sum_{k \in A, k \le n} 3^{-k} \le \sum_{k=1}^n 3^{-k} = \frac{1}{3}\frac{1-3^{-n}}{1-3^{-1}} \le \frac{1}{2}
> $$
> 所以 $\varphi(A)$ 上有界，根据完备性公理则上确界存在。
>
> 接下来，需要证明 $\varphi$ 是一个单射。这等价于对任何不相等的 $A, B \in \mathbb{P}(\mathbb{N})$ 都有 $\varphi(A) \ne \varphi(B)$。自然数的幂集可以等价表示为一个无限长的二进制序列 $b_1b_2...$，其中每一项 $b_n$ 代表自然数 $n$ 是否出现在该集合中。由于这两个集合 $A = a_1a_2..., B = b_1b_2...$ 不相等，让我们假设其第一个不相同的地方为 $m_0$。即：
> $$
> m_0 = \min\{m \mid a_{m} \ne b_{m}\}
> $$
> 这暗示了对于任何 $j < m_0$ 都有 $a_j = b_j$。假设：
> $$
> \alpha = \sum_{j \in A, j < m_0} 3^{-j} = \sum_{j \in B, j < m_0}3^{-j}
> $$
> 则对于 $A$ 有：
> $$
> \varphi(A) \ge \alpha + \frac{1}{3^{m_0}}
> $$
> 对于 $B$，记：
> $$
> \varphi_k(B) = \sum_{j \in B, j \le k}3^{-j} = \alpha + \sum_{j \in B, m_0 < j \le k}3^{-j} \qquad (k > m_0)
> $$
> 进行进一步推导则有：
> $$
> \varphi_k(B) \le \alpha + \sum_{j \in B, j > m_0}3^{-j} \le \alpha + \frac{1}{3^{m_0+1}}\frac{3}{2} = \alpha +  \frac{1}{2\cdot 3^{m_0}} < \alpha + \frac{1}{3^{m_0}} = \varphi(A)
> $$
> 因此对任选的 $k > m_0$ 都有 $\varphi_k(B) < \varphi(A)$。因此我们得到：
> $$
> \varphi(B) \le \sup \sum_{j \in A, j \le k} 3^{-j} = \varphi_k(B) < \varphi(A)
> $$
> 也即 $\varphi(A) \ne \varphi(B)$。这就证明了 $\varphi$ 是单射。此时我们有 $\operatorname{Im}(\varphi) \subset [0, 1]$，后者一定是不可数集。由于 $\mathbb{R}$ 的子集不可数，其本身也一定不可数。

> **阿基米德性质**：对于任意自然数 $a \in \mathbb{N}$，总存在 $n \in \mathbb{N}$ 使得 $n > a$。

> **证明**：假设对于某个 $a$ 不存在 $n \in \mathbb{N}$ 使得 $n > a$。此时 $\mathbb{N}$ 存在上界。假设 $S = \sup \mathbb{N}$，此时有：
>
> - 对于任意 $n \in \mathbb{N}$ 均有 $n \le S$。
> - 对于任意正数 $\epsilon$ 都存在 $n \in \mathbb{N}$ 使得 $S < a + \epsilon$。
>
> 因此，一定存在 $n$ 使得 $S - \frac{1}{2} < n < S$。此时有 $n + 1 > S - \frac{1}{2} + 1 = S + \frac{1}{2} > S$，和条件矛盾。

## 数列与极限

### 实数列

一个 **实数列（Sequence of Real Numbers）** 定义了一个函数 $x : \mathbb{N} \to \mathbb{R}$，我们将 $x(n), n \in \mathbb{N}$ 记作 $x_n$。由于本篇我们不会考虑复数，所以后文中提及 **数列（Sequence）** 时总是指实数列。一个数列是 **单调的（Monotone）**，如果满足下面两个条件之一：

- 对于任何 $n \in \mathbb{N}$，都有 $x_n \le x_{n+1}$，此时我们称其为递增（或非递减）数列。如果将前面的条件换为 $x_n < x_{n+1}$，则称其为严格递增数列。
- 对于任何 $n \in \mathbb{N}$，都有 $x_n \ge x_{n+1}$，此时我们称其为递减（或非递增）数列。如果将前面的条件换为 $x_n > x_{n+1}$，则称其为严格递减数列。

数列有一个很有趣的性质：

> **定理**：任何数列都有一个单调的子数列。定义 $\{s_n\}$ 的子数列 $\{t_{n}\}$ 可以定义为 $t_k = s_{n_k}$，其中 $n_k$ 是一个严格递增的自然数列。

> **证明**：首先定义数列的 **峰点（Peak）**：
>
> > **定义**：数列 $\{s_n\}$ 的峰点 $k \in \mathbb{N}$ 满足对于任意 $n \ge k$ 都有 $s_{n} < s_{k}$。
>
> 记 $P$ 为数列 $\{s_n\}$ 的峰点集，它或者是一个有限集，或者是一个无限集：
>
> - $P$ 有限时，只需选取任意 $n_1 \in \mathbb{N}$ 令 $n_1 > \sup P$，由于它不是峰点，则一定存在 $n_2 \in \mathbb{N}, n_2 > n_1$ 使得 $s_{n_2} \ge s_{n_1}$。然后，也一定存在 $n_3 \in \mathbb{N}, n_3 > n_2$ 使得 $s_{n_3} \ge s_{n_2}$。以此类推我们就得到了一个单调递增的子数列：$\{s_{n_1}, s_{n_2}, s_{n_3}, ...\}$。
> - $P$ 无限时，假设其中的元素为 $p_1, p_2, p_3,...$。由于这些元素在原数列中按照顺序排列，根据峰点的定义有p_1 > p_2 > p_3 > ...$。这样我们就得到了一个单调递减的子数列。
>
> 上面由于峰点的定义是严格序关系，因此构建的子数列的序严格性不一样，但这并不影响结果。

数列的极限定义如下：

> **定义**：对于数列 $\{s_n\}$，其 **极限（Limit）** 为 $s$，如果满足下面的条件：
>
> - 对于任意 $\epsilon > 0$，都存在 $N \in\mathbb{N}$，使得当 $n > N$ 时，$|s_n - s| < \epsilon$。
>
> 如果我们能为一个数列找到这样的值 $s \in \mathbb{R}$，则称该数列是 **收敛的（Convergent）**。

将极限的定义和上确界（与下确界）似乎有些相似，都是找到一个“无法越过”的边界。只不过对于极限来说，这个边界同时对两个方向都有要求：数列中所有大于极限的数的“趋势”不能从上方越过极限，而所有小于极限的数的“趋势”不能从下方越过极限。作为例子，$s_n = (-1)^n\frac{1}{n}$ 就是一个极限为 $0$ 的数列。

> **命题**：所有有界单调数列都收敛。一个数列如果其所有数组成的集合有界，我们就称该数列有界。

> **证明**：不失一般性地，假设 $\{x_n\}$ 是一个有界单调递增数列。
>
> > **声明**：$\sup x_n = \lim x_n$。
>
> > **证明**：令 $s = \sup x_n$。根据上确界的定义，我们得到下面的条件：
> >
> > - 对于任意 $n$，均有 $a_n \le s$。
> > - 存在 $N$ 使得 $s - \epsilon < x_N \le s$。
> >
> > 这样我们就有对于任意 $n > N$，此时 $x_N < x_n < s$，因此 $|x_n - s| < \epsilon$，正好符合 $\lim x_{n}$ 的定义。
>
> 这也就证明了有界单调数列收敛，且收敛于其上确界（递增时）或下确界（递减时）。

> **定理**：所有有界数列都有一个收敛的子序列。

> **证明**：根据前面提到的[定理]()，可以找到一个单调的子序列。由于原数列有界，子数列也一定有界。根据上一条命题可以得到该子序列收敛。

上面这个定理其实有一个更广泛的版本，即“所有有界区间都是 **序列紧致（Sequentially Compact）** 的”。其中，序列紧致定义为其中的每个序列都有一个收敛子序列。

下面让我们定义数列的极限上确界：

> **定义**：定义数列的 **极限上确界（Limit Supremum）** 为 $\lim\sup x_n = L$，如果满足下面的条件：
>
> - 对任意 $\epsilon > 0$ 都存在 $N \in \mathbb{N}$，令任意 $n > N$ 都有 $x_n < L + \epsilon$。
> - 对任意 $\epsilon > 0$ 以及 $N \in \mathbb{N}$，存在 $n > N$ 使得 $x_n > L - \epsilon$。

极限下确界的定义是类似的。由这个定义可以得到一个引理：

> **引理**：若数列 $\{x_n\}$ 上有界，则：
>
> - $\displaystyle\lim\sup x_n = \inf_m\sup_{n > m} x_n$。
> - $\displaystyle\lim\sup x_n = \sup\{\lim_{k}x_{n_k}\}$，其中 $\{x_{n_k}\}$ 是 $\{x_n\}$ 的收敛子数列。

> **证明**：
>
> 对于第一条，假设等式右侧等于 $L'$，则对于任意 $\epsilon > 0$ 都存在 $m$ $\displaystyle\sup_{n > m}x_n - L' < \epsilon$。这满足了极限上确界成立的第一项。现在任选 $\epsilon, m > 0$，记 $\displaystyle \sup_{n>m}x_n = s_m$，则存在 $n > m$ 使得：
> $$
> x_n > s_m - \epsilon \ge \inf s_m - \epsilon = L' - \epsilon
> $$
> 因此左右等式相等。
>
> 第二条的证明首先需要一个备注：
>
> > **备注**：若数列上有界且极限为 $L$，则其极限小于等于数列的任意上界。
>
> > **证明**：设 $\lim x_n = L$，且有 $x_n \le b$，即 $b$ 是数列的一个上界。我们需要证明 $L \le b$。不然，则存在 $\epsilon > 0$ 使得 $b = L - \epsilon$。此时如果存在 $N \in \mathbb{N}$ 使得 $n > N$ 时有 $|x_n - L| < \epsilon$，则有：
> > $$
> > L - \epsilon < x_n < L + \epsilon
> > $$
> > 由左边的不等式推出 $x_n > L - \epsilon = b$，这和 $b$ 是 $\{x_n\}$ 上界相矛盾。因此一定有 $L \le b$。
>
> 设 $\sup\{\lim_k x_{n_k}\} = L''$，其中我们只选取存在极限的子数列。
>
> > **声明**：$\displaystyle L'' \le \lim\sup x_n = \inf_m\sup_{n > m}x_n$。
>
> > **证明**：记 $x = \displaystyle \lim_kx_{n_k}$，则对于任意 $\epsilon > 0$，$m \in \mathbb{N}$，都存在 $K \in \mathbb{N}$ 令任意 $k > K$ 都有 $x < x_{n_k} + \epsilon$。让我们取一个令 $n_k > m$ 的 $k > K$。此时有：
> > $$
> > x - \epsilon < x_{n_k} \le \sup_{n > m} x_n \xRightarrow{m\ \text{是任取的}} x - \epsilon \le \inf_m\sup_{n > m}x_n \xRightarrow{\epsilon\ \text{是任取的}} x \le \inf_m\sup_{n > m}x_n
> > $$
> > 因此所有 $x$ 的上确界一定小于等于不等式右侧，我们得到了结论。
>
> > **声明**： $\displaystyle L'' \ge \lim\sup x_n = \inf_m\sup_{n > m}x_n$。
>
> > **证明**：记 $s_m = \displaystyle \sup_{n>m}x_n$，$\displaystyle s_\infty = \inf_ms_m$，$\epsilon_k = 2^{-k}$。现在尝试证明命题 $A(k)$：存在 $n_1 < ... < n_k$ 使得 $s_\infty - \epsilon_j < x_{n_j} < s_\infty + \epsilon_j$。
> >
> > - $A(1)$，由下确界定义，一定存在 $m_1$ 使得 $s_\infty \le s_{m_1} < s_\infty + \epsilon_1$。再根据上确界定义，一定存在 $n_1 > m_1$，使得 $s_{m_1} - \epsilon_1 < x_{n_1} < s_{m_1}$。将两个不等式合并后得到 $s_\infty - \epsilon_1 \le s_{m_1} - \epsilon_1 < x_{n_1} < s_{m_1} < s_\infty + \epsilon_1$。
> > - $A(k) \implies A(k + 1)$。首先构建 $n_{j, j < k}$。定义 $\tilde{m} = n_k$，则存在 $m > \tilde{m}$ 令 $s_m \le s_\infty +\epsilon_{k+1}$。$\lackproof$
>
> 至此我们就证明了引理。下有界的情形是完全对称的，这里我们就不再证明。

> 
