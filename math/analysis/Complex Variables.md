# Complex Variables

本篇是对应 *UIUC MATH 448 Complex Variables* 的学习笔记，其中包括了复数等知识点。



[TOC]

$$
\newcommand{\arg}{\operatorname{Arg}}
$$







## 复平面

### 复数和复平面

复变函数的理论是建立在 **复数（Complex Number）** 上的。定义复数为以下形式的数：
$$
\begin{equation*}
	z = x + iy \in \mathbb{C}
\end{equation*}
$$
其中 $x, y \in \mathbb{R}$ 是我们熟悉的实数，可以分别记作 $\Re(z)$ 和 $\Im(z)$，称为 $z$ 的 **实部（Real Part）** 和 **虚部（Imaginary Part）**。而 $i$ 是满足下面规律的一个数：
$$
\begin{equation*}
	i^2 = 1
\end{equation*}
$$
值得一提的是，根据基本的代数规律，$i'^2 = (-i)^2 = 1$ 也成立，这里的 $i'$ 和 $i$ 是两个不同的数，而将哪一个作为 $i$ 是一个习惯，没有特别意义。

一个常用的理解复数的工具是 **复平面（Complex Plane）**，其等效于一个二维的笛卡尔平面，只不过 $x$ 轴代表复数的实部，而 $y$ 轴代表复数的虚部。复数 $z = x + iy$ 在该平面上等效于一个点 $(x, y)$：

<img src="graphs/image-20220512172625851.png" alt="image-20220512172625851" style="zoom:67%;" />

如上图所示，我们定义复数 $z$ 的 **模（Modulus）** 为原点到 $z$ 的欧几里德距离：
$$
\begin{equation*}
	|z| = \sqrt{x^2 + y^2}
\end{equation*}
$$
复数 $z = x + iy$ 的共轭 $\bar{z}$ 定义为：
$$
\begin{equation*}
	\bar{z} = x - iy
\end{equation*}
$$
显然 $|\bar{z}| = |z|$。同时复数的实部和虚部也可以如下计算：
$$
\begin{equation*}
	\Re(z) = \frac{1}{2}(z + \bar{z}) \qquad \Im(z) = \frac{1}{2}(z - \bar{z})
\end{equation*}
$$
观察此前给出的复平面示意图，如果设 $z$ 和实数轴的夹角（从实数轴逆时针旋转到 $z$ 需要的角度）为 $\theta$，我们就可以给出复数的三角函数表示形式：
$$
\begin{equation*}
	z = |z|(\cos\theta + i\sin\theta)
\end{equation*}
$$
这也称为复数的 **极坐标表示（Polar Representation）**，其中 $\theta \in [-\pi, \pi)$ 称为复数 $z$ 的 **辐角（Argument）**，记为 $\text{arg}\ z$。 举个例子，$z = 1 + i$ 的极坐标表示是 $\sqrt{2}(\cos\frac{\pi}{4} + i\sin\frac{\pi}{4})$。因此这里 $\frac{\pi}{4}$ 就是辐角。值得一提的是另一种定义下的辐角：
$$
\arg z = \{\theta \mid \tan\theta = y/x\}
$$
此时我们得到了一个包含无数个相差 $2k\pi, k \in \mathbb{Z}$ 的角度集。

我们可以根据三角函数的泰勒级数展开得到极坐标表示的一个更简单的形式。考虑 $e^{ix}$ 这个目前尚未定义的表达式：
$$
\begin{align*}
	e^{ix}
		&= \sum_{n = 0}^\infty \frac{(e^{ix})^{(n)}(0)}{n!}x^n \\
		&= 1 + ix - \frac{1}{2}x^2 - \frac{1}{6}ix^3 + \frac{1}{24}x^4 + \frac{1}{120}ix^5 - \dots \\
		&= \left(1 - \frac{1}{2}x^2 + \frac{1}{24}x^4 - \dots\right) + i\left(x - \frac{1}{6}x^3 + \frac{1}{120}x^5 - \dots\right) \\
		&= \sum_{n = 0}^\infty \frac{(-1)^n}{(2n)!}x^{2n} + \sum_{n = 0}^\infty \frac{(-1)^n}{(2n + 1)!}x^{2n + 1}
\end{align*}
$$
上面我们将实数项和虚数项分开，就得到了 $\cos x$ 和 $\sin x$ 的泰勒级数。这也就是鼎鼎大名的 **欧拉公式（Euler's Formula）**：
$$
\begin{equation*}
	e^{i\theta} = \cos\theta + i\sin\theta
\end{equation*}
$$
等式两边在代数上是等价的。后文中我们将会在复数域上建立指数函数，彼时会发现欧拉公式完美地融入了那个系统当中；现在我们暂且认为上式是一个定义。此后我们谈到复数的极坐标表示时，也包括 $|z|e^{i\theta}$ 这种形式。复数 $z = |z|e^{i\theta}$ 的共轭 $\bar{z}$ 可以写成 $|z|e^{i\theta}$。

复数的另一种构造方式是通过多项式商环 $\mathbb{R}[x]/(x^2 + 1)$ 定义的，即：
$$
\begin{equation*}
	\mathbb{R}[x]/(x^2 + 1) = \{a + bx \mid a, b \in \mathbb{R}\}
\end{equation*}
$$
这里的 $a, b$ 就对应了普通形式的 $x, y$。这个定义的巧妙之处在于其 $bx$ 项。考察 $i$ 对应的 $-x$，其平方有：
$$
(-x)^2 = x^2 = (x^2 + 1) - 1 = -1
$$
因此 $a + bx$ 等价于 $a + bi$ 的形式。

下面让我们给出复数的运算性质。

### 复数的运算性质

让我们首先看看复数作为普通形式 $z = x + yi$ 时的运算性质：

- 加法：将两个复数的实部和虚部分别相加，即：
  $$
  z_1 + z_2 = [x_1 + x_2] + i[y_1 + y_2]
  $$

