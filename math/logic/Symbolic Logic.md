# Symbolic Logic

本篇是 *UIUC PHIL 202 Symbolic Logic* 和 *PHIL 454 Advanced Symbolic Logic* 的学习笔记。



[TOC]



本篇笔记中我们研究的重点是 **一阶逻辑（First Order Logic, FOL）**，其是一个语言，即一系列特定字符组成的字符串集（详细可见自动机理论），包含的典型表达式如下：
$$
\forall x. F(x) \and G(x)
$$
其中的 $\forall$ 是一个 **量词（Quantifier）**，$x$ 是一个 **变量（Variable）**， $F$ 与 $G$ 都是个 **谓词（Predicate）**，而 $\and$ 是一个 **连接符（Connective）**。它们分别类似于变量所在的集合（作用域），变量，函数以及运算符。



## 命题逻辑

### 原子句

在 FOL 中，最简单的句子是通过一个谓词作用于一系列对象组成的，我们将这样的形式称为 **原子句（Atomic Sentence）**。举例来说，如果记 $C$ 为判断某个对象是否为正方体的谓词，那么 $C(\text{x})$ 就是一个原子句，而 $C(\text{x})\and C(\text{y})$ 就不是。谓词和函数一样，可以接多个参数，比如记 $\text{Cmp}$ 为比较两个对象的大小，则 $\text{Cmp}(\text{x}, \text{y})$ 是合法的且是一个原子句。

> 惯用：当谓词的名字长度大于 1 时，我会将其写成文本的格式，比如 $\text{Cmp}$ 而非 $Cmp$。谓词的第一个字母总是大写的。

上面这样通过谓词和变量结合的形式得到的是一个 **命题（Proposition）**，或 **声明（Claim）**。它只可能是对或者错的。这个对或错的性质被称为 **真值（Truth Value）**。当变量确定为某个对象时，我们可以确定某个命题的成立与否。比如上面 $\text{x}$ 是一个球体时， $C(\text{x})$ 求值为假。

FOL 中，我们将名词写成 **项（Term）** 的形式。比如 $\text{alice}$ 可以表示 Alice 这个人，$\text{apple}$ 可以表示苹果这个物体。有时候，我们也会利用 **函数（Function）** 来表示某个项的变形规则，如 $\text{father}$ 可以表示某个人的父亲，那么 $\text{father}(\text{alice})$ 同样也是一个项。

> 惯用：项以及项的函数都用小写字母表示，且一律使用文本格式。我有时会将其称为对象。

> 举例：在集合论中，我们可以构建下面的 FOL。其存在两个谓词，$=$ 和 $\in$。前者判断两个元素是否相同，后者则判断某个元素是否属于某个集合。集合论的项可以是任意的数（这里我们将集合限制为数的集合）。
>
> 在算术中，可以定义两个谓词 $=$ 和 $<$，项 $0$ 与 $1$。同时定义项的函数 $+$ 和 $\times$。

逻辑学中研究的一个重点是，什么时候某个声明可以从逻辑上推导出另一个声明？下面让我们正式地引入这个理论相关的词汇。一个 **论据（Argument）** 中，总是有一个论证的目标，即 **结论（Conclusion）**，和一系列支持这个目标的声明，即 **前提（Premise）**。举例来说，“人皆有一死，苏格拉底是人，因此苏格拉底会死”中，前两个小句是前提，而最后一个小句是结论。自然语言中，有一些提示词暗示我们究竟哪个小句是前提，而哪个是结论。比如”因此“、”所以“等词汇暗示后面是结论，而”因为“、”由于“等词汇暗示后面是前提。

如果某个论据的前提成立时，其结论不可能不成立时，就称这个结论是这些前提的 **逻辑后承（Logical Consequence）**，同时这个论据也是 **逻辑上有效（Logically Valid）** 的。举一个反例，比如”苏格拉底会死，人皆有一死，因此苏格拉底是人“中，如果苏格拉底是某个热带鱼的名字，前两个前提皆真并不能推出结论来。

