# Logic Theory

本篇是有关数学符号逻辑基础的个人笔记。



[TOC]

数学中的一些概念，是建立在一系列 **符号（Symbol）** 之上的；这些符号描述数学中的 **结构（Structure）**，即对象的构成性质，以及结构上的 **公理（Axiom）**，即结构需要满足的条件。虽然数学分支众多，性质庞杂，我们依然能够找出每个领域共有的一些数学概念。本篇笔记将使用一系列自洽的符号，来描述数学中常见的结构和公理。
$$
\newcommand{is}[0]{\qquad\Leftrightarrow\qquad}
$$




## 朴素集合论

### 外延性

**集合论（Set Theory）** 是大多数数学分支使用的数学语言，因此即使我们还没有足够知识储备以严肃地讨论其本质，也不得不先引入集合的概念。

> 定义：**集合（Set）** 是由一系列 **元素（Element）** 构成的对象。这里的“元素”本身也是对象，只不过在谈论集合的语境时我们会将构成集合的对象称为元素。

> 记号：集合会使用大写的拉丁或希腊字母表示，如 $A, Z, \Gamma$ 等；元素会用小写拉丁字母或希腊字母表示，如 $a, z, \gamma$ 等。如果某个元素 $a$ 参与构成了某个集合 $A$，那么我们称 $a$ 属于 $A$，记作 $a \in A$（罕见地也写作 $A \ni a$）。

> 举例：数学中常见的数集，如 $\mathbb{N}$（自然数集）、$\mathbb{Q}$（有理数集）、$\mathbb{C}$（复数集）等顾名思义都是集合。因为集合本身也是元素，所以完全可以构建一个包含集合的集合。

不过，只用一个字母来表示某个集合，我们很难分析它的性质，除了不包含任何元素的集合，即 **空集（Empty Set）**，记作 $\emptyset$；下面介绍两种表示集合的记号。

集合可以通过 **枚举（Enumeration）** 得到，比如“我的家族”是由“我父亲”、“我母亲”和“我”三个元素组成的集合。这样的集合可以通过下面的形式表示：
$$
\begin{equation}
	\{ 我父亲, 我母亲, 我 \}
\end{equation}
$$
此外，也可以通过一个 **谓词（Predicate）**，即判断条件来定义集合，比如“蚂蚁”是由“世界上所有蚂蚁”组成的集合，我们可以通过下面的形式表示：
$$
\begin{equation*}
	\{ x \mid x 是蚂蚁 \}
\end{equation*}
$$
上面这样由大括号包围的，里面或利用枚举或利用谓词展示出来的集合记号被称为 **集合概括（Set Comprehension）**。

> 讨论：并不是填入任何内容都能构成一个集合概括，考虑下面这个”集合“：
> $$
> \begin{equation*}
> 	\{ x \mid \lnot (x \in x) \}
> \end{equation*}
> $$
> 乍一看这是一个比较怪异的定义：怎么会有 $x \in x$ 这样的写法呢？回顾此前我们所说的，集合的元素可以是集合，那么其自身作为集合也可能是属于自己的一个元素。进一步探究，如果记上面这个集合为 $R$，让我们考虑 $R$ 是否是属于自己的一个元素（即判断是否 $R \in R$）。
>
> 从朴素的集合思想出发，一个元素要么属于一个集合，要么不属于它。假设 $R \in R$，那么根据里面给出的谓词 $\lnot (x \in x)$，我们就有 $\lnot (R \in R)$，产生了矛盾。如果假设 $\lnot (R \in R)$，那么由于 $R$ 满足了谓词的要求，我们又得到 $R \in R$，再次矛盾。因此这样的集合不应该存在。
>
> 上面这个似是而非的命题被称为 **罗素悖论（Russell's Paradox）**；对”怎样的集合才是一个集合“这样的问题，我们还缺少一些关键的工具；在本章介绍的朴素集合论中，我会避开这样病态的集合。

集合中如果只有有限个元素，那么称其为 **有限集（Finite Set）**，否则是 **无限集（Infinite Set）**。

> 举例：$\emptyset$、$\{\emptyset\}$、$\{x \in \mathbb{Z} \mid x^2 \le 100 \}$ 都是有限集，而 $\mathbb{R}$、$\{ x \in \mathbb{Q} \mid x^2 < 1 \}$ 是无限集。

集合最简单的性质是 **外延性（Extensionality）**：如果两个集合互相不包含对方没有的元素，那么这两个集合相等，用符号 $=$ 表示。换句话说，对于任何集合 $A$ 中的元素 $a$，如果另一个集合 $B$ 总满足 $a \in B$，那么 $A$ 和 $B$ 相等。记作 $A = B$。

> 记号：逻辑学中，有两个常用的量词 $\forall$ 和 $\exists$ 分别用来声明某个元素的任意性和存在性。比如，$\forall x \in A. x = 0$ 描述了“对于任意 $A$ 中的元素 $x$，其都满足 $x = 0$ ”这样的命题。此外，我使用逻辑连接符 $\lnot$、$\land$、 $\lor$、$\to$ 来表示自然语言中的“不是”、“且” 、 “或者” 和“如果……那么”。为了表述的简洁性，我在笔记中会习惯性利用这些量词描述定理和命题。在后面的章节中，我会给出它们更加正式的定义。
>
> 否定连接符 $\lnot$ 配合其它符号使用时，常常简写为后者和 $/$ 符号并用的形式。比如$\lnot a \in A$ 会写为 $a \notin A$ 等。