- 乘法：我们利用乘法分配律将两个复数展开为实数和 $i$ 的混合乘积，整理后可以得到：
  $$
  z_1z_2 = [x_1x_2 - y_1y_2] + i[x_1y_2 + x_2y_1]
  $$

- 逆元：复数域的两个单位元和实数一致，是 $0$ 和 $1$。与之对应的两个逆元则是：
  $$
  -z = -x - iy \\
  \frac{1}{z} = \frac{1}{x^2 + y^2}[x - iy]
  $$

- 复数和它共轭的积满足：
  $$
  \begin{align*}
  	z\bar{z} 
  		&= [x + iy][x - iy] \\
  		&= x^2 + iyx - ixy + y^2 \\
  		&= x^2 + y^2 \\
  		&= |z|^2
  \end{align*}
  $$
  这也可以作为复数模的另一种定义。

- 两个复数之积的模等于其各自模的积：
  $$
  \begin{align*}
  	|z_1z_2|
  		&= (x_1x_2 - y_1y_2)^2 + (x_1y_2 + x_2y_1)^2 \\
  		&= x_1^2x_2^2 - 2x_1x_2y_1y_2 + y_1^2y_2^2 + x_1^2y_2^2 + 2x_1y_2x_2y_1 + x_2^2y_1^2 \\
  		&= (x_1^2 + y_1^2)(x_2^2 + y_2^2) \\
  		&= |z_1||z_2|
  \end{align*}
  $$

- 两个复数之和的模满足三角不等式：
  $$
  \begin{equation*}
  	|z_1 + z_2| \le |z_1| + |z_2|
  \end{equation*}
  $$
  其中我们用到了 $|z_2| = |\bar{z}_2|$。证明如下：
  $$
  \begin{align*}
  	|z_1 + z_2|^2 
  		&= (x_1 + x_2)^2 + (y_1 + y_2)^2 \\
  		&= x_1^2 + y_1^2 + x_2^2 + y_2^2 + 2(x_1x_2 + y_1y_2) \\
  		&= |z_1|^2 + |z_2|^2 + 2\Re(z_1\bar{z}_2) \\
  		&\le |z_1|^2 + |z_2|^2 + 2|z_1\bar{z}_2| \\
  		&= |z_1|^2 + |z_2|^2 + 2|z_1||z_2 \\
  		&= (|z_1| + |z_2|)^2
  \end{align*}
  $$
  三角不等式也可以写成差的形式：
  $$
  \begin{equation*}
  	|z_1| - |z_2| \le |z_1 - z_2|
  \end{equation*}
  $$
  其可以通过上面的证明稍微变换得到。
  
- 两个复数之积的共轭等于其各自共轭的积：
  $$
  \begin{align*}
  	\overline{z_1z_2}
  		&= (x_1x_2 - y_1y_2) - i(x_1y_2 + x_2y_1) \\
  		&= (x_1 - iy_1)(x_2 - iy_2) \\
  		&= \bar{z}_1\bar{z}_2
  \end{align*}
  $$

- 两个复数之和的共轭等于其各自共轭的和：
  $$
  \begin{align*}
  	\overline{z_1 + z_2}
  		&= (x_1 + x_2) - i(y_1 + y_2) \\
  		&= (x_1 - iy_1) + (x_2 - iy_2) \\
  		&= \bar{z}_1 + \bar{z}_2
  \end{align*}
  $$

现在，让我们改用极坐标表示。不难猜到，对于复数的乘法，这种表示有奇效：

- 乘法：直接利用指数的运算法则：
  $$
  z_1z_2 = |z_1||z_2|e^{i(\theta_1 + \theta_2)}
  $$
  我们可以用复数的普通形式验证上面的式子：
  $$
  \begin{align*}
  	z_1z_2
  		&= |z_1|(\cos\theta_1 + i\sin\theta_1)|z_2|(\cos\theta_2 + i\sin\theta_2) \\
  		&= |z_1||z_2|[\cos\theta_1\cos\theta_2 - \sin\theta_1\sin\theta_2 + i(\sin\theta_1\cos\theta_2 + \cos\theta_1\sin\theta_2)] \\
  		&= |z_1||z_2|[\cos(\theta_1 + \theta_2) + i\sin(\theta_1 + \theta_2)] \\
  		&= |z_1||z_2|e^{i(\theta_1 + \theta_2)}
  \end{align*}
  $$
  
- 除法：和乘法没有本质的不同：
  $$
  \frac{z_1}{z_2} = \frac{|z_1|}{|z_2|}e^{i(\theta_1 - \theta_2)}
  $$

- 幂：复数的幂在极坐标下非常容易得到：
  $$
  z^n = |z|^ne^{in\theta}
  $$
  以等价的三角函数写出来的形式被称为 **迪莫瓦定理（De Moivre's Theorem）**：
  $$
  (\cos\theta + i\sin\theta)^n = \cos n\theta + \sin n\theta
  $$

- 辐角和：我们发现两个辐角的和出现在了复数之积的地方，因此有：
  $$
  \arg(z_1z_2) = \arg(z_1) + \arg(z_2) \qquad(\text{mod}\ 2\pi)
  $$

至此，我们对复数的常见运算有了基本的认识：加减法类似于向量的运算，而乘除法可以理解为在复平面上的旋转（乘法为逆时针，除法为顺时针）和放缩。复数集作为域的性质也很显然，我们在此不作特别说明了（包括加法和乘法各自的结合律、交换律以及两者之间的分配律）。

最后让我们以根号运算作为本节的结尾。为了解决开平方的问题，我么引入了 $i$，并令 $i^2 = -1$。那么更高次方也能通过复数来表示么？答案是肯定的。事实上，由 **代数基本定理（Fundamental Theorem of Algebra）**，复数域上的任意一元 $n$ 次方程都有 $n$ 个复数解：
$$
\begin{equation*}
	a_0 + a_1x + a_2x^2 + \dots + a_nx^n = 0