还要注意的一点是，逻辑上有效和前提的有关与否无关。比如”苏格拉底是人，人是永生的，因此苏格拉底不会死“中，确实在前前提成立时结论也必然成立，因此它是逻辑上有效的，即使”人是永生的“显然有误。特别地，如果一个论据不仅逻辑上有效，且其前提都为真，此时它是一个 **合理的论据（Sound Argument）**。逻辑学中对论据的合理与否不感兴趣，因为一些论据的论证需要逻辑学以外的知识；我们关注的只是论据的有效与否。

### 证明

本篇中我们采用的论证格式是 **Fitch 记号（Fitch Notation）**，举例如下：
$$
\begin{equation*}
	\left|
	\begin{split}
		&\ 1.\ C(\text{x}) \\
		&\underline{\ 2}.\ \text{x} = \text{y} \\
		&\ 3.\ C(\text{y})
	\end{split}
	\right.
\end{equation*}
$$
这里我们将前两个小句作为前提，得到了第三个小句的结论。这里我们用到了没有定义过的记号，$=$。逻辑学中我们认为如果两个名字指代对象的所有性质都相同，此时两者就是相等的，用 $=$ 符号表示，这被称为 **同一对象的不可区分性（Indiscernibility of Identicals）**。对于某个小句中出现的名字 $x$，如果我们同时有 $x = y$，则可以将前面那个小句中的任意多个 $x$ 都替换为 $y$，这被称为 **同一消除（Identity Elimination）**。正式的逻辑证明中，我们需要将这个规则写出：
$$
\begin{equation*}
	\left|
	\begin{split}
		&\ 1.\ C(\text{x}) \\
		&\underline{\ 2}.\ \text{x} = \text{y} \\
		&\ 3.\ C(\text{y})
	\end{split}
	\right. \qquad
	\begin{split}
		\\
		\\
		= \textbf{Elim}: 2, 1
	\end{split}
\end{equation*}
$$
对象间的同一关系是 **自反的（Reflexive）**，即 $\text{a}=\text{a}$。在逻辑证明中，这也被称为 **同一引入（Identity Introduction）**。我们可以由这个特性推出同一关系的 **对称性（Symmetry）**：
$$
\begin{equation*}
	\left|
	\begin{split}
		&\ 1.\ \text{a} = \text{b} \\
		&\underline{\ 2.}\ \text{a} = \text{a} \\
		&\ 3.\ \text{b} = \text{a}
	\end{split}
	\right. \qquad
	\begin{split}
		&\\
		&= \textbf{Intro} \\
		&= \textbf{Elim}: 2, 1
	\end{split}
\end{equation*}
$$
类似地，我们也可以证明同一关系的 **传递性（Transitivity）**。

下面让我们用更加正式的结构定义同一关系的两个定律。

1. 同一引入：
   $$
   \begin{equation*}
   	\begin{split}
   		\triangleright
   	\end{split}
   	\left|
   	\begin{split}
   		\ i.\ \text{a} = \text{a}
   	\end{split}
   	\right. \qquad
   	\begin{split}
   		= \textbf{Intro}
   	\end{split}
   \end{equation*}
   $$

2. 同一消除：
   $$
   \begin{equation*}
   	\begin{split}
   		\\ \\ \\ \\ \\ \triangleright
   	\end{split}
   	\left|
   	\begin{split}
   		&\ i.\ P(\text{a}) \\
   		&\ \ \vdots \\
   		&\ j.\ \text{a} = \text{b} \\
   		&\ \ \vdots \\
   		&\ k.\ P(\text{b})
   	\end{split}
   	\right. \qquad
   	\begin{split}
   		\\ \\ \\ \\ \\ = \textbf{Elim}: j, i
   	\end{split}
   \end{equation*}
   $$

最后，还有一个非常基本的定律，即 **重述（Reiteration）**：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P \\
		&\ \ \vdots \\
		&\ j.\ P
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \textbf{Reit}: i
	\end{split}
\end{equation*}
$$
所有证明都可以分为下面两个类型：证明后承，证明非后承。前者需要说明在所有前提为真时，结论一定为真；后者则需要说明在所有前提为真时，结论不一定为真。某个前提为真而结论不为真的例子被称为 **反例（Counterexample）**。

### 布尔逻辑

现在让我们跳出原子句，利用符号构建更加复杂的句子。