两个集合的相等可以通过下面的描述定义：
$$
\begin{equation}
	A = B \quad \Leftrightarrow \quad (\forall x \in A. x \in B) \land (\forall x \in B. x \in A)
\end{equation}
$$

### 子集

除了相等关系，两个集合还可以有 **包含（Inclusion）** 关系，用符号 $\subseteq$ 表示：
$$
\begin{equation}
	A \subseteq B \is \forall x \in A. x \in B
\end{equation}
$$
罕见地也有 $B \supseteq A$ 的写法。可以看到这个定义是 $A = B$ 定义的一部分。因此有下面这个真命题：
$$
\begin{equation}
	A = B \is A \subseteq B \land B \subseteq A
\end{equation}
$$
如果两个集合具有包含关系，比如 $A \subseteq B$，此时就称 $A$ 是 $B$ 的一个 **子集（Subset）**。从定义可以得到，任何集合都是它本身的子集；如果某个集合的子集不是其自身，则称这个子集为 **真子集（Proper Subset）**，这两个集合的关系称为 **真包含（Proper Inclusion）** 关系 ，用符号 $\subset$ 表示。特别地，$\emptyset$ 是任意集合的子集。

> 举例：数集中，不难得到 $\mathbb{N} \subseteq \mathbb{Z} \subseteq \mathbb{Q} \subseteq \mathbb{R} \subseteq \mathbb{C}$。事实上，它们前后之间都是真包含关系。

> 习惯：许多时候真包含关系并不重要，因此虽然两个集合之间有（明显的）真包含关系，我依然会使用符号 $\subseteq$。

> 讨论：关于空集是否是任意集合的子集，这里值得详细说明一下。按照定义我们需要有：
> $$
> \forall x \in \emptyset. x \in A
> $$
> 然而空集本身并没有任何元素，也就是说这个语句的前提都不成立，这种情况应该怎么处理呢？事实上，此前我们介绍的 $\forall x \in A$ 这样的式子，其更加接近真相的记法是 $\forall x. x \in A$：即对任意的 $x$，以满足 $x \in A$ 这一性质的元素为前提。因此比如 $\forall x \in A. x > 0$ 实际上应该理解为 $\forall x. (x \in A \to x > 0)$。是故，$A \subseteq B$ 的定义可以写成 $\forall x. (x \in A \to x \in B)$。显然，此时当 $A = \emptyset$ 时，由于不可能存在任何 $x \in A$ 的对象 $x$，所以这个语句总是真的。我们会在介绍逻辑的章节中详细讲解这一情形。

一个集合 $A$ 的所有子集构成的集合称为它的 **幂集（Power Set）**，记作 $2^A$。这个记号看起来或许有些怪异，但我们之后会理解它的深意。幂集的正式定义是：
$$
\begin{equation}
	2^A = \{ B \mid B \subseteq A \}
\end{equation}
$$

> 举例：$\{a, b\}$ 的幂集是 $\{ \emptyset, \{a\}, \{b\}, \{a, b\}\}$。正如上个小节所说，集合中也可以包含集合。



### 集合的运算

集合上定义了一系列运算：

- **并（Union）**：用符号 $\cup$ 表示。$A \cup B = \{x \mid x \in A \land x \in B \}$。
- **交（Intersection）**：用符号 $\cap$ 表示。$A \cap B = \{ x \mid x \in A \lor x \in B \}$。
- **差（Difference）**：用符号 $\backslash$ 表示。$A \backslash B = \{x \mid x \in A \land x \notin B \}$。

>  记号：对于所有元素都是集合的集合，我们会采用下面的形式来表示其所有元素的并：
> $$
> \begin{equation}
> 	\bigcup \mathcal{A} = \{x \mid \exists A_i \in \mathcal{A}, x \in A_i \}
> \end{equation}
> $$
> 类似地，所有元素的交可以写成：
> $$
> \begin{equation}
> 	\bigcap \mathcal{A} = \{ x \mid \forall A_i \in \mathcal{A}. x \in A_i \}
> \end{equation}
> $$

> 习惯：对于其元素拥有”特殊性质“的集合，我会将其记成书写体。上面这样的集合的集合就是一个例子。

集合运算有一些不难证明的性质：

- 并和交都满足 **交换律（Commutativity）** 和 **结合律（Associativity）**：
  - $A \cup B = B \cup A$ 且 $A \cap B = B \cap A$。
  - $(A \cup B) \cup C = A \cup (B \cup C)$ 且 $(A \cap B) \cap C = A \cap (B \cap C)$。
- 任何集合和空集的并都是它自身；和空集的交都是空集。
- 并和交互相满足 **分配律（Distributivity）**：
  - $A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$。
  - $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$。
- 差、并与交：
  - $(A\backslash B)\cup (B\backslash A) = (A \cup B)\backslash(A\cap B)$。

