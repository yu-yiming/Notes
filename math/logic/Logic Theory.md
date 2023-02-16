# Logic Theory

本篇是有关数学符号逻辑基础的个人笔记。



[TOC]

数学中的一些概念，是建立在一系列 **符号（Symbol）** 之上的；这些符号描述数学中的 **结构（Structure）**，即对象的构成性质，以及结构上的 **公理（Axiom）**，即结构需要满足的条件。虽然数学分支众多，性质庞杂，我们依然能够找出每个领域共有的一些数学概念。本篇笔记将使用一系列自洽的符号，来描述数学中常见的结构和公理。
$$
\newcommand{is}[0]{\qquad\Leftrightarrow\qquad} \nonumber
\newcommand{deq}[0]{\operatorname{\dot{=}}}
\newcommand{interp}[3]{{#1}^{\mathbf{#2}}[\mathbf{#3}]}
$$

## 朴素逻辑

本章让我们快速地介绍基本的逻辑概念。由于我们甚至还没有学习集合论的概念，这里的许多说明将借助自然语言。

### 命题

逻辑存在于 **论述（Argument）** 当中。在每一个论述中，我们希望通过一系列条件，称为 **前提（Premise）**，推出一个 **结论（Conclusion）**。在自然语言中，我们会通过一些 **提示词（Indicator）**，如“因为”、“由于”等表示前提，而“所以”、“因此”表示结论。每个论述中，都存在一系列 **句子（Sentence）** 或 **命题（Proposition）**，这里指的并不是自然语言中的句子（以句号为分割的语法结构），而是任意可以判断为 **真（True）** 或 **假（False）** 的结构。

> 举例：人有三只眼睛、太阳总是从西边升起、明天的明天是后天，这些都是句子。这三个句子分别是假、假、真的。通常来讲，除了疑问句、祈使句和感叹句之外都是句子。几个句子通过提示词可以拼接成论述，比如：因为太阳总是从西边升起，因此人有三只眼睛。从这个例子可以看到，论述本身不必合理，也不必“正确”。

> 讨论：有些命题的真假取决于其上下文的选择，此时称其为 **偶真论述（Contingent Argument）**。有些论述一定是正确的，称为 **必要事实（Necessary Truth）**；另一些一定错误的论述则称为 **必要虚假（Necessary Falsehood）**。比如“现在正在下雨”是偶真论述，“现在或者正在下雨，或者不在下雨”是必要事实，而“现在既在下雨，又不在下雨”是必要虚假。
>
> 如果两个命题总是同为真或同为假，则称其为 **必要等价（Necessary Equivalence）**。

如果某个论述中，在其所有前提都为真时，其结论不可能为假时，这个论述便是 **有效（Valid）** 的。反之，称其为 **无效（Invalid）** 的。

> 举例：人都是龙族的后代，而苏格拉底是人，因此苏格拉底是龙族的后代。这是一个有效的论述：*如果*人确实都是龙族的后代，而苏格拉底确实是人，那么苏格拉底不可能不是龙族的后代。可以看到，论述的有效与否和前提的正确性无关（显然人类不是龙族的后代），而只和句子之间的某种“逻辑联系”相关。有些看起来“合理”的论述反而并不有效，比如“过去的两千年间，人都居住在地球上，因此接下来的两千年人依然会居住在地球，这样以过去的某些情况推测未来情形的论述称为 **归纳论述（Inductive Argument）**，这里的归纳论述是无效的，因为完全有可能五千年内人类成功星际殖民。

在一个有效的论述中，我们将其结论称为其前提的 **后承（Consequence）**，两者的关系被称为 **衍推关系（Entailment）**。比如“人都是龙族的后代”和“苏格拉底是人”可以衍推出“苏格拉底是龙族的后代”。

> 讨论：一些论述中的衍推关系不是那样显然，有时这和某些名词的定义（以及它们之间的关系）有关。比如“因为外边在下雨，因此地面是湿的”看起来是一个有效的论述，但句子中我们从来没有详细定义过“外边”究竟是哪里，而“地面”究竟是不是室外的地面，是不是下雨的地方。如果某个论述不需要任何与定义有关的信息就能判断其一定有效，我们称其为 **正式有效（Formally Valid）** 的。此外，如果某个论述不违反自然法则，则称其为 **法则有效（Nomologically Valid）** 的；如果论述不违反一些定义上的规则，则称为 **概念有效（Conceptually Valid）** 的。比如 “因为我们花了十秒钟跑完了一圈操场，因此操场一圈的长度不会超过三百万千米“中，判断其有效的一个隐含前提是物理定律”任何物体的运动速度不会超过光速“以及”光速是三十万千米每秒“。另外，“因为 $A$ 是一个非空集，因此存在某个元素 $a \in A$“，判断其有效的一个隐含前提是非空集合的定义。

如果某个论述的前提都为真且其有效，则称这个论述是 **合理（Sound）** 的。由于合理性通常需要对特定的命题进行判断，其需要的知识超出了逻辑学本身研究的范围。因此我们在本章中不会讨论论述的合理性。

如果两个命题可以同时为真，那么称其为 **共同可能（Jointly Possible）** 的；反之如果两个命题可以同时为假，则称其为 **共同不可能（Jointly Impossible）** 的。

### 真值函数逻辑（TFL）

逻辑学中，为了研究论述的有效性，我们会将命题和论述都符号化。所有的命题都可以抽象成某个 **符号（Symbol）**，而不同命题的结合用语也可以用特殊的 **连接词（Connective）** 表示。

> 记号：每个命题都通过大写的拉丁字母表示，如 $\text{P}$、$\text{Q}$，本篇笔记中用文本格式写出。同时，我们用连接符代替自然语言中的一些连词，即 $\lnot, \land, \lor, \to, \leftrightarrow$。最后，利用括号 $($ 和 $)$ 组合连接词组成的句子。比如 $(\text{P}\lor\text{Q})\land\text{R}$ 和 $\text{P}\lor(\text{Q}\land\text{R})$ 表示的是不同的含义。

下面是各个逻辑连接词的定义，其中 $\text{P}$ 定义为”今天下雨“，而 $\text{Q}$ 定义为”地上湿了“：

- **否定（Negation）** 连词：“不”。$\lnot \text{P}$ 表示“今天不下雨”。
- **合取（Conjunction）** 连词：“并且”、“但是”。$\text{P}\land\text{Q}$ 表示“今天下雨，且地上湿了”。
- **析取（Disjunction）** 连词：“或”、“除非”。$\text{P}\lor\text{Q}$ 表示“今天下雨，或地上湿了”。
- **条件（Condition）** 连词：“那么“。$\text{P}\to\text{Q}$ 表示”如果今天下雨，那么地上会湿“。
- **双条件（Bicondition）** 连词：”当且仅当“。$\text{P}\leftrightarrow\text{Q}$ 表示”今天下雨当且仅当地上湿了“。

我们可以用 **真值表（Truth Table）** 来精确地定义连接符的含义：

| $\text{P}$   | $\text{Q}$   | $\lnot\text{P}$ | $\text{P}\land\text{Q}$ | $\text{P}\lor\text{Q}$ | $\text{P}\to\text{Q}$ | $\text{P}\leftrightarrow\text{Q}$ |
| ------------ | ------------ | --------------- | ----------------------- | ---------------------- | --------------------- | --------------------------------- |
| $\mathbb{T}$ | $\mathbb{T}$ | $\mathbb{F}$    | $\mathbb{T}$            | $\mathbb{T}$           | $\mathbb{T}$          | $\mathbb{T}$                      |
| $\mathbb{T}$ | $\mathbb{F}$ | $\mathbb{F}$    | $\mathbb{F}$            | $\mathbb{T}$           | $\mathbb{F}$          | $\mathbb{F}$                      |
| $\mathbb{F}$ | $\mathbb{T}$ | $\mathbb{T}$    | $\mathbb{F}$            | $\mathbb{T}$           | $\mathbb{T}$          | $\mathbb{F}$                      |
| $\mathbb{F}$ | $\mathbb{F}$ | $\mathbb{T}$    | $\mathbb{F}$            | $\mathbb{F}$           | $\mathbb{T}$          | $\mathbb{T}$                      |

这样通过真值表定义的逻辑代数称为 **真值函数逻辑（Truth-Functional Logic, TFL）**。

> 讨论：逻辑学（以及数学）中，析取连词 $\lor$ 表示的是 **兼或（Inclusive Or）**，即 $\text{P}\lor\text{Q}$ 接受两个命题都为真的情形，这和自然语言中“或”在有些情形下的含义是不同的：比如“今天我或者去看电影，或者在家睡觉”中，“或”是排他的 **异或（Exclusive Or）**。
>
> 我们引入这些连词，是直接从自然语言中选取常见的逻辑抽象成符号；实际上它们并不都是表示所有真值函数逻辑必须的。当我们已经有 $\lnot$ 和 $\lor$ 后：
>
> - $\text{P}\land\text{Q}$ 可以表示成 $\lnot((\lnot\text{P})\land(\lor\text{Q}))$。
> - $\text{P}\to\text{Q}$ 可以表示成 $(\lnot \text{P})\lor Q$。
> - $\text{P}\leftrightarrow\text{Q}$ 可以表示成 $((\lnot\text{P})\lor\text{Q})\land((\lnot\text{Q})\lor\text{P})$。
>
> 在真值函数逻辑中，所有通过真值表决定定义的连接词都称为 **真值函数连接词（Truth-Functional Connective）**。比如我们可以定义全新的连接词 $\sim$，令 $\text{P}\sim\text{Q} = \lnot(\text{P}\leftrightarrow\text{Q})$。

现在我们可以用 **归纳（Induction）** 的方式来给出真值函数逻辑中，句子的定义了。

- $\mathscr{p}$ 是一个句子，如果它是一个命题符号 $\text{P}$。
- $\lnot\mathscr{p}$ 是一个句子，若 $\mathscr{p}$ 是句子。
- $(\mathscr{p}\land\mathscr{q})$ 是一个句子，若 $\mathscr{p}$ 和 $\mathscr{q}$ 都是句子。
- $(\mathscr{p}\lor\mathscr{q})$ 是一个句子，若 $\mathscr{p}$ 和 $\mathscr{q}$ 都是句子。
- $(\mathscr{p}\to\mathscr{q})$ 是一个句子，若 $\mathscr{p}$ 和 $\mathscr{q}$ 都是句子。
- $(\mathscr{p}\leftrightarrow\mathscr{q})$ 是一个句子，若 $\mathscr{p}$ 和 $\mathscr{q}$ 都是句子。

因此，从严格的定义来讲，$(\text{P}\land(\text{Q}\lor(\lnot\text{R})))$ 是一个句子，而 $\text{P}\land\lor\text{Q}$ 不是。

> 记号：上面在谈论句子定义的时候，我用花体小写拉丁字母来表示某个句子。这是因为这里我们跳出了“真值函数逻辑”所能表示的语言之外。这种”跳出某个领域用以描述该领域“的语言称为 **元语言（Metalanguage）**。此前我们主要用的元语言是自然语言（汉字）。元语言中的概念我将一律以花体符号表示。

> 习惯：为了简便性，最外层的括号总是被省略。对于顺次出现的多个相同连接符，有时候它们之中的组合顺序并不重要，此时我们会省略括号。比如 $((\text{P}\land\text{Q})\land\text{R})$ 和 $(\text{P}\land(\text{Q}\land\text{R}))$ 是必要等价的，因此我们会直接将其写成 $\text{P}\land\text{Q}\land\text{R}$。

> 讨论：能否省略连续多个相同连接符中括号取决于连接符是否满足 **结合律（Associativity）**。对于符号 $@$，如果 $((\text{P}@\text{Q})@\text{R})$ 与 $(\text{P}@(\text{Q}@\text{R}))$ 必要等价，称其为 **结合（Associative）** 的。利用真值表我们不难证明 $\land$、$\lor$ 和 $\leftrightarrow$ 都满足结合律，但 $\to$ 不满足结合律。

在 TFL 中，将必要事实称为 **重言式（Tautology）**，而必要虚假称为 **矛盾式（Contradiction）**。如果两个 TFL 中的句子在任何时候都同为真或同为假，则称他们是 **等价的（Equivalent）**。

> 举例：$\text{P}\lor\lnot\text{P}$ 是重言式，而 $\text{P}\land\lnot\text{P}$ 是矛盾式。$\lnot(\text{P}\land\text{Q})$ 和 $\lnot\text{P}\lor\lnot\text{Q}$ 是等价的（可以通过真值表证明）。

如果某个 TFL 句子的真值表中存在某一个结果为真，则称这个句子为 **可满足（Satisfiable）** 的。如果一系列 TFL 句子在某一行结果同时为真，则称它们 **共同可满足（Jointly Satisfiable）** 的；反之，如果不存在任一行令每个 TFL 句子都为真，则称它们为 **共同不可满足（Jointly Unsatisfiable）** 的。

对于句子 $\mathscr{p}_1, \dots \mathscr{p}_n$ 在同时为真时，$\mathscr{q}$ 不可能为假，则称 $\mathscr{p}_1, \dots \mathscr{p}_n$ **衍推（Entail）** 了 $\mathscr{q}$。这是对命题逻辑中论述前提和结论关系的抽象。

> 记号：在 TFL 的元语言中，用 $\vDash$ 表示衍推关系。比如我们可以用 $\mathscr{p}_1, \dots, \mathscr{p}_n \vDash \mathscr{q}$ 来表示上面的衍推关系。

> 举例：$\text{P}, \text{P}\to\text{Q} \vDash \text{Q}$，当 $\text{P}$ 与 $\text{Q}$ 同为真时，显然 $\vDash$ 右侧被满足。

衍推符号左右两侧都可以为空。如果某个衍推关系中左侧为空，即 $\vDash \mathscr{q}$，则 $\mathscr{q}$ 是一个重言式；如果右侧为空，即 $\mathscr{p}\vDash$，则 $\mathscr{p}$ 是一个矛盾式。

> 讨论：一个容易令人迷惑的点是 $\to$ 和 $\vDash$ 的区别。一言以蔽之，前者是一个 TFL 的连接词，而后者是一个元语言的符号。$\mathscr{p}\to\mathscr{q}$ 可以建立一个真值表；而 $\mathscr{p}\vDash\mathscr{q}$ 则叙述了 $\mathscr{p}$ 和 $\mathscr{q}$ 的一个关系（这个关系是由 $\mathscr{p}$ 和 $\mathscr{q}$ 的真值表共同决定的）。

### TFL 的推导系统

本节中我们将介绍建立在 TFL 上的 **正式证明（Formal Proof）**。对于每个 TFL 中的连接词，都可以给出一个 **引入（Introduction）** 规则和 **消去（Elimination）** 规则。凭借此，我们就可以从一系列前提，衍推至某个想要的结论。我们将使用 **Fitch 记号（Fitch Notation）** 来进行正式证明。

- 合取规则：当我们在证明过程中分别得到了 $\mathscr{p}$ 和 $\mathscr{q}$，那显然可以推出 $\mathscr{p}\land \mathscr{q}$。
  $$
  \begin{equation}
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p} \\ &\ \mathscr{q} \\ &\ \mathscr{p}\land\mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \land\mathbf{I}. i, j
  \end{split}
  \end{equation}
  $$
  而当我们已经得到 $\mathscr{p}\land\mathscr{q}$ 时，我们可以自然地得到 $\mathscr{p}$ 或 $\mathscr{q}$ 中任一个：
  $$
  \begin{equation}
  \begin{split}
  	i \\ j
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\land\mathscr{q} \\ &\ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \land\mathbf{E}. i
  \end{split}
  \end{equation}
  $$

- 条件规则：条件句的引入需要在证明中给出某个 **假设（Assumption）**，并得到一个结论。
  $$
  \begin{equation}
  \begin{split}
  	i \\ \phantom{\frac{}{}}  j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \left|
  	\begin{split}
  		&\ \mathscr{p} \\ &\overline{\ \mathscr{q}}
  	\end{split}
  	\right. \\
  	&\ \mathscr{p}\to\mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \phantom{\frac{}{}} \\ \to\mathbf{I}. i\text{-}j
  \end{split}
  \end{equation}
  $$
  如果已经拥有某个条件 $\mathscr{p}\to\mathscr{q}$ 以及 $\mathscr{p}$ 时，我们可以消去这个条件。
  $$
  \begin{equation}
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\to\mathscr{q} \\ &\ \mathscr{p} \\ &\ \mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \to\mathbf{E}. i, j
  \end{split}
  \end{equation}
  $$

- 双条件规则：双条件的引入相当于要证明两个方向上的条件都成立。
  $$
  \begin{equation}
  \begin{split}
  	i \\ \phantom{\frac{}{}} j \\\phantom{\frac{}{}} k \\  l \\ m
  \end{split}
  \left|
  \begin{split}
  	&\ \left|
  	\begin{split}
  		&\ \mathscr{p} \\ &\overline{\ \mathscr{q}}
  	\end{split}
  	\right. \\
  	&\ \left|
  	\begin{split}
  		&\ \mathscr{q} \\ &\overline{\ \mathscr{p}}
  	\end{split}
  	\right. \\
  	&\ \mathscr{p}\leftrightarrow\mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\\phantom{\frac{}{}} \\ \phantom{\frac{}{}} \\ \\ \leftrightarrow\textbf{I}. i\text{-}j, k\text{-}l
  \end{split}
  \end{equation}
  $$
  当我们已经得到双条件句，已经两边中任一个句子时，都可以得到另一个句子。
  $$
  \begin{equation}
  \begin{split}
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\leftrightarrow\mathscr{q} \\ &\ \mathscr{p} \\ &\ \mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \leftrightarrow\mathbf{E}. i, j
  \end{split}
  \end{split}
  \qquad\qquad
  \begin{split}
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\leftrightarrow\mathscr{q} \\ &\ \mathscr{q} \\ &\ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \leftrightarrow\mathbf{E}. i, j
  \end{split}
  \end{split}
  \end{equation}
  $$

- 析取规则：在证明中如果我们已经有了 $\mathscr{p}$ 或 $\mathscr{q}$ 中的任一个，都可以直接引入 $\mathscr{p}\lor\mathscr{q}$。
  $$
  \begin{equation}
  \begin{split}
  	i \\ j
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p} \\ &\ \mathscr{p}\lor\mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \lor\mathbf{I}. i
  \end{split}
  \end{equation}
  $$
  析取的消去规则比较复杂：我们需要证明以两边的任一个句子作为假设都能推出某个结论。
  $$
  \begin{equation}
  \begin{split}
  	i \\ \phantom{\frac{}{}} j \\\phantom{\frac{}{}} k \\  l \\ m \\ n
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\lor\mathscr{q} \\
  	&\ \left|
  	\begin{split}
  		&\ \mathscr{p} \\ &\overline{\ \mathscr{r}}
  	\end{split}
  	\right. \\
  	&\ \left|
  	\begin{split}
  		&\ \mathscr{q} \\ &\overline{\ \mathscr{r}}
  	\end{split}
  	\right. \\
  	&\ \mathscr{r}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\\phantom{\frac{}{}} \\ \phantom{\frac{}{}} \\ \\ \leftrightarrow\textbf{E}. j\text{-}k, l\text{-}m
  \end{split}
  \end{equation}
  $$