- 符号 $\lnot$ 用来表示某个句子的 **否定（Negation）**。比如如果 $P$ 为假，那么 $\lnot P$ 就是真的。显然对于任何句子 $S$，都有 $S = \lnot\lnot S$。特别地，对于同一关系 $=$，记 $\lnot(\text{a}=\text{b})$ 为 $a\ne b$。

  自然语言中的”不“可以转写成否定连接符。比如记 $R$ 为判断某个物体是否为红色，则 $\lnot R(\text{a})$ 则给出 $\text{a}$ 是否*不是*红色。

- 符号 $\and$ 用来表示两个句子的 **合取（Conjunction）**。当且仅当两个句子 $P$ 和 $Q$ 都为真时，它们的合取 $P\and Q$ 才为真。合取满足交换律和结合律，且其拥有 **幂等性（Idempotence）**，即 $P\and P$ 为真当且仅当 $P$ 为真。

  自然语言中”并且“、”但是“等词语可以转写成合取连接符。

- 符号 $\or$ 用来表示两个句子的 **析取（Disjuction）**。当且仅当两个句子 $P$ 和 $Q$ 都为假时，它们的析取 $P \or Q$ 才为假。析取同样满足交换律、结合律以及幂等性。

  自然语言中”或者“可以转写成析取连接符。注意在自然语言中”或“有时带有两者选一的含义，但在逻辑学中，”或“不包含这种含义，即两者皆取也是可以接受的。

当混用上面几个符号时，运算的顺序会出现歧义，比如 $P\and Q\or R$ 究竟是先判断 $P\and Q$ 还是 $Q\or R$ 呢？为了解决这个问题，我们要求在可能出现歧义的句子中使用括号。这样 $(P \and Q)\or R$ 就是先判断 $P\and Q$，而 $P\and (Q\or R)$ 就是先判断 $Q\or R$。



上面这些连接符也被称为 **真值函数连接符（Truth Functional Connective）**，这是因为它们总能通过一系列真值得到确定的真值结果。

回忆*逻辑后承*是指当前提成立时一定为真的句子。有一些句子在任何给定前提下都能恒成立，比如 $a=a$，我们称其为 **逻辑上必然（Logically Necessary）** 的。如果一个句子在一些情况下可能成立，我们称其为 **逻辑上可能（Logically Possible）** 的。值得一提的是，逻辑上可能的只在逻辑角度考虑，因此可能违反常识（毕竟常识本身也只是一个”可能为真或假“的前提），比如某个物体的运动速度超过光速，这是逻辑上可能的（即使物理上不可能）。

有一些逻辑上必然的句子，其形式上对于任意的输入都是成立的，此时我们称其为 **重言式（Tautology）**。比如 $P\or\lnot P$ 在任何时候都是真的。但大多数逻辑必然的句子都不是重言式，如 $a=a$，它本身是一个句子，因此可以取真或假。尽管从同一对象的不可区分性这个句子一定为真（因此它是逻辑必然），但在更一般的角度来看，$a=a$ 和 $a=b$ 没有区别，两者都是可以取真或假的句子。相比之下，$P\lor\lnot P$ 是一句”冗余“的句子，它等价于真。

对于两个句子，如果它们取真值的情形完全相同，则称两者是 **逻辑等价（Logically Equivalent）** 的，记作 $P\Leftrightarrow Q$。如果两者的真值表完全相同，则称两者是 **重言等价（Tautologically Equivalent）** 的。举例来说，$\text{a}=\text{b}\and P(\text{a})$ 和 $\text{a}=\text{b}\and P(\text{b})$ 是逻辑等价的，然而两者并不是重言等价的（因为 $P(\text{a})$ 和 $P(\text{b})$ 完全可能有不同的真值）。

把等价的要求放宽些，如果一个句子取真值时，另一个句子一定为真值，则后者是前者的 **逻辑后承（Logical Consequence）**，记作 $P\Rightarrow Q$。如果真值表中，一个句子取真值的情况也是另一个句子取真值的情况，则后者是前者的 **重言后承（Tautological Consequence）**。举例来说，$\text{a}=\text{c}$ 是 $\text{a}=\text{b}\and\text{b}=\text{c}$ 的逻辑后承，但不是后者的重言后承（因为逻辑上存在后面一个句子取真，而前面一个句子取假的情况，虽然这是不可能的，但参考此前的 $a=a$，情况是相似的）。另一个例子，$\text{a}\or\text{b}$ 是 $\text{a}\and\text{b}$ 的重言后承。