\end{equation*}
$$
不过，超过五次的一元方程不存在通解，这里我们只讨论最简单的非平凡情形：
$$
z^n = 1
$$
如果这个问题可以解决，我们就能得到任意实数的所有 $n$ 次方根了。通过极坐标表示，这个方程非常好解。设 $z = re^{i\theta}$：
$$
\begin{align*}
	r^ne^{in\theta} = 1 \implies 
	\begin{cases}
		r = 1 \\
		n\theta = 2k\pi \implies \theta = \frac{2k\pi}{n}
	\end{cases}
\end{align*}
$$
至此我们得到一个 $n$ 边形（其顶点由方程的解在复平面上的点组成）。如果是任意复数的 $n$ 次方根，情况看起来会复杂一些，但并无大碍：
$$
z^n = e^{i\phi}
$$
注意到此前是当前设置的特殊情况（取 $\phi = 0$）。设 $z = e^{i\theta}$（如此前所示，$|z| = r$ 一定为 $1$，因此这里不必设出来了）：
$$
e^{in\theta} = e^{i\phi} \implies \theta = \frac{\phi + 2k\pi}{n}
$$
对于一般的情况 $z^n = w = re^{i\phi}$，我们得到的解是：
$$
\begin{equation*}
	z = \sqrt[n]{r}e^{i(\phi + 2k\pi)/n}
\end{equation*}
$$

### 复平面的子集

本节中介绍一些拓扑的基本知识，这在后续的学习中会作为基本语言。

对于某个复数 $z_0$ 和正实数 $R$，所有满足 $|z - z_0| < R$ 的复数 $z$ 的集合称为以 $z_0$ 为中心，$R$ 为半径的 **开碟（Open Disc）**。如果关于 $w_0 \in D$ 能找到一个开碟 $D_0 \subseteq D$，就称 $w_0$ 是 $D$ 的一个 **内点（Interior Point）**。如果 $D$ 中的所有点都是它的内点，我们就称 $D$ 是一个 **开集（Open Set）**。从名字中就可以看出来，开碟一定是开集：对于半径为 $R$ 的开碟 $D$，取任意的 $w_0 \in D$， 并令 $D_0$ 是以 $w_0$ 为中心，半径为 $R - r$ 的开碟。此时对于任意 $z \in D_0$：
$$
|z - z_0| \le |z - w_0| + |w_0 - z_0| < R - r + r = R
$$
因此我们也有 $z \in D$，这样就符合了开集的条件。

复平面中开集的概念可以认为是实数轴上开区间并的概念扩展。比如 $\{z \mid |z| < 1\}$，$\mathbb{C}$ 都是开集。

记集合 $S$ 的内点集为 $\operatorname{Int} S$，则 $S$ 的 **边界（Boundary）** 定义为 $\overline{\operatorname{Int} S \cap \operatorname{Int} \bar{S}}$。比如 $\{z \mid |z| = 1\}$ 是 $\{z \mid |z| < 1\}$ 的边界。

开集 $S$ 的补 $\bar{S}$ 称为 **闭集（Closed Set）**。需要注意一个集合的开闭性是正交的，比如空集 $\empty$ 即开又闭，而集合 $S = \{z \mid |z| \le 1, \Re(z) > 0\}$ 即不开也不闭。

集合 $S$ 称为 **连通的（Connected）**，若其中任两个元素 $a, b$ 都可以通过某个折线 $L \subseteq S$ 连接。

### 复变函数

本篇笔记的研究重点，是形如 $f(z), z \in D \subseteq \mathbb{C}$ 的函数。由于 $z = x + iy$，且 $w = f(z) = u + iv$，我们实际上可以认为任意复变函数 $f(z)$ 等价于下面的函数组：
$$
\begin{equation*}
	u = u(x, y) \qquad v = v(x, y)
\end{equation*}
$$
由于复数的参与，函数的性质可能和我们的期望不太一样。考虑函数 $f(z) = \bar{z}$，其导数（按照和实分析中的定义）为：
$$
f'(z) = \frac{f(z + \Delta z) - f(z)}{\Delta z} = \frac{\overline{z + \Delta z} - \bar{z}}{\Delta z} = \frac{\overline{\Delta z}}{\Delta z}
$$
当 $\Delta z \in \mathbb{R}$ 时，其结果为 $1$，否则结果为 $-1$。因此 $f(z)$ 不可导。

#### 直线和圆

让我们先从一些简单的图形开始熟悉复变函数。首先是直线。我们知道在二维平面中，直线的通式是 $ax + by = c$，其中 $a, b, c \in \mathbb{R}$。我们可以将式子左侧看作两个复数乘积的实数部分，即令 $\alpha = a - ib$，则有：
$$
\begin{equation*}
	\Re(\alpha z) = c
\end{equation*}
$$
此式便是复平面上直线的表达式。当然，如果需要更加“普遍”的式子，我们可以令 $\beta = -c + id$（这里的 $d$ 是任意的实数），则上式可以写为：
$$
\Re(\alpha z + \beta) = 0
$$
类似地，$\mathbb{R}^2$ 中的圆可以通过 $(x - x_0)^2 + (y - y_0)^2 = r^2$ 表示，不难看出其中和复数相关的模式，令 $z_0 = x_0 + iy_0$，则有：
$$
\begin{equation*}
	|z - z_0| = r
\end{equation*}
$$
根据这个定义不难发现复平面上两点 $p$、$q$ 的中垂线可以定义为：
$$
|z - p| = |z - q|
$$
如果某条曲线上各点与 $p$、$q$ 的距离成固定比例 $\rho \in (0, 1)$，即：
$$
|z - p| = \rho|z - q|
$$
令 $z = w + q$ 及 $c = p - q$，我们可以将上式进行化简：
$$
\begin{align*}
	|w - c| &= \rho|w| \\
	|w|^2 + |c|^2 + 2\Re(w\bar{c}) &= \rho^2|w|^2 \\
	(1 - \rho^2)|w|^2 + \frac{1}{1 - \rho^2}|c|^2 + 2\Re(w\bar{c}) &= \frac{\rho^2}{1 - \rho^2}|c|^2 \\
	\left(w + \frac{c}{1 - \rho^2}\right)^2 &= \frac{\rho}{1 - \rho}|c|