- 否定规则：为了介绍否定的推导定律，我们还需要引入一个特殊的符号 $\bot$，它表示矛盾。

  否定的引入需要让其推出矛盾。
  $$
  \begin{equation}
  \begin{split}
  	i \\ \phantom{\frac{}{}} j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \left|
  	\begin{split}
  		&\ \mathscr{p} \\ &\overline{\ \bot}
  	\end{split}
  	\right. \\
  	&\ \lnot \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \phantom{\frac{}{}} \\ \lnot\textbf{I}. i\text{-}j
  \end{split}
  \end{equation}
  $$

  如果要消除某个否定句，唯一的可能就是出现了矛盾。
  $$
  \begin{equation}
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \lnot\mathscr{p} \\ &\ \mathscr{p} \\ &\ \bot
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \lnot\textbf{E}. i\text{-}j
  \end{split}
  \end{equation}
  $$
  否定与引入有一个非常类似的变形，称为 **间接证明（Indirect Proof）**：
  $$
  \begin{equation*}
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \left|\begin{split}
  		&\ \lnot\mathscr{p} \\
  		&\!\overline{\,\ \bot}
  	\end{split}
  	\right. \\
  	&\ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \textbf{IP}. i\text{-}j
  \end{split}
  \end{equation*}
  $$
  
- 爆炸规则：最后一个规则是，通过 $\bot$ 可以推出任意的结论。
  $$
  \begin{equation}
  \begin{split}
  	i \\ j
  \end{split}
  \left|
  \begin{split}
  	&\ \bot \\ &\ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \mathbf{X}.i
  \end{split}
  \end{equation}
  $$

### 其它的 TFL 推导规则

本节中介绍其它常用的 TFL 推导规则：它们都可以由上一节给出的推导规则定义。

- 一个显然的规则是 **重述（Reiteration）**。某个句子在证明的不同地方可以重复出现。
  $$
  \begin{equation}
  \begin{split}
    i \\ j
  \end{split}
  \left|
  \begin{split}
    \ \mathscr{p} \\ \ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
    \\ \textbf{R}. i
  \end{split}
  \end{equation}
  $$
  下面是一个正式证明；
  $$
  \begin{equation*}
  \begin{split}
  	1 \\ 2 \\ 3
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p} \\
  	&\ \mathscr{p}\land\mathscr{p} \\
  	&\ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \land&\textbf{I}. 1, 1 \\ \land&\text{E}. 2
  \end{split}
  \end{equation*}
  $$
  
- 如果我们有 $\mathscr{p}\lor\mathscr{q}$ 和 $\lnot\mathscr{p}$，那么显然 $\mathcal{q}$ 成立。
  $$
  \begin{equation}
  \begin{split}
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\lor\mathscr{q} \\ &\ \lnot\mathscr{p} \\ &\ \mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \textbf{DS}. i, j
  \end{split}
  \qquad\qquad
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\lor\mathscr{q} \\ &\ \lnot\mathscr{q} \\ &\ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \textbf{DS}. i, j
  \end{split}
  \end{split}
  \end{equation}
  $$
  这被称为 **析取三段论（Disjunction Syllogism）**。下面是一个正式证明：
  $$
  \begin{equation*}
  \begin{split}
  	1 \\  2 \\ \phantom{\frac{}{}} 3 \\ \phantom{\frac{}{}} 4 \\ 5 \\ 6 \\ 7 \\ 8
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\lor\mathscr{q} \\
  	&\ \lnot{p} \\
  	&\ \left|
  	\begin{split}
  		&\ \mathscr{p} \\
  		&\!\overline{\,\ \bot} \\
  		&\ \mathscr{q}
  	\end{split}
  	\right. \\
  	&\ \left|\begin{split}
  		&\ \mathscr{q} \\
  		&\!\overline{\,\ \mathscr{q}}
  	\end{split}
  	\right. \\
  	&\ \mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \phantom{\frac{}{}} \\ \phantom{\frac{}{}} &\lnot\textbf{I}. 2, 3 \\ &\textbf{X}. 3 \\ \\ &\text{R}. 6 \\ &\lor\text{E}. 3\text{-}5, 6\text{-}7
  \end{split}
  \end{equation*}
  $$

- 如果我们有 $\mathscr{p}\to\mathscr{q}$ 和 $\lnot\mathscr{q}$，那么显然 $\lnot{p}$ 也成立。
  $$
  \begin{equation}
  \begin{split}
  	i \\ j \\ k
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\to\mathscr{q} \\ &\ \lnot\mathscr{q} \\ &\ \lnot\mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \textbf{MT}. i, j
  \end{split}
  \end{equation}
  $$
  这被称为 **否定后件（Modus Tollens）**。下面是一个正式证明：
  $$
  \begin{equation*}
  \begin{split}
  	1 \\ 2 \\  3 \\ 4 \\ 5 \\ 6
  \end{split}
  \left|
  \begin{split}
  	&\ \mathscr{p}\to\mathscr{q} \\
  	&\ \lnot\mathscr{q} \\
  	&\ \left|
  	\begin{split}
  		&\ \mathscr{p} \\
  		&\! \overline{\,\ \mathscr{q}} \\
  		&\ \bot
  	\end{split}
  	\right. \\
  	&\ \lnot\mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \\ \to&\textbf{E}. 1, 3 \\ \lnot&\text{E}. 2, 4 \\ \lnot&\text{I}. 3\text{-}5
  \end{split}
  \end{equation*}
  $$

- 否定的否定就是肯定。
  $$
  \begin{equation}
  \begin{split}
  	i \\ j
  \end{split}
  \left|
  \begin{split}
  	&\ \lnot\lnot\mathscr{p} \\ &\ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \textbf{DNE}. i
  \end{split}
  \end{equation}
  $$

  这被称为 **双重否定消去（Double Negation Elimination）**。下面是一个正式证明：
  $$
  \begin{equation*}
  \begin{split}
  	1 \\ 2 \\ 3 \\ 4
  \end{split}
  \left|
  \begin{split}
  	&\ \lnot\lnot\mathscr{p} \\
  	&\ \left|
  	\begin{split}
  		&\ \lnot\mathscr{p} \\
  		&\!\overline{\,\ \bot}
  	\end{split}
  	\right. \\
  	&\ \mathscr{p}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \\ \lnot&\textbf{E}. 1, 2 \\ &\textbf{IP}. 2\text{-}3
  \end{split}
  \end{equation*}
  $$
  
- 如果无论在 $\mathscr{p}$ 还是 $\lnot\mathscr{p}$ 的假设下，$\mathscr{q}$ 都成立，那么显然 $\mathscr{q}$ 恒成立。
  $$
  \begin{equation}
  \begin{split}
  	i \\ j \\ k \\ l \\ m
  \end{split}
  \left|
  \begin{split}
  	&\ \left|\begin{split}
  		&\ \mathscr{p} \\
  		&\!\overline{\,\ q}
  	\end{split}
  	\right. \\
  	&\ \left| \begin{split}
  		&\ \lnot\mathscr{p} \\
  		&\!\overline{\,\ q}
  	\end{split}
  	\right. \\
  	&\ \mathscr{q}
  \end{split}
  \right.
  \begin{split}
  	\\ \\ \\ \\ \textbf{LEM}. i\text{-}j, k\text{-}l
  \end{split}
  \end{equation}
  $$

- 这是指下面这四个和 $\lnot$、$\land$ 和 $\lor$ 相关的定律：
  $$
  \begin{equation}
  \begin{split}
  \begin{split}
  	i \\ j
  \end{split}
  \left|
  \begin{split}
  	&\ \lnot(\mathscr{p}\land\mathscr{q}) \\
  	&\ \lnot\mathscr{p}\lor\lnot\mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \textbf{DM}.i
  \end{split}
  \end{split}
  \qquad
  \begin{split}
  \begin{split}
  	i \\ j
  \end{split}
  \left|
  \begin{split}
  	&\ \lnot(\mathscr{p}\lor\mathscr{q}) \\
  	&\ \lnot\mathscr{p}\land\lnot\mathscr{q}
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \textbf{DM}.i
  \end{split}
  \end{split}
  \qquad
  \begin{split}
  \begin{split}
  	i \\ j
  \end{split}
  \left|
  \begin{split}
  	&\ \lnot\mathscr{p}\lor\lnot\mathscr{q} \\
  	&\ \lnot(\mathscr{p}\land\mathscr{q})
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \textbf{DM}.i
  \end{split}
  \end{split}
  \qquad
  \begin{split}
  \begin{split}
  	i \\ j
  \end{split}
  \left|
  \begin{split}
  	&\ \lnot\mathscr{p}\land\lnot\mathscr{q} \\
  	&\ \lnot(\mathscr{p}\lor\mathscr{q})
  \end{split}
  \right. \qquad
  \begin{split}
  	\\ \textbf{DM}.i
  \end{split}
  \end{split}
  \end{equation}
  $$