#### 范式

每个句子总是可以写成：

- $\lnot P$ 的形式，此时其称为 **否定范式（Negation Normal Form）**。
- $P_1\land\dots\land P_n$ 的形式，此时其称为 **合取范式（Conjunctive Normal Form）**。
- $P_1\lor\dots\lor P_n$ 的形式，此时其称为 **析取范式（Disjunctive Normal Form）**。

合取和析取相互满足分配律，即：$P\and(Q\or R) = (P\and Q)\or(P\land R)$ 及 $P\or(Q\land R) = (P\or Q)\land (P\or R)$，因此我们很容易将任意的句子变成合取或析取范式。

#### 逻辑规则

本节让我们介绍否定、合取和析取的逻辑规则。

如果某个假设不成立，那么我们自然可以推出其否定句成立，这是否定的引入规则：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ \left|
		\begin{split}
			&\underline{\ i}.\ P \\
			&\ \vdots \\
			&\ j.\  \bot
		\end{split}
		\right. \\
		&\ k.\ \lnot P
	\end{split}
	\right.\qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \lnot\ \textbf{Intro}: i, j
	\end{split}
\end{equation*}
$$
其中 $\bot$ 表示 **矛盾（Contradiction）**；内嵌的“子证明”则表示假设某个句子成立，称为 **临时假设（Temporary Assumption）**，上面假设的是 $P$ 为真。

其次，我们可以通过双重否定得到某个句子本身的正确性，这是否定的消除规则：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ \lnot\lnot P \\
		&\ \vdots \\
		&\ j.\ P
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \lnot\ \textbf{Elim}: i
	\end{split}
\end{equation*}
$$
如果一个句子和它的否定同时出现，我们就引入了矛盾：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P \\
		&\ \vdots \\
		&\ j.\ \lnot P \\
		&\ \vdots \\
		&\ \bot
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \bot\ \textbf{Intro}: i, j
	\end{split}
\end{equation*}
$$
以矛盾作为前提时，我们可以推出任意的结论：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ \bot \\
		&\ \vdots \\
		&\ j.\ P
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \bot\ \textbf{Elim}: i
	\end{split}
\end{equation*}
$$


某个句子成立当且仅当其合取范式中每个小句都为真，这是合取的引入规则：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P_1 \\
		&\ \vdots \\
		&\ j.\ P_{j-i+1} \\
		&\ \vdots \\
		&\ k.\ P_1 \land \dots \land P_{j-i+1}
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \land\ \textbf{Intro}: i, \dots j
	\end{split}
\end{equation*}
$$
不过，如果某个合取范式为真，则其任一个小句都是真的，这是合取的消除规则：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P_1 \land \dots \land P_m \land \dots \land P_n \\
		&\ \vdots \\
		&\ j.\ P_m
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \land\ \textbf{Elim}: i
	\end{split}
\end{equation*}
$$
当一个句子为真时，任何包含它为小句的析取范式都成立，这是析取的引入规则：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P_m \\
		&\ \vdots \\
		&\ j.\ P_1 \lor \dots \lor P_m \lor \dots \lor P_n
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \lor\ \textbf{Intro}: i
	\end{split}
\end{equation*}
$$
析取的消除规则要求假设每个小句成立时都能推得某个结果：
$$
\begin{equation*}
	\begin{split}
		\\ \\ \\ \\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P_1 \lor \dots \lor P_n \\
		&\ \vdots \\
		&\ \left|
		\begin{split}
			&\underline{\ j_1}.\ P_1 \\
			&\ \vdots \\
			&\ k_1.\ S
		\end{split}
		\right. \\
		&\ \vdots \\
		&\ \left|
		\begin{split}
			&\underline{\ j_n}.\ P_n \\
			&\ \vdots \\
			&\ k_n.\ S
		\end{split}
		\right. \\
		&\ \vdots \\
		&\ l.\ S
	\end{split}
	\right. \qquad
	\begin{split}
        \\ \\ \\ \\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \lor\ \textbf{Elim}: i, j_1\text{-}k_1, \dots, j_n\text{-}k_n
	\end{split}
\end{equation*}
$$

### 条件式

