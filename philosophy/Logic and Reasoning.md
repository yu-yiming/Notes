# Logic and Reasoning

本篇是 *UIUC PHIL 102 Logic and Reasoning*  的学习笔记，其中介绍了论述的相关概念和评估论述的方法。

[TOC]

## Argument

**论述（Argument）** 在英文中根据不同语境有不同含义，可以理解为争吵，或（计算机领域的）函数实参，或辩论时的论述。我们将其定义如下，其中还附带了 **前设（Premise）** 和 **结论（Conclusion）** 两个重要的组成部分：

> An **Argument** is a set of reasons offered in support of a claim.
>
> The set of reasons or the evidence is/are the **Premise(s)**.
>
> The claim that the premise(s) are offered in support of is the **Conclusion**.

此外，我们也给出 **推断（Inference）** 的定义：

> An **Inference** is the mental action of moving from some input information to some output information.

比如，我们看到地上湿就会想到下雨，看到冒烟就会想到着火，这个过程就是推断。推断是一个基本的逻辑过程，经常会出现在论述中。

需要指出，虽然我们定义论述为为了对某个主张给予支持而提供的一系列理由，但并不代表这个论述是合理的，因为为了给予支持不代表实际上给予了支持，这些理由可能是无效的。比如下面这个论述：

- 我们应该停止吃香蕉，因为香蕉拥有意识且伤害有意识的生命是罪恶的

“香蕉拥有意识”作为理由并不正确，因此它不能对“我们应该停止吃香蕉”的主张产生有效支持。但这并不妨碍上面满足论述的定义。我们会在后续小节中介绍如何判断一个论述是否有效。

下面我们再举一些论述的例子：

- 吸烟不健康（结论），因为它会导致肺癌（前设）。
- 199 是一个素数（结论），因为它只能被 1 和其本身整除（前设）。
- 我们知道要么小王，要么小李是凶手（前设）；通过推断发现小王有不在场证明（前设），因此小李是凶手（结论）。

用另一套语系来说明论述的结构，就是每一段论述都有一个论点（即结论），而这个论点是通过一到多个论据（即前设）来支撑的。每当看到一个论述时，我们都去思考这个论述想要说服我们什么，并给出了什么理由，这样就能解析论述的结构。有时一段论述非常长，因此我们需要这样的方法。

我们也可以用更宽泛的方式定义论述，它不需要是一段文字，也可以是一张图，只要其中包含了论点和论据。

### Arguers and Audience

**论述者（Arguer）** 是从论述衍生的词语，它指代提出论述的人。而论述者尝试说服的对象被称为 **听众（Audience）**。对于论述者来说，确认听众群体的特征是非常重要的。这是因为进行论述时，我们会将一些事视作理所应当的，就类似于数学中的公理一样。我们将其称为 **相信体系（Belief System）**。论述中经常会将相信体系中的内容作为前设。但如果听众的相信体系与论述者不同，这就不能形成好的论述。比如一个宗教人士可能会将上帝存在纳入自己的相信体系中，但它对更广泛的听众发表论述时，就不应该将此当作前设，因为不是所有人都信仰上帝。

人与人的相信体系差异可能会非常之大，一个精神病人可能会认为这个世界是他小说中的故事。为了找到一个公有的相信体系的子集，我们假设一群听众都是 **理智的（Reasonable）**，称其为 **普遍听众（Universal Audience）**。

根据听众的特点，我们还可以将其分为 **友善的听众（Sympathetic Audience）** 和 **不友善的听众（Hostile Audience）**。前者对于一个论述倾向于支持，而后者倾向于不支持。如果听众中存在一个个体，其反对一个论述，我们称其为 **对手（Opponent）**。论述者受到对手的批评后，会针对其观点进行回应，然后对手可能再次进行批评。这种你来我往的过程被称为 **辩论（Dialectic）**。

### Argument Dressing

一条论述的结构可以是简单的，也可以复合其它的论述：

> A **Simple Argument** has one or more isolated premises in support of a claim.
>
> An **Extended Argument** has at least one premise, called **Sub-argument** that is supported by some other premises, making it a premise of the main arugment and a conclusion of the sub-argument.

我们可以根据一些关键词来锁定论述中的结构，比如 “作为结果（Consequently）”、“因而（Hence）”、“所以（So）”、“因此（Thus，Therefore）” 等提示接下来的内容是结论；而 “因为（Since，Because，As，For）”、“给定（Given that）” 等提示接下来的内容是前设。不过，也不能一味依赖这些连接词。