### 完备性与合理性

利用前两节介绍的规则，如果我们可以经过一系列步骤从 $\mathscr{p}_1, \dots, \mathscr{p}_n$ 推导得到 $\mathscr{q}$，则称 $\mathscr{p}_1, \dots, \mathscr{p}_n$ **证明（Prove）** 了 $\mathscr{q}$。

> 记号：在 TFL 的推导系统中，用 $\vdash$ 表示证明关系。比如我们可以用 $\mathscr{p}_1, \dots, \mathscr{p}_n \vdash \mathscr{q}$ 来表示上面的证明关系。

证明符号的左侧为空，即 $\vdash \mathscr{q}$ 时，称 $\mathscr{q}$ 是一个 **定理（Theorem）**。如果从一系列句子 $\mathscr{p}_1, \dots, \mathscr{p}_n$ 中推导得到了 $\bot$，就称它们是 **共同不一致（Jointly Inconsistent）** 的。反之，它们就是 **共同一致（Jointly Consistent）** 的。

如果两个句子可以互相证明，则称它们为 **可证等价（Provably Equivalent）** 的。

看起来，TFL 系统和它的推导系统有很强的相似性：$\mathscr{p}$ 的真伪可以通过真值表确定（$\vDash\mathscr{p}$ 则为必要事实），也可以通过推导得到（$\vdash\mathscr{p}$ 则为定理）。虽然通过不同的名词描述，但这两个系统想表达的性质应该是一样的。下面是一张表描述两者之间的相似性：

| 概念                | 真值表定义（语义）                                     | 证明理论定义（语法）                         |
| ------------------- | ------------------------------------------------------ | -------------------------------------------- |
| 重言式/定理         | 一个句子的真值表的结果全部为真                         | 一个句子可以不通过任何前提推导得到           |
| 矛盾式/不一致句     | 一个句子的真值表的结果全部为假                         | 一个句子的否定可以不通过任何前提推导得到     |
| 偶真式              | 一个句子的真值表的结果中有真有假                       | 一个句子及其否定都不能不通过任何前提推导得到 |
| 等价句              | 两个句子的真值表中结果完全一致                         | 两个句子可以互相推导得到                     |
| 可满足式/一致句     | 多个式子的真值表中存在某一行均为真                     | 多个式子不能推导出矛盾                       |
| 不可满足式/不一致句 | 多个式子的真值表中不存在任一行均为真                   | 多个式子可以推导出矛盾                       |
| 合理论述            | 前提和结论的真值表中，不存在任一行前提为真而结论为假的 | 前提可以推导得到结论                         |

本篇笔记并不给出两个系统等价的证明。不过证明过程包含了两个步骤：其一是任意语义有效的论述都有一个对应的证明推导，这被称为 **完备性（Completement）**；另一方面，任意证明有效的论述不会在真值表中得到假，这被称为 **合理性（Soundness）**。 



### 一阶逻辑

TFL 逻辑体系缺少量词的存在，因此“某一个人是龙族”、“所有人都是龙族”都只能苍白地通过一个符号表示。现在，我们想要更加细致地描述一个命题，首先需要引入最小的名词单元，**名字（Name）**。

> 记号：我们用小写拉丁字母表示某个名字，如 $\text{a}$。这个符号用来代表某个特定的个体。

> 举例：我家的猫可以用 $\text{c}$ 表示；桌上的水杯可以用 $\text{b}$ 表示。

从名字出发，我们加上 **谓词（Predicate）** 来组成一个命题。谓词是需要和特定个数的名字共同使用的符号，此时它拥有一个真值。

> 记号：我们用大写拉丁字母表示某个谓词，如 $\text{P}$。谓词需要和一到多个名字一起使用以得到一个真值，比如 $\text{P}(\text{a})$。

> 举例：xxx 是我家的可以用谓词 $\text{H}$ 表示；具体来说，如果某只猫 $\text{c}$ 是我家的，可以表示成 $\text{H}(\text{c})$。

现在让我们引入 **量词（Quantifier）**。量词总是和 **变量（Variable）** 一起使用，将“特定量“的对象引入句子中。这些对象所在的范围称为 **作用域（Domain）**。

> 记号：我们用斜体的小写拉丁字母表示某个变量，如 $x$。有两个量词，**全称量词（Universal Quantifier）** $\forall$ 和 **存在量词（Existential Quantifier）** $\exists$。

> 举例：顾名思义，全称量词和存在量词分别声明“全部”和“存在某个”对象有特定的性质。比如“所有东西都是我家的”可以表示成 $\forall x. \text{H}(x)$。这里我们利用全称量词引入了“所有对象” $x$，随即和谓词 $\text{H}$ 一同使用变量 $x$ 表示对于每个引入的对象 $\text{o}$ 都有 $\text{H}(\text{o})$ 的性质。“有人很生气”则可以表示成 $\exists x. \text{A}(x)$。

TFL 引入量词之后的系统称为 **一阶逻辑（First Order Logic, FOL）**。下面是一些常用的自然语言和 FOL 之间的转换：

- “所有 $f$ 都是 $g$”，可以用 $\forall x.(\text{F}(x)\land\text{G}(x))$ 表示。
- “存在某个 $f$ 是 $g$“，可以用 $\exists x. (\text{F}(x)\to\text{G}(x))$ 表示。
- ”不存在 $f$ 是 $g$“，可以用 $\forall x.(\text{F}(x)\to\lnot\text{G}(x))$ 表示。
- “只有 $f$ 才是 $g$”，可以用 $\forall x. (\text{G}(x)\to\text{F}(x))$ 表示。

一个句子中可以出现多个量词；量词的先后次序会影响句子的含义，看下面的例子：

> 举例：对任何人，存在一个人是他的朋友可以通过 $\forall x.\exists y. \text{F}(x, y)$。而存在一个人，任何人都是他的朋友可以通过 $\exists y. \forall x. \text{F}(x, y)$ 表示。可以看到仅仅交换了一对量词，其含义就产生了巨大的变化。

在 FOL 中，所有单独出现的字母都称为 **项（Term）**。我们可以通过项和符号构成 **原子句（Atomic Sentence）**：

- $\mathscr{p}$，其中 $\mathscr{p}$ 是一个句子。
- $\mathscr{f}(\mathscr{t}_1, \dots, \mathscr{t}_n)$，其中 $\mathscr{f}$ 是一个谓词而 $\mathscr{t}_1, \dots, \mathscr{t}_n$ 是项。
- $\mathscr{t}_1 = \mathscr{t}_2$，其中 $\mathscr{t}_1, \mathscr{t}_2$ 是项。

随后，我们就可以定义 FOL 中的 **公式（Formula）** 了：

- $\lnot\mathscr{p}$，其中 $\mathscr{p}$ 是一个公式。
- $(\mathscr{p}\land\mathscr{q})$，其中 $\mathscr{p}$ 和 $\mathscr{q}$ 都是公式。
- $(\mathscr{p}\lor\mathscr{q})$，其中 $\mathscr{p}$ 和 $\mathscr{q}$ 都是公式。
- $(\mathscr{p}\to\mathscr{q})$，其中 $\mathscr{p}$ 和 $\mathscr{q}$ 都是公式。
- $(\mathscr{p}\leftrightarrow\mathscr{q})$，其中 $\mathscr{p}$ 和 $\mathscr{q}$ 都是公式。
- $\forall \mathscr{x}. \mathscr{p}$，其中 $\mathscr{x}$ 是一个变量，而 $\mathscr{p}$ 是一个公式。
- $\exists \mathscr{x}. \mathscr{p}$，其中 $\mathscr{x}$ 是一个变量，而 $\mathscr{p}$ 是一个公式。

在最后两种情形中，$\mathscr{x}$ 会被引入到整个 $\mathscr{p}$ 当中，我们称这个作用范围为 **作用域（Scope）**。变量出现在它的作用域中时，我们称其为 **约束出现（Bound Occurrence）**，反之则称其为 **自由（Free Occurrence）**。

> 举例：$\forall x. (x\land \exists y. \text{P}(y))$ 中的 $x$ 和 $y$ 都是约束出现，而 $x\lor y$ 中的 $x$ 和 $y$ 都是自由出现。

FOL 不允许变量的自由出现。





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
少见地也有 $B \supseteq A$ 的写法。可以看到这个定义是 $A = B$ 定义的一部分。因此有下面这个真命题：
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
> 值得说明的是，上面我虽然用 $A_i$ 来表示某个 $\mathcal{A}$ 中的元素，但并不代表 $\mathcal{A}$ 可以通过某个序数集 $\mathbb{I} = \{1, 2, \dots\}$ 的方式枚举出来；这涉及到集合的可数性，我们会在之后介绍。这里只需要将 $A_i$ 看作某个 $\mathcal{A}$ 的元素即可。

> 习惯：对于其元素拥有”特殊性质“的集合，我会将其记成书写体。上面这样的集合的集合 $\mathcal{A}$ 就是一个例子。

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

> 记号：习惯上会将 $((a, b), c)$ 简写为 $(a, b, c)$。类似地，$(\dots((a_1, a_2), a_3)\dots, a_n)$ 可以简写成 $(a_1, \dots, a_n)$。我们将这种结构称为 **元组（Tuple）**。元组通常用粗体的字母表示，如 $\mathbf{a}$，$\mathbf{v}$ 等。

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

关于多元组，我们采用下标的形式得到元组中的某个元素。比如 $\mathbf{a}_i$ 表示 $\mathbf{a}$ 中的第 $i$ 个元素。



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

> 讨论：再一次地，定义 $(a, b)$ 为 $\{\{a\}, \{a, b \}\}$ 而非 $\{\{b\}, \{a, b\}\}$ 只是一个选择而已。此外，从这个定义来看，元组中的元素并非其集合论定义中的元素，因为显然 $(a, b)$ 的元素是 $\{a\}$ 和 $\{a, b\}$ 而非 $a$ 和 $b$。后面我会用 **成员（Memeber）** 来表示这个定义下的元组“元素”。

#### 字符串

给定一个有限集 $\Sigma$，称为 **字母表（Alphabet）**；在 $\Sigma$ 上定义的 **拼接（Concatenation）** 操作是对笛卡尔积（即元组）的一个简写：
$$
\forall a_i \in \Sigma, a_1\dots a_n \equiv (a_1, \dots, a_n)
$$
显然 $a_1\dots a_n \in \Sigma^n$。特别地，定义 $\Sigma^0$ 为只包含元素 $\epsilon$ 的一个特殊集合。定义 $\Sigma$ 的 **Kleene 闭包（Kleene Closure）** 为 $\Sigma$ 所有非负有限次幂的交，即：
$$
\begin{equation}
	\Sigma^* = \bigcup_{i=0} \Sigma^i
\end{equation}
$$
定义 **字符串（String）** 为 $\Sigma^*$ 的元素。特别地，称 $\epsilon$ 为 **空字符串（Empty String）**。

> 记号：通常情况下我们谈论的字符串都是有限的（尤其是在计算理论中，因为计算机无法处理无限的信息），但在一些语境中，也会出现“无限字符串”的应用。其所在的集合记为 $\Sigma^\omega$。

> 举例：对于字母表 $\mathbb{B} = \{0, 1\}$，字符串 $w = 0101 \in \mathbb{B}^4$，字符串 $w = 110011 \in \mathbb{B}^6$。当然，它们都是 $\mathbb{B}^*$ 的元素。

> 讨论：如果将字符串理解成笛卡尔积中元素的简写，那么在遇到字符串集之间的笛卡尔积时或许会令人迷惑。比如考虑 $\Sigma^2\times\Sigma^3$ 理论上应该定义成：
> $$
> \Sigma^2\times\Sigma^3 = \{(w_1w_2, x_1x_2x_3) \mid w_i, x_j \in \Sigma \}
> $$
> 然而它们的元素却是 $y_1y_2y_3y_4y_5 \in \Sigma^5$。如果写成有序元组的形式会是 $((((y_1, y_2), y_3), y_4), y_5)$，和上面的 $(w_1w_2, x_1x_2x_3)$ 形式完全不同。说到底，这就是因为逗号运算符不满足结合律。我们在处理这种情况时，会将其组合在一起的部分写成幂的形式，而明确分开的部分用 $\times$ 分隔。

#### 关系

集合 $A$ 上的 **n 元关系（n-ary Relation）** 定义为子集 $\mathcal{R} \subseteq A^n$，可以等效写成：
$$
\begin{equation}
	\mathcal{R} = \{x \in A^n \mid \mathscr{r}_x \}
\end{equation}
$$
其中 $\mathscr{r}_x$ 是一个定义 n 元关系的和 $x$ 相关的式子。

> 举例：$\mathbb{R}$ 上的小于等于关系 $\le$ 定义为 $\mathcal{R}_\le = \{ (x, y) \in \mathbb{R}^2 \mid \text{sgn}_{x-y} = -1 \}$，其中包括了 $(1, 1)$、$(\pi, 2e)$ 等元素。这里用到的符号 $\text{sgn}_i$ 用来判断实数 $i$ 的符号，定义为：
> $$
> \begin{equation*}
> 	\text{sgn}_i = \begin{cases} 1 & i > 0 \\ 0 & i = 0 \\ -1 & i < 0 \end{cases}
> \end{equation*}
> $$
> 通常它被称为 **符号函数（Sign Function）**；不过我们还没有正式接触函数的定义，因此我避免使用函数的记号，而采用下标的形式。

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