对于某个元素 $A$ 和一个的 **全集（Universal Set）** $U$，$A$ 在 $U$ 中的 **补（Complement）** 用符号 $\complement$ 表示。$A^\complement = \{ x \in U \mid x \notin A \}$。

> 习惯：在讨论集合的补时，其全集是所有目标元素组成的集合。比如讨论实数集的子集时（比如 $A = \{x \mid x^2 < 2 \}$），$A^\complement$ 应该被理解为 $\mathbb{R}\backslash A$。

集合的补也有一些不难证明的性质：

- 补集的补是集合自身，即 $(A^\complement)^\complement = A$。
- **De Morgan 定律**：
  - $(A \cup B)^\complement = A^\complement \cap B^\complement$。
  - $(A \cap B)^\complement = A^\complement \cup B^\complement$。



### 笛卡尔积

数学中一些结构要求以特定顺序列出一些元素；一个最简单的这样的结构是 **有序对（Ordered Pair）**，用 $(\cdot, \cdot)$ 符号表示。

> 记号：在定义一些符号的时候，我会用 $\cdot$ 当作占位符；在实际使用时，在对应的位置上填入对象即可。

> 举例：$(1, 2)$ 是一个有序对；它和 $(2, 1)$ 是不同的。

两个有序对 $(a, b)$ 和 $(c, d)$ 的相等关系定义如下：
$$
\begin{equation}
	(a, b) = (c, d) \is a = c \land b = d
\end{equation}
$$
定义两个集合的 **笛卡尔积（Cartesian Product）** 为从两者各取元素组成的有序对集：
$$
\begin{equation}
	A \times B = \{(a, b) \mid a \in A \land b \in B \}
\end{equation}
$$
从有序对，我们可以定义多元的有序组，比如三元组可以写成 $((a, b), c)$，它是 $(A \times B)\times C$ 的元素。

> 记号：习惯上会将 $((a, b), c)$ 简写为 $(a, b, c)$。

一个集合对自身的笛卡尔积 $A\times A$ 会被记为 $A^2$。我们可以用递归的方式定义一个集合的 **笛卡尔幂（Cartesian Power）**：
$$
\begin{equation}
	A^1 = A \qquad \dots \qquad
	A^{k+1} = A^k \times A
\end{equation}
$$
显然笛卡尔积不满足交换律；事实上它也不满足结合律，因为 $((a, b), c)$ 和 $(a, (b, c))$ 被视为不同的元素。

> 讨论：从语法形式来看，$((a, b), c) \ne (a, (b, c))$ 展示的似乎是运算符 $,$ （逗号）不具有结合性；这是用圆括号作为有序数对符号带来的缺陷。一些教材中采用尖角括号如 $\langle a, b \rangle$ 可以比较好地避免这个问题。不过由于圆括号有非常广泛的使用度，我依然采用这个记号。
>
> 同时，将 $(a, b, c)$ 作为 $((a, b), c)$ 而非 $(a, (b, c))$ 的简写看起来只是定义上的偏颇。但事实上这和笛卡尔幂的定义也是相互照应的（注意到 $A^{k+1} = A^k \times A$ 中将前面 $k$ 个元素和最后一个分割开来；由于二元运算符一般都是左结合的，这样定义让诸如 $A\times B \times C$ 的元素 $(a, b, c)$ 的具体含义更加符合直观。



### 应用

利用朴素集合论，我们可以建立起一个比较可观的数学体系了。

#### 有序对

我们可以用集合的方式定义有序对，此时 $(\cdot, \cdot)$ 就是一个简写了：
$$
\begin{equation}
	(a, b) = \{ \{a\}, \{a, b\} \}
\end{equation}
$$
 可以看到，通过创建一个不对称的元素集，我们反映了有序对中”有序“的本质。

> 讨论：再一次地，定义 $(a, b)$ 为 $\{\{a\}, \{a, b \}\}$ 而非 $\{\{b\}, \{a, b\}\}$ 只是一个选择而已。

#### 字符串

给定一个有限集 $\Sigma$，称为 **字母表（Alphabet）**；在 $\Sigma$ 上定义的 **拼接（Concatenation）** 操作是对笛卡尔积的一个简写：
$$
\forall a_i \in \Sigma, a_1\dots a_n \equiv (a_1, \dots, a_n)
$$
显然 $a_1\dots a_n \in \Sigma^n$。特别地，定义 $\Sigma^0$ 为只包含元素 $\epsilon$ 的一个特殊集合。定义 $\Sigma$ 的 **Kleene 闭包** 为 $\Sigma$ 所有非负有限次幂的交，即：
$$
\begin{equation}
	\Sigma^* = \bigcup_{i=0} \Sigma^i
\end{equation}
$$
定义 **字符串（String）** 为 $\Sigma^*$ 的元素。特别地，称 $\epsilon$ 为 **空字符串（Empty String）**。

> 记号：通常情况下我们谈论的字符串都是有限的（尤其是在计算理论中，因为计算机无法处理无限的信息），但在一些语境中，也会出现“无限字符串”的应用。其所在的集合记为 $\Sigma^\omega$。

