# Algorithm I

本篇是对应 *UIUC CS 225 Introduction to Data Structures and Algorithms* 以及 *UIUC CS 374 Introduction to Algorithms and Models of Computation* 的学习笔记。其中包括了基础的数学知识（关于算法复杂度）、数据结构、以及和数据结构相关的算法介绍。本篇是三篇算法相关笔记的第一篇，后两篇将分别专注于常见的算法和进阶算法知识。参考书籍是 *Introducation To Algorithms, Third Edition: Thomas H. Cormen, Charles E. Leiserson, et al.*、*Algorithms, Fourth Edition: Robert Sedgewick, Kevin Wayne* 以及 *Data Structures and Algorithms Analysis in C: Mark Allen Weiss*。

[TOC]



## 基础

### 计算问题和算法

**算法（Algorithm）** 是一个数学以及 **计算机科学（Computer Science, CS）** 中常见的概念，它由有限的一系列指令组成，能够解决一些 **计算问题（Computational Problem）**。通常，算法会接受外界一些 **输入（Input）**，然后在经过一系列指令后得到一系列 **输出（Output）**；因此我们也可以将算法视为将输入转化为特定输出的一个“黑箱”。举个例子，一个 **排序算法（Sorting Algorithm）** 接受一列数字 $a_1,...a_n$ 作为输入，然后输出这些数字的重排列 $a_1', ..., a_n'$ ，使得 $a_1' \le ... \le a_n'$ 成立。

如果一个算法能够对任意输入得到正确的输出，并保证停机，这个算法就是 **正确的（Correct）**。一个正确的算法可以 **解决（Solve）** 一个计算问题。不过，解决某一个计算问题的算法可能有很多。比如排序问题就是一个非常常见的计算问题，而不同的排序算法在输入规模、需要的排序程度、机器的软硬件配置等条件不同时，各有不同的表现。因此即使设计了一个正确的算法，我们仍有必要探索不同情况下更有优势的算法。

算法被广泛应用于许多领域，大到企业运营的最优策略，小到计算正方形的面积，任何解决计算问题的地方都有算法的身影：不少简单到你信手拈来（比如整数乘法算法——列竖式），也有相当神秘复杂的（比如人工智能）。如果想要窥探一些本系列中会详细介绍的例子，可以看下面的列表（来源于参考书籍）：

- 如何计算地图（有各种道路以及交叉点）上两点之间的最短路径？
- 如何寻找两个序列 $X = (x_1, ..., x_m)$ 与 $Y = (y_1, ..., y_n)$ 中最长的相同的子序列？比如序列 $(a, b, a, b, c)$ 和 $(b, a, b, a)$ 的最长相同子序列就是 $(b, a, b)$。
- 如何合理准备建造一间图书馆的材料？其中许多部件中有重复的零件，且有严格的安装先后顺序。我们需要找到一种排列方式使得每次要求某一个零件时它已经准备好了。
- 如何找到包含平面上一系列点的最小凸多边形？

这几个计算问题只是我们将要介绍的冰山一角。或许你已经从它们之中看出一些端倪，但许多算法问题都有下面的有趣性质：

- 有很多可能的解决方案，但是证明它数学上的严格性可能并不容易；如果要证明一个算法是最优的，那就更加困难了（事实上很多时候很可能并没有绝对最优的算法）。
- 它们真的很实用。许多现实中的问题都可以抽象为使用数学语言的算法问题。虽然我们倾向于讨论后面这种形式的问题，但不要忘记它们可以解决实际问题。

虽然我们能够给出大部分问题的算法，但是我们对于不少常见问题依然无法给出更好的解（我们无法证明不存在更好的解，也没办法给出更好的解），其中有一些问题非常特殊，我们将其称为 **NP 完全问题（NP-Complete Problem）**。其有趣的点在于，如果我们能够为任何一个 NP 完全问题找到更高效的解，则对于其它所有 NP 完全问题都 *一定* 可以找到高效算法；而且部分 NP 完全问题只要稍微修改设置，就会变成一个能够用高效算法解决的问题。作为举例，著名的“旅行商问题”介绍如下：假设一个旅行商想要不重复地在一些城市间旅行，且出发和结束的城市是同一个。如果每两个城市间交通的价格是固定且已知的，求其旅行的最小花费。我们会在后文中详细介绍这个问题的算法，并给出一个能够得出“贴近于最优解”的高效算法。实际上，NP 完全问题对于我们平时的最大作用在于，一旦确认某个问题是 NP 完全问题，我们大可不必花费大量时间尝试找出高效的正确算法；我们可以尝试设计出一个能够给出近似最优解的高效算法。

本系列笔记将算法分为一些主要的类别，并对一些算法进行原理、效率、实现上的解析。本篇将花大篇幅介绍 **数据结构（Data Structure）**，一种将数据以特定方式组织起来的形式，它在算法设计中无处不在，我们很快会具体介绍。

### 伪代码

在正式开始章节之前，我想简单介绍本笔记中用到的记录工具。本文中出现的所有代码都是 **伪代码（Pseudocode）**，它是一种将自然语言和程序语言的混用的代码形式，能够最小化语言细节的噪声而专注于逻辑；这也是我选择伪代码的原因，不希望让编程语言本身的细节淡化算法本身的思想（而且参考书籍用的也是伪代码）。伪代码可以轻松地转化为任何一种高级编程语言（多数情况下；命令式会非常简单，但如果是声明式可能会遇到一些困难）。

作为个人风格，我将采用缩进的方式来表示块结构（区分于 **C/C++** 等语言中的大括号），不使用行尾分号（因此每一行最多只能有一个指令），较少使用注释（这是因为只有在不需要文字说明时才使用代码，除此之外都应该用自然语言说明），且尽量少地使用符号（大部分只在进行运算时使用）。一些常用的符号由下表进行说明：