对于一个偏序集 $A$ 和其子集 $S$，若存在 $u \in A$ 对任意 $x \in S$ 都满足 $x \le u$，则称 $u$ 是 $A$ 的一个 **上界（Upper Bound）**。如果某个 $A$ 的上界 $v$ 满足对任意上界 $u$ 都有 $v \le u$，则称其为 $A$ 的 **上确界（Least Upper Bound）**，用 $\sup{A}$ 表示。类似地，我们可以定义 **下界（Lower Bound）** 和 **下确界（Greatest Lower Bound）** $\inf{A}$。不难证明上确界和下确界总是唯一的。

如果某个全序集中的每个非空子集都有一个在该序关系下的最大元素，则称其是一个 **良序集（Well-Ordered Set）**。

#### 函数

**函数（Function）** 是一个从某个集合 $A$ 到另一个集合 $B$ 的映射集合，记作 $f: A \to B$。其中 $A$ 称为 $f$ 的 **定义域（Domain）**，$B$ 是 $f$ 的 **陪域（Codomain）**。我们可以将函数构造为 $A\times B$ 上的一个二元关系（注意这里 $A, B \subseteq S$ 是我们选定的子集，而 $A\times B$ 上的二元关系是指满足 $a \in A$，$b \in B$ 的有序对 $(a, b)$ 组成集合的子集）满足：
$$
\begin{equation}
	\forall a \in A, \exists! b \in B. (a, b) \in \mathcal{R}_f
\end{equation}
$$
用平易的方式解释这个定义，即定义域 $A$ 中任意的元素都能找到唯一一个对应的元素 $b \in B$。

> 记号：
>
> - 由于每个 $a \in A$ 能映射到的元素是唯一的，我们将其记为 $f(a)$。
> - 函数 $f$ 的定义域记为 $\operatorname{dom}{f}$。
> - 函数有时会定义成 $x \mapsto y$ 的形式，表示 $x$ 映射到 $y$。比如 $x \mapsto x + 1$ 表示函数 $\{(0, 1), (1, 2), \dots\} \subseteq \mathbb{N}^2$。
> - 对于子集 $S \subseteq A$，记经过 $f$ 映射到 $B$ 的子集为 $f(S) = \{b \in B \mid \exists x \in S. f(x) = b\}$。这实际上定义了 $f: \mathbb{P}(A) \to \mathbb{P}(B)$，它和 $f: A \to B$ 使用了相同的记号，这实际上也更方便理解。

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

#### 数集构造

首先，定义自然数集 $\mathbb{N}$ 为 $\{0, 1, 2, \dots\}$，其中记号 $0, 1, 2, \dots$ 的正式定义如下：
$$
\begin{equation*}
	0 = \{\} = \emptyset \qquad 1 = \{0\} = \{\emptyset\} \qquad 2 = \{0, 1\} = \{\emptyset, \{\emptyset\}\}, \qquad \dots
\end{equation*}
$$
在 $\mathbb{N}$ 上定义了一个单射 $S: \mathbb{N} \to \mathbb{N}$：
$$
\begin{equation}
	S(n) = n \cup \{n\}
\end{equation}
$$
这也被称为 **后继函数（Successor Function）**。通过这个函数，我们就可以定义自然数集上的加法：

- $+: \mathbb{N}^2 \to \mathbb{N}$ 是一个满足交换律和结合律的运算符。
- $x + 0 = x$。
- $x + S(n) = S(x + n)$。注意这个定义成立的基础是 $S$ 是一个单射。

> 举例：比如 $1 + 1$：
> $$
> \begin{equation*}
> 	1 + 1 = 1 + S(0) = S(1 + 0) = S(1) = 2
> \end{equation*}
> $$
> 这和我们预想中的结果一致。

随后，考虑自然数集形成的有序对集 $\mathbb{N}^2$，且定义上面的二元关系 $\mathcal{R}$ 为：
$$
\begin{equation}
	((a, b), (c, d)) \in \mathcal{R} \is a + d = b + c
\end{equation}
$$
这是一个等价关系：

- 显然 $a + a = a + a$，因此 $((a, a), (a, a)) \in \mathcal{R}$。
- 若 $((a, b), (c, d)) \in \mathcal{R}$，则有 $a + d = b + c \implies c + b = a + d$，因此 $((c, d), (a, b)) \in \mathcal{R}$。
- 若 $((a, b), (c, d)), ((c, d), (e, f)) \in \mathcal{R}$，则有 $a + d = b + c$ 且 $c + f = d + e$。两个等式相加后得到 $a + f = b + e$，即 $((a, b), (e, f)) \in \mathcal{R}$。

> 讨论：上面我们证明 $\mathcal{R}$ 的传递性时省略了重要的一步：两个等式相加后我们得到的是：
> $$
> \begin{equation*}
> 	a + d + c + f = b + c + d + e
> \end{equation*}
> $$
> 虽然两侧都有 $c$ 和 $d$ 两项，但我们需要证明 $+$ 满足 **消去律（Cancellation Law）** 才能执行这一步，即 $a + c = b + c \implies a = c$。为了证明这一点，我们可以套用加号的定义：
> $$
> \begin{align*}
> 	a + 0 = b + 0 &\implies a = b  \\
> 	a + S(c) = b + S(c) &\implies S(a + c) = S(b + c) \\
> 	&\implies a + c = b + c \\
> 	&\implies \dots \\
> 	&\implies a + 0 = b + 0 \\
> 	&\implies a = b
> \end{align*}
> $$
> 注意到我们在证明中用到了 $S$ 是单射这个特性。
>
> 随后，再根据加法的交换律和结合律，不难得到我们想要的结论：自然数加法两端相同的任意项都可以自由消去。

凭借消去律，我们可以在每个同个等价集中选择某一项为零的作为代表（比如 $[(a + b, b)]_\mathcal{R} = [(a, 0)]_\mathcal{R}$），并定义整数集 $\mathbb{Z}$ 为 $\mathcal{R}$ 的等价集构成的集合 $\{[(0, 0)]_\mathcal{R}, [(0, 1)]_\mathcal{R}, [(1, 0)]_\mathcal{R}, \dots\}$。

> 记号：我们将 $[(n, 0)]_\mathcal{R}$ 简记为 $n$，而 $[(0, n)]_\mathcal{R}$ 简记为 $-n$；注意到 $[(0, 0)]_\mathcal{R}$ 可以同时被 $0$ 和 $-0$ 表示，我们使用前者。另外，记 $\mathbb{Z}^+ = \{[(n, 0)]_\mathcal{R} \mid n \in \mathbb{N}\backslash\{0\}\}$。

> 讨论：单纯从记号 $n$ 对应的构造来看，自然数的 $n$ 和整数的 $n$ 是完全不同的：前者是一个集合族，后者则是 $\mathbb{N}^2$ 上某个关系的等价类，更具体说则是 $(\mathbb{N}^2)^2$ 的一个子集。在需要区分两者时，我会将整数集的元素记为 $n_\mathcal{R}$ 和 $(-n)_\mathcal{R}$。不过，我们可以认为两者结构是完全相同的。只需要证明 $\mathbb{Z}$ 定义下的 $n = [(n, 0)]_\mathcal{R} \in \mathbb{Z}^+ \cup \{[(0, 0)]_\mathcal{R}\}$ 满足 $\mathbb{N}$ 定义下的所有性质即可：
>
> 首先定义单射 $S: (\mathbb{Z}^+\cup\{[(0, 0)]_\mathcal{R}\})^2 \to \mathbb{Z}^+$：
> $$
> \begin{equation}
> 	S([(n, 0)]_\mathcal{R}) = [(S(n), 0)]_\mathcal{R}
> \end{equation}
> $$
> 随后，定义 $\mathbb{Z}^+\cup\{[(0, 0)]_\mathcal{R}\}$ 上的函数 $+$：
>
> - $[(x, 0)]_\mathcal{R} + [(0, 0)]_\mathcal{R} = [(x, 0)]_\mathcal{R}$。
> - $[(x, 0)]_\mathcal{R} + [(1, 0)]_\mathcal{R} = S([(x, 0)]_\mathcal{R})$。
> - $[(x, 0)]_\mathcal{R} + S([(n, 0)]_\mathcal{R}) = S([(x, 0)]_\mathcal{R} + [(n, 0)]_\mathcal{R})$。
>
> ~~上面的括号真的多到头晕，不过为了不让符号太过随意我没有简化。~~不难验证上面的定义满足我们对自然数的理解。此外，对于 $\mathbb{Z}\backslash\mathbb{Z}^+$，我们也可以定义得到和 $\mathbb{N}$ 完全相同的性质。

在整数集上，我们可以定义满足交换律和结合律的加法：

- $a_\mathcal{R} + b_\mathcal{R} = (a+b)_\mathcal{R}$。
- $a_\mathcal{R} + (-b)_\mathcal{R} = [(a, b)]_\mathcal{R}$。
- $(-a)_\mathcal{R} + b_\mathcal{R} = [(b, a)]_\mathcal{R}$。
- $(-a)_\mathcal{R} + (-b)_\mathcal{R} = (-(a+b))_\mathcal{R}$。

减法则不满足交换律和结合律：

- $x_\mathcal{R} - y_\mathcal{R} = x_\mathcal{R} + (-y)_\mathcal{R}$。

乘法是满足交换律和结合律的运算符：

- $x\times 0 = 0$。
- $x\times S(y) = x\times y + x$。
- $x\times (-y) = -x\times y$。

> 举例：比如 $(-1)\times(-1)$：
> $$
> \begin{equation*}
> 	(-1)\times(-1) = -(-1)\times 1 = -1\times (-1) = 1\times 1 = 1\times S(0) = 1\times 0 + 1 = 0 + 1 = S(0) = 1
> \end{equation*}
> $$
> 这个我们预想中的结果一致。

构建 $\mathbb{Q}$ 的方式和构建 $\mathbb{Z}$ 的过程类似。考虑整数集形成的有序对集 $\mathbb{Z}^2$，定义上面的二元关系 $\mathcal{R}$ 为：
$$
\begin{equation}
	((x, y), (z, w)) \in \mathcal{R} \is x \times w = y \times z
\end{equation}
$$
类似在 $\mathbb{N}^2$ 上定义的二元关系，这里也是一个等价关系。因此我们可以定义 $\mathbb{Q}\backslash\{0\}$ 为 $\mathbb{Z}\backslash\{0\}$ 经过 $\mathcal{R}$ 的分划：
$$
\begin{equation*}
	\mathbb{Q}\backslash\{0\} = \{[(p, q)]_\mathcal{R} \mid p, q \in \mathbb{Z}\backslash\{0\} \}
\end{equation*}
$$
这里，我们没法像之前那样用简单的方式表示等价类：虽然乘法也有消去律，但对于 $[(p, q)]_\mathcal{R}$，其是否能在两侧消去一个“项”需要做出的判断要更加复杂，这里需要铺垫一些数论的概念。如果对于 $x \in \mathbb{Z}$，若存在 $m, n \in \mathbb{Z}$ 满足 $x = m\times n$，则称 $m$ 和 $n$ 是 $x$ 的 **因数（Factor）**。如果某个正整数的因数只有 $1$ 和它本身，则称它是一个 **素数（Prime）**。两个整数 $a, b$ 共同的因数称为 **公因数（Common Factor）**，其中最大的公因数称为 **最大公因数（Greatest Common Factor, gcd）**，记为 $\gcd(a, b)$。若 $\gcd(a, b) = 1$，则称两个整数 **互素（Co-prime）**。

实际上，有理数集 $\mathbb{Q}$ 是由所有互素的整数对为代表的等价集，以及特别的符号 $0$ 组成的：
$$
\begin{equation}
	\mathbb{Q} = \{[(p, q)]_\mathcal{R} \mid \gcd(p, q) = 1\} \cup \{0\}
\end{equation}
$$

> 记号：对于 $[(a, b)]_\mathcal{R}$，我们将其简记为 $\frac{a}{b}$。注意这里并没有要求 $\gcd(a, b) = 1$。此后我们会统一使用 $\frac{a}{b}$ 而非繁琐的 $[(a, b)]_\mathcal{R}$。

有理数集上的加法和乘法可以如下定义：

- $\frac{a}{b} + \frac{c}{d} = \frac{a\times d + b\times c}{b\times d}$。
- $\frac{a}{b} \times \frac{c}{d} = \frac{a\times c}{b\times d}$。

相对应地，可以定义减法和除法如下：

- $\frac{a}{b} - \frac{c}{d} = \frac{a\times d - b\times c}{b\times d}$。
- $\frac{a}{b}/\frac{c}{d} = \frac{a\times d}{b\times c}$，其中 $c \ne 0$。

因此，有理数集是“第一个”定义了四则运算的数集；历史上，一度认为有理数可以填充整个数轴，不过考虑下面这个集合：
$$
\begin{equation*}
	A = \{ x \mid x^2 < 2 \}
\end{equation*}
$$
显然这个集合有一个上确界，即某个平方等于 $2$ 的数；但它可能是有理数吗？假设 $(\frac{p}{q})^2 = 2$，其中 $\gcd(p, q) = 1$。那么显然有 $p^2 = 2q^2$。因此 $2$ 一定是 $p^2$ 的因数。由于奇数的平方一定是奇数，因此 $p$ 一定是偶数，假设 $p = 2r$，便有 $4r^2 = 2q^2 \implies 2r^2 = q^2$。同样的理由我们得到 $q$ 也是偶数。这样就和此前 $\gcd(p, q) = 1$ 的假设相悖了。所以我们遇到了一个很合理的数，但它不在现有的数集 $\mathbb{Q}$ 中。我们可以用数集的 **完备性（Completeness）** 来描述弥补这一缺失的性质：如果集合的任何有上界非空子集都有上确界，那么就称该集合具有完备性。下面我们将尝试从 $\mathbb{Q}$ 出发，构建出具有完备性的数集。

定义 **Dedekind 分割（Dedekind Cut）** 为 $\mathbb{Q}$ 的子集 $A$，其满足：

- $A \ne \emptyset$ 且 $A \ne \mathbb{Q}$。
- 如果 $x \in A$，那么对任意 $y < x$ 都有 $y \in A$。
- 对任意 $x \in A$ 都存在 $y > x$ 且 $y \in A$，即 $A$ 中不存在最大元素。