> 讨论：如果将字符串理解成笛卡尔积中元素的简写，那么在遇到字符串集之间的笛卡尔积时或许会令人迷惑。比如 $\Sigma^2\times\Sigma^3$ 理论上应该定义成：
> $$
> \Sigma^2\times\Sigma^3 = \{(w_1w_2, x_1x_2x_3) \mid w_i, x_j \in \Sigma \}
> $$
> 然而它们的元素却是 $y_1y_2y_3y_4y_5 \in \Sigma^5$。如果写成有序元组的形式会是 $((((y_1, y_2), y_3), y_4), y_5)$，和上面的 $(w_1w_2, x_1x_2x_3)$ 形式完全不同。说到底，这就是因为逗号运算符不满足结合律。

#### 关系

集合 $A$ 上的 **n 元关系（n-ary Relation）** 定义为子集 $\mathcal{R} \subseteq A^n$，可以等效写成：
$$
\begin{equation}
	\mathcal{R} = \{x \in A^n \mid \mathscr{r}_x \}
\end{equation}
$$
其中 $\mathscr{r}_x$ 是一个定义 n 元关系的和 $x$ 相关的表达式。

> 举例：$\mathbb{R}$ 上的小于等于关系 $\le$ 定义为 $\mathcal{R}_\le = \{ (x, y) \in \mathbb{R}^2 \mid \text{sgn}(x - y) = -1 \}$，其中包括了 $(1, 1)$、$(\pi, 2e)$ 等元素。

多数情况下，我们研究的都是二元关系。下面让我们列出二元关系的一些性质：

- **自反性（Reflexivity）**：对于任意 $x \in A$ 都有 $(x, x) \in \mathcal{R} \subseteq A^2$。
- **对称性（Symmetry）**：对于任意 $x, y \in A$，若 $(x, y) \in \mathcal{R} \subseteq A^2$，则 $(y, x) \in \mathcal{R}$。
- **传递性（Transitivity）**：对于任意 $x, y, z \in A$，若 $(x, y) \in \mathcal{R} \subseteq A^2$ 且 $(y, z) \in \mathcal{R}$，则 $(x, z) \in \mathcal{R}$。
- **反对称性（Anti-Symmetry）**：对于任意 $x, y \in A$，若 $(x, y) \in \mathcal{R} \subseteq A^2$ 且 $(y, x) \in \mathcal{R}$，则 $x = y$。
- **连通性（Connectivity）**：对于任意 $x, y \in A$，若 $x \ne y$，则要么 $(x, y) \in \mathcal{R} \subseteq A$，要么 $(y, x) \in \mathcal{R}$。
- **不自反性（Irreflexivity）**：对于任意 $x \in A$，$(x, x) \notin \mathcal{R} \subseteq A^2$。
- **不对称性（Asymmetry）**：对于任意 $x, y \in A$，不可能同时 $(x, y) \in \mathcal{R} \subseteq A^2$ 且 $(y, x) \in \mathcal{R}$。

同时满足自反性、对称性和传递性的关系称为 **等价关系（Equivalence Relation）**。如果将 $A$ 中的元素根据其等价关系 $\mathcal{R}$ 分组，可以得到它的一个 **分划（Partition）**，其中每个元素都是 $A$ 的一个子集，称为 **等价类（Equivalence Class）**，记为 $[\cdot]_\mathcal{R}$ 的形式：
$$
\begin{equation}
	[x]_\mathcal{R} = \{ y \in A \mid (x, y) \in \mathcal{R} \}
\end{equation}
$$
从中可以发现，这里的 $x$ 换成其等价类中的任一个元素都不改变这个等价类的定义。称 $x$ 是 $[x]_\mathcal{R}$ 的一个 **代表（Representative）**。等价关系 $\mathcal{R}$ 的所有等价类组成的集合（即 $A$ 的分划）记为 $A/\mathcal{R}$：
$$
\begin{equation}
	A/\mathcal{R} = \{ [x]_R \mid x \in A \}
\end{equation}
$$
上面这个定义就利用到了集合的延拓性（虽然会重复列举等价类，但最终构成的集合依然是我们想要的）。

> 举例：$\mathbb{Z}$ 可以通过 $\{(x, y) \in \mathbb{Z}^2 \mid x - y = 2k, k \in \mathbb{Z}\}$ 分成下面这个分划：
> $$
> \mathbb{Z}/\mathcal{R} = \{[0], [1]\}
> $$
> 通过 **同一关系（Identity Relation）** $\mathcal{R}_= = \{(x, x) \mid x \in A\}$ 可以得到下面的分划：
> $$
> \mathbb{Z}/\mathcal{R}_= = \{ \{0\}, \{+1\}, \{-1\}, \dots \}
> $$

同时满足自反性和传递性的关系称为 **预序关系（Preorder Relation）**；满足反对称性的预序关系称为 **偏序（Partial Order）** 关系；满足连通性的偏序关系称为 **全序（Total Order）** 关系。同时满足不自反性、不对称性和传递性的关系称为 **严格序（Strict Order）** 关系；满足联通关系的严格序关系就是全序关系。

> 举例：利用字符串长度函数 $\text{len}$ 定义的关系：$\mathcal{R}_\preceq = \{ (w, x) \mid w, x \in \Sigma^*, \text{len}(w) \le \text{len}(x) \}$ 是一个预序关系。包含关系是一个偏序关系。实数集上的 $\le$ 关系是一个全序关系，$<$ 是一个严格序关系。