\end{align*}
$$
不难发现这个轨迹是一个圆，其半径为：
$$
R = \frac{\rho}{1 - \rho}|c| = \frac{\rho}{1 - \rho}|p - q|
$$
其圆心为：
$$
\begin{align*}
	z_0 &= q + \frac{c}{1 - \rho^2} \\
	&= \frac{(1 - \rho^2)q + (p - q)}{1 - \rho^2} \\
	&= \frac{1}{1 - \rho^2}p - \frac{\rho^2}{1 - \rho^2}q
\end{align*}
$$
一个引申的优雅结论是，如果令 $C_1$ 是上面所有这些圆的集合（并包括 $\rho = 1$，即 $p$ 和 $q$ 中垂线 $L$ 的情形），且令 $C_2$ 为所有圆心在 $L$ 上并经过 $p$，$q$ 两点的圆的集合，则对于任意 $c_1 \in C_1$ 和 $c_2 \in C_2$，两者均在其交点处垂直。为了简化讨论，让我们变换坐标系，令 $p$，$q$ 在实轴上且和原点距离相等。图示如下：

<img src="graphs/Screenshot from 2022-05-20 21-55-57.png" alt="Screenshot from 2022-05-20 21-55-57" style="zoom:90%;" />

此时我们可以将 $C_1$ 中的元素记为：
$$
\begin{align*}
	|z - p| &= \rho|z + p| \\
	|z|^2 + |p|^2 - 2\Re(z\bar{p}) &= \rho^2|z|^2 + \rho^2|p|^2 + 2\rho^2\Re(z\bar{p}) \\
	(1 - \rho^2)(x^2 + y^2) + (1 - \rho^2)p^2 - 2(1 + \rho^2)xp &= 0 \\
	x^2 + y^2 &= 2\frac{1 + \rho^2}{1 - \rho^2}xp - p^2  \tag{记 $s = \frac{1 + \rho^2}{1 - \rho^2} p$}\\
	(x - s)^2 + y^2 &= s^2 - p^2
\end{align*}
$$
而 $C_2$ 中的元素，若记其圆心为 $i\alpha$ 则有：
$$
\begin{align*}
	|z - i\alpha| &= |p - i\alpha| \\
	|z|^2 + |i\alpha|^2 - 2\Re(iz\alpha) &= |p|^2 + |i\alpha|^2 \\
	x^2 + y^2 + \alpha^2 - 2\alpha y &= p^2 + \alpha^2 \\
	x^2 + (y - \alpha)^2 &= p^2 + \alpha^2
\end{align*}
$$
为了证明 $C_1 \perp C_2$，我们只需证明其交点和两曲线的连线相互垂直即可。熟悉向量分析的同学应该能想到下面的判断方式（也即将复数视作复数域上的向量）：
$$
\Re[\overline{(z - i\alpha)}(z - s)] = 0
$$
计算得到：
$$
\begin{align*}
	\Re[\overline{(z - i\alpha)}(z - s)]
		&= x(x - s) + y(y - \alpha) \\
		&= x^2 - xs + y^2 + y\alpha \\
		&= \frac{1}{2}(x - s)^2 + \frac{1}{2}(y - \alpha)^2 + \frac{1}{2}(x^2 + y^2 - s^2 - \alpha^2) \\
		&= \frac{1}{2}(s^2 - p^2 - y^2) + \frac{1}{2}(p^2 + \alpha^2 - x^2) + \frac{1}{2}(x^2 + y^2 - s^2 - \alpha^2) \\
		&= 0
\end{align*}
$$
至此大功告成。

***

圆和直线之间可以相互转换，通过下面的函数：
$$
\begin{equation*}
	T(z) = \frac{az + b}{cz + d} \qquad a, b, c, d \in \mathbb{C}
\end{equation*}
$$
作为例子，考虑函数 $f(z) = \frac{1}{z}$，以及直线 $z(t) = 1 + it$（即 $\Re(z) = 1$）。此时：
$$
\begin{align*}
	f(z)	
    	&= \frac{1}{1 + it} \\
        &= \frac{1}{1 + t^2} - \frac{it}{1 + t^2}
\end{align*}
$$
令 $u = \frac{1}{1 + t^2}$，$v = \frac{-t}{1 + t^2}$，并观察 $u^2 + v^2$：
$$
\begin{align*}
	u^2 + v^2 
		&= \frac{1}{(1 + t^2)^2} + \frac{t^2}{(1 + t^2)^2} \\
		&= \frac{1}{1 + t^2} \\
		&= u
\end{align*}
$$
因此 $u$ 和 $v$ 之间的关系是：
$$
\left(u - \frac{1}{2}\right)^2 + v^2 = \left(\frac{1}{2}\right)^2
$$
这是一个以 $\frac{1}{2}$ 为圆心，半径为 $\frac{1}{2}$ 的圆，等价的复数表达式是 $|z - \frac{1}{2}| = \frac{1}{2}$。

#### 黎曼球

本节探索从三维中的球面到复平面的 **立体投射（Stereographic Projection）**。考虑一个单位球，令复平面和 $x$-$y$ 平面重合，实轴对应 $x$ 轴，虚轴对应 $y$ 轴。从 $(0, 0, 1)$ 出发向复平面上任一点作直线，都会和单位球交于唯一的一点，因此我们找到了除去 $(0, 0, 1)$ 点外的，从单位球到复平面的一一映射。

对于复平面上的点 $a + ib$，其于对应的单位球上的点的连线可以通过参数方程设出：
$$
L(t) = t(a, b, 0) + (1 - t)(0, 0, 1) = (ta, tb, 1 - t)
$$
因此 $L$ 与单位球的交点 $P$ 满足：
$$
\begin{align*}
	(ta)^2 + (tb)^2 + (1 - t)^2 &= 1 \\
	t^2(a^2 + b^2 + 1) - 2t &= 0 \\
	t &= \frac{2}{a^2 + b^2 + 1}