## Bias

**偏误（Bias）** 是指对某个观点带有偏向的支持或反对。并不是所有的偏误都是错误的，比如 **确认偏误（Confirmation Bias）**：

> People tend to favor evidence that confirms their current, especially entrenched, beliefs while dismissing evidence that disconfirms their current beliefs. We call this **Confirmation Bias**, which affects both how we gather information and how convincing evidence is once we encounter it, and how strong we see inferences as.

详细来说，确认偏误使得我们首先不会去寻找不利于我们论述的论点，即使是见到了不利的论点，我们也会认为它并没有说服力，或认为其对论述的推断关系很弱。

我们平时更常用的对偏误的理解则是下面这个 **不合法偏误（Illegitimate Bias）**，常直接称为 **偏误（Bias）**：

> A bias that interferes with our ability to reason and to evaluate others' reasoning is called an **Illegitimate Bias**.

需要注意的是，我们所强调的偏误是指 *阻碍理性思考和评估他人观点* 的状态。当一些观点脱离了常规范围时，对其否定并不是偏误。比如如果我声明月球是奶酪做的却无法给出合理证据，此时反对我的观点并非是偏误。最后，拥有观点不代表着偏误，不同人可以拥有自己的观点。有信仰的人声明的宗教学说以及无神论者论述以反对这些学说只是不同的观点，并不构成偏误。

### Vested Interests and Conflicts of Interest

**既得利益（Vested Interest）** 和 **利益冲突（Conflict of Interest）** 是偏误产生的根源之一：

> An arguer has a **Vested Interst** when he or she would benefit from the issues being seen in a certain way.
>
> There is a conflict of Interests when someone is in a position to make a decision that leads to that person unfairly benefiting if issues are seen in a certain light.

既得利益经常和金钱利益相关；利益冲突的例子比如如果一个医生去评判某款药物是否安全，却能因这款药的批准收到报酬，他就处于一个利益冲突的情景。

### Slanting

偏误通常通过几种形式表现出来，主要为两种，**忽略偏颇（Slanting by Omission）** 及 **曲解偏颇（Slanting by Distortion）**：

> **Slanting by Omission** is when an arguer fails to provide information that would weaken his or her view.
>
> **Slanting by Distortion** is when an arguer exaggerates or colors the facts in order to strenthen his or her view.

忽略偏颇常用于将一个人的发言拿出其说话的语境并作为反对他的论据。这种偏误比较难以分辨，因为其内容都是事实。曲解偏颇常出现于政治宣传，可以通过论述者的措辞来分辨；如果其中存在煽动性或情绪化的语言，则很可能存在曲解偏颇。为了避免收到偏颇干扰，可以分析不同角度的观点以得到更全面的认识。

### Understanding the Nature of Bias

根据一些理论，人脑有两个进行思考的进程，它们对所有思考内容进行分工。

其一是快速系统，其进行“思考”的一些例子如下：

- 发现两个物体的远近关系
- 寻找瞬间声源
- 补完短语，比如 “团结就是——”
- 看到恐怖图片后表情扭曲
- 在别人声音中感受到敌意
- 回答简单算术，比如 “1+1=？”
- 阅读大海报牌上的词
- 在空旷的街道上开车
- 理解简单的句子

另一个是缓慢系统，其基于一些规则，意识，且需要注意力集中：

- 在比赛中等待发令枪
- 在吵闹的环境中专心听某个人的声音
- 寻找一个白发的女人
- 在记忆中搜索某个令人惊奇的声音
- 快于平常走路
- 在社交场合监控自己的行为是否得体
- 在一页纸中数某个字出现的个数
- 告诉别人自己的电话号码
- 计算复杂算术，比如“189+981”
- 检查某个复杂论述的合理性

第一个系统相当有用，它来自于生物进化的本能，通常是无意识间进行的，却经常会出现错误。第二个系统则更加先进，我们可以将其定义为 **推理（Reasoning）**。

### Social Bias

有了思考的两种系统，让我们引入 **隐式偏误（Implicit Bias）** 的概念：

> **Implicit Bias** is an unconscious bias that we have toward a member of a group just because they are in that group.

我们通常也称此为 **刻板印象（Stereotype）**。刻板印象可能对其对应群体有好处或坏处：

> A **Stereotype Threat/Lift** is when a group is reminded of a sterotype, they perform worse/better because of it.

刻板印象的例子有很多，其可能依据种族、国籍、性别或兴趣爱好等分类方式作用于不同群体。