如果用比较通俗的方法描述，Dedekind 分割是一个左至无限右开的有理数区间。显然我们可以定义一个分割集上的偏序关系：$A < B$ 当且仅当 $A \subseteq B$。实数集 $\mathbb{R}$ 定义为所有有理数 Dedekind 分割的集合，即：
$$
\begin{equation}
	\mathbb{R} = \{A \subset Q \mid \text{$A$ 是一个分割} \}
\end{equation}
$$

下面让我们证明实数集 $\mathbb{R}$ 的完备性：考虑某个分割的并集 $\lambda = \bigcup_i S_i$ 有上界 $u$，则它实际上也是一个分割：

- 由于 $S_i$ 都是分割，它们不会是空集，因此 $\lambda \ne \emptyset$。同时由于上界 $u$ 的存在，$\lambda \ne \mathbb{Q}$。
- 对于 $x < y \in \lambda$，一定存在某个 $S_i$ 有 $y \in S_i$，根据分割的特性有 $x \in S_i$，因此 $p \in \lambda$。
- 对于 $x \in \lambda$，一定存在某个 $S_i$ 根据分割的特性有 $y > x$ 且 $y \in S_i$，因此 $y \in \lambda$。

注意到对于任意 $S_i$ 都有 $S_i \subseteq \lambda$，因此 $\lambda$ 是一个 $S$ 的上界。此外，对于任意其它的上界 $\mu \in \mathbb{R}$，则对于任意 $p \in \mathbb{Q}$，若 $p \in \lambda$，则一定有某个 $S_i$ 满足 $p \in S_i$，因此也有 $p \in \mu$。这样我们也就得到了 $\lambda \subseteq \mu$，即 $\lambda \le \mu$ 的结论，$\lambda$ 是 $S$ 的上确界。至此我们证明了 $\mathbb{R}$ 的完备性。

通过分割，我们也很方便定义 $\mathbb{R}$ 上的四则运算：

- $\lambda + \mu = \{x + y \mid x \in \lambda \land y \in \mu \}$。
- $\lambda\times\mu = \{x\times y \mid x \in \lambda \land y \in \mu\}$。
- $\lambda - \mu = \{x-y \mid x \in \lambda \land y \in \mu\}$。
- $\lambda/\mu = \{x/y \mid x \in \lambda \land y \in \mu\}$。

> 讨论：我们已经成功建立了所有的数集（至于复数集 $\mathbb{C}$，它只是建立在 $\mathbb{R}$ 上的一个代数结构）。只不过，这样搭建的结果会导致意想不到的状况发生。以实数 $1_\mathbb{R}$ 为例，它的完整定义是：
>
> - $1_\mathbb{R} = \{x \in \mathbb{Q} \mid x < 1_\mathbb{R}\}$。
> - 对于每个 $x \in 1$，它或者是 $x = [(p, q)]_{\mathcal{R}_\times}$，或者是 $0_{\mathcal{R}_\times}$。这里我用 $\mathcal{R}_\times$ 表示此前在 $\mathbb{Z}$ 上建立的二元关系。
> - 对于每个 $[(p, q)]_{\mathcal{R}_\times}$ 中的 $p, q \in \mathbb{Z}$，它或者是 $[(m, 0)]_{\mathcal{R}_+}$，或者是 $[(0, n)]_{\mathcal{R}_+}$。这里我用 $\mathcal{R}_+$ 表示此前在 $\mathbb{N}$ 上建立的二元关系。
> - 对于每个 $m, n \in \mathbb{N}$，它一定是我们此前定义的符号 $0, 1, 2, \dots$ 中的一个，即 $\emptyset, \{\emptyset\}, \{\emptyset, \{\emptyset\}\}, \dots$。
>
> 值得说明的是，上面的符号 $0$、$[(0_\mathbb{N}, 0_\mathbb{N})]_{\mathcal{R}_+}$ 、$0_{\mathcal{R}_\times}$ 和 $0_\mathbb{R}$ 表示的都是零，但它们显然有不同的定义（因为他们属于不同数集）；类似地，$1$、 $[1_\mathbb{N}, 0_\mathbb{N}]_{\mathcal{R}_+}$、$[(1_{\mathcal{R}_+}, 1_{\mathcal{R}_+})]_{\mathcal{R}_\times}$，还有 $1_\mathbb{R}$ 表示的都是一，但却有不同的定义。因此严格来讲，$\mathbb{N} \subseteq \mathbb{Z} \subseteq \mathbb{Q} \subseteq \mathbb{R}$ 是错误的；然而它们在性质上确实有某种程度的“包含”关系（比如将 $\mathbb{R}$ 中的某个子集拿出来，其四则运算的性质与 $\mathbb{Q}$ 中的完全相同），我们会在后面用新的语言描述这种关系。



### 集合的大小与势

#### 可数集与可数选择公理

本节让我们介绍集合的一些重要性质，在有了函数的定义之后谈论这些会方便许多。首先，集合的 **大小（Size）** 是一个非常显然的性质，它表示集合中元素的个数。比如 $\{a, b, c\}$ 的大小显然是 $3$，而 $\mathbb{N}$ 的大小是无穷大。不过，同样大小为无穷大的集合，如 $\mathbb{Z}$ 和 $\mathbb{R}$ 之间，是否存在大小的相对关系呢？

> 记号：集合 $A$ 的大小记为 $|A|$。

> 讨论：涉及到无穷的话题很容易就出现奇异的结论。比如集合 $\mathbb{N}$ 和 $2\mathbb{N} = \{2n \mid n \in \mathbb{N}\}$，虽然看起来 $\mathbb{N}$ 的元素个数更多（因为它更密），但我们总可以让任意 $n \in \mathbb{N}$ 对应 $2n \in 2\mathbb{N}$；从这个映射的角度来看两者的元素个数是相同的。

> 举例：有限集 $A$ 的幂集 $2^A$ 满足 $|2^A| = 2^{|A|}$，这也是记号 $2^A$ 的来源。这个命题的证明非常简单，只需要考虑双射 $f: 2^A \to \mathbb{B}^{|A|}$，令：
> $$
> \begin{equation*}
> 	S \mapsto b_1\dots b_{|A|}, \qquad b_i = \begin{cases} 1 & A_i \in S \\ 0 & A_i \notin S \end{cases} \qquad i \in 1,\dots,|A|
> \end{equation*}
> $$
> 显然 $|\mathbb{B}^{|A|}| = 2^{|A|}$，因此我们也就得到了上面的结论。

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

**可数选择公理（Axiom of Countable Choice, $\text{AC}_\omega$)**：对于任意非空集合族 $\mathcal{A} = \{ A_i \mid i \in \mathbb{N} \}$，都存在一个函数 $f: \mathbb{N} \to \mathcal{A}$ 满足 $f(i) \in A_i$。这个公理其实是描述了可数个集合拥有的性质。

可数集的子集总是可数的（显然）；可数个可数集的并也是可数的，即对于可数集 $A_1, A_2, \dots$，下面的集合也是可数的。
$$
\begin{equation*}
	\mathcal{A} = \bigcup_{i=1}^\infty A_i
\end{equation*}
$$
让我们用上面的公理证明这个命题：可以找到满射 $m_i: \mathbb{N} \to \bigcup\mathcal{A}$，使得 $m_i(j) = a_{ij} \in A_i$。注意到 $m_i$ 本身也可以看作 $m: \mathbb{N} \to \mathcal{M}$，其中 $\mathcal{M} = \{m_i \mid i \in \mathbb{N}\}$。 这样，我们就可以将 $\bigcup\mathcal{A}$ 中所有元素表示成 $m(i, j)$ 的形式，这和此前的 $\mathbb{N}^2$ 是一样的。

#### 不可数集与集合势

与可数集相反地，所有其它的集合称为 **不可数的（Uncountable）**。一个典型的例子是 $\mathbb{B}^\omega$，其中 $\mathbb{B} = \{0, 1\}$。下面是一个经典的证明：

> 证明：假设 $\mathbb{B}^\omega$ 是可数的，则可以找到 $s_1, s_2, s_3, \dots \in \mathbb{B}^\omega$。定义 $c: \mathbb{B}^\omega\times\mathbb{N}\to \mathbb{B}$ 为查询字符串特定位置的索引函数（比如 $c(1011\dots, 1) = 1$）。因此，$s_i$ 也可以表示成：
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

从前面的内容可以看到，无限集之间也存在“大小”的差异，这样就引出了“势”的概念。两个集合定义为 **等势的（Equinumerous）**，若能在它们之间建立一个双射。显然，有限集之间等势当且仅当两者大小相同；可数的无限集之间是等势的。记为 $A \approx B$。

**等势（Equinumerosity）** 关系是一个等价关系，我们会在下一节中证明这一点。既然能定义和集合势相关的等价关系，那么自然也有序关系：如果存在单射 $f: A \to B$，则称 $A$ 不大于 $B$，记为 $A \preceq B$；如果 $A$ 不大于 $B$ 且两者不等势，则称 $A$ 小于 $B$，记为 $A \prec B$。

集合论中用 **势（Cardinality）** 来表示集合势的大小；为了不和集合大小 $|\cdot|$ 搞混，让我们用 $\operatorname{card}A$ 来表示集合 $A$ 的势。根据我们此前的定义有 $\operatorname{card}{A} \le \operatorname{card}{B} \Leftrightarrow A \preceq B$。对于有限集，令 $\operatorname{card}{A} = |A|$。对于无限集，显然 $\operatorname{card}{\mathbb{N}}$ 是一个很重要的势，我们用记号 $\aleph_0$ 来表示它。根据此前的例子，我们也知道 $\operatorname{card}{\mathbb{R}} \ne \aleph_0$，我们将其记为 $\aleph_1$。那么如何找到其它可能的势呢？让我们先了解一个关键的定理。

**康托定理（Cantor Theorem）**：任意集合 $A$ 都满足 $A \prec \mathbb{P}(A)$。

> 证明：$f: a \mapsto \{a\}$ 显然是一个单射，因此 $A \preceq \mathbb{P}(A)$，所以我们只需证明 $A \not\approx \mathbb{P}(A)$ 即可。即：不存在满射 $f: A \to \mathbb{P}(A)$。定义 $\bar{A} = \{ x \in A \mid x \notin f(x) \}$。这个集合显然是一个 $A$ 的子集；无论它是什么样的集合，它都不可能出现在 $\operatorname{Im}{f}$ 中，即不存在 $x \in A$ 令 $f(x) = \bar{A}$。对任意 $x \in A$：
>
> - 假设 $x \in f(x)$，显然此时 $x \notin \bar{A}$。若令 $f(x) = \bar{A}$，则有 $x \notin f(x)$，矛盾。
> - 假设 $x \notin f(x)$，显然此时 $x \in \bar{A}$。若令 $f(x) = \bar{A}$，则有 $x \in f(x)$，矛盾。
>
> 因此不存在这样的单射 $f$，而 $A \prec \mathbb{P}(A)$。

因此，从满足 $\operatorname{card}(A) = \aleph_1$ 的集合 $A$ 出发，不停地取当前集合的幂集，我们会得到越来越大的集合。

> 讨论：有一个非常吸引人的猜测：究竟存不存在某个可能的集合势 $\gimel$ 使得 $\aleph_0 < \gimel < \aleph_1$？如果记 $\beth_0 = \aleph_0$，$\beth_{i+1} = 2^{\beth_i}$，那么是否有 $\beth_1 = 2^{\beth_0}$？更一般化地，对于任意的 $\beth_i$，是否存在 $\gimel$ 令 $\beth_i < \gimel < \beth_{i+1}$？认为不存在这样的集合势的假说被称为 **连续统假设（Continuum Hypothesis）**。狭义的连续统假设相当于说明 $\aleph_1 = \beth_1$。

#### 基数

集合论中，有一套代数体系来研究集合的势，那就是 **基数（Cardinal Number）**。基数可以通过等势关系定义：基数是等势关系的等价类。

> 记号：我们用斜体的小写希腊字母表示基数，如 $\alpha$、$\beta$ 等。为了方便地表示某个集合的基数，让我们记集合 $A$ 的基数为 $\mathscr{c}(A)$。

有两个重要的基数，$\aleph_0 = \mathscr{c}(\mathbb{N})$ 和 $\mathfrak{c} = \mathscr{c}(\mathbb{R})$。

基数中可以定义全序关系 $\le$ 为 $\mathscr{c}(A) \le \mathscr{c}(B) \Leftrightarrow A \preceq B$。

可以在基数集上定义算术运算。定义加法 $+$ 为 $\mathscr{c}(A) + \mathscr{c}(B) = \mathscr{c}(A\cup B)$，其中 $A\cap B = \emptyset$。基数加法有下面的性质：

* $\alpha + \beta = \beta + \alpha$。
* 若 $\alpha \le \beta$ 且 $\beta \ge \aleph_0$，则 $\alpha + \beta = \beta$。

基数的乘法 $\times$ 定义为 $\mathscr{c}(A) \times \mathscr{c}(B) = \mathscr{c}(A\times B)$。它有下面的性质：

* $\alpha\times\beta = \beta\times\alpha$。
* 若 $\alpha\le\beta$、$\alpha\ne 0$ 且 $\beta \ge \aleph_0$，则 $\alpha\times\beta = \beta$。

基数的幂 $\cdot^\cdot$ 定义为 $\mathscr{c}(A)^{\mathscr{c}(B)} = \mathscr{c}(\{f: A \to B\})$。特别地，对于 $A$ 的幂集 $2^A$，其基数恰好是 $\mathscr{c}(2^A) = 2^{\mathscr{c}(A)}$.



## 一阶逻辑：语义

### $\sigma$-结构

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
	\sigma = (\mathfrak{C}, \mathfrak{F}, \mathfrak{R}, \mathfrak{a})