\end{align*}
$$
因此我们得到了 $P(a, b)$：
$$
P(a, b) = \left(\frac{2a}{a^2 + b^2 + 1}, \frac{2b}{a^2 + b^2 + 1}, \frac{a^2 + b^2 - 1}{a^2 + b^2 + 1}\right)
$$
也可以用极坐标的形式写出：
$$
P(r, \theta) = \left(\frac{2r\cos\theta}{r^2 + 1}, \frac{2r\sin\theta}{r^2 + 1}, \frac{r^2 - 1}{r^2 + 1}\right)
$$
最后，为了将 $(0, 0, 1)$ 也包含进来，我们可以设其映射到复平面的无穷远处。这个设置的重要性在于，单位球本身是一个紧致集（而挖掉 $(0, 0, 1)$ 的单位球不是），因此 $\mathbb{C}\cup\{\infty\}$ 也是一个紧致集。

#### 函数和极限

本节将实变函数的一些概念扩展到复变函数中。函数的两个基本性质是 **作用域（Domain）** 和 **值域（Range）**。前者表示函数有定义的区域，而后者表示函数能够映射到的区域。比如，对于函数：
$$
f(z) = \frac{1 + z}{1 - z}
$$
其作用域显然是 $\mathbb{C}\backslash\{1\}$。为了得到它的值域，我们可以设 $f(z) = w$，则：
$$
\begin{equation*}
	w = \frac{1 + z}{1 - z} \implies w - wz = 1 + z \implies z = \frac{w - 1}{w + 1}
\end{equation*}
$$
发现 $w \in \mathbb{C}\backslash\{-1\}$，这便是 $f(z)$ 的值域。为了了解这个函数的更多性质，我们可以再将其实部和虚部显式写出。令 $z = x + iy$，而 $w = u + iv$，则有：
$$
\begin{equation*}
	u + iv = \frac{1 + x + iy}{1 - x - iy} = \frac{1 - x^2 - y^2 + 2iy}{(1 - x)^2 + y^2}
\end{equation*}
$$
因此有：
$$
\begin{equation*}
	u = \frac{1 - x^2 - y^2}{(1 - x)^2 + y^2} \qquad v = \frac{2y}{(1 - x)^2 + y^2}
\end{equation*}
$$
这个结果的特别之处在于，我们可以发现当 $1 - x^2 - y^2 > 0$ 时，$u > 0$；另外两种情况（等于零或小于零也是一一对应的）。注意到 $x^2 + y^2 = |z|$，因此函数 $f(z)$ 分别将复平面上单位圆内部，边界和外部的点映射到了 $f(z) > 0$、$f(z) = 0$ 和 $f(z) < 0$ 的区域。

复数列的极限定义和我们熟悉的非常相似。设 $z_n = x_n + iy_n$，则：
$$
\lim_{n \to \infty} z_n = A
$$
当且仅当对任意 $\epsilon > 0$ 都能找到 $N$ 使得 $n \ge N \implies |z_n - A| < \epsilon$。不难证明此时必定有：
$$
\lim_{n \to \infty} x_n = \Re(A) \qquad \lim_{n \to \infty} y_n = \Im(A)
$$
极限的和、差、积、商依然满足相应的定律（比如和的极限等于极限的和）。复级数的定义也和我们熟悉的一致：
$$
\sum_{i=0}^\infty z_i = \lim_{N \to \infty}\sum_{j = 0}^N z_i
$$
现在让我们看向复变函数。定义 $f(z)$ 在 $z_0$ 处的极限为：
$$
\lim_{z \to z_0} f(z) = A
$$
当且仅当对任意 $\epsilon > 0$ 都能找到 $\delta > 0$ 使得 $|z - z_0| < \delta \implies |f(z) - f(z_0)| < \epsilon$。这和实分析中的定义依然一致。举个例子，令 $f(z) = \bar{z}$，只需令 $\delta = \epsilon$，便有 $|\bar{z} - \bar{z}_0| = |\overline{z - z_0}| = |z - z_0| < \delta = \epsilon$。

复变函数 $f(z)$ 在 $z_0$ 处连续，当且仅当其在此处的极限等于 $f(z_0)$。连续函数的和、差、积、商依然连续。因此，下面这些函数都是连续的：

- $f(z) = z$。
- $f(z) = z^n$。
- $f(z) = \sum_{k = 0}^N z^k$。

最后让我们讨论一下几何级数：
$$
\sum_{n = 0}^\infty z^n = 1 + z + z^2 + \dots
$$
通过数学归纳法不难证明其前 $N$ 项和为：
$$
S_n = \sum_{n = 0}^N z^n = \frac{1 - z^{N + 1}}{1 - z}
$$
当 $|z| < 1$ 时，观察可以发现其极限为：
$$
\lim_{n \to \infty}S_n =\frac{1}{1 - z}
$$
为了证明这一点，我们应该对于任意的 $\epsilon > 0$ 都能找到某个 $N$ 使得：
$$
n \ge N \implies \left|S_n - \frac{1}{1 - z}\right| < \epsilon
$$
将前面的式子代入可以化简为：
$$
\begin{align*}
	\left|\frac{z^{n + 1}}{1 - z}\right| < \epsilon
\end{align*}
$$
如果取 $r = |z| < 1$，根据三角不等式：
$$
|z| + |1 - z| \ge 1 \implies |1 - z| \le 1 - r
$$
此外 $|z^{n + 1}| = r^{n + 1}$，因此：
$$
\begin{align*}
	\left|\frac{z^{n + 1}}{1 - z}\right| \le \frac{r^{n + 1}}{1 - r}