> 记号：当没有指定特定的偏序集时，我们用 $\le$ 符号来表示偏序关系；类似地，用 $<$ 表示严格序关系。

如果某个集合上定义了特定的x序关系，我们就称它是一个相应的x序集。比如 $\mathbb{R}$ 上可以定义全序关系 $\mathcal{R}_\le$，因此 $\mathbb{R}$ 是一个全序集。偏序集的子集一定也是偏序集。

在偏序集 $A$ 中如果存在 $u \in A$ 对任意 $x \in A$ 都满足 $x \le u$，则称 $u$ 是 $A$ 的一个 **上界（Upper Bound）**。如果存在某个上界 $u$ 满足对任何上界 $v$ 都有 $u \le v$，就称其为一个 **上确界（Least Upper Bound）**，用 $\sup{A}$ 来表示。类似地，我们也可以定义 **下界（Lower Bound）** 和 **下确界（Greatest Lower Bound）** $\inf{A}$。不难证明上确界和下确界总是唯一的。

#### 函数

**函数（Function）** 是一个从某个集合 $A$ 到另一个集合 $B$ 的映射，记作 $f: A \to B$。其中 $A$ 称为 $f$ 的 **定义域（Domain）**，$B$ 是 $f$ 的 **陪域（Codomain）**。我们可以将函数构造为 $A\times B$ 上的一个二元关系（注意这里 $A, B \subseteq S$ 是我们选定的子集，而 $A\times B$ 上的二元关系是指满足 $a \in A$，$b \in B$ 的有序对 $(a, b)$ 组成集合的子集）满足：
$$
\begin{equation}
	\forall a \in A, \exists! b \in B. (a, b) \in \mathcal{R}_f
\end{equation}
$$
这个定义说的是，定义域 $A$ 中任意的元素都能找到唯一一个对应的元素 $b \in B$。习惯上我们将其记为 $f(a)$。

> 记号：
>
> - 函数 $f$ 的定义域记为 $\operatorname{dom}{f}$。
> - 函数有时会定义成 $x \mapsto y$ 的形式，表示 $x$ 映射到 $y$。比如 $x \mapsto x + 1$ 表示函数 $\{(0, 1), (1, 2), \dots\} \subseteq \mathbb{N}^2$。
> - 对于子集 $S \subseteq A$，记经过 $f$ 映射到 $B$ 的集合为 $f(S) = \{b \in B \mid \exists x \in S. f(x) = b\}$。这实际上定义了 $f: \mathbb{P}(A) \to \mathbb{P}(B)$。

函数 $f$ 的 **像（Image）** 是其定义域映射到陪域的集合，即：
$$
\begin{equation}
	\operatorname{Im} f = f(A)
\end{equation}
$$
如果函数的像等于其陪域，那么称这是一个 **满射（Surjection）**。如果对任意 $y \in \operatorname{Im} f$ 都存在且仅存在一个 $x \in A$ 满足 $f(x) = y$，那么称这是一个 **单射（Injection）**。同时为满射且单射的函数称为 **双射（Bijection）**。

对于一个单射，我们可以为 $\operatorname{Im} f$ 中任意的元素 $b$ 找到唯一的一个 $a \in A$。因此我们可以定义函数 $f^{-1}: \operatorname{Im}f \to A$。特别地，如果 $f$ 同时是一个满射，那么称 $f^{-1}$ 为 $f$ 的 **逆（Inverse）**。对于任意 $a \in A$ 都有 $f^{-1}(f(a)) = a$。

对不是单射的函数，我们也可以定义一个 $f^{-1}: \mathbb{P}(B) \to \mathbb{P}(A)$，对任意 $S \subseteq B$：
$$
\begin{equation}
	f^{-1}(S) = \{a \mid f(a) \in S, a \in A \}
\end{equation}
$$
在这个定义下，称 $f^{-1}(B)$ 为 $f$ 的 **逆像（Inverse Image）**。

下面给出一些特殊的函数：

- 定义 **单位函数（Identity Function）** 为 $\text{id}_A: A \to A$，由 $\mathcal{R}_{\text{id}_A} = \{(a, a) \mid a \in A\}$ 决定。
- 定义 **投影函数（Projection Function）** 为 $p_A: A\times B \to A$ 和 $p_B: A\times B \to B$，分别由 $\mathcal{R}_{p_A} = \{((a, b), a) \mid a \in A, b \in B\}$ 和 $\mathcal{R}_{p_B} = \{((a, b), b) \mid a \in A, b \in B\}$ 决定。

对于两个函数 $f: A \to B$、$g: B \to C$，两者的 **复合（Composition）** 用符号 $\circ$ 表示：
$$
\begin{equation}
	g \circ f : a \mapsto g(f(a))
\end{equation}
$$
如果用二元关系的语言来描述，函数的复合定义了新的二元关系 $\mathcal{R}_{g\circ f}$ 满足：
$$
\begin{equation}
	\mathcal{R}_{g\circ f} = \{(a, c) \mid \exists b \in B. (a, b) \in \mathcal{R}_f \land (b, c) \in \mathcal{R}_g \}