\end{equation}
$$
这里的 $\mathfrak{C}$、$\mathfrak{F}$ 与 $\mathfrak{R}$ 与对应的 $\mathcal{C}$、$\mathcal{F}$、$\mathcal{R}$ 区别在于它们包含的都是符号的名字，而非常数、函数和关系本身。最后一个成员 $\mathfrak{a}: \mathfrak{F}\cup\mathfrak{R}\to\mathbb{N}$ 称为 **元数函数（Arity Function）**：对于任意符号 $s \in \mathfrak{F}$，如果它对应的函数需要 $n$ 个参数，或 $s \in \mathfrak{R}$ 且需要 $n$ 个元素参与，都有 $s\mapsto n$。同时，为了方便描述签名 $\sigma = (\mathfrak{C}, \mathfrak{F}, \mathfrak{R}, \mathfrak{a})$ 的元素，定义 $\mathscr{C}(\sigma) \equiv \mathfrak{C}$，$\mathscr{F}(\sigma) \equiv \mathfrak{F}$，$\mathscr{R}(\sigma) \equiv \mathfrak{R}$。

> 举例：
>
> - 图的签名是 $\sigma_\text{graph} = (\emptyset, \emptyset, \{E\}, (E \mapsto 2))$，其中 $E$ 表示图中的边。
> - 幺半群的签名是 $\sigma_\text{monoid} = (\{e\}, \{\circ, \cdot^{-1}\}, \emptyset, (\circ \mapsto 2, \cdot^{-1}\mapsto 1))$，其中 $e$ 是幺半群的单位元。
> - 集合的签名是 $\sigma_\text{set} = (\emptyset, \emptyset, \{\in\}, (\in \mapsto 2))$。

> 习惯：显然上面这种表示方式非常啰嗦，因此我会省略所有的空集，省略元数函数，并将所有集合都展开。比如上面集合的签名可以简写成 $\sigma_\text{set} = (\in)$。

> 讨论：其实，我们可以将签名的定义缩减为一个二元组 $\sigma = (\mathfrak{R}, \mathscr{a})$。首先函数本身就是一个二元关系；同时，常数完全可以看作一个一元关系。不过由于常数、函数和关系的用法非常不同，我还是将它们分开对待。

定义 **$\sigma$-结构** 为一个二元组 $\mathbf{S} = (S, \iota)$，其中 $S$ 称为 **全集（Universe）**，而 $\iota$ 是一个定义在 $\mathscr{C}(\sigma)$、$\mathscr{F}(\sigma)$ 和 $\mathscr{R}(\sigma)$ 上的函数，用来得到符号所指向的实际对象：

- $\iota: \mathfrak{C} \to S$，从常数集中得到一个常数。
- $\iota: \mathfrak{F} \to (S^{\mathscr{a}(\mathscr{f} \in \mathfrak{F})} \to S)$：从函数集中得到一个函数。
- $\iota: \mathfrak{R} \to \mathscr{r} \subseteq S^{\mathfrak{a}(\mathscr{r} \in \mathfrak{R})}$：从关系集中得到一个关系。

> 记号：在明确了特定的 $\sigma$-结构后，我们用 $x^\mathbf{S}$ 来代替 $\iota(\mathscr{x})$，其中 $\mathscr{x}$ 是某个常数、函数或关系的符号。此外，由于我们几乎总是用同样的符号命名一个 $\sigma$-结构和它的全集，仅仅将前者用粗体加以区分，因此对于已经给定的 $\sigma$-结构 $\mathbf{S}$，我们约定俗成地直接将其全集记作 $S$。

现在，一个 $\sigma$-结构可以写成如下的形式了：
$$
\begin{equation}
	\mathbf{S} = (S, \{\mathscr{c}^\mathbf{S}\}_{\mathscr{c}\in\mathfrak{C}}, \{\mathscr{r}^\mathbf{S}\}_{\mathscr{r} \in \mathfrak{R}}, \{\mathscr{f}^\mathbf{S}\}_{\mathscr{f} \in \mathfrak{F}})
\end{equation}
$$

> 举例：整数加法群的 $\sigma_\text{group}$-结构是 $\mathbf{Z} = (\mathbb{Z}, 0^\mathbf{Z}, +^\mathbf{Z}, (-\cdot)^\mathbf{Z})$，其中 $0^\mathbf{Z}$ 就是 $0 \in \mathbb{Z}$，$+: (a, b) \mapsto a + b$，$-: a \mapsto -a$。

> 讨论：对于特定的 $\sigma$-结构，其成员定义不一定遵循习惯中的行为。比如我们可以参考群的签名 $\sigma_\text{group}$ 来定义群 $\mathbf{Z}$，但其中的 $+$ 定义为 $(a, b) \mapsto \sin(a+b)$，此时它虽然依然拥有群的签名，但其结构性质与群相差甚远。

> 习惯：对于行为和传统定义一致的常数、函数或关系，我们会省略上标。

####  子结构

对于两个 $\sigma$-结构 $\mathbf{A}$ 和 $\mathbf{B}$，如果两者满足下面的性质：

- $A \subseteq B$。回忆 $A$ 和 $B$ 分别是 $\mathbf{A}$ 和 $\mathbf{B}$ 的全集。
- 对任意 $\text{c}\in \mathscr{C}(\sigma)$ 都有 $\text{c}^\mathbf{A} = \text{c}^\mathbf{B}$。这意味着两个结构中的常数完全相同。
- 对任意 $f \in \mathscr{F}(\sigma)$ 都有 $f^\mathbf{A} = f^\mathbf{B}|_{A^{\mathfrak{a}(f)}}$。即，$\mathbf{A}$ 中的函数相当于 $\mathbf{B}$ 中对应函数在 $A^{\mathfrak{a}(f)}$ 上的限制。
- 对任意 $\mathcal{R} \in \mathscr{R}(\sigma)$ 都有 $\mathcal{R}^\mathbf{A} = \mathcal{R}^\mathbf{B} \cap A^{\mathfrak{a}(\mathscr{\mathcal{R}})}$。

此时称 $\mathbf{A}$ 是 $\mathbf{B}$ 的一个 **子结构（Substructure）**，记为 $\mathbf{A} \subseteq \mathbf{B}$。

> 举例：拥有环的签名 $\sigma_\text{ring}$ 的结构 $\mathbf{A} = (\{x \in \mathbb{Z}, x^2 \le 100\}, 0, 1, +, -, \cdot)$ 是 $\mathbf{B} = (\mathbb{R}, 0, 1, +, -, \cdot)$ 的子结构。注意 $\mathbf{B}$ 是一个环但 $\mathbf{A}$ 并不是。

如果一个结构中只有关系符号，那么显然它全集的任一个子集都是其某个字结构的全集。比如图 $(V, E)$ 中，显然对任意 $S\subseteq V$，我们都不难证明 $(S, E\cap S^2)$ 是一个子结构（同时也是一个子图）。对于子结构 $\mathbf{A} \subseteq \mathbf{B}$，如果任意 $f^\mathbf{B} \in \mathscr{F}(\sigma)$ 都满足 $f^\mathbf{B}(A^{\mathfrak{a}(f^\mathbf{B})}) \subseteq A$，则称它在 $\mathbf{B}$ 的函数下 **封闭（Closed）**。

如果 $A$ 是 $\mathbf{B}$ 全集的子集，那么以 $A$ 为全集的 $\mathbf{B}$ 的子结构最多只有一个，即当 $\mathbf{A}$ 包含 $\mathbf{B}$ 中所有常数且在 $\mathbf{B}$ 的函数下封闭时。

某个子结构 $\mathbf{B}$ 的任意多个子结构的交同样是 $\mathbf{B}$ 的子结构。这可以通过上面的定理立刻推出。

对于结构 $\mathbf{B}$ 的全集的某个子集 $S$，定义其 **生成的子结构（Generated Substructure）** 为最小的在全集中包含 $S$ 的子结构 $\mathbf{A}$，记这个子结构为 $\langle S \rangle_\mathbf{B}$。我们可以用归纳的方式定义它的全集：

- $S_0 = S\cup\{\text{c}^\mathbf{B} \mid \text{c} \in \mathscr{C}(\sigma)\}$。这是一切的基础：$S$ 中的元素以及（作为子结构必须的）常数。
- $S_{n+1} = S_n \cup \bigcup_{f \in \mathscr{F}(\sigma)} f^\mathbf{B}(S_n^{\mathfrak{a}(f)})$。可以选择利用函数迭代一次。
- $S_* \equiv \bigcup_{n = 0}S_n$。生成的子结构是每一个步骤的并。

从 $S$ 生成的子结构，其集合势不大于 $S$ 的集合势和 $\aleph_0$ 中的最大值。这实际上是显然的（因为 $S_*$ 是一系列势与 $S$ 相同集合的可数并）。严格来说，其实还需要考虑 $\mathscr{C}(\sigma)$ 的集合势，不过这里默认它是有限集。

> 举例：$\emptyset$ 生成的 $\mathbf{R} = (\mathbb{R}, 0, 1, +, \times)$ 的子结构是 $(\mathbb{N}, 0, 1, +, \times)$。

除了上面定义的子结构外，还有一种奇特的情形，那就是从结构的签名中取“子集”。比如 $\mathbf{A}$ 的签名 $\sigma_\mathbf{A}$ 是 $\mathbf{B}$ 的签名 $\sigma_\mathbf{B}$ 中部分成员组成的，此时称 $\mathbf{A}$ 是 $\mathbf{B}$ 的一个 **约减（Reduct）**，或 $\mathbf{B}$ 是 $\mathbf{A}$ 的一个 **扩充（Expansion）**，记为 $\mathbf{A} = \mathbf{B}|_{\sigma_{\mathbf{A}}}$。

> 举例：$(\mathbb{R}, 0, +)$ 是 $(\mathbb{R}, 0, 1, +, \times)$ 的一个约减。

#### 同态与嵌入

对于 $\sigma$-结构 $\mathbf{A}$ 和 $\mathbf{B}$，定义 **$\sigma$-同态（$\sigma$-Homomorphism）** 为函数 $h: A \to B$，其满足：

- 对任意 $\text{c} \in \mathscr{C}(\sigma)$ 都有 $h(\text{c}^\mathbf{A}) = \text{c}^\mathbf{B}$。
- 对任意 $f \in \mathscr{F}(\sigma)$ 和 $\mathbf{a} \in A^{\mathfrak{a}(f)}$ 都有 $h(f^\mathbf{A}(\mathbf{a})) = f^\mathbf{B}(h(\mathbf{a}))$。
- 对任意 $\mathcal{R} \in \mathscr{R}(\sigma)$ 和 $\mathbf{a} \in A^{\mathfrak{a}(\mathcal{R})}$ 都有 $\mathcal{R}^\mathbf{A}(\mathbf{a}) \implies \mathcal{R}^\mathbf{B}(h(\mathbf{a}))$。

如果一个 $\sigma$-同态是一个双射，则称其为 **$\sigma$-同构（$\sigma$-Isomorphism）**，记作 $\mathbf{A}\cong \mathbf{B}$。注意到此时上面的第三条就是 $\Leftrightarrow$ 而非 $\Rightarrow$ 了。

对于任意 $\sigma$-同态 $h: A \to B$，$h(A)$ 总是 $\mathbf{B}$ 子结构 $\mathbf{B}'$ 的全集。如果 $h$ 在 $\mathbf{B}'$ 上是一个同构，则称其是一个 **$\sigma$-嵌入（$\sigma$-Embedding）**。

> 记号：之后我们会将 $\sigma$-同态、$\sigma$-同构和 $\sigma$-嵌入简写成 $f: \mathbf{A}\to\mathbf{B}$、$f: \mathbf{A}\xrightarrow{\sim}\mathbf{B}$ 和 $f: \mathbf{A}\hookrightarrow \mathbf{B}$。



### 一阶逻辑语言

#### 语言和解释

有了 $\sigma$-结构的铺垫，现在我们可以正式地定义 FOL 语言的构成了。下面我们将默认记 $\sigma$ 为 FOL 的 $\sigma$-结构。

FOL 的字母表 $\Sigma_\sigma$ 是所有在 $\sigma$ 中出现的符号（常数、函数和关系的符号）以及下面列出的符号：

- 逻辑符号 $\deq$、$\lnot$、$\land$、$\lor$、$\to$、$\forall$、$\exists$。
- 标点符号 $($ 和 $)$ 和 $.$。
- 变量符号 $\mathscr{v}_0, \mathscr{v}_1, \dots$。

> 讨论：我们使用 $\deq$ 表示 FOL 中的同一式，这是为了和元语言中的“相等”符号 $=$ 混淆。举例来说，$\mathscr{p} \deq \mathscr{q}$ 是一个 FOL 的式子，而 $\mathscr{p} = \mathscr{q}$ 说明了 $\mathscr{p}$ 和 $\mathscr{q}$ 完全相同。

定义 **$\sigma$-项（$\sigma$-Term）** 为下面的单词：

- $\text{c} \in \mathscr{C}(\sigma)$，即常数都是 $\sigma$-项。
- $\mathscr{v}_0, \mathscr{v}_1, \dots$，即变量符号都是 $\sigma$-项。
- $f(\mathscr{t}_1, \dots, \mathscr{t}_n)$，其中 $f \in \mathscr{F}(\sigma)$ 且 $\mathfrak{a}(f) = n$，$\mathscr{t}_1, \dots, \mathscr{t}_n$ 是 $\sigma$-项。

我们用 $\mathscr{T}(\sigma)$ 来表示 $\sigma$-项组成的集合，$\mathfrak{V}$ 是所有变量的符号组成的集合。

> 举例：设 $x, y, z \in \mathfrak{V}$。则 $x + y\times z \in \mathscr{T}(\sigma_\text{ring})$。注意到这里的 $+$ 和 $\times$ 都是 $\mathscr{F}(\sigma_\text{ring})$ 的元素。