\end{align*}
$$
只要我们令不等式右侧小于 $\epsilon$，左侧一定满足要求：
$$
\begin{align*}
	\frac{r^{n + 1}}{1 - r} < \epsilon \implies r^{n + 1} < \epsilon(1 - r) \xRightarrow{\ln r < 0} n > \frac{\ln \epsilon(1 - r)}{\ln r} - 1
\end{align*}
$$

#### 指数函数和对数函数

本节中我们将介绍复数域上的指数函数 $e^z$ 和对数函数 $\log{z}$。

指数函数理应有在实数域上相同的性质，即：
$$
e^{z_1 + z_2} = e^{z_1}e^{z_2}
$$
此外在 $\Im(z) = 0$ 的情况下，指数函数应该退化为实数域的情形：
$$
e^{x + i0} = e^x
$$
因此实际上，我们可以将指数上的实部和虚部分开。让我们设 $e^{iy}$ 拥有下面的形式：
$$
e^{iy} = f(y) + ig(y)
$$
对其求导可以得到：
$$
ie^{iy} = f'(y) + ig'(y)
$$
另一方面根据 $e^{iy}$ 的定义：
$$
ie^{iy} = if(y) - g(y)
$$
因此：
$$
\begin{cases}
	f'(y) = -g(y) \\
	g'(y) = f(y)
\end{cases} \implies 
\begin{cases}
	f(y) = -f''(y) \\
	g(y) = -g''(y)
\end{cases}
$$
其通解正是：
$$
f(y) = \cos y \qquad \qquad g(y) = \sin y
$$
至此我们通过不同的方式得到了欧拉公式（对比第一节中用级数的方式得到）。因此复数的指数函数定义为：
$$
\begin{equation*}
	e^{z} = e^x\cos{y} + ie^x\sin{y}
\end{equation*}
$$
这是一个连续函数，因此其所有部分（实数的指数函数和三角函数）都是连续的。复数幂的模只和其实部有关：
$$
\begin{equation*}
	|e^z| = e^x
\end{equation*}
$$
对于复平面上和虚轴垂直的一条直线 $x + i\theta$（这里的 $\theta$ 是常数），其通过指数函数映射到：
$$
e^{x + i\theta} = e^x(\cos\theta + i\sin\theta)
$$
这是一条辐角为 $\theta\ (\text{mod}\ 2\pi) - \pi$ 的从原点出发的射线（不包含原点）。如果是与实轴垂直的直线 $t + iy$（这里的 $t$ 是常数），则其通过指数函数映射到：
$$
e^{t + iy} = e^t(\cos y + i\sin y)
$$
这是一个半径为 $e^t$ 的圆，其半径至少为 $1$（在 $t = 1$ 时取得）。更一般的情况下，对于经过原点的直线 $x + iy$，我们可以得到：
$$
e^{x + iy} = e^x(\cos y + i\sin y)
$$
其形状是一个螺线，起始点为 $e^x$。

***

既然指数函数已经成功定义出来，对数函数实际上就是满足 $e^w = z$ 的解 $w = \log z$。设 $w = u + iv$，我们有：
$$
e^u(\cos v + i\sin v) = |z|(\cos \text(\text{arg}\ z) + i\sin(\text{arg}\ z))
$$
此时有：
$$
\begin{cases}
	u = \ln |z| \\
	v = \text{arg}\ z
\end{cases} \implies \log{z} = \ln|z| + i\ \text{arg}\ z
$$
这个定义看似美好，但它是不连续的，因为 $\text{arg}\ z$ 只能取 $[-\pi, \pi)$ 区间，因此当 $z$ 经过 $-1$ 时，$\text{arg}$ 会从 $\pi$ 陡然变为 $-\pi$。所以一个更通用的定义应该使用 $\arg$：
$$
\text{Log}\ z = \ln|z| + i\arg z = \ln|z| + i\ \text{arg}\ z + 2k\pi
$$
因此，复数的对数有无穷多个。

#### 三角函数和双曲函数

根据指数函数的定义，我们不难得到下面的式子：
$$
\begin{cases}
	\cos x = \dfrac{e^{ix} + e^{-ix}}{2} \\
	\sin x = \dfrac{e^{ix} - e^{-ix}}{2i}
\end{cases}
$$
不过这里的 $x$ 是实数。我们希望这个关系可以延展到复数域中，即将复数的三角函数定义为：
$$
\begin{equation*}
	\cos z = \frac{e^{iz} + e^{-iz}}{2} \qquad \sin z = \frac{e^{iz} - e^{-iz}}{2i}
\end{equation*}
$$
当然，他们可以写成 $x, y$ 的表达式：
$$
\begin{align*}
	\cos z = \frac{e^y + e^{-y}}{2}\cos x - \frac{e^{y} - e^{-y}}{2}i\sin x \\
	\sin z = \frac{e^y + e^{-y}}{2}\sin x + \frac{e^{y} - e^{-y}}{2}i\cos x
\end{align*}
$$
不难验证它们的平方和依然是 $1$（这里不展示了）。值得一提的是，复数域上的三角函数不再映射到 $[-1, 1]$ 区间，而是全复数域。

和三角函数息息相关的另一组函数是双曲函数，其在复数域中的定义为（我们像此前一样将其实数域上的定义拓展了）：
$$
\cosh z = \frac{e^{z} + e^{-z}}{2} \qquad \sinh z = \frac{e^{z} - e^{-z}}{2}
$$
我们很容易发现下面的规律：
$$
\cosh iz = \cos z \qquad \sinh iz = i\sin z
$$
因此此前三角函数的复杂公式可以用双曲函数辅助写出来：
$$
\begin{align*}
	\cos z 
		&= \cosh iz \\
		&= \cosh ix\cosh y - \sinh ix\sinh y \\
		&= \cos x\cosh y - i\sin x\sinh y \\
	\sin z 
		&= -i\sinh iz \\
		&= -i(\sinh ix\cosh y - \cosh ix\sinh y) \\
		&= \sin x\cosh y + i\cos x\sinh y