除了布尔连接符外，我们还很容易注意到其它连接符的存在。比如 **实质条件（Material Conditional）**，记为 $\to$。$P\to Q$ 当且仅当 $P$ 为真且 $Q$ 为假时为假（因为此时可以确定 $P$ 无法作为条件”推出“ $Q$）。在自然语言中，”如果……那么“可以转写成实质条件符。

在一个实质条件句 $P\to Q$ 中，$P$ 称为 $Q$ 的 **充分条件（Sufficient Condition）**，而 $Q$ 称为 $P$ 的 **必要条件（Necessary Condition）**；前者在自然语言中常用的词汇是”当 $P$ 则 $Q$“，后者则是”仅当 $P$ 则 $Q$“。如果 $P$ 同时是 $Q$ 的充分条件与必要条件，即“当且仅当 $P$ 才 $Q$”时，两者形成了 **双条件（Bicondition）**，写作 $P\leftrightarrow Q$。

虽然符号很相似（甚至概念上也有相似之处），但条件与双条件和逻辑等价与逻辑后承时完全不同的；前者描述了句子的结构，而后者描述了句子和句子的关系。

我们很容易注意到实质条件的一些性质：

- 若 $P$ 且 $P\to Q$，那么 $Q$。
- 若 $P\to Q$，那么 $\lnot Q \to \lnot P$，两者也相互称为对方的 **逆否命题（Contraposition）**。
- $\leftrightarrow$ 满足交换律和结合律。

下面让我们给出其正式的逻辑规则。

首先，如果某个临时假设 $P$ 可以推出 $Q$，那么我们就能引入一个实质条件：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ \left|
		\begin{split}
			&\underline{\ i}.\ P \\
			&\ \vdots \\
			&\ j.\ Q
		\end{split}
		\right. \\
		&\ k.\ P\to Q
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \to\ \textbf{Intro}: i, j
	\end{split}
\end{equation*}
$$
如果一个实质条件句中充分条件被满足，那么就能消除这个实质条件：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P\to Q \\
		&\ \vdots \\
		&\ j.\ P \\
		&\ \vdots \\
		&\ k.\ Q
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\  \\ \phantom{\vdots} \\ \to\ \textbf{Elim}: i, j
	\end{split}
\end{equation*}
$$
双条件的引入需要两者互相为对方的充分条件：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P\to Q \\
		&\ \vdots \\
		&\ j.\ Q\to P \\
		&\ \vdots \\
		&\ k.\ P \leftrightarrow Q
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \\ \phantom{\vdots} \\ \leftrightarrow\ \textbf{Intro}: i, j
	\end{split}
\end{equation*}
$$
反过来，通过双条件我们可以推出任一个方向的实质条件：
$$
\begin{equation*}
	\begin{split}
		\\ \phantom{\vdots} \\ \triangleright
	\end{split}
	\left|
	\begin{split}
		&\ i.\ P\leftrightarrow Q \\
		&\ \vdots \\
		&\ j.\ P\to Q
	\end{split}
	\right. \qquad
	\begin{split}
		\\ \phantom{\vdots} \\ \leftrightarrow\ \textbf{Elim}: i
	\end{split}
\end{equation*}
$$
虽然我们将实质条件看作全新的连接符，但它可以通过此前介绍过的连接符表示。实际上，$P\to Q$ 正是 $\lnot P\or Q$ 的重言式。任何真值函数类型的句子都可以通过否定、合取与析取连接符构建，因此它们三个具有 **真值函数完备性（Truth Functional Completeness）**。不过，更多的连接符以及建立在它们上面的逻辑规则可以大大简化证明的步骤，这也是为什么我们会引入更多的连接符。

### 合理性与完备性

记 $\mathcal{F}_T$ 为所有通过 $\lnot$、$\land$、$\lor$、$\to$、$\leftrightarrow$ 和 $\bot$ 的证明规则构成的推导系统。记 $P_1, \dots, P_n \vdash_T S$ 若存在 $\mathcal{F}_T$ 的一个元素（即一个证明）使得 $P_1, \dots, P_n$ 可以推导出 $S$。 特别地，如果存在证明满足 $\vdash_T S$（即不需要任何前提），则 $S$ 是一个重言式。

$\mathcal{F}_T$ 具有 **合理性（Soundness）**，因为对任意 $P_1, \dots, P_n \vdash_T S$，我们都可以得到 $S$ 是 $P_1, \dots, P_n$ 的重言后承。