| 符号 | 名称     | 说明                                                         |
| ---- | -------- | ------------------------------------------------------------ |
| `<-` | 赋值操作 | 将右侧计算得到的值赋给左侧的元素。`a <- 0` 和数学中的 `let a = 0` 等同，不过伪代码允许重新定义 |
| `[]` | 下标操作 | 取一列元素中的某一个。`a[n]` 和数学中的 `a_n` 等同           |
| `()` | 函数调用 | 将一个元素通过函数映射得到值域中的一个元素。`f(a)` 得到 `b` 和数学中的 $f: a \mapsto b$ 意义等同 |

每一个算法被称为一个 **过程（Procedure）**，定义算法时会使用 `procedure algorithm(arg_1, ..., arg_n)` 的语法表明算法的开始。其中：

- `procedure` 是一个关键字，表明接下来定义的名字是一个算法。
- `algorithm` 是算法的名字。在定义该算法之后我们可以使用 `algorithm` 来调用它。
- `arg_1, ..., arg_n` 是算法的输入，也成为 **参数（Parameters）**。参数是可以在算法定义中使用的名字。
- `return T` 是（可选的）**返回类型（Return Type）**，即算法输出的数据类型。当算法没有输出时，这部分可以省略。理论上算法可以返回多个输出；这种情况下我们可以让 `T` 是一个 *元组* 类型，我们会在后面介绍元组。
- `where cls_1, ..., cls_n` 是（可选的）算法的参数和返回类型 **约束（Constraints）**。每一个约束可以限制参数或返回值的类型为特定类型或满足特定要求。

下面是一个示例程序：

```pseudocode
procedure fibonacci(n) return Int 
                       where n is Int
    if n = 0 or n = 1
        return 1
    else
        return fibonacci(n - 1) + fibonacci(n - 2)
```

另一个示例程序：

```pseudocode
procedure add_input_numbers() return Int
    Get i is Int from command-line input
    Get j is Int from command-line input
    return i + j
```

另一个示例程序：

```pseudocode
procedure least_common_multiple(a, b) return Int 
                                      where a is Int
                                            b is Int
    res is Int <- a
    until res mod b = 0
        res <- res + a
    return res
```

此外，为了支持 **OOP（Object Oriented Programming）** 的代码风格，本篇的伪代码中存在对象的概念。比如 `Array` 和 `String` 类型，以及任何通过 `type` 关键字定义的类型：

```pseudocode
type Gender = male or female or other /* Enumeration type, defined by listing out all possibilities */

type Person where
    name is String /* A member is declared, but not initialized, in the type definition */
    age is Int
    gender is Gender

/* A constructor (ctor) named after the custom class that returns a Person object */
procedure Person(name, age, gender) return Person
                                    where name   is String
                                          age    is Int
                                          gender is Gender
    /* Initialize a custom type by initializing all its members in the order they're declared  */
    result is Person <- { "Alice", 38, female }
    return result
    
/* Constructors may share their names, which is called overloading in programming language pragmatics */
/* A copy constructor that clones another object of the Person type. This will be implicitly defined for all custom types */
procedure Person(other) return Person
                        where other is Person
    result is Person <- { other.name, other.age, other.gender } /* Access members by the . operator */
    return result
   
/* Procedures that takes a certain type as the first parameter can be called (member) functions */
/* I'll declare it in the syntax of `function Type.procedure_name(...)` */
function Person.show() /* Quite like `procedure show(Person this)` but the latter cannot use the . operator */
    Print format("My name is {}, I'm {} years old.", this.name, this.age) /* `this` refers to the implicit first argument */
```

最后，让我们特别介绍一种特殊的类型，即 **引用（Reference）**。在实际应用中，为了方便不同类型的嵌套使用，我们存储的成员往往不是它们本身，而是一个能够唯一对应到它们的标签。作为例子，先看下面这段不合理的代码：

```pseudocode
type A where
    b is B
    data is Int
    
type B where 
    a is A
    data is Int
```