\end{align*}
$$
最后，让我们给出三角函数和双曲函数的一个颇有趣的几何性质。三角函数也叫圆函数；单位圆上，从原点出发的射线划过的角度 $\Delta\theta = 1$ 对应的圆弧长总是 $1$；类似地，在单位双曲线上，从原点出发的射线划过的角度 $\Delta \theta = 1$ 对应的双曲线弧长总是 $1$。这也是双曲函数得名的由来。

#### 线积分和格林定理

线积分在复变函数中有重要的地位；在介绍它之前，让我们先引入曲线的概念。

一条 **曲线（Curve）** $\gamma(t)$ 是在实轴区间 $[a, b]$ 上定义的复值函数 $\gamma : \mathbb{R} \to \mathbb{C}$。如果对于任意 $a \le t_1 < t_2 < b$ 都有 $\gamma(t_1) \ne \gamma(t_2)$，我们称该曲线是 **简单的（Simple）**。此外，若 $\gamma(a) = \gamma(b)$，则该曲线是 **闭合的（Closed）**。当曲线同时为简单且闭合时，著名的  **Jordan 曲线定理（Jordan Curve Theorem）** 声明其补集是两个不相交集的并，分别称为该曲线的 **内部（Inside）** 和 **外部（Outside）**。我们这里不给出它的证明。

我们可以将 $\gamma(t)$ 写成实部和虚部的和：$\gamma(t) = x(t) + iy(t)$，其中 $x(t)$ 和 $y(t)$ 都是实变函数。当两者都在 $t_0$ 处可导时，我们便称 $\gamma(t)$ 在这一点可导。如果 $\gamma(t)$ 在某个区间可导且 $\gamma'(t)$ 连续，则称其 **光滑的（Smooth）**。如果一条连续曲线由有限个光滑子曲线组成，我们称其为 **分段光滑的（Piecewise Smooth）**。

下面举一个参数化曲线的例子：

![image-20220606101248286](graphs/image-20220606101248286.png)

其由两个不完整的同心圆以及边缘处的连线组成。外部圆的半径为 $R$，内部圆的半径则为 $\epsilon$。“缺口”处角度为 $\delta$。两个圆的方程很容易写出，只需要控制 $\theta$ 的取值e范围即可；从 $B$ 到 $C$、从 $A$ 到 $D$ 的线段则需要引入参数 $t$。需要注意的是，上图中箭头方向提示了参数增加的方向，因此总结得到的参数方程如下：
$$
\gamma: z = 
\begin{cases}
	Re^{i\theta} & \delta \le \theta \le 2\pi - \delta \\
	te^{i(2\pi - \delta)} & R \ge t \ge \epsilon \\
	\epsilon e^{i\theta} & 2\pi - \delta \ge \theta \ge \delta \\
	te^{i\delta} & \epsilon \le t \le R
\end{cases}
$$
现在让我们考虑一段曲线上的线积分。设 $\gamma(t) = x(t) + iy(t)，且 $ $f(z)= u(x, y) + iv(x, y)$。则在 $[a, b]$ 区间上的线积分是：
$$
\begin{align*}
	\int_\gamma f(z)\,dz
	&=\int_{t = a}^b u(x(t), y(t)) + iv(x(t), y(t))\left(\frac{dx}{dt} + i\frac{dy}{dt}\right)\,dt \\
	&= \int_{t = a}^b \left(u\frac{dx}{dt} - v\frac{dy}{dt}\right) + i\left(u\frac{dy}{dt} + v\frac{dx}{dt}	\right)\,dt \\
	&= \int_\gamma (u\,dx - v\,dy) + i\int_\gamma (v\,dx + u\,dy) \\
	&= \int_\gamma (u + iv)\,dx + i\int_\gamma (u + iv)\,dy
\end{align*}
$$
现在，设函数 $P(x, y)$、$Q(x, y)$ 及且其一阶偏导数均连续，则



## 解析函数

### 解析函数与谐函数

在定义域 $D$ 上的复变函数 $f(z)$ 在 $z_0$ 处 **可微（Differentiable）**，如果下面的极限存在：
$$
\lim_{z\to z_0}\frac{f(z) - f(z_0)}{z - z_0} = \lim_{h\to 0}\frac{f(z_0 + h) - f(z_0)}{h}
$$
 如果该函数在 $D$ 上处处可微，则称其为 **解析函数（Analytic Function）**。当 $D = \mathbb{C}$ 时，它也被称为 **整函数（Entire Function）**。值得一提的是，上面的极限并不指定 $z \to z_0$ 及 $h \to 0$ 的路径；只需满足 $|z - z_0| \to 0$ 及 $|h| \to 0$ 即可。

举例来说，$f(z) = z^n$ 在 $n = 1, 2, \dots$ 时是整函数且 $f'(z) = nz^{n-1}$：

$$
\begin{align*}
	(z_0 + h)^n - z_0^n 
		&= nz_0^{n-1}h + \frac{n(n-1)}{2}z_0^{n-2}h^2 + \dots + h^n \\
		&= h(nz_0^{n-1} + \frac{n(n-1)}{2}z_0^{n-2}h + \dots + h^{n-1}) \\
	\lim_{h\to 0}\frac{(z_0 + h)^n - z_0^n}{h}
		&= \lim_{h\to 0} \left[nz_0^{n-1} + \frac{n(n-1)}{2}z_0^{n-2} + \dots h^{n-1}\right] \\
		&= nz_0^{n-1}
\end{align*}
$$
另外一个重要的整函数是指数函数 $e^x$：
$$
\begin{align*}
	e^{z_0 + h} - e^{z_0}
		&= e^{z_0}(e^{h} - 1) \\
	\lim_{h\to 0} \frac{e^{z_0 + h} - e^{z_0}}{h}
		&= \lim_{h\to 0}e^{z_0}\frac{e^h - 1}{h} \\
		&= e^{z_0}\lim_{h\to 0}\frac{e^h - 1}{h}