\end{equation}
$$
复合操作满足结合性，也即 $(f \circ g)\circ h = f\circ (g\circ h)$，因为实际的效果都是 $x \mapsto f(g(h(x)))$。

定义函数 $f: A \to B$ 在 $S \subseteq A$ 中的 **限制（Restriction）** 为：
$$
\begin{equation}
	f|_C: C \to B, \quad (f|_C)(x) = f(x)
\end{equation}
$$

### 集合的大小

本节让我们介绍集合的一些重要性质，在有了函数的定义之后谈论这些会方便许多。首先，集合的 **大小（Size）** 是一个非常显然的性质，比如 $\{a, b, c\}$ 的大小显然是 $3$，而 $\mathbb{N}$ 的大小是无穷大。不过，同样大小为无穷大的集合，如 $\mathbb{Z}$ 和 $\mathbb{R}$ 之间，是否存在大小的相对关系呢？

> 记号：集合 $A$ 的大小记为 $|A|$。

> 讨论：涉及到无穷的话题很容易就出现奇异的结论。比如集合 $\mathbb{N}$ 和 $2\mathbb{N} = \{2n \mid n \in \mathbb{N}\}$，虽然看起来 $\mathbb{N}$ 的元素个数更多（因为它更密），但我们总可以让任意 $n \in \mathbb{N}$ 对应 $2n \in 2\mathbb{N}$；从这个映射的角度来看两者的元素个数是相同的。

非空集合 $A$ 的 **枚举（Enumeration）** 定义为某个满射 $f : \mathbb{N} \to A$；如果 $A$ 有一个枚举，则称 $A$ 是 **可数的（Countable）**。

> 举例：显然此前我们提到的 $\mathbb{N}$ 和 $2\mathbb{N}$ 都是可数的。对于稍微复杂的集合，比如 $\mathbb{N}^2$，我们可以通过画表法找到一个枚举：
>
> |         | 0       | 1       | 2       | 3       | $\dots$ |
> | ------- | ------- | ------- | ------- | ------- | ------- |
> | 0       | (0, 0)  | (0, 1)  | (0, 2)  | (0, 3)  | $\dots$ |
> | 1       | (1, 0)  | (1, 1)  | (1, 2)  | (1, 3)  | $\dots$ |
> | 2       | (2, 0)  | (2, 1)  | (2, 2)  | (2, 3)  | $\dots$ |
> | 3       | (3, 0)  | (3, 1)  | (3, 2)  | (3, 3)  | $\dots$ |
> | $\dots$ | $\dots$ | $\dots$ | $\dots$ | $\dots$ | $\dots$ |
>
> 如上表所示，从左上角出发，穿针引线地依次经过 $(0, 0)$、$(0, 1)$、$(1, 0)$、$(2, 0)$、$(1, 1)$、$(0, 2)$、$\dots$。显然我们可以按照这样地顺序建立一个枚举。
>
> 从这个方法出发，可数集合有限的笛卡尔幂总是可数的。一个特殊的例子是 $\mathbb{Q}$，意识到它的定义是：
> $$
> \mathbb{Q} = \{\frac{p}{q} \mid p, q \in \mathbb{Z}, q \ne 0 \}
> $$
> 因此它类似于 $\mathbb{Z}^2$，也是可数的。

与之相反地，其它的集合称为 **不可数的（Uncountable）**。一个典型的例子是 $\mathbb{B}^\omega$，其中 $\mathbb{B} = \{0, 1\}$。下面是一个经典的证明：

> 证明：假设 $\mathbb{B}^\omega$ 是可数的，则可以找到 $s_1, s_2, s_3, \dots \in \mathbb{B}^\omega$。定义 $c: \mathbb{B}^\omega\times\mathbb{N}\to \mathbb{B}$ 为查询某个字符串 $s$ 特定位置的函数（比如 $c(1011\dots, 1) = 1$）。因此，$s_i$ 也可以表示成：
> $$
> s_i = c(s_i, 1)c(s_i, 2)\dots
> $$
> 此时让我们将所有字符串按照顺序和字母位置列成表：
>
> |         | 1           | 2           | 3           | $\dots$ |
> | ------- | ----------- | ----------- | ----------- | ------- |
> | 1       | $c(s_1, 1)$ | $c(s_1, 2)$ | $c(s_1, 3)$ | $\dots$ |
> | 2       | $c(s_2, 1)$ | $c(s_2, 2)$ | $c(s_2, 3)$ | $\dots$ |
> | 3       | $c(s_3, 1)$ | $c(s_3, 2)$ | $c(s_3, 3)$ | $\dots$ |
> | $\dots$ | $\dots$     | $\dots$     | $\dots$     | $\dots$ |
>
> 定义 $\bar{c}: \mathbb{B}^\omega \times \mathbb{N} \to \mathbb{B}$ 为将某个字符串 $s$ 特定位置翻转的函数（比如 $c(1011\dots, 1) = 0$）。现在考虑满足 $c(s_x, n) = \bar{c}(s_n, n)$ 的字符串 $s_x$：由于它总是在第 $i$ 位和表中第 $i$ 个字符串不同，因此这个字符串不可能在表中。然而显然 $s_x  \in \mathbb{B}^\omega$。这就产生了矛盾。

