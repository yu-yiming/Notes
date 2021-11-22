# C++ 模式匹配

本篇是 **C++** 提案 [P2392](https://open-std.org/JTC1/SC2/WG21/docs/papers/2021/p2392r1.pdf) 的阅读笔记，其中给出了一个可行的 **C++** 模式匹配标准。

[TOC]

## 核心内容

### 引入

**C++** 的模式匹配在很长一段时间是缺失的，先不提 **Haskell** 那样强大且随处可用的，类似 **Java** 那样支持字符串匹配的功能都没有，只能用 `switch` 语句来匹配整型值。模版参数列表确实提供了某种程度上的模式匹配，但是直到 **C++20** 的 **概念（Concept）** 被支持前都不能说非常易用，且其模式匹配仅限于编译期（对浮点数 **非类型模版参数（Non-Type Template Parameter, NTTP）** 则是直到 **C++20** 才支持）。因此，非常有必要引入一系列特性来弥补这些。

不过，比起重新创建一个语法体系（这在 **C++** 中并不少见），我们更希望能够接纳已有的语法，这就是 **C++17** 中引入的 **结构化绑定（Structured Binding）**（可以参考[P0144](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0144r2.pdf)）。可以说，结构化绑定就是 **C++** 标准向模式匹配迈出的一大步：

```cpp
int main() {
    int arr[2] = { 1, 2 };
    // 这里将 arr 拷贝入一个新的数组 brr，然后 first 和 second 是 brr[0]，brr[1] 的引用
    auto [first, second] = arr;
    // rfirst 和 rsecond 是 arr[0]，arr[1] 的引用
    auto& [rfirst, rsecond] = arr;
    
    int i; char c; float f;
    std::tuple<int, char&, float&&> tup(i, c, std::move(f));
    // x 是 int const 类型变量的引用，y 是 char& 类型变量的引用，z 是 float&& 类型变量的引用
    auto const& [x, y, z] = tup;
    
    struct { mutable int a : 2; double volatile b; } temp;
    // temp_a 是一个 2 字节位域常量左值的引用，temp_b 是一个 const volatile 的左值的引用
    auto const [temp_a, temp_b] = temp;
    return 0;
}
```

可以看到，结构化绑定对于数组、元组以及任何有公有成员的（非 `union`）类型对象都可以生效。不过绑定是对整个对象进行的，因此不需要的部分也要为其命名，这点并不算方便。

接下来，让我们从结构化绑定的语法（即 `[]` 包围的标识符列表）出发，来建立一个合适的模式匹配语法体系：

- `[]` 语法同时可以应用于对象结构的拆解（和结构化绑定一致），如 `[a, [b, _], c]`；也可以用于模式的描述，如 `[0, [_, even]]`
- `is` 关键字用于匹配所有 **约束（Constraint）**，比如谓词、具体类型、具体值等
- `as` 关键字用于匹配合法的类型转换

下面让我们用一个 `inspect` 语句的例子来进行演示。`inspect` 语句是一个类似于 `switch` 的语句，只不过其中的 `case` 标签及之后的语句块变为了 `=>` 分隔符分开来的语句：

```cpp
constexpr auto even(auto const& x) { return x % 2 == 0; }
void f(auto const& x) {
 	inspect (x) {
     	i as int 				=> std::cout << "int: " << i;
        is std::integral 		=> std::cout << "non-int integral: " << x;
        [a, b] is [int, int]	=> std::cout << "(int, int) tuple: " << a << " " << b;
        [_, y] is [0, even]		=> std::cout << "point on y-axis with even y:" << y;
        s as std::string		=> std::cout << "string: \"" << s << "\"";
        is _					=> std::cout << "no matching";
    }
}
```

上面出现的 `_` 是一个通配符，在对象结构中出现时，表明忽略当前这个部分；在模式描述中出现时，指不存在约束或类型转换限制。

当然，`inspect` 并不独占模式匹配。上面的语句只是一个语法糖，我们可以将其写为等价的 `if` 语句：

```cpp
void f(auto const& x) {
 	if (auto i as int = x) { std::cout << "int: " << i; }
    else if (x is std::integral) { std::cout << "non-int integral: " << x; }
    else if (auto [a, b] is [int, int] = x) { std::cout << "(int, int) tuple: " << a << " " << b; }
    else if (auto [_, y] is [0, even] = x) { std::cout << "point on y-axis with even y: " << y; }
    else if (auto s as std::string = x) { std::cout << "string \"" << s << "\""; }
    else { std::cout << "no matching"; }
}
```

可以看到，这些语句本身有布尔值的性质，因此也可以尝试使用 `requires` 子句来使用函数重载：

```cpp
void f(auto const& x) requires requires { x as int; }
	{ auto i = x as int; std::cout << "int: " << i; }
void f(auto const& x) requires (x is std::integral)
	{ std::cout << "non-int integral: " << x; }
void f(auto const& x) requires (x is [int, int])
	{ auto [a, b] = x; std::cout << "(int, int) tuple: " << a << " " << b; }
void f(auto const& x) requires (x is [0, even])
	{ auto [_, y] = x; std::cout << "point on y-axis with even y: " << y; }
void f(auto const& x) requires requires { x as std::string; }
	{ auto s = x as std::string; std::cout << "string \"" << s << "\""; }
void f(auto const& x) { std::cout << "no matching"; }
```

### 设计原理

这部分设计的目标是 **概念完整性（Conceptal Integrity）**（来源于 *Brooks* 的[著作](https://en.wikipedia.org/wiki/The_Mythical_Man-Month)），即连贯且可靠地完成用户期待的事情。更细节来说，包括下面几个原理：

- **一致**：不要让类似的东西不一样，包括拼写、行为或能达成的效果。不要让不一样的东西长得相似。比如，本提案中使用和结构化绑定一样的语法，且为两者都扩展出嵌套的语法。
- **正交**：避免随意的耦合，让不同特性可以自由组合。比如，本提案中让 `is` 关键字可以自由地用于所有匹配（包括类型谓词、特定类型、值谓词、特定值等)，让 `as` 关键字用于所有转换（包括直接支持动态类型）。
- **通用**：不要局限于内在，不要随意限制于一集特殊的用法。避免特殊情况和部分特性。比如，本提案中的 `is` 和 `as` 完全可以扩展到类型匹配之外使用，并且可以避免再引入只有单一用处的关键字。

## `is` 和 `as` 表达式

下文中，我们将 `std::remove_cvref_t<decltype(x)>` 统一简写为 `typeof(x)`。同时我们将允许进行解引用，即 `operator*` 或 `operator->` 操作的概念记为 `Pointer`。

### 通用约束表达式：`is`

`is` 表达式提供了一个类型和值匹配的一致语法，其可以静态或动态（或自定义）地进行匹配。

一个类型或值约束 `C` 可以是一个类型谓词（支持 `C<x>` 这样的语法，其中 `x` 是一个类型。比如概念或格式如 `_v` 的类型特质）、特定类型、值谓词（支持 `C(x)` 这样的语法，其中 `x` 是一个表达式）、特定值、或通用的占位符 `_` 用以匹配。

`is` 表达式的语法如下：

```cpp
x is C			// x 是一个表达式或类型
```

`is` 是一个可以重载的运算符，其运算符优先级仅次于成员访问符，和 `&` 位于同一优先级。这个表达式返回一个 `bool` 类型的值，并且有以下性质（每次进行匹配时会从上到下依次判断，如果任一条被满足，则直接返回）：

- 如果 `C` 是占位符 `_`，则返回 `true`。
- 如果 `C` 是一个表达式：
  - 如果 `operator is` 在 `x` 的类型中被重载，则调用这个函数。
  - 如果 `x == C` 是合法的，则返回 `x == C`。
  - 如果 `C` 是一个表达式且 `x is typeof(C)` 是合法的且返回 `true`，则返回 `x as typeof(C) == C`。
  - 如果 `C(x)` 是合法的且其返回值可以转换为 `bool` 类型，则返回 `C(x)`。
  - 如果 `x is Pointer` 且 `x as C` 是合法的，则返回 `x as C is nullptr`（可以参考 `as` 介绍）。
  - 如果 `x is Pointer` 成立且` C` 是一个被 `&` 修饰的类型（引用类型），则返回 `x as C` 且不会抛出异常。
  - 如果 `C<x>` 是合法的且返回值可以转换为 `bool` 类型，则返回 `typeof(x) is C`。
  - 否则 `x is C` 就是非良构的。
- 如果 `C` 是一个类型（这是剩下的唯一情况）：
  - 如果 `C` 是一个特定类型，则返回 `std::is_same_v<C, x>`。
  - 如果 `C` 是一个模版名，则在 `x` 是 `C` 的一个特化时返回 `true`，否则为 `false`。
  - 如果 `C<x>` 是合法的且可以转换为 `bool`，则返回 `C<x>`。
  - 否则 `x is C` 就是非良构的。

有五种 `is` 表达式可能成立的语法情形：

- `type-id is type-id`
- `type-id is type-constraint`
- `expression is expression`。这种情况包含了 `expression is value` 和 `expression is value-constraint` 两种情况。
- `expression is type-id`
- `expression is type-constraint`

当 `x` 是可以通过结构化绑定分解的对象时，我们可以将 `is` 右侧的部分进行解构：

```cpp
x is [C1, C2, ..., Cn]		// 这个式子合法的充要条件是 auto&& [x1, x_2, ..., xn] = x; 可以通过编译
    						// 它等价于 x1 is C1 && x1 is C2 && ... && xn is Cn
```

不过，为了和 lambda 表达式不发生冲突，`is` 关键字后面紧跟的 lambda 表达式应该加上小括号。

#### `is` 用于变量声明和结构化绑定

为了通用性，我们允许对结构化绑定和 `is` 表达式都使用可嵌套的解构语法，比如：

```cpp
std::pair<int, std::pair<int, int>> data;
auto [a, [_, c]] = data;		// 结构化绑定
if (data is [_, [1, _]]) {		// is 表达式
    // 省略细节
}
```

`is` 表达式用于 `if` 语句时，当且仅当其返回值为 `true` 时才会进行初始化操作：

```cpp
if (auto&& [a, [_, c]] is [_, [1, _]] = data) {		// 如果匹配失败，这个分支就不会进行，也就无需进行初始化了
    // 省略细节
}
```

不过其它的情形和结构化绑定一致，也即必须满足匹配要求，否则会抛出异常（或直接不通过编译）：

```cpp
auto&& [a, [_, c]] is [_, [1, _]] = data;	// 这里要求 is 必须匹配成功
auto i is std::integral = f();				// 要求 is 必须匹配成功
```

你或许已经察觉这和 **C++20** 中利用概念声明变量（或直接使用存储类型）颇为类似，下面我们进行一个对比：

```cpp
int a = f();						// 编译期检查 f() 返回值必须可以隐式转换为 int 类型
std::intgeral auto b = g();			// 编译期检查 g() 返回值必须满足 std::integral 概念
auto c is int = f();				// 编译/运行期检查 f() 返回值必须可以隐式转换为 int 类型
auto d is std::integral = g();		// 编译/运行期检查 g() 返回值必须满足 std::integral 概念
```

可以看到，概念用于变量声明时，所有的检查都是编译期的，出错一定是以编译期错误的形式出现，而 `is` 表达式并不要求这一点，因此可能会在运行期抛出异常。类似地，在 `if` 语句中，它们的行为也有这样的异同：

```cpp
if (int a = f()) {}					// 编译期检查
if (std::integral auto b = g()) {}	// 编译期检查
if (auto c is int = f()) {}
if (auto d is std::integral = g()) {}
```

最后，我们可以将 `is` 表达式写在初始化器中，它的含义不言自明：

```cpp
auto flag = v is std::pair<int, int>;
```

#### `is` 的用例

在 `if` 表达式中：

```cpp
// 一个约束生成器
constexpr auto in(auto min, auto max) {
    return [=](auto const& x) { return min <= x && x <= max; };
};
// 完全静态（编译期）的等价版本
template<auto min, auto max>
constexpr bool in(auto const& x) { return min <= x && x <= max; };

void test() {
    if (x is 3) {}							// 判断是否为特定值
    else if (x is in(1, 2)) {}				// 判断是否满足一个值约束。如果需要编译期生成结果可以写成 int<1, 2>
    else if (x is std::pair<int, int>) {}	// 判断是否为特定类型
    else if (x is std::pair) {}				// 判断是否为特定模版，等价于 x is std::pair<_, _>
    else if (x is std::integral) {}			// 判断是否满足一个类型约束，等价于 x is std::integral<typeof(x)>
}
```

在 `requires` 子句中：

```cpp
template<typename T, auto Size>
auto make_array() { /* 省略定义 */ }			// 无约束的模版
template<typename T, auto Size>
	requires Size is 3							// 要求特定值 
auto make_array() { /* 省略定义 */ }
template<typename T, auto Size>
	requires Size is in(1, 2)					// 要求满足值约束
auto make_array() { /* 省略定义 */ }
template<typename T, auto Size>
	requires (Size is std::pair<int, int>)		// 要求是特定类型
auto make_array() { /* 省略定义 */ }
template<typename T, auto Size>
	requires (Size is std::pair)				// 要求是特定模版，等价于 requires (Size is std::pair<_, _>)
auto make_array() { /* 省略定义 */ }
template<typename T, auto Size>
    requires Size is std::integral		// 要求是特定类型约束，等价于 requires Size is std::integral<typeof(x)>
auto make_array() { /* 省略定义 */ }
```

最后，`is` 是一个上下文关键字，因此可以将其声明为一个变量：

```cpp
int is = 42;
assert(is is int);				// 没有问题
```

### 通用转换表达式：`as`

`as` 表达式提供了通用的转换语法，支持静态、动态或自定义的转换方式。我们约定将 `refto(T, x)` 定义为：

- `T` 是引用类型时，返回 `T`。
- 如果 `x` 是一个左值，则返回 `T&`。
- 如果 `x` 是一个右值（这是剩余的唯一情形），则返回 `T&&`。

`as` 表达式的语法如下：

```cpp
x as T						// x 是一个表达式
```

`as` 和 `is` 拥有相同的运算优先级，`T` 是一个类型谓词或特定类型。这个表达式返回类型是 `refto(T, x)`，详细如下（会从上到下判断，一旦有一个满足则返回该结果）：

- 如果 `std::is_same_v<T, typeof(x)>`，则返回 `x` 的引用。
- 如果 `x` 可以被 `refto(T, x)` 类型的变量绑定，则返回一个绑定到 `x` 的 `refto(T, x)` 类型引用。
- 如果 `operator as<T>(x)` 或 `x.operator as<T>()` 有定义（被重载），则返回它的结果。
- 如果 `typeof(x)` 可以隐式转换为`T`，则返回 `T` 类型的右值，其为 `x` 转换为 `T` 类型后的值。
- 如果 `T(x)` 是合法的且结果可以被解引用（即可以使用运算符 `operator *`），则返回 `*T(x)`。
- 如果 `dynamic_cast<refto(T, x)>(x)` 是合法的，则返回这个转换后的值。
- 如果 `dynamic_cast<T>(x)` 是合法的，则返回这个转换后的值。
- 如果 `!(x is Pointer)` 且 `T(x)` 合法，则返回 `T(x)`。
- 否则 `x as T` 就是非良构的。

有两种 `as` 表达式可能成立的情形：

- `expression as type-id`
- `expression as expression`

和 `is` 表达式类似，右侧的部分可以被解构：

```cpp
x as [C1, C2, ..., Cn]		// 当且仅当 auto&& [x1, x2, ..., xn] = x; 可以通过编译时才合法
    						// 它等价于 forward_as_tuple(x1 as C1, x2 as C2, ..., xn as Cn)
```

#### `as` 用于变量声明和结构化绑定

这部分和 `is` 的用法比较相似：

```cpp
std::variant<std::pair<int, int>, std::string> v;
auto&& [a, b] as std::pair<int, int> = v;			// 要求 as 表达式必须匹配成功
if (auto&& [a, b] as std::pair<int, int> = v) {		// 检查 as 表达式是否成功匹配，若否则不进入分支（也不赋值）
    // 省略细节
}
```

我们可能会不禁将上面的用法将显示写出存储类型作比较：

```cpp
int a = f();					// 要求 f() 返回一个可以隐式转换为 int 的值（比如通过构造函数）
auto a as int = g();			// 要求 g() 返回一个可以强制转换为 int 的值（比如将 std::variant<int, std::stirng> 转换为 int
```

`if` 语句的情况是类似的。最后，`as` 表达式也可以写在初始化器中：

```cpp
auto [a, b] = v as std::pair<int, int>;
```

#### `as` 的用例

我们可以利用 `as` 完成显式参数转换：

```cpp
void f(std::string);
char a = 'a';
f(std::string(a));			// 错误！不允许将构造函数像函数式类型转换一样使用（尽管理应没有问题）
f(std::string{a});			// 使用列表初始化
f(a as std::string);		// 没有问题
```

在有继承关系的类型中切换会更加直观：

```cpp
namespace NS {
	struct A { void f() {} };
    struct B : A {};
    struct C { B i; };
}
struct D : NS::C {
    void foo() {
        this->NS::C::i.NS::A::f();				// 只使用 ::
        ((*this as NS::C).i as NS::A).f();		// 利用 as 表达式
    }
}
```

和 `is` 关键字一样，`as` 是一个上下文关键字，因此可以定义同名的标识符：

```cpp
short as = 42;
assert(as as int == 42);
```

### `is/as` 和动态类型：自定义以及 `std`