\end{align*}
$$
设 $h = u + iv$，则：
$$
\begin{align*}
	e^h - 1 - h 
		&= e^u\cos{v} + ie^u\sin{v} - 1 - (u + iv) \\
		&= (e^u\cos{v} - 1 - u) + i(e^u\sin{v} - v) \\
		&= [e^u(\cos{v} - 1) + e^u - 1 - u] + i[e^u(\sin{v} - v) + v(e^u - 1)]
\end{align*}
$$
因此：
$$
\begin{align*}
	\left|\frac{e^h - 1}{h} - 1\right|
		&= \left|\frac{e^h - 1 - h}{h}\right| \\
		&\le e^u\left|\frac{1 - \cos{v}}{h}\right| + \left|\frac{e^u - 1 - u}{h}\right| + e^u\left|\frac{\sin{v} - v}{h}\right| + \left|\frac{v(e^u - 1)}{h}\right| \\
		&\le e^u\left|\frac{1 - \cos{v}}{v}\right| + \left|\frac{e^u - 1 - u}{u}\right| + e^u\left|\frac{\sin{v} - v}{v}\right| + \left|\frac{v(e^u - 1)}{u}\right|
\end{align*}
$$
显然，不等式右侧在 $|h| \to 0$，即 $u, v \to 0$ 时也趋近于 $0$。这样我们就得到：
$$
\lim_{h\to 0}\frac{e^{z_0 + h} - e^{z_0}}{h} = e^{z_0}
$$
故指数函数是整函数，且导数为其本身。

两个解析函数在定义域的交集上之和/积都依然是解析函数。解析函数 $g$ 的值域落在解析函数 $f$ 的作用域上时，两者的复合 $f \circ g$ 也是解析函数。因此，所有只包含幂函数、指数函数、三角函数的复变函数都是整函数（回忆三角函数的指数形式）。

函数的可微性可以推导出连续性。

#### 柯西-黎曼方程组

若 $f = u + iv$ 在 $D$ 上是解析的，则在 $D$ 上有：
$$
\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y} \qquad \frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}
$$
上面的式子就是 **柯西-黎曼方程组（Cauchy-Riemann Equations）**。其证明并不复杂。我们只需在求 $f$ 的导数时，先沿着和实轴平行的方向求导：
$$
\begin{align*}
	f'(z_0) 
		&= \lim_{h\to 0}\left[\frac{u(x_0 + h), y_0) - u(x_0, y_0)}{h} + i\frac{v(x_0 +h, y_0) - v(x_0, y_0)}{h}\right] \\
		&= \frac{\partial u}{\partial x}(x_0, y_0) + i\frac{\partial v}{\partial x}(x_0, y_0) 
\end{align*}
$$
随后，我们沿着虚轴方向求导：
$$
\begin{align*}
	f'(z_0) &= \lim_{k\to 0}\left[\frac{u(x_0, y_0 + k) - u(x_0, y_0)}{ik} + i\frac{v(x_0, y_0 + k) - v(x_0, y_0)}{ik}\right] \\
	&= \frac{1}{i}\frac{\partial u}{\partial y}(x_0, y_0) + \frac{\partial v}{\partial y}(x_0, y_0)
\end{align*}
$$
为了让实部和虚部的结果对应，我们不难得到之前的方程组。

利用柯西-黎曼方程组，我们可以通过解析函数的实部或虚部得到另一部分。举例来说，对于 $u(x, y) = x^3 - 3xy^2$，不难得到 $\partial_x v = 6xy$，$\partial_y v = 3x^2 - 3y^2$，因此 $v(x, y) = 3x^2y - y^3 + c$。不过，显然不是所有的函数 $u(x, y)$ 都能作为解析函数的实部或虚部。如果 $u$ 是 $D$ 上的解析函数 $f$ 的实部，则它满足 **拉普拉斯方程（Laplace's Equation）**：
$$
\begin{align*}
	\Delta u
		&= \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} \\
		&= \frac{\partial}{\partial x}\left(\frac{\partial u}{\partial x}\right) + \frac{\partial}{\partial y}\left(\frac{\partial u}{\partial y}\right) \\
		&= \frac{\partial }{\partial x}\left(\frac{\partial v}{\partial y}\right) + \frac{\partial }{\partial y}\left(-\frac{\partial v}{\partial x}\right) \\
		& = 0
\end{align*}
$$
这样的函数被称为 $D$ 上的 **调和函数（Harmonic Function）**。类似地，能够成为解析函数的虚部的函数，也能成为拉普拉斯方程的解，因此也是调和*函数。

对于调和函数 $u$，如果另一个谐函数 $v$ 和它满足柯西-黎曼方程组，就称它为前者的 **调和共轭（Harmonic Conjugate）**。如果 $v_1$、$v_2$ 是 $u$ 的调和共轭，不难发现：
$$
\frac{\partial }{\partial y}(v_1 - v_2) = \frac{\partial u}{\partial x} - \frac{\partial u}{\partial x} = 0 \\
\frac{\partial }{\partial x}(v_1 - v_2) = -\frac{\partial u}{\partial y} + \frac{\partial u}{\partial y} = 0
$$
因此它们相差一个常数。

现在我们已经认识到函数的解析性使得柯西-黎曼方程组成立。实际上反过来的情况下，即满足该方程组的函数在特定条件下亦能组成解析函数。下面是相关的定理：

> **定理**：如果在一个圆盘 $\Omega$ 上各处 $u$、$v$ 以及其一阶偏导数均连续，则若 $\Omega$ 的中心 $z_0$ 处 $u$、$v$ 满足柯西-黎曼方程组，则 $f$ 在 $z_0$ 处可微。

以上面的定理为基础再做拓展，便有：

> **定理**：如果 $f(z) = u(x, y) + iv(x, y)$ 在 $D$ 上满足柯西-黎曼方程组且 $u$、$v$ 在 $D$ 上可微，则 $f$ 是 $D$ 上的解析

#### 流与场

物理学中，有两个概念可以通过解析函数设立出来；本节中我们将给出它们的性质。

### 幂级数