> 记号：一些比较微妙的符号，比如幂运算 $\cdot^\cdot$，严格地来说应该显式出现在 $\sigma$-项当中，比如 $x^y$ 应该写成 $x\cdot^\cdot y$。此外，由于运算符并不在 FOL 的描述范围内，一些习惯中会将所有的运算符写成前缀或后缀的形式，或函数调用，比如 $1+2\times 3^4$ 的正式写法是：
> $$
> \begin{equation*}
> 	+(1, \times(2, \cdot^\cdot(3, 4)))
> \end{equation*}
> $$
> 不过，它的可读性非常有限，因此我们还是采用一直以来用的格式。此外，对于多次连续调用的函数 $f \in \mathscr{F}(\sigma)$，我们还是沿用幂的缩写形式，即：
> $$
> \begin{equation}
> 	f^n(\mathscr{t}) \equiv \underset{\text{$n$ 次嵌套}}{f(\dots f(\mathscr{t})\dots)}
> \end{equation}
> $$
> 最后，对于 $\sigma$ 中的变量，我一般会用结尾的几个小写拉丁字母，如 $x, y, z$ 表示。

定义 **$\sigma$-公式（$\sigma$-Formula）** 为满足下面规则的式子：

1. $\mathscr{s} \deq \mathscr{t}$，其中 $\mathscr{s}$ 和 $\mathscr{t}$ 是项。
2. $\mathcal{R}(\mathscr{t}_1, \dots, \mathscr{t}_n)$，其中 $\mathcal{R} \in \mathscr{R}_n(\sigma)$，且 $\mathscr{t}_1, \dots, \mathscr{t}_n$ 是项。
3. $\lnot(\psi)$、$(\psi)\land(\phi)$、$(\psi)\lor(\phi)$、$(\psi)\to(\phi)$，其中 $\psi$ 和 $\phi$ 是公式。
4. $\forall \mathscr{v}. \psi$ 和 $\exists \mathscr{v}. \phi$，其中 $\mathscr{v}$ 是变量，$\psi$ 和 $\phi$ 是公式。

这里面的 1 和 2 也被称为 **原子式（Atomic Formula）**，而 4 之外的合称为 **无量词式（Quantifier Free Formula）**。

> 记号：量词式中的标点符号 $.$ 其实并不是必须的，因为根据 3 的括号规则，任何公式都不可能存在歧义。这里为了美观会加上 $.$。

> 举例：下面列出的式子都是 $\sigma$-公式：
>
> * $(\lnot(x))\land(y)$。
> * $\forall x. (x\dot{=}y)\lor(\mathcal{R}(x, y))$。
>
> 下面列出的则不是（虽然可能合理）：
>
> * $x\land y$。
> * $\lnot x\land y\deq z$。

注意到虽然原子式中出现的关系和函数都有相应的元数，但我们完全可以将其声明为更多元数的函数：
$$
\begin{equation*}
	f'(u_1, \dots, u_m) = f(v_1, \dots, v_n), \qquad \{v_i\} \subseteq \{u_i\}
\end{equation*}
$$
一种常用的扩展是将其变成接受所有变量空间中的变量。这引出了扩展公式的概念。

如果 $\mathscr{t}$ 是一个项，设 $\mathbf{v} = (v_1, \dots, v_n)$ 是所有变量空间中变量组成的 $n$ 元组，称 $\mathscr{t}[\mathbf{v}]$ 为一个 **扩展 $\sigma$-项（Extended $\sigma$-Term）** 若 $\mathscr{t}$ 中出现了所有 $\mathbf{v}$ 中的变量。我们用 $\mathscr{T}_\text{ext}(\sigma)$ 来表示所有扩展 $\sigma$-项组成的集合。对于结构 $\mathbf{A}$，$\mathscr{t}[\mathbf{v}]$ 在其中的 **解释（Interpretation）** 为函数 $\mathscr{t}^\mathbf{A}[\mathbf{v}]: A^{|\mathbf{v}|}\to A$，其定义如下：

* 若 $\mathscr{t} = \text{c} \in \mathscr{C}(\sigma)$，则 $\mathscr{t}^\mathbf{A}[\mathbf{v}](\mathbf{a}) = \text{c}$。
* 若 $\mathscr{t} = \mathbf{v}_i \in \mathfrak{V}$，则 $\mathscr{t}^\mathbf{A}[\mathbf{v}](\mathbf{a}) = \mathbf{a}_i$。
* 若 $\mathscr{t} = f(\mathscr{t}_1, \dots, \mathscr{t}_n)$，其中 $f \in \mathscr{F}(\sigma)$ 而 $\mathscr{t}_1, \dots, \mathscr{t}_n$ 是 $\sigma$-项，则 $\mathscr{t}^\mathbf{A}[\mathbf{v}](\mathbf{a}) = f^\mathbf{A}(\mathscr{t}_1^\mathbf{A}[\mathbf{v}](\mathbf{a}), \dots, \mathscr{t}_n^\mathbf{A}[\mathbf{v}](\mathbf{a}))$。

> 举例：考虑 $\mathbf{A} \equiv (\mathbb{Z}, e^\mathbf{A}, \circ^\mathbf{A})$、$\mathbf{B} \equiv (\mathbb{Z}, e^\mathbf{B}, \circ^\mathbf{B})$，其中 $e^\mathbf{A} \equiv 0$ 而 $\circ^\mathbf{A} \equiv +$，而 $e^\mathbf{B} \equiv 1$ 而 $\circ^\mathbf{B} \equiv \times$。此时对于项 $t = (v_1\circ e)\circ v_2$，在 $\mathbf{A}$ 中的解释为 $(v_1, v_2) \mapsto(v_1 + 0) + v_2$，而在 $\mathbf{B}$ 中的解释为 $(v_1, v_2) \mapsto (v_1\times 1)\times v_2$。

扩展公式集的基数和公式集的基数相同，即 $\operatorname{card} \mathscr{T}_\text{ext}(\sigma) = \operatorname{card} \mathscr{T}(\sigma) = \max\{\aleph_0, \sigma\}$。

类似地，我们也可以将结构 $\mathbf{A}$ 上扩展公式的解释定义为关系 $\varphi^\mathbf{A}[\mathbf{v}] \subseteq A^{|\mathbf{v}|}$，其定义如下：

* 若 $\varphi = \mathscr{s} \deq \mathscr{t}$，则 $\varphi^\mathbf{A}[\mathbf{v}](\mathbf{a})$ 成立当且仅当 $\mathscr{s}^\mathbf{A}[\mathbf{v}](\mathbf{a})  = \mathscr{t}^\mathbf{A}[\mathbf{v}](\mathbf{a})$ 成立。
* 若 $\varphi = \mathcal{R}(\mathscr{t}_1, \dots, \mathscr{t}_n)$，则 $\interp{\varphi}{A}{v}(\mathbf{a})$ 成立当且仅当 $\mathcal{R}^\mathbf{A}(\interp{t_1}{A}{v}(\mathbf{a}), \dots, \interp{t_n}{A}{v}(\mathbf{a}))$ 成立。
* 若 $\varphi = \lnot\psi$，则 $\interp{\phi}{A}{v}(\mathbf{a})$ 成立当且仅当 $\interp{\psi}{A}{v}(\mathbf{a})$ 不成立。
* 若 $\varphi = \psi\land\theta$，则 $\interp{\phi}{A}{v}(\mathbf{a})$ 成立当且仅当 $\interp{\psi}{A}{v}(\mathbf{a})$ 和 $\interp{\theta}{A}{v}(\mathbf{a})$ 同时成立。
* 若 $\varphi = \psi\lor\theta$，则 $\interp{\phi}{A}{v}(\mathbf{a})$ 成立当且仅当 $\interp{\psi}{A}{v}(\mathbf{a})$ 或 $\interp{\theta}{A}{v}(\mathbf{a})$ 至少一个成立。
* 若 $\varphi = \forall x.\psi(\mathbf{v}, y)$，其中 $y$ 不是 $\mathbf{v}$ 的成员，且 $\psi[\mathbf{v}, u]$ 是一个扩展公式，那么 $\interp{\varphi}{A}{v}(\mathbf{a})$ 成立当且仅当对任意 $b \in A$，$\varphi[\mathbf{v}, y](\mathbf{a}, b)$ 都成立。
* 若 $\varphi = \exists x.\psi(\mathbf{v}, y)$，其中 $y$ 不是 $\mathbf{v}$ 的成员，且 $\psi[\mathbf{v}, u]$ 是一个扩展公式，那么 $\interp{\varphi}{A}{v}(\mathbf{a})$ 成立当且仅当存在 $b \in A$ 令 $\varphi[\mathbf{v}, y](\mathbf{a}, b)$ 成立。

如果 $\interp{\varphi}{A}{v}(\mathbf{a})$ 成立，我们称 $\mathbf{A}$ **满足（Satisfy）** $\interp{\varphi}{}{v}(\mathbf{a})$，记作 $\mathbf{A} \models \interp{\varphi}{}{v}(\mathbf{a})$。特别地，如果 $\mathbf{v} = \emptyset$，则 $\mathbf{A}\models \varphi$ 是一个零元关系，即常数。因此它或者为真，或者为假。这里我们称 $\varphi$ 在 $\mathbf{A}$ 中为真。

> 习惯：至此，我们已经熟悉了扩展项和扩展公式，下面我们将默认变量集 $\mathfrak{V}$ 是确定的，此时我们直接将扩展项写成 $\mathscr{t}(\mathbf{a})$，扩展公式写成 $\varphi(\mathbf{a})$。此外，我们不会再使用“拓展”这个词汇，除非出现歧义。

> 举例：
>
> * 考虑 $\sigma_\text{arith}$-公式 $\varphi \equiv S(S(0)) \deq v_0$，有 $\mathbf{N} \equiv (\mathbb{N}, 0, S, +, \times)\models \varphi[v_0](2)$。也可以简单写成 $\mathbf{N} \models S^2(0) \deq 2$。
> * $\mathbf{R} \models \exists y. (a\deq y\times y)$ 对任意非负 $a \in \mathbb{R}$ 都成立。

对于两个结构 $\mathbf{A}$ 和 $\mathbf{B}$，若 $h: \mathbf{A}\to\mathbf{B}$ 是一个 **同态（Homomorphism）**，则对于任意项 $\mathscr{t}(\mathbf{v})$ 和 $\mathbf{a} \in A^{|\mathbf{v}|}$，我们有：
$$
\begin{equation}
	h(\mathscr{t}^\mathbf{A}(\mathbf{a})) = \mathscr{t}^\mathbf{B}(h(\mathbf{a}))
\end{equation}
$$
以及：
$$
\begin{equation}
	\mathbf{A} \models \varphi(\mathbf{a}) \is \mathbf{B} \models \varphi(h(\mathbf{a}))