## Argument Evaluation

现在让我们尝试评估一个论述（的有效性），以判断一个论述是 **有力的（Strong）** 或 **无力的（Weak）**。首先我们判断在什么情形下需要给出论述？我们将驱使一个人给出论述的动机定义为 **举证责任（Burden of Proof）**：

> A **Burden of Proof** is an obligation to argue for one's view.

作为举例，在美国的法院系统中，嫌疑人总是被预想为无罪的，因此检举方有举证责任以证明其有罪；作为对比，英国的法院系统则要求被告方证明其无罪。通常，我们约定在下列情形中，当事人具有举证责任：

- 当事人给出的论述（的一部分）遭到挑战时，他有义务给出子论述来支持他被挑战的论述（的部分）
- 当事人不承认一个受到公认的观点时

第二条比第一条有更高的优先级，这是因为否认一个公认的观点时总应该给出支持。比如，我给出论述“因为太阳是一颗恒星，而恒星拥有有限的寿命，因此太阳有一天会消失”，而有人质疑我的前设“太阳是一颗恒星”。此时他拥有举证责任，而不是让我举出子论述支持这一点，因为“太阳是一颗恒星”是一个世人公认的观点。

### Strong Arugment

回忆一个论述的组成部分：一系列前设和一个结论，其中前设是为了支持这个结论的。因此如果要从中间找毛病，可以通过下面的思考方式：

- 每一个前设是否有说服力
- 在所有前设都成立的情况下，是否能够自然地得到这个结论

以此为引导，我们可以给出有力论述和无力论述的定义：

> A **Strong Argument** has acceptable premises from which the conclusion follows.
>
> A **Weak Argument** either has i) unacceptable premises, ii) a conclusion that doesn't follow from the premises, or iii) both.

最好的论述毫无疑问拥有无争议的前设以及作为其直接结果的结论；不过很多情况下它们都具有一定不确定性。因此我们对于这些情形下的论述有力性进行分类，其包含了前面介绍的两个部分，**前设可接纳性（Premises Acceptability）** 与 **逻辑推论（Logical Consequence）**：

> **Acceptable Premises** are those that a reasonable audience should accept.
>
> A conclusion is a **Logical Consequence** of premises if the conclusion follows from the premises. In this case, the premises must be sufficient to establish the conclusion’s truth. In other words, supposing the premises are true, they render the conclusion acceptable.

在一个有力论述中，其逻辑推论性质还可以分为两类，**演绎有效性（Deductive Validity）** 与 **归纳有效性（Inductive Validity）**：

> **Deductive Validity**: The connection between premises and conclusion is so strong that the conclusion follows necessarily from the premises. Put another way, an argument is deductively valid if, but only if, it is impossible for the conclusion to be false if the premises are true.
>
> **Inductive Validity**: The connection between the conclusion and premises makes the conclusion likely if the premises are true.

作为例子，“胚胎是无辜的人类生命，杀害无辜的人类生命是错误的，因此堕胎是错误的”拥有一个演绎有效的逻辑推论；“地球在过去45亿年间都绕着太阳公转而没有飞到其它星系中，因此明天地球也不会飞到其它星系中“则有一个归纳有效的逻辑推论。

### Examples

下面是一些和论述有力性判断相关的例子：

- 人们可能在同一个时刻出生却拥有完全不同的命运，因此占星术是不可信的。
- 大型食肉动物都是可敬的生物且对生态系统至关重要，因此我们不应该无差别地毁灭它们
- 这是一个三角形：$\triangle$，因此它有三条边
- 上面第二条的论述中，不可能在前设都为正确的情况下，结论是错误的，因此这个论述不是演绎有效的。

下面我们来逐个分析：

- 前设是可以接受的，因为确实可能有同一时刻出生的人却拥有不同的命运。但是结论显得有些仓促：在此我们对占星术一无所知，它可能不只基于一个人的出生时刻；它可能是*影响*而非*决定*一个人的命运。因此，虽然这个结论显得比较显然，我们需要更多关于占星术的信息来确定这一点
- 第一个前设包含了个人的审美观，不过是可以理解的；第二个前设缺乏一些证据证明大型食肉动物对生态系统的重要性。在两个前设成立的情况下，其结论是可以成立的，因此其构成归纳有效的逻辑推论。
- 前设没有任何问题；一个三角形一定拥有三条边，因此构成了演绎有效的逻辑推论
- 前设是可以接受的，因为第二条论述中，可能出现前设郡正确但结论错误的情况。从这个前设可以直接推导出结论，因此这是一个演绎有效的逻辑推论