> 举例：$\mathbb{R}$ 可以理解为 $(\mathbb{D}^\omega)^2$，其中 $\mathbb{D} = \{0, 1, \dots, 8, 9\}$，因为以小数点为界，两边分别都是一个无穷十进制数字符串（对于空位可以全部填零，如 $10.2 = \dots 0010.200\dots$）。因而 $\mathbb{R}$ 是不可数的。
>
> 一个稍复杂的例子是 $\mathbb{P}(\mathbb{N})$。对于任意 $N \in \mathbb{N}$，我们可以通过下面这种方式描述它：定义 $\varphi_N: \mathbb{P}(\mathbb{N}) \to \mathbb{B}$，若 $n \in N$ 则 $\varphi_N(n) = 1$，否则 $\varphi_N(n) = 0$。此时我们可以建立一个 $\mathbb{P}(\mathbb{N})$ 到 $\mathbb{B}^\omega$ 的一个双射：
> $$
> N \mapsto \{ b_1b_2b_3\dots \in \mathbb{B}^\omega \mid b_i = \varphi_N(i) \}
> $$
> 由于 $\mathbb{B}^\omega$ 是不可数的，显然 $\mathbb{P}(\mathbb{N})$ 也不可数。

从前面的内容可以看到，无限集之间也存在“大小”的差异。两个集合定义为 **等势的（Equinumerous）**，若能在它们之间建立一个双射。显然，有限集之间等势当且仅当两者大小相同；可数的无限集之间是等势的。

> 记号：若 $A$ 和 $B$ 等势，则记 $A \approx B$。

**等势（Equinumerosity）** 关系是一个等价关系，这是显而易见的。

既然能定义和集合势相关的等价关系，那么自然也有序关系：如果存在单射 $f: A \to B$，则称 $A$ 不大于 $B$，记为 $A \preceq B$；如果 $A$ 不大于 $B$ 且两者不等势，则称 $A$ 小于 $B$，记为 $A \prec B$。

**康托定理（Cantor Theorem）**：任意集合 $A$ 都满足 $A \prec \mathbb{P}(A)$。下面是证明。

> 证明：$f: a \mapsto \{a\}$ 显然是一个单射，因此 $A \preceq \mathbb{P}(A)$，所以我们只需证明 $A \not\approx \mathbb{P}(A)$ 即可。即：不存在满射 $f: A \to \mathbb{P}(A)$。定义 $\bar{A} = \{ x \in A \mid x \notin f(x) \}$。这个集合显然是一个 $A$ 的子集；无论它是什么样的集合，它都不可能出现在 $\operatorname{Im}{f}$ 中，即不存在 $x \in A$ 令 $f(x) = \bar{A}$。对任意 $x \in A$：
>
> - 假设 $x \in f(x)$，显然此时 $x \notin \bar{A}$。若令 $f(x) = \bar{A}$，则有 $x \notin f(x)$，矛盾。
> - 假设 $x \notin f(x)$，显然此时 $x \in \bar{A}$。若令 $f(x) = \bar{A}$，则有 $x \in f(x)$，矛盾。
>
> 因此不存在这样的单射 $f$，而 $A \prec \mathbb{P}(A)$。



## 一阶逻辑

### 结构

#### $\sigma$-结构

数学中的对象通常可以归纳到特定的结构中，我们可以借用朴素集合论的语言来描述它们。

> 举例：
>
> - **图（Graph）** 是一个二元组 $\mathbf{G} = (V, E)$，其中 $V$ 是结点集，$E \subseteq V^2$ 是 $V$ 上的一个二元关系。
> - **群（Group）** 是一个四元组 $\mathbf{G} = (G, e, \circ, \cdot^{-1})$，其中 $G$ 是一个集合，$e \in G$ 是单位元，$\circ: G^2 \to G$ 是一个二元函数，$\cdot^{-1}: G \to G$ 是逆元函数。此外，这些元素需要满足：
>   - 运算符 $\circ$ 满足结合律。
>   - 对任意 $x \in G$，$e\circ x = x \circ e = x$。
>   - 对任意 $x \in G$，$x\circ x^{-1} = x^{-1}\circ x = e$。
> - 偏序集是一个二元组 $\mathbf{P} = (P, \le)$，其中 $P$ 是一个集合，$\mathcal{R}_\le \subseteq P^2$ 是 $P$ 上的一个二元关系。此外，二元关系还要满足：
>   - 自反性。
>   - 反对称性。
>   - 传递性。

类似的结构还有很多。将它们抽象出来，我们可以将 **结构（Structure）** 定义为四元组 $\mathbf{S} = (S, \mathcal{C}, \mathcal{F}, \mathcal{R})$，其中：

- $S$ 是一个集合。
- $\mathcal{C} \subseteq S$ 是常数集。
- $\mathcal{F} = \{f : S^n \to S \mid n \in \mathbb{N} \}$ 是函数集。
- $\mathcal{R} = \{ R \subseteq S^n \mid n \in \mathbb{N} \}$ 是关系集。

其中，最能体现出结构特性的是 $S$ 以外的这三个元素（比如实数加法群和整数加法群除了集合不同，其它完全相同），因此也就引出了 **签名（Signature）** 的定义：
$$
\begin{equation}
	\sigma = (\mathcal{C}, \mathcal{F}, \mathcal{R}, \mathscr{a})