这里的问题很大：我们无法估计 `A` 或 `B` 中任一个的大小。如果设 `A` 的大小是 $x$，那么包含 `A` 的 `B` 类型大小应该是 $x + \text{sizeof(int)}$，此时再看 `A` 的定义就得到下面这个式子：
$$
(x + \text{sizeof(Int)}) + \text{sizeof(Int)} = x
$$
这个等式是没有解的，因此这种定义不可行。然而我们确实对类似的场景有需求（可以参考后面对[链表](#链表)的定义），此时 **引用** 的意义就体现出来了。为了让大家理解起来更简单，先把引用理解成一个 *神奇的整数*，每个变量在初始化时都能生成一个唯一对应它的整数；此时下面的定义就显得非常好用：

```pseudocode
type A where
    b is Ref of B /* Just an integer, so it's bounded in real world */
    data is Int
    
type B where
    a is Ref of A /* Here a doesn't have to be a reference; I make it for symmetry */
    data is Int
```

现在两者的大小都是 `sizeof(Ref of Int) + sizeof(Int)` 了。引用在计算机中的含义就是 **内存地址（Memory Address）**；由于每个对象通常都存储在内存当中，其存储地址正好可以用来当作唯一对应的整数。引用类型在 **C/C++** 中的对等概念是 **指针（Pointer）**，如果需要从引用访问对象需要手动进行 **解引用（Dereference）** 操作；而 **Java** 等语言中的绝大多数类型都默认为该类型的引用类型，从引用访问原来对象不需要手动进行解引用操作。用伪代码演示如下：

```pseudocode
procedure c_style_reference()
    x is Ref of String <- Ref of "abc"
    deref of x <- deref of x + "def"
    Print deref of x on the screen
    
procedure java_style_reference()
    x is String <- "abc"
    x <- x + "def"
    Print x on the screen
```

后者显然要简洁得多。不过我不希望让对象过于“碎片化”，且某种程度上也有效率上的考量（引用和解引用都是潜在耗时的工作）。本篇的伪代码中，引用类型会被显式声明出来，但引用和解引用操作都是隐式的；此外，只在必要的时候使用引用类型。

```pseudocode
/* This procedure modifies its parameter, which is passed by reference */
procedure my_style_reference(str) where str is Ref of String
    x <- x + "def"
    Print x on the screen
```

当一个引用类型的变量没有引用任何对象时，它的值是 `null`。尝试解引用 `null` 的行为是禁止的：

```pseudocode
procedure use_null()
    x is Ref of Int <- null
    Print x on the screen /* causes an error */
```

此外，为了表达的简洁性，`Array` 类型总是表示数组的引用类型。

### 数学基础

本系列笔记所需要的数学基础并不多，通常只需要中学的知识，如四则运算、幂运算、对数运算、余数运算的一些法则，以及求和符号的使用等。不过，由于算法时间复杂度的估计中会经常出现级数的求和，我认为有必要进行一些复习。

首先，是指数和对数运算的一些法则：
$$
a^{-n} \equiv \frac{1}{a^n} (a \ne 0) \qquad a^ma^n \equiv a^{m+n} \qquad \frac{a^m}{a^n} \equiv a^{m-n} \qquad (a^m)^n \equiv a^{mn} \\
\log_n\frac{1}{a} \equiv -\log_n a(a > 0) \qquad \log_n a + \log_n b \equiv \log_n ab \qquad \log_n a - \log_n b \equiv \log_n \frac{a}{b} \qquad \log_n a^b \equiv b\log_n a
$$
对数还有一个非常重要的恒等式，即换底公式：
$$
\log_a b = \frac{\log_n b}{\log_n a}
$$
上面公式的证明是非常初等的，大家不妨自己动手证明。值得一提的是在算法中，我们通常假定对数的底是 2，因此后文中如非特殊情况，我们都省略对数的底。

下面我们将介绍一些常见的级数求和。比如算数级数：
$$
\sum_{i=1}^N i \equiv \frac{N(N+1)}{2} \approx \frac{N^2}{2}
$$
这个正是高斯那个故事中得到的公式。注意到我们给出了这个级数的近似结果。虽然现实中它和实际结果有不小的差距，但它们是在同一个数量级上的。当运算规模增加（$N$ 很大）时，上式中占主导地位的就是 $N^2$ 项。这种处理方法在我们即将介绍的算法分析中非常常见。

其它类似的一些级数：
$$
\sum_{i=1}^N i^2 \equiv \frac{N(N+1)(2N+1)}{6} \approx \frac{N^3}{3} \\
\sum_{i=1}^N i^3 \equiv \frac{N^2(N+1)^2}{4} \approx \frac{N^4}{4}
$$
以及在指数不等于 $-1$ 时的通用近似结果：
$$
\sum_{i=1}^N i^k \approx \frac{N^{k+1}}{|k+1|} \quad (k \ne -1)
$$
这里的 $k$ 可以是除了 $-1$ 以外的任意整数。当 $k = -1$ 时，我们有另一个级数和近似公式：
$$
\sum_{i=1}^N\frac{1}{i} \approx \ln N = \log_e N
$$
这里的 $\ln$ 是以常数 $e \approx 2.71828$ 为底的对数。这个近似值是 *相当* 贴近准确值的。当 $N$ 不断增大时，两者之间的误差仅为一个很小的数，即 **欧拉常数（Euler's Constant）**：
$$
\gamma \approx 0.57721566
$$
如果有微积分基础的朋友可能发现上面这系列公式与 **原函数（Antiderivative）** 的相似性，这里也一并列出来（但这并不是本篇笔记需要掌握的内容）：
$$
\int x^n\,dx = \frac{1}{n+1}x^{n+1} + C \quad (n \ne -1) \qquad \int \frac{1}{x}\,dx = \ln x + C
$$
几何级数也相当常见：
$$
\sum_{i=0}^N A^i \equiv \frac{A^{N+1} - 1}{A-1} \quad (A \ne 1)
$$
特别地，当 $A = 2$ 时我们有：
$$
\sum_{n=0}^N 2^i \equiv 2^{N+1} - 1
$$
如果 $0 < A < 1$，我们发现 $A^{N+1}$ 约等于 $0$，我们可以对公式进行一个近似：
$$
\sum_{n=0}^N A^i \approx \frac{1}{1-A} \quad (0 < A < 1)
$$
因此在 $N$ 非常大时，这个近似值是比较精确的。

最后让我们介绍另一个常见的级数和：
$$
\sum_{i=1}^N \frac{i}{2^i} = \sum_{i=0}^N\left(\frac{1}{2}\right)^i \approx 2
$$

### 数学证明

算法中最常用的两种证明方法是 **数学归纳法（Mathematical Inductioon）** 以及 **反证法（Proof By Contradiction）**。让我们分别用一个例子来展示这两种方法的论述过程。

> **例**：证明 $\displaystyle \sum_{i=1}^N i^2 = \frac{N(N+1)(2N+1)}{6}$，其中 $N \in \mathbb{N}$。

> **证**：对于基本情形，即 $N = 1$ 时左式等于 $1^1 = 1$，右式等于 $1\cdot 2\cdot 3/6 = 1$，因此等式成立。假设当 $1 \le k \le N, k \in \mathbb{N}$ 时等式均成立，则当 $N = k + 1$ 时有：
> $$
> \begin{align*}
> 	\sum_{i=1}^{k + 1} i^2 &= \sum_{i=1}^k i^2 + (k+1)^2 \\
> 	&= \frac{k(k+1)(2k + 1)}{6} + (k+1)^2 \\
> 	&= \frac{(k+1)(2k^2 + k + 6k + 6)}{6} \\
> 	&= \frac{(k+1)(k+2)(2k+3)}{6} = \frac{(k+1)((k+1) + 1)(2(k+1) + 1)}{6}
> \end{align*}
> $$
> 因此得证。

> **例**：证明素数有无穷多个。

> **例**：假设素数有限，则全体素数集 $P$ 是一个有限集，其中的元素为 $p_1, ..., p_k$。现在考虑下面的数：
> $$
> N = p_1...p_k + 1
> $$
> 它一定不是一个合数，因为根据余数的运算规则，$p_1...p_k + 1$ 对任何素数 $p_1, ..., p_k$ 都同余于 $1$。这就矛盾了。因此素数的个数无限。

数学归纳法有效的原因并不难理解，它将一个问题 $A(i)$ 转化为等效的一到多个已经解决的子问题 $A(n_1), ..., A(n_k), k < i$。算法中我们时常会进行类似的操作，我们将其称为 **递归（Recursion）**。通俗来讲，递归就是一个算法中使用了它自身，比如此前我们给出示例的斐波那契数算法，就可以给出类似于 `fib(k) = fib(k - 1) + fib(k - 2)` 这样的恒等式（其中 $k > 2$）。不过，如果一个算法每次执行都再次调用自身，显然此算法没法停机。因此递归的算法中总会有一到多个基本情况（数学归纳法中，$N = 1$ 就是一个常见的基本情况），使得程序能够最终归纳为一系列确定的结果。不过，即使有了基本情况，一个错误设计的算法可能永远达不到这个基本情况。看下面的例子：

```pseudocode
procedure infinite_loop(n) return Int 
                           where n is int
    if n == 0
        return 0
    else 
        return infinite_loop(n / 2 + 1) + n
```

显然这个算法对于输入值为 `1` 或 `2` 的情形会造成无限循环；更糟糕的是，由于递归调用处有一个 `+ 1`，大于 `0` 的输入永远也不会达到基本情况。因此总结下来，如果要一个递归算法能够停机，其至少应该：

- 拥有基本情形。基本情形不一定是输入为 $0$ 的情况，可能是任意指定的一个或多个位置。
- 每次递归时都需要朝着基本情形的方向前进。

在很多语言中，递归是一个相对低效的算法设计模式，这和编程语言中过程调用的设计有关。细心的朋友可能发现此前我们定义的 `fibonacci` 算法存在大量不合理的重复调用。比如 `fibonacci(10)` 会调用 `fibonacci(9)` 与 `fibonacci(8)`，而 `fibonacci(9)` 本身会调用 `fibonacci(8)` 和 `fibonacci(7)`，其中 `fibonacci(8)` 被重复调用了。要知道它们会一步步往下调用，直到基本情况（`fibonacc(1)` 或 `fibonacci(0)`）为止，此时产生的重复调用次数是指数级的，因此这个算法 *一点都不* 高效。好在我们有不少改进方案，在后文中会详细介绍。

虽然存在效率问题，但递归常常能清晰地向我们展示算法的逻辑；某种程度上它更有数学定义的风格，因此对一般问题进行分析时，可能最好想到的就是递归算法。一些声明式语言就青睐递归，比如 **ML**、**Haskell** 等。我们也可以将伪代码写成类似于声明的形式：

```pseudocode
define fibonacci(0) return Int = 1
define fibonacci(1) return Int = 1
define fibonacci(n) return Int = fibonacci(n - 1) + fibonacci(n - 2) where n is int
```

可以看到声明式的表达力相当优秀。本系列笔记中只采用命令式的伪代码。

### 算法分析

为了能够分析算法的效率，也即对资源（运行时间或存储空间）的消耗程度进行定量的分析，我们需要建立一个框架。下面的五个定义将作为我们的理论基础：

- 如果存在正常数 $c$ 和 $n_0$ 使得 $N > n_0$ 时总有 $T(N) \le cf(N)$，则记 $T(N) = O(f(N))$。这也被称为大 $O$ 记号，其反映了函数 $T(N)$ 的上界。
- 如果存在正常数 $c$ 和 $n_0$ 使得 $N > n_0$ 时总有 $T(N) \ge cg(N)$，则记 $T(N) = \Omega(g(N))$。这也被称为大 $\Omega$ 记号，其反映了函数 $T(N)$ 的下界。
- 如果 $T(N) = O(h(N))$ 且 $T(N) = \Omega(h(N))$，则记 $T(N) = \Theta(h(N))$，这也被称为大 $\Theta$ 记号，其反映了函数 $T(N)$ 的近似。
- 如果 $T(N) = O(p(N))$ 且 $T(N) \ne \Theta(p(N))$，则记 $T(N) = o(p(N))$，这也被称为小 $o$ 记号，其反应了函数 $T(N)$ 的非近似上界。
- 如果 $T(N) = \Omega(q(N))$ 且 $T(N) \ne \Theta(q(N))$，则记 $T(N) = \omega(q(N))$，这也被称为小 $\omega$ 记号，其反应了函数 $T(N)$ 的非近似下界。

引入这些概念的原因是，对于任意的两个函数 $f(N), g(N)$，我们很难说它们的大小关系，因为这和 $N$ 的取值有关。比如 $f(N) = N + 1000$ 与 $g(N) = N^2$，虽然显然后者增加的“潜力”更大，但是在 $N$ 比较小时并不能看出这一点。但是显然可以找到常数 $c, n_0 > 0$，使得 $N > n_0$ 时总有 $g(N) \ge cf(N)$。比如这里可以取 $c = 0.001, n_0 = 1$。这种比较方式实际上比较的是函数的相对增长率。对于一些典型的函数，我们可以轻松地对它们的相对增长率进行排序：
$$
c < \log N < \log^k N < N < N\log N < N^k < 2^N \quad (k > 1)
$$
根据上面这个排序我们可以轻易地写出诸如 $3\log N = O(N)$、$N^2 = \Omega(c)$ 之类的式子。需要注意，一个函数的上下界是无穷多的，因此函数 $N\log N$ 可以同时由 $O(N^2)$、$\Theta(N\log N)$、$\Omega(\log N)$ 等式子描述，它们之间互不矛盾。不过，在实际运用中，最常出现的是大 $O$ 记号，因为它可以表明算法资源消耗的（最小）上界，这也是我们对算法效率最关心的点。

当算法结合在一起时，设 $T_1(N) = O(f(N)), T_2(N) = O(g(N))$，有两个性质：

- $T_1(N) + T_2(N) = \max(O(f(N)), O(g(N)))$。因此两个算法效率的低阶项可以被忽略。
- $T_1(N)T_2(N) = O(f(N)g(N))$。

有了这些工具后，我们就可以着手进行算法分析了。通常有两个衡量方向，时间（程序耗时）或空间（内存占用），我们分别将算法在这两个方向的资源消耗称为 **时间复杂度（Time Complexity）** 与 **空间复杂度（Time Complexity）**，均使用大 $O$ 记号表示。我们对前者通常有更高的兴趣（事实上有一类型的算法会牺牲空间消耗来换取更低的时间复杂度），下面将用一些例子来展示如何分析算法的时间复杂度。

#### 级数求和

求平方级数的部分和：

```pseudocode
procedure square_sum(n) return Int 
                        where n is Int
    sum is Int <- 0
    for i <- 0 until n
    	sum <- i * i
    return sum
```

第 2 行 `x` 初始化需要一个时间单元，第 3 行 `i` 初始化和第一次比较 `i` 与 `n` 各需要一个时间单元，第 3 至 4 行的循环会进行 $n$ 次，每次都会执行一次乘法（`i * i`）、一次赋值（`sum <- i * i`）、一次递增（令 `i` 加一）、一次比较（将 `i` 与 `n` 进行比较），因此一共是 $4n$ 次操作。所以全部加起来是 $4n + 3$ 次操作。结论：这个程序的时间复杂度是 $O(N)$。

这里我们计算循环以外的时间消耗似乎是不必要的，因为它们只需要常数时间复杂度；真正有效果的是循环以内的语句，且只有执行次数会影响到最终的大 $O$ 记号。因此一个通用的计算方式是首先统计最内侧循环的执行次数，在这个循环其并列的语句的复杂度中取最大的那个；如果有外层的循环，就将它们的执行次数相乘得到复杂度，并回到上一步，直到分析完整个算法的复杂度。这也是我们此前给出的两个性质的实际应用。

值得一提的是，对于 `if` 分支语句，我们应该取多个分支中复杂度最高的那个（时刻记住大 $O$ 记号得到的是算法复杂度的上界，因此任何时候都应该考虑最差情形）。

#### 二分查找

从一列升序排列的数 $A_0, ..., A_{N-1}$ 中找到某个整数 $X$ 并返回其下标；如果不存在这个数，则返回 $-1$：

```pseudocode
procedure binary_search(arr, N, X) return Int 
                                   where arr is Array of Int, 
                                         N   is Int, 
                                         X   is Int
    low is Int <- 0
    high is Int <- N - 1
    while low <= high
        mid is Int <- (low + high) / 2
        if arr[mid] < X
            low <- mid + 1
        else if arr[mid] > X
            high <- mid - 1
        else
            return mid
    return -1
```

这个算法中的循环每次都会更新 `low`、`high` 中的一个，或输出结果，直到 `low > high` 为止。由于 `mid` 是它们的平均数，每次更新之后 `high - low` 的值都至少会折半。因此最差的循环次数出现在 `high - low` 是 2 的幂的情形，此时的复杂度是 $\log N$，因此该算法的复杂度是 $O(\log N)$。



## 数据结构

### 基本结构

让我们首先对基本的数据结构进行介绍，它们在多数编程语言中都有内置支持。

**数组（Array）** 是一种将同种类型元素连续存储的数据结构。在计算机中，在内存中连续存储的数据会在其中任一个地址被访问时被一起拷贝到 **缓存（Cache）** 中，后者的访问速度是内存的 10 到 100 倍，因此使用数组时可以享受其超高的效率。不过，数组的局限性在于其大小是固定的，因此我们不能向数组中添加元素，也不能删除元素。数组的使用范例如下：

```pseudocode
procedure use_array()
    arr is Array of Int <- { 1, 2, 3 }
    foreach x in arr
        Print x on the console
    arr[0] <- 0 /* 1 -> 0 */
    Print arr[2] on the console /* 3 */
```

虽然我们通常指的是一维数组，但数组本身可以是多维的；在计算机中其每一个低维“切片”都是连续存储的。下面的例子中，二维数组 `mat` 本质还是一个一维数组，只不过访问 `mat[i][j]` 时，我们实际上想访问的是 `mat[i * # of rows + j]`。

```pseudocode
procedure use_multiarray()
    mat is Array of Array of Int <- { { 1, 2, 3 }, { 4, 5, 6 }, { 7, 8, 9 } }
    foreach row in mat
        foreach x in row
            Print x on the console
    arr[1][0] <- 0 /* 4 -> 0 */
    Print arr[2][2] on the console /* 9 */
```

数组的成员只需要一个，即 `size`。前面我们提到过，数组类型是引用类型，因此它应该还有一个引用内存中数据的成员 `data`。不过由于我们已经让数组本身可以通过运算符 `[]` 来访问内存上的数据，这个成员也就不重要了。

**元组（Tuple）** 是一种将零至多个元素按顺序罗列的数据结构。它和数组类似，也是可以高效利用缓存的结构。同样，元组不允许添加或删除元素。我们可以通过元组模拟复杂的结构。

```pseudocode
procedure use_tuple()
    tup is (Int, Number, String) <- (42, 3.14, "abc")
    (i, num, str) is (Int, Number, String) <- tup
    Print num on the console for i times
    Print str
    empty is () <- () /* () is the empty-tuple type as well as the only possible value of this type */
    Print yes or no for empty = null /* no */
```

元组常用的两个方法是 `first` 和 `rest`：

```pseudocode
procedure use_tuple()
    tup is (Int, Number, String) <- (42, 3.14, "abc")
    i is Int <- tup.first
    tup2 is (Number, String) <- tup.rest
    tup3 is (String) <- tup2.rest
    tup4 is () <- tup3.rest
```



此后我们构建的所有结构都以这两个基本结构为基础。

### 动态数组

我们希望利用数组的高效，但也同时希望能够灵活地添加或删除元素，这就催生了 **动态数组（Dynamic Array）** 的概念。动态数组（在一些编程语言中可能叫做 **数组表（Array List）** 或 **向量（Vector）**）是对数组的一种抽象，它时刻记录着当前元素的数量以及数组的大小；当元素数量要超过数组大小时，会重新在内存中创建一个更大的数组，然后将原来的数组内容复制过去，这个过程也叫做 **重分配（Reallocation）**。

```pseudocode
type Vector where
    size is Int /* # of elements */
    capacity is Int /* max # of element possible */
    data is Array of Object /* array of data */
```

这些成员分别被称为数组的 **元素数量**、**容量** 以及 **数据**。对于动态数组，我们可以执行的主要操作如下：

```pseudocode
/* O(1) check for emptyness */
function Vector.empty() return Bool
    return this.size = 0
 
/* O(1) access to first element */
function Vector.front() return Object
    Check i >= 0 and i < this.size
    return this.data[0]

/* O(1) access to last element */
function Vector.back() return Object
     Check i >= 0 and i < this.size
     return this.data[this.size - 1]

/* O(1) random access */
function Vector.at(i) return Object where i is Int 
    Check i >= 0 and i < this.size
    return this.data[i]

/* O(1) data modification */
function Vector.put(i, val) where i   is Int, 
                                  val is Object
    Check i >= 0 and i < this.size
    this.data[i] <- val
    
/* O(n) reserve for memory */
function Vector.reserve(n) where n is Int
    if n > this.capacity
        new_cap is Int <- this.capacity
        while new_cap < n
            new_cap <- new_cap * 2
        new_arr is Array of Object with size new_cap
        Copy elements from this.data to new_arr
        Deallocate this.data
        this.data = new_arr

/* Amortized O(1) insertion to the end (explained later) */
function Vector.push_back(val) where val is Object
    this.reserve(this.size + 1)
    this.data[this.size] <- val
    this.size <- this.size - 1

/* O(1) removal from the end */
function Vector.pop_back() return Object
    this.size <- this.size - 1
    return this.data[this.size]

/* O(n) reverse */
function Vector.reverse()
    for i <- 0 until this.size / 2
        Swap this.data[i], this.data[this.size - i - 1]
```

以上便是动态数组的基本操作。可以看到该结构多数操作都是 $O(1)$ 的，这得益于数组 $O(1)$ 的随机访问。不过其中有一个函数 `push_back` 明明调用了复杂度为 `O(n)` 的 `reserve` 却仍被标记为 $O(1)$，这是为什么呢？这里指的其实是 **分摊成本（Amortized Cost）** 为 $O(1)$。虽然 `reserve` 复杂度为 $O(1)$，但并不是每次调用都会进行重分配。让我们假设数组开始的容量是 $n$，`reserve` 耗时为 $kN = O(N)$，在 $n$ 次 `push_back` 后，第 $n + 1$ 次时会触发重分配，耗时为 $kn$，新的容量是 $2n$。接下来的 $n - 1$ 次 `push_back` 依然是相安无事，直到第 $2n + 1$ 次触发重分配，耗时为 $2kn$，之后又是 $2n - 1$ 次普通的 `push_back`，直到 $4n + 1$ 次出发重分配，耗时为 $4kn$。这样不断进行后，我们发现每耗时 $kn$，就能 `push_back` 同样比例的元素。因此平均分摊下来，一次 `push_back` 只需要 $O(1)$ 的时间。

动态数组是非常常用的数据结构，我们无需知道数组的大小，就可以轻松地使用数组。不过由于其连续存的性质，如果想要在数组中间添加或删除一个元素就是 $O(n)$ 的操作了。因此如果需要频繁在数组中间做这样的操作，可以考虑后面介绍的数据结构。但为了保证功能的完整性，下面我们还是给出一些动态数组的低效率算法：

```pseudocode
/* O(n) random insertion */
function Vector.insert(i, val) where i   is Int
                                     val is Object
    this.reserve(this.size + 1)
    Copy elements within [this.data[i], this.back()] to a range starting from this.data[i + 1]
    this.data[i] <- val
    this.size <- this.size + 1

/* O(n) random removal */
function Vector.erase(i) where i is Int
    Copy elements within [this.data[i + 1], this.back()] to a range starting from this.data[i]
    this.size <- this.size - 1
    
/* O(n) insertion to the front */
function Vector.push_front(val) where val is Object
    this.insert(0, val)

/* O(n) removal from the front */
function Vector.pop_front() return Object
    front <- this.front()
    this.erase(0)
    return front
```

下面是一个使用示例：

```pseudocode
procedure filter_greater_than(arr, n) where arr is Array of Int, n is Int
    vec is Vector <- {}
    foreach x in arr
        if x > n
            vec.push_back(x)
    return vec
```

### 链表

**链表（Linked List）** 是为了解决数组连续存储导致非结尾元素的插入删除低效而设计的结构。链表由多个 **结点（Node）** 组成，每个结点都存储着下一个结点的地址，这样就可以一步步遍历整个列表了。

```pseudocode
type Node where
    next is Ref of Node /* next node */
    prev is Ref of Node /* previous node */
    data is Object /* stored data */
    
type LinkedList where
    size is Int /* number of elments in the list */
    head is Node /* dumb node at the front of the list */
    tail is Node /* sentinel node at the back of the list */
```

这些成员分别被称为链表的 **长度**、**头结点** 与 **尾结点**。对于链表，我们可以进行的操作如下：

```pseudocode
/* O(n) getting size of linked nodes */
function Node.size() where node is Ref of Node
    size is Int <- 0
    curr is Ref of Node <- this
    until curr is null
        curr <- curr.next
        size <- size + 1
    return size

/* O(1) insertion after a node */
function Node.insert_after(node) where node is Ref of Node
    node.next <- this.next
    node.prev <- this
    this.next.prev <- node
    this.next <- node
 
/* O(1) insertion before a node */
function Node.insert_before(node) where node is Ref of Node
    node.prev <- this.prev
    node.next <- this
    this.prev.next <- node
    this.prev <- node
   
/* O(1) splice after a node */
function Node.splice_after(start, stop) where start is Ref of Node, 
                                              stop  is Ref of Node
    this.next.prev <- stop
    stop.next <- this.next
    this.next <- start
    start.prev <- this
 
/* O(1) unlink a node */
procedure unlink(node) where node is Ref of Node
    node.prev.next <- node.next
    node.next.prev <- node.prev
    node.next <- null
    node.prev <- null
    
/* O(1) unlink a sequence */
procedure Node.unlink_sequence(start, stop) where start is Ref of Node,
                                                  stop  is Ref of Node
    start.prev.next <- stop.next
    stop.next.prev <- start
    start.prev <- null
    stop.next <- null
    
/* O(n) calculation for distance between two nodes */
procedure distance(n1, n2) return Int
                           where n1 is Ref of Node,
                                 n2 is Ref of Node 
    dist is Int <- 0
    until n1 = n2
        n1 <- n1.next
        dist <- dist + 1
    return dist
    
/* Constructor */
procedure LinkedList() return LinkedList
    result is LinkedList <- {}
    result.size is Int <- 0
    result.head is Node <- { null, null, {} }
    result.tail is Node <- { null, null, {} }
    result.head.next <- result.tail
    result.tail.prev <- result.head
    return result
    
/* Destructor */
function LinkedList.destroy()
    if this.size = 0
        return
    curr is Ref of Node <- this.head.next
    temp is Ref of Node <- null
    until curr = this.tail
        temp <- curr.next
        Deallocate curr on the memory
        curr <- temp

/* O(1) check for emptyness */
function LinkedList.empty() return Bool
    return this.size = 0

/* O(1) access to first element */
function LinkedList.front() return Object
    Cehck size > 0
    return this.head.next.data
 
/* O(1) access to last element */
function LinkedList.back() return Object
    Check size > 0
    return this.tail.prev.data

/* O(1) insertion at given position */
function LinkedList.insert(pos, val) where pos is Ref of Node, 
                                           val is Object
    prev is Ref of Node <- pos.prev
    new_node is Node allocated on the memory
    new_node.data <- val
    pos.insert_before(new_node)
    this.size <- this.size + 1

/* O(1) removal at given position */
function LinkedList.erase(pos) where pos is Ref of Node
    pos.unlink()
    Deallocate pos on the memory
    this.size <- this.size - 1

/* O(1) splice */
function LinkedList.splice(pos, other) where pos is Ref of Node, other is LinkedList
    if other.size = 0
        return
    start <- other.head.next
    stop <- other.tail.prev
    unlink_sequence(start, stop)
    pos.splice_before(start, stop)
    this.size <- this.size + other.size
    other.size <- 0
```

上面我们将“裸链表”（即 `Node` 相关的操作）和封装过的链表操作都定义了出来。可以看到不论是裸链表还是链表，大多数操作都是高效的 $O(1)$。唯一的缺点在于随机访问，以及任何涉及链表局部长度的算法，其复杂度是 $O(n)$。下面给出了这些操作的定义：

```pseudocode
function LinkedList.node_at(i) return Ref of Node
                                       where i is Int
    Check i >= 0 and i < this.size
    curr is Ref of Node <- this.first
    until i < 0
        curr <- curr.next
        i <- i - 1
    return curr
    
function LinkedList.at(i) return Object
                          where i is Int
    node is Ref of Node <- this.node_at(i)
    return node.data
    
function LinkedList.put(i, val) where i   is Int
                                      val is Object
    node is Ref of Mpde <- this.node_at(i)
    node.data <- val
    
/* O(n) splice */
function LinkedList.splice_at(pos, start, stop) where pos   is Ref of Node
                                                      start is Ref of Node
                                                      stop  is Ref of Node
    dist is Int <- distance(start, stop)
    pos.slice_before(start, stop)
    this.size <- this.size + dist
```

### 二叉树

此前我们介绍的都是线性数据结构（我们在下一章会再次提及这个特点），但许多非线性的结构也相当常用。其中之一是 **二叉树（Binary Tree）**。二叉数由一系列二叉树结点组成；每个结点都存储了一个左结点和右结点的引用及数据。

```pseudocode
type Node where
    left is Ref of Node
    right is Ref of Node
    data is Object
    
type BinaryTree where
    size is Int
    root is Ref of Node
```

二叉树的操作如下：

```pseudocode
/* O(n) getting size of a subtree */
function Node.size() return Int
    result is Int <- 1
    result <- result + this.left.size()  when this.left <> null
    result <- result + this.right.size() when this.right <> null
    return result
   
/* O(n) getting height of a subtree */
function Node.height() return Int
    left_height  is Int <- this.left.height()  when this.left <> null  else -1
    right_height is Int <- this.right.height() when this.right <> null else -1
    return 1 + Bigger one in { left_height, right_height }
    
/* O(n) getting depth of a subtree */
function Node.depth(node) return Int
                          where node is Ref of Node
    return 1 when this = node
    left_depth  is Int <- this.left.depth(node)  when this.left <> null  else 0
    right_depth is Int <- this.right.depth(node) when this.right <> null else 0
    return 1 + Bigger one in { left_depth, right_depth }
    
/* O(n) getting mirror of a subtree */
function Node.mirror()
    Swap this.left, this.right
    this.left.mirror()  when this.left <> null
    this.right.mirror() when this.right <> null
    
procedure BinaryTree(n) return BinaryTree
                        where n is Int
    Check n > 0
    nodes is Array of Ref of Node with size n
    for i <- 0 until n - 1
       nodes[i] <- { nodes[i*2 + 1] when i*2 + 1 < n else null,
                     nodes[i*2 + 2] when i*2 + 2 < n else null,
                     {} }
    return { n, nodes[0] }
    
function BinaryTree.depth(pos) return Int
                               where pos is Ref of Node
    return this.root.depth(pos)
    
function BinaryTree.rotate_left(pos) return Ref of Node
                                     where pos is Ref of Node
    right is Ref of Node <- pos.right
    pos.right <- right.left
    right.left <- pos
    return right
    
function BinaryTree.rotate_right(pos) return Ref of Node
                                      where pos is Ref of Node
    left is Ref of Node <- pos.left
    pos.left <- left.right
    left.right <- pos
    return left
    
function BinaryTree.mirror(pos)
    pos.mirror()
```



## 抽象数据类型

我们此前介绍的数据结构，它们的操作还不够丰富，用处也尚不明朗。很多时候我们需要用于特定用途的一些操作，此时就需要定义 **抽象数据类型（Abstract Data Type, ADT）** 了。一个抽象数据类型规定了一些特定的操作，但它底层的数据结构是未限定的，甚至可能同时利用了多种数据结构。因此有时一个 **ADT** 利用不同数据结构实现时，在不同情况下会有不同的优势。

### 表

**表（List）** 是用于存放一串数据的结构，其中的元素总能按照顺序排列为 $A_1, ..., A_N$ 的形式，此时这个表的大小是 $N$。我们称大小为 $0$，即没有任何元素的表为 **空表（Empty List）**。下面是这个结构的一些操作：

```pseudocode
class List where
    function List.empty() return Bool
    function List.size() return Int
    function List.front() return Object
    function List.back() return Object
    function List.at(idx) return Object
                          where idx is Int
    function List.push_front(val) where val is Object
    function List.pop_back()
    function List.push_back(val) where val is Object
    function List.pop_back()
```

这些操作的特点在于，只需要直接调用底层结构的相应函数即可。不过现在我们有必要定义一个特殊的 **ADT** 来表示表中元素的位置，以便于后面设计其它操作接口，这就是 **迭代器（Iterator）**。回忆之前在动态数组中我们使用序号来访问元素，而链表中使用结点的地址来访问元素；我们可以将它们抽象为一种类型，这个类型需要：

- 访问元素：这是它最主要的功能。对于动态数组，我们可以用下标运算得到数据；对于链表，我们可以通过解引用得到数据。
- 前进到下一个元素：迭代器为了遍历整个数据结构，需要能够随时找到下一个元素的位置。对于动态数组，我们可以将地址加上元素的大小找到下一个元素的位置；对于链表，我们可以通过 `next` 成员得到下一个元素的位置。
- 后退到上一个元素：我们也可以类似地将迭代器设计为可以倒序遍历数据结构。

```pseudocode
class Iterator Iter where
    function Iter.get() return Object
    function Iter.has_next() return Bool
    function Iter.next() return Iter
    function Iter.has_previous() return Bool
    function Iter.previous() return Iter
```

有了迭代器的定义后，我们可以定义更多表支持的接口：

```pseudocode
class List where
    function List.insert(pos, val) return pos
                                   where pos is Iterator
                                         val is Object
    function List.erase(pos) where pos is Iterator
```

#### 表的其它实现

我们此前已经介绍过动态数组和（双向）链表两种表的实现，除此之外，还有其它可能的数据结构：

- **单链表（Singly Linked List）**：由头指针和只包含 `next` 的结点组成的链表。此时所有 `push_back` 和 `pop_back` 也变成 $O(n)$ 复杂度了。
- **循环链表（Circular Linked List）**：由“头”指针和循环连接的单链结点或双链结点组成的链表。此时我们不需要额外存储尾指针就能访问表的末尾。
- **游标链表（Cursor Linked List）**：给定结点数量上限，用下标代替地址的链表。某种角度来看这是一种数组形式的链表，我们下面会给出简单介绍。

#### 游标链表

在没有地址（**C** 中的指针或 **Java** 中的引用）概念的语言中，我们可以用游标列表来模仿链表的行为：

```pseudocode
type Node where
    next is Int /* index of the next node */
    prev is Int /* index of the previous node */
    data is Object
    
type CursorLinkedList where
    static cursor_space is Array of Node
    head is Int /* index of the first node */
```

下面是游标列表对表 **ADT** 的实现：

```pseudocode
procedure CursorLinkedList() return CursorLinkedList
    result is CursorLinkedList <- {}
    next_idx <- 1
    foreach elem in result.cursor_space
        elem <- next_idx
        next_idx <- next_idx + 1
    Let last element in result.cursor_space be 0
    head <- 0
    return result
    
function CursorLinkedList.empty() return Bool
    return this.head.next = 0
```



### 栈

**栈（Stack）** 是将功能大幅限制之后的表。这个结构只提供对末尾元素的访问和增删操作，它得名于与其行为相似的栈结构。我们可以轻松地通过任意可以实现表的数据结构来实现栈：

```pseudocode
class Stack where
    function Stack.empty() return Bool
    function Stack.top() return Object
    function Stack.push(val) where val is Object
    function Stack.pop()
```

其中，`top` 操作对应了表 **ADT** 中的 `back` 操作；`push` 和 `pop` 则分别对应了表 **ADT** 中的 `push_back` 和 `pop_back` 操作。

#### 栈的应用

- 括号有效性检查：一篇语法正确的文章中，左右括号总是成对出现且相互匹配。在把所有括号以外的字符忽略后，例如 `(()[])()` 的模式是有效的，而例如 `(]{(})` 的模式是无效的。栈是简洁地解决这个问题的得力工具。我们只需要每次遇到左括号时将其 `push` 到栈当中，然后遇到右括号时和栈顶的元素对比，如果括号类型不对应则表明无效，否则将栈顶元素 `pop` 出来。最后，只要栈为空，括号就是有效的。
- 后缀表达式计算：形如 `1 2 * 3 + 4 5 6 * + *` 的表达式被称为后缀表达式。