### Contextual Relevance and Fallacies

提出论述的上下文可能是多样的，我们在对论述进行评估时，应该确保其上下文和提出论述的上下文是相关的。下面我们将介绍 **谬误（Fallacy）** 的概念，以及一些和上下文相关性有关的谬误：

> A **Fallacy** is a common mistake in reasoning.
>
> The **Red Herring Fallacy** is an attempt to shift attention away from the topic of an argument, in another direction that is not contextually relevant.
>
> The **Strawman Fallacy** is a type of diversion that attempts to shift attention from the proper topic of an argument against an opponent’s point of view by misrepresenting the views that are the subject of criticism

上面所介绍的 **红鲱鱼谬误（Red Herring Fallacy）** 与 **稻草人谬误（Strawman Fallacy）** 的共同点是，它们都偏离了原有的话题。下面我们用例子来区分它们的差别：

- 假设我提出了一个论述：“美国不应该单方面袭击叙利亚，因为这违反了国际法”，而有人反驳：“你不希望美国袭击叙利亚，因为美国没有得到俄罗斯的首肯；但是美国不需要听从俄罗斯的命令”。这属于稻草人谬误，因为我的论述被扭曲并因此收到攻击。
- 假设对于同一个论述，另一个人说：“你可能是一个社会主义者”。那么这就是一个红鲱鱼谬误，因为它和原来的话题完全不是一回事，原来的话题是关于 *美国是否应该袭击叙利亚*，而他试图将话题转为 *我是否是一个社会主义者*。

### Schemes and Deductive Validity

对于一些论述，我们可以从逻辑上就推断出其不合理性，也即其前设的正确性无法保证结论的正确性。下面给出一些例子迅速判断一个论述的逻辑推论是否有效：

- 若 $p$ 则 $q$；$p$，因此 $q$。这个推论是有效的
- 若 $p$ 则 $q$；非 $q$，因此非 $p$。这个推论是有效的
- **肯定结果（Affirming Consequent）**：若 $p$ 则 $q$；$q$，因此 $p$。这个推论是无效的
- **否定前提（Denying Antecedent）**：若 $p$ 则 $q$；非 $p$，因此非 $q$。这个推论是无效的

对于后两种无效推论，我们可以给出很多例子。比如：

- 天上下雨，地上就会湿；地上湿了，因此下雨了。显然往地上洒水也能让地面变湿，下雨不是地湿的唯一诱因
- 每周六餐馆都会歇业；今天不是周六，因此餐馆正常开放。显然餐馆可能在法定节假日也歇业，周六不是餐馆歇业的唯一诱因

如果对数学逻辑比较熟悉的朋友，可能已经认识到，前面列出的四个命题分别对应着原命题、逆否命题、逆命题与否命题。前两者和后两者分别等价，但两组之间没有相互关系。因此在原命题为真的情况下（若 $p$ 则 $q$），只能推断出原命题和逆否问题，另外两个命题无法得到推论。

## Explanation

现在让我们转向一个可能会和论述搞混的概念，**解释（Expalanation）**。我们之前有提到过一些能够反映论述结构的提示词，但它们同样也会在解释中出现。看看下面这些例子，它们具有论述所需的所有内容，但它们是论述么：

- 因为天上下雨了（前设？），因此地上湿了（结论？）
- 我今天迟到了（结论？），因为路上实在太堵了（前设？）

可以看到虽然给出了因果关系，但是这两个命题都没有需要 *证明* 的事情。在论述中给出的结论都是值得争论的，而在解释中给出的结论是既定的事实（地上确实是湿的，以及我今天确实迟到了），听者关心的只有给出的原因。因此我们给出解释的定义：

> An **Explanation** is an attempt to provide the reasons, a.k.a. **Explanans** why something is the way that it is, a.k.a. **Explanadum**. Reasons, in an explanation, shed light on something whereas in an argument, reasons are reasons to believe.

这里面出现的 **解释项（Explanan）** 与 **被解释项（Explanandum）** 与论述中的前设和结论是对应成分。当一句话中的重点是“为什么 $X$ 发生了”，而答案是 $Y$ 时，这就是一个解释，且 $X$ 是解释项，$Y$ 是被解释项。

我们可以通过一句话中不同成分的受争议程度来判断其是论述还是解释：论述中，因为的部分（前设）比所以的部分（结论）争议更小；解释中，所以的部分（被解释项）比因为的部分（解释项）争议更小。