$\mathcal{F}_T$ 也具有 **完备性（Completeness）**，因为对于任意 $P_1, \dots, P_n$ 的重言后承 $S$，我们都可以找到一个证明令 $P_1, \dots, P_n \vdash_T S$。

上面这两个定理实际上说明了在 $\mathcal{F}_T$ 中，所有可以证明的命题都是重言式。



## 量词

自然语言中，存在一些表示存在的量的词汇，比如没有、一个、一些、多数、每一个等。它们对句子含义显然有很重要的影响：

- 每一个人都会死；苏格拉底是人；苏格拉底会死。
- 存在某一个人会死；苏格拉底是人；苏格拉底会死。
- 没有人会死；苏格拉底是人；苏格拉底会死。

如果去掉“每一个”、“某一个”和“没有”，上面三个论述表示的含义是一样的；但是从自然语言的含义来看，只有第一个论述是有效的。

在 FOL 中，有两个 **量词（Quantifier）**，**全称量词（Universal Quantifier）** 和 **存在量词（Existential Quantifier）**，分别用符号 $\forall$ 和 $\exists$ 表示。

有了量词之后，我们就可以引入 **变量（Variable）** 的概念了。它总是通过量词引入到句子中，表示某个可能的对象。比如：

- $\forall x. \text{Good}(x)$ 表示对于任意 $x$ 都满足谓词 $\text{Good}$。
- $\exists xy. \text{Cool}(x, y)$ 表示存在 $x$ 和 $y$ 满足谓词 $\text{Cool}$。

我在引入变量的后面添加了符号 $.$ 用于分隔句子的部分。

### 句子

**良构公式（Well-Formed Formula, WFF）** 定义为下面的几种形式之一：

- 原子句，正如我们在一开始介绍的那样。
- $\lnot P$，若 $P$ 是 WFF。
- $(P_1 \and \dots \and P_n)$，若 $P_1, \dots, P_n$ 是 WFF。
- $(P_1 \or \dots \or P_n)$，若 $P_1, \dots, P_n$ 是 WFF。
- $P \to Q$，若 $P$、$Q$ 是 WFF。
- $P \leftrightarrow Q$，若 $P$、$Q$ 是 WFF。
- $\forall x. P$，若 $P$ 是 WFF。
- $\exists x. P$，若 $P$ 是 WFF。

变量分为 **自由变量（Free Variable）** 和 **约束变量（Bound Variable）**，两者是互补的。

- 原子句中的所有变量都是自由变量。
- $P$ 中的自由变量在 $\lnot P$ 中也是自由变量。
- $P_1, \dots, P_n$ 中的自由变量在 $P_1\and\dots\and P_n$ 中也是自由变量。
- $P_1, \dots, P_n$ 中的自由变量在 $P_1\or\dots\or P_n$ 中也是自由变量。
- $P$ 和 $Q$ 中的自由变量在 $P\to Q$ 中也是自由变量。
- $P$ 和 $Q$ 中的自由变量在 $P\leftrightarrow Q$ 中也是自由变量。
- $P$ 中的自由变量在 $\forall x. P$ 中也是自由变量，除了 $x$ 之外。
- $P$ 中的自由变量在 $\exists x. P$ 中也是自由变量，除了 $x$ 之外。

现在我们定义， **句子（Sentence）** 是不存在自由变量的良构公式。换句话说，我们想要研究的是一个由 $\lnot$、$\and$、$\or$、$\to$、$\leftrightarrow$、$\forall$、$\exists$ 组成的合法的，不包含没有声明的变量的句子。

现在有了理论基础，我们来尝试构建四个典型的带有量词的句子：

- 所有 $P$ 都是 $Q$。$\forall x. (P(x) \to Q(x))$。
- 一些 $P$ 是 $Q$。$\exists x. (P(x) \and Q(x))$。
- 没有 $P$ 是 $Q$。$\forall x. (P(x) \to \lnot Q(x))$。
- 一些 $P$ 不是 $Q$。$\exists x. (P(x) \and \lnot Q(x))$。

这里我们将 $x$ 是 $P$ 记为 $P(x)$，是 $Q$ 则记为 $Q(x)$。



### 量词逻辑