\end{equation}
$$
旧的符号 $\mathcal{C}$、$\mathcal{F}$ 和 $\mathcal{R}$ 现在也有了新的含义：它们包含的都是符号的名字，而非常数、函数和关系本身。同时新加入的元素 $\mathscr{a}: \mathcal{F}\cup\mathcal{R}\to\mathbb{N}$ 称为 **元数函数（Arity Function）**。因此，对于任意符号 $s \in \mathcal{F} \cup \mathcal{R}$，我们可以通过得到 $n$。同时，为了方便描述签名 $\sigma = (\mathcal{C}, \mathcal{F}, \mathcal{R}, \mathscr{a})$ 的元素，定义 $\operatorname{Const}(\sigma) \equiv \mathcal{C}$，$\text{Func}(\sigma) \equiv \mathcal{F}$，$\text{Rel}(\sigma) = \mathcal{R}$。

> 举例：
>
> - 图的签名是 $\sigma_\text{graph} = (\emptyset, \emptyset, \{E\}, (E \mapsto 2))$，其中 $E$ 表示图中的边。
> - 幺半群的签名是 $\sigma_\text{monoid} = (\{e\}, \{\circ, \cdot^{-1}\}, \emptyset, (\circ \mapsto 2, \cdot^{-1}\mapsto 1))$，其中 $e$ 是幺半群的单位元。
> - 集合的签名是 $\sigma_\text{set} = (\emptyset, \emptyset, \{\in\}, (\in \mapsto 2))$。

> 习惯：显然上面这种表示方式非常啰嗦，因此我会省略所有的空集，省略元数函数，并将所有集合都展开。比如上面集合的签名可以简写成 $\sigma_\text{set} = \{\in\}$。

定义 **$\sigma$-结构** 为一个二元组 $\mathbf{S} = (S, \iota)$，其中 $S$ 称为 **全集（Universe）**，而 $\iota$ 是一个定义在 $\text{Const}(\sigma)$、$\text{Func}(\sigma)$ 和 $\text{Rel}(\sigma)$ 上的函数，用来得到符号所指向的实际对象：

- $\iota: \mathcal{C} \to S$，从常数集中得到一个常数。
- $\iota: \mathcal{F} \to (S^{\mathscr{a}(f \in \mathcal{F})} \to S)$：从函数集中得到一个函数。
- $\iota: \mathcal{R} \to R \subseteq S^{\mathscr{a}(R \in \mathcal{R})}$：从关系集中得到一个关系。

> 记号：为了简化，我们用 $x^\mathbf{S}$ 来代替 $\iota(x)$，其中 $x$ 是某个常数、函数或关系。

现在，一个 $\sigma$-结构可以写成如下的形式了：
$$
\begin{equation}
	\mathbf{S} = (S, \{c^\mathbf{S}\}_{c\in\mathcal{C}}, \{R^\mathbf{S}\}_{R \in \mathcal{R}}, \{f^\mathbf{S}\}_{f \in \mathcal{F}})
\end{equation}
$$

> 举例：整数加法群的 $\sigma_\text{group}$-结构是 $\mathbf{Z} = (\mathbb{Z}, 0^\mathbf{Z}, +^\mathbf{Z}, (-\cdot)^\mathbf{Z})$，其中 $0^\mathbf{Z}$ 就是 $0 \in \mathbb{Z}$，$+: (a, b) \mapsto a + b$，$-: a \mapsto -a$。

> 讨论：对于特定的 $\sigma$-结构，其成员定义不一定遵循习惯中的行为。比如我们可以参考群的签名 $\sigma_\text{group}$ 来定义群 $\mathbf{Z}$，但其中的 $+$ 定义为 $(a, b) \mapsto \sin(a+b)$，此时它虽然依然拥有群的签名，但其结构性质与群相差甚远。

#### 子结构

对于两个 $\sigma$-结构 $\mathbf{A}$ 和 $\mathbf{B}$，如果两者满足下面的性质：

- 对任意 $c \in \text{Const}(\sigma)$ 都有 $c^\mathbf{A} = c^\mathbf{B}$。
- 对任意 $f \in \text{Func}(\sigma)$ 都有 $f^\mathbf{A} = f^\mathbf{B}|_{A^{\mathscr{a}(f)}}$。
- 对任意 $R \in \text{Rel}(\sigma)$ 都有 $R^\mathbf{A} = R^\mathbf{B} \cap A^{\mathscr{a}(R)}$。

此时称 $\mathbf{A}$ 是 $\mathbf{B}$ 的一个 **子结构（Substructure）**，记为 $\mathbf{A} \subseteq \mathbf{B}$。

> 举例：拥有环的签名 $\sigma_\text{ring}$ 的结构 $\mathbf{A} = (\{x \in \mathbb{Z}, x^2 \le 100\}, 0, 1, +, -, \cdot)$ 是 $\mathbf{B} = (\mathbb{R}, 0, 1, +, -, \cdot)$ 的子结构。注意 $\mathbf{B}$ 是一个环但 $\mathbf{A}$ 并不是。