\end{equation}
$$
最后，一个结构满足某个公式当且仅当其约减满足这个公式，即：
$$
\begin{equation}
	\mathbf{A} \models \varphi(\mathbf{a}) \is \mathbf{A}|_{\sigma'} \models \varphi(\mathbf{a})
\end{equation}
$$

#### 可定义性

本节让我们从任意的结构出发，探索其能张成的整个空间。

若 $\mathbf{A}$ 是一个结构而 $P\subseteq A$，则称 $S \subseteq M^n$ 为 **$P$-可定义的（$P$-Definable）**，若存在某个公式 $\varphi(\mathbf{x}, \mathbf{y})$，其中 $|\mathbf{y}| = n$ 且存在 $\mathbf{p} \in P^{|\mathbf{x}|}$ 满足 $S = \{\mathbf{a} \in A^n \mid \mathbf{A} \models \varphi(\mathbf{p}, \mathbf{a}) \}$。特别地，如果 $P = \emptyset$，则称 $S$ 是 $0$-可定义的。

> 讨论：某种程度上，0-可定义的应该称为 $\emptyset$-可定义的，因为 $P$ 这里就是空集。

> 习惯：我们也会称某个元素 $\mathbf{a} \in A^n$ 是 $P$-可定义的，若其单元素集 $\{\mathbf{a}\}$ 是 $P$-可定义的。类似地，某个函数 $f: D^n \to A$ 是 $P$-可定义的，若其等价图 $\{(\mathbf{a}, b) \in D^n\times A \mid f(\mathbf{a}) = b\}$ 是 $P$-可定义的。

> 记号：我们将 $A^n$ 的所有 $P$-可定义集组成的族记为 $\mathcal{D}_n^\mathbf{A}(P)$；这个族的并记为 $\mathcal{D}^\mathbf{A}(P)$。

$\operatorname{card}\mathcal{D}^\mathbf{A}(P)$ 的基数不超过 $\max\{\aleph_0, \sigma, |P|\}$。 

> 举例：
>
> * $\mathbf{R} \equiv (\mathbb{R}, 0, 1, +, \times)$ 中，所有正数都是 $0$-可定义的，考虑 $\varphi_{>0}(x) \equiv \lnot(x\deq 0)\land \exists y. (x \deq y^2)$。类似地，我们也可以 $0$-定义所有的负数。
> * $(\mathbb{Q}, <)$ 中的正数是不可定义的。
> * $\mathbf{C} \equiv (\mathbb{C}, 0, 1, +, \times)$ 中，$\{\sqrt{2}, -\sqrt{2}\}$ 是 $0$-可定义的，考虑 $\varphi(z) \equiv z^2 \deq 2$，其中 $2 \equiv 1 + 1$。
> * $\mathbf{N} \equiv (\mathbb{N}, 0, S, +, \times)$ 可以定义的所有集合称为 **算术的（Arithmetical）**。
> * 图 $\mathbf{G} \equiv (V, E)$ 中，所有距离不超过 $2$ 的结点集合是 $0$-可定义的，考虑 $\varphi(x, y) \equiv (x, y) \in E \lor \exists z. ((x, z) \in E \land (z, y) \in E)$。不过，所有连通的结点集是不可定义的。

对于集合 $A$ 和 $P \subseteq A$，称一个集族 $\mathcal{S} \subseteq \bigcup_{n\ge 1}\mathcal{P}(A^n)$ 为 **$P$-构造封闭（$P$-Constructively Closed）**，若满足下面列出的条件。记 $\mathcal{S}_n \equiv \mathcal{S}\cap \mathcal{P}(A^n)$：

* （布尔代数）：每个 $\mathcal{S}_n$ 都是布尔代数，即包含 $\emptyset$ 且 $A^n$ 在补集和有限并中封闭。
* （对称）：每个 $\mathcal{S}_n$ 都是对称的，即其坐标在置换操作中封闭。
* （投影）：$\mathcal{S}_{n+1}$ 中任意集合的前 $n$ 个成员构成的投影在 $\mathcal{S}_n$ 中。
* （提升）：$\mathcal{S}_n$ 中任意集合和 $A$ 的笛卡尔积都在 $\mathcal{S}_{n+1}$ 中。
* （$P$-纤维）：对任意 $p \in P$，它在 $\mathcal{S}_{n+1}$ 中的纤维都是 $\mathcal{S}_n$ 的元素。

$\mathcal{D}^\mathbf{A}(P)$ 是包含 $\{\text{c}^\mathbf{A}\}$、$f^\mathbf{A}$ 的等价图以及所有 $\mathcal{R}^\mathbf{A}$（对所有的 $\text{c} \in \mathscr{C}(\sigma)$、$f \in \mathscr{F}(\sigma)$ 和 $\mathcal{R} \in \mathscr{R}(\sigma)$）的最小的 $P$-构造封闭的子集。

#### 理论、模型和公理化

我们称一集 $\sigma$-句子为一个 **$\sigma$-理论（$\sigma$-Theory）**，称理论 $T$ 中的每个句子为 **公理（Axiom）**。如果某个非空的 $\sigma$-结构 $\mathbf{M}$ 满足理论 $T$ 中的每个公理，则称这个结构是一 $T$ 的一个 **$\sigma$-模型（$\sigma$-Model）**，记为 $\mathbf{M}\models T$。所有 $T$ 的 $\sigma$-模型构成一个集族 $\mathcal{M}_\sigma(T)$。对于一集 $\sigma$-结构 $\mathcal{C}$，若其对理论 $T$ 满足 $\mathcal{C} = \mathcal{M}_\sigma(T)$，则称 $T$ 是 $\mathcal{C}$ 的一个 **公理化（Axiomatization）**。如果 $\mathcal{C}$ 存在公理化，则称其 **可公理化（Axiomatizable）** 的。如果某个 $\sigma$-理论 $S$ 是另一个理论 $T$ 的所有模型的公理化，即 $S = \mathcal{M}_\sigma(T)$，则称 $S$ 是 $T$ 的公理化。

> 举例：
>
> * 对任意签名 $\sigma$ 和 $n \in \mathbb{N}^+$，集族 $\mathcal{C}_{\le n}$ 描述了所有不超过 $n$ 个元素的 $\sigma$-结构，其公理如下：
>   $$
>   \begin{equation*}
>   	\varphi_{\le n} \equiv \exists x_1\dots\exists x_n\forall y. \bigvee_{i=1}^n y\deq x_i
>   \end{equation*}
>   $$
>   另一方面，$\mathcal{C}_{\ge n}$ 的公理如下：
>   $$
>   \begin{equation*}
>   	\varphi_{\ge n} \equiv \exists x_1\dots\exists x_n \bigwedge_{1\le i < j \le n}x_i \deq x_j
>   \end{equation*}
>   $$
>   因此，所有恰好有 $n$ 个元素的 $\sigma$-结构可以通过公理 $\varphi_{=n} \equiv \varphi_{\le n}\land \varphi{\ge n}$ 公理化得到。
>
> * 对任意签名 $\sigma$，集族 $\mathcal{C}_\infty$ 由所有无限 $\sigma$-结构组成，其可以通过下面的理论公理化：
>   $$
>   \begin{equation}
>   	T_\infty \equiv \{\varphi_{\ge n} \mid n \in \mathbb{N}^+\}
>   \end{equation}
>   $$
>
> * 无向简单图中，存在两个公理：
>
>   * （无向）：$\forall x\forall y. ((x, y) \in E \to (y, x) \in E)$。
>   * （无自环）：$\forall x. \lnot((x, x) \in E)$。
>
>   下面我们提到的图都是无向简单图。
>   
> * 偏序结构 $\sigma_\text{partial} \equiv (\le)$ 可以通过下面的公理描述：
>
>   * （自反性）：$\forall x. x \le x$。
>   * （反对称性）：$\forall x\forall y. (x\le y \land y \le x \to x \deq y)$。
>   * （传递性）：$\forall x\forall y\forall z. (x \le y \land y \le z \to x \le z)$。
>
> * 群 $\sigma_\text{group} = (0, +, \cdot^{-1})$ 可以通过下面的公理描述：
>
>   * （结合性）：$\forall x\forall y\forall z. ((x+y)+z\deq x+(y+z))$。
>   * （单位元）：$\forall x. (0 + x \deq x + 0 \deq x)$。
>   * （逆元）：$\forall x\exists y. (x + y \deq y + x \deq 0)$。
>
>

需要注意，并不是所有结构都可以通过 FOL 公理化的，比如连通集、循环群等。

给定一个结构 $\mathbf{A}$，记 $\text{Th}(\mathbf{A})$ 为所有可以被 $\mathbf{A}$ 满足的公理集。判断某个公理是否属于 $\text{Th}(\mathbf{A})$ 有时相当困难（比如哥德巴赫猜想），因此如果能找到结构的更简单的公理化是最好的。考虑由 $\sigma_\text{arithmetic} = (0, S, +, \times)$ 建成的 **皮亚诺代数公理（Peano Arithmetic, PA）**

* $\forall x. (\lnot S(x) \deq 0)$。

* $\forall x\forall y. (S(x) \deq S(y) \to x \deq y)$。

* $\forall x. (x + 0 \deq x)$。

* $\forall x\forall y. (S(x + y) \deq x + S(y))$。

* $\forall x. (x \times 0 \deq 0)$。

* $\forall x\forall y. (x\times S(y) \deq x\times y + x)$。

* （归纳公理模式）：对任意 $\sigma_\text{arithmetic}$-公式 $\varphi(x, \mathbf{y})$，其中 $x \in \mathfrak{V}$ 而 $\mathbf{y} \in \mathfrak{V}^n$，下面是一个公理：
  $$
  \left( \varphi(0, \mathbf{y}) \land \forall x. (\varphi(x, \mathbf{y}) \to \varphi(S(x), \mathbf{y})) \right) \to \forall x. \varphi(x, \mathbf{y})
  $$

其中最后一个称为 **公理模式（Axiom Schema）**，因为它包含了无穷多个公理（对于任意公式都能写出以其为模版的一个公式）。显然，自然数结构 $\mathbf{N} \equiv (\mathbb{N}, 0, S, +, \times) \models \text{PA}$。不过后面我们会提到，$\text{PA}$ 不能公理化 $\text{Th}(\mathbf{N})$。

这里让我们也给出 Zermelo-Fraenkel 集合论（ZFC）的所有公理，它基于集合结构 $\sigma_\text{set} = (\in)$：

* （外延性公理）：$\forall x\forall y. (\forall z. (z \in x \leftrightarrow z \in y) \to x \deq y)$。

* （规律性公理）：$\forall x. ((\exists a. a \in x) \to \exists y. (y \in x \land \lnot \exists z. (z \in y \land z \in x)))$。

* （规定公理模式）：$\forall z\forall w_1\dots\forall w_n\exists y\exists x. (x \in y \leftrightarrow (x \in z \land \varphi(x, w_1, \dots, w_n, z)))$。

* （配对公理）：$\forall x\forall y\exists z. (x \in z \land y \in z)$。

* （并公理）：$\forall \mathcal{F}\exists z\forall y\forall x. (x \in y \land y \in \mathcal{F} \to x \in z)$，其中 $\mathcal{F}$ 是一个集族。

* （替换公理模式）：
  $$
  \forall w_1\dots\forall w_n\forall z. (\forall x. (x \in z \to \exists!y. \varphi(x, y, w_1, \dots, w_n, z))) \to \exists s\forall y. (y \in s \leftrightarrow \exists x \in z \varphi(x, y, w_1, \dots, w_n, z))
  $$

* （无限公理）：令 $S(x) \equiv x \cup \{w\}$。
  $$
  \exists x. (\exists e. (\forall z. \lnot(z \in e) \land e \in x) \land \forall y. (y \in x \to S(y) \in x))
  $$

* （幂集公理）：需要子集的定义 $y \subseteq x \Leftrightarrow \forall z. (z \in y \to z \in x)$，则 $\forall x\exists y\forall z. (z \subseteq x \to z \in y)$。

* （良序公理）：$\forall x\exists r. (\text{$r$ 是 $x$ 上的一个良序关系})$。良序公理、选择公理和 Zorn 引理三者是等价的。

这便是我们期盼已久的集合论的正式定义，下面做一个简单的讨论。

> 讨论：

#### 蕴含、一致性和完备性

如果 $T$ 的每个模型都满足一个 $\sigma$-句子 $\varphi$，即 $\forall \mathbf{M}\models T. (\mathbf{M}\models \varphi)$，则称 $T$ 满足 $\varphi$，记作 $T \models \varphi$。

一个 $\sigma$-理论 $T$ 称为：

* **可满足（Satisfiable）** 的，若其拥有一个模型。
* **语义上 $\sigma$-完备（Semantically $\sigma$-Complete）** 的，若对任意 $\sigma$-句子 $\varphi$，都有 $T \models \varphi$ 或 $\lnot (T \models \varphi)$。如果一个理论 $T$ 的超集是 $\sigma$-完备的，则也称其为 $T$ 的语义完备化。

为了方便之后的讨论，记 $\top$ 为 $\forall x.(x \deq x)$ 的简写，而 $\bot \equiv \lnot \top$。显然，在 $T$ 中下面的三个命题是等价的：

* $T$ 是可满足的。
* $\lnot (T \models \bot)$。
* 存在 $\sigma$-句子 $\varphi$ 使得 $\lnot (T \models \varphi)$。

如果两个结构 $\mathbf{A}$ 和 $\mathbf{B}$ 满足 $\text{Th}(\mathbf{A})$ 和 $\text{Th}(\mathbf{B})$，则称两者是 **基本等价（Elementarily Equivalent）** 的。语义完备性也可以定义为，对任何结构 $\mathbf{A}$、$\mathbf{B}$ 若满足 $\mathbf{A} \models T$ 且 $\mathbf{B} \models T$，则两者基本等价。

称一个理论为 **最大 $\sigma$-完备（Maximally Complete）** 的，若对任意 $\sigma$-句子 $\varphi \in T$，或者 $\varphi \in T$，或者 $\lnot \varphi \in T$（当且仅当 $T$ 不可满足时才同时成立）。称 $T$ 的某个最大完备的超集为 $T$ 的最大完备化。

观察到，对任意结构 $\mathbf{M}$，$\text{Th}(\mathbf{M})$ 总是可满足且最大完备的；同时，所有可满足的理论都有一个可满足的最大完备化。

#### 基本性

对于两个有包含关系的结构 $\mathbf{A} \subseteq \mathbf{B}$，我们会对它们同时满足或不满足的公式感兴趣。实际上，对于任意无量词的公式 $\varphi$ 和变量元组 $\mathbf{a}$，都有：
$$
\begin{equation}
	\mathbf{A} \models \varphi(\mathbf{a}) \is \mathbf{B} \models \varphi(\mathbf{a})
\end{equation}
$$
如果一个 $\sigma$-公式中所有的量词都是 $\forall$，则称其为一个 **全称（Universal）** 的；类似地，如果其中所有量词都是 $\exists$，则称其为 **存在（Existential）** 的。对于量词公式 $\varphi$：

* 如果 $\varphi$ 是全称的，则 $\mathbf{B} \models \varphi(\mathbf{a}) \Rightarrow \mathbf{A} \models \varphi(\mathbf{a})$。
* 如果 $\varphi$ 是存在的，则 $\mathbf{A} \models \varphi(\mathbf{a}) \Rightarrow \mathbf{B} \models \varphi(\mathbf{a})$。

设 $\mathbf{A}$ 和 $\mathbf{B}$ 是 $\sigma$-结构，对于同态 $h: \mathbf{A} \to \mathbf{B}$。如果任何公式 $\varphi(\mathbf{v})$ 和元组 $\mathbf{a} \in A^{|\mathbf{v}|}$，都有：
$$
\begin{equation}
	\mathbf{A} \models \varphi(\mathbf{A}) \is \mathbf{B} \models \varphi(h(\mathbf{a}))
\end{equation}
$$
则称 $h$ 是一个 **基本嵌入（Elementary Embedding）**，记作 $h: \mathbf{A} \hookrightarrow_e \mathbf{B}$。如果存在一个从 $\mathbf{A}$ 到 $\mathbf{B}$ 的基本嵌入，则称 $\mathbf{A}$ 可以基本嵌入 $\mathbf{B}$，记作 $\mathbf{A} \hookrightarrow_e \mathbf{B}$。特别地，如果 $\mathbf{A}$ 到 $\mathbf{B}$ 的包含映射 $x_\mathbf{A} \mapsto x_\mathbf{B}$ 是一个基本嵌入，则称 $\mathbf{A}$ 是 **基本（Elementary）** 的，记作 $\mathbf{A} \preceq \mathbf{B}$。注意到任意基本子结构都和该结构基本等价。

Tarski-Vaught 测试：$\mathbf{A}$ 是 $\mathbf{B}$ 的基本结构当且仅当 $\mathbf{A} \subseteq \mathbf{B}$ 且对任意公式 $\varphi(\mathbf{x}, y)$ 和 $\mathbf{a} \in A^{|\mathbf{x}|}$：
$$
\begin{equation}
	\mathbf{B} \models \exists y. \varphi(\mathbf{a}, y) \is \exists a' \in A. (\mathbf{B} \models \varphi(\mathbf{a}, a'))
\end{equation}
$$
