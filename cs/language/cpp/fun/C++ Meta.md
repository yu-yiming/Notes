# C++ Meta

本篇是利用 **C++** 的模板元编程实现编译期的函数式编程接口的设计笔记。虽然我不清楚最后是不是真的能够完成，但是我非常相信 **C++** 模板的实力。语法大体会参考 **Haskell**，尝试支持惰性求值、模式匹配、高阶函数、柯里化、类族，且类型自身也是一等公民（可以作为函数的参数）。本来我已经实现了一些了（`type_list` 和一些高阶函数），但是遇到了比较严重的设计瓶颈，于是决定还是先通过笔记规划一下，再进行实现。

[TOC]

## 基本结构

### 基本类型

**C++ Meta** 库（以下简称 **Meta**）是一个编译期函数式编程库，其中所有的实体都是 **C++** 中的类型或模板类。这句话意味着 **Meta** 中的常量和变量都是类型。作为例子，一个整数会被写作：

```cpp
using x = integer<42>;
```

可以看到一经定义之后我们无法修改 `x` 绑定的值（也就是 `42`），所以 **Meta** 中所有“变量”都是常量。包括整数类型，**Meta** 支持下面这些基本类型：

- `boolean`：布尔类型，只可能是 `True` 或者 `False`。
- `character`：字符类型，可能是任何 **ASCII** 字符。
- `integer`：整数类型，实现为 64 位有符号整数类型。
- `real`：实数类型，也就是双精度浮点数类型。
- `ratio`：分数类型，分子和分母都是 64 位有符号整数类型。
- `complex`：复数类型，实部和虚部都是双精度浮点数类型。

它们的实现非常直接，就是一个带有非类型模板参数的模板：

```cpp
template<bool val> struct boolean;
template<char val> struct character;
template<int64_t val> struct integer;
template<double val> struct real;
template<int64_t num, int64_t denom> struct ratio;
template<double real, double imag> struct complex;
```

其中每一个模板都提供了静态成员变量 `value`，作为统一的取值接口。前面四个的很好定义，直接取它们的非类型模板参数就行了；后面两个的 `value` 拟定为一个 `std::pair` 的封装，在进行运算时调用特制的接口：

```cpp
class ratio_impl {
public:
    constexpr ratio_impl(int64_t num, int64_t denom) noexcept
        : m_data(std::make_pair(num, denom)) {}
    
    friend constexpr ratio_impl operator +(ratio_impl const& r1, ratio_impl const& r2) noexcept {
        auto [num, denom] = std::pair(
            r1.m_data.first * r2.m_data.second + r2.m_data.first * r1.m_data.second,
        	r1.m_data.second * r2.m_data.second);
        auto const gcd = std::gcd(num, denom);
        num /= gcd; 
        denom /= gcd;
        return { num, denom };
    }
    // other operators
    
    constexpr auto numerator() const noexcept {
        return m_data.first;
    }
    
    constexpr auto denominator() const noexcept {
        return m_data.second;
    }
private:
    std::pair<int64_t, int64_t> m_data;
};

template<int64_t num, int64_t denom>
struct ratio {
    static constexpr auto value = ratio_impl(num, denom);
};
```

此外，我们也需要一个将 **C++** 中的常量转化为 **Meta** 中对应量的“函数”，也就是 `wrap`：

```cpp
namespace detail {
    template<auto val>
    struct wrap {
        using value_type = decltype(val);
        using type = std::conditional_t<
            std::is_same_v<value_type, bool>,            boolean<val>,   std::conditional_t<
            std::is_same_v<value_type, char>,            character<val>, std::conditonal_t<
            std::is_integral_v<value_type>,              integer<val>,   std::conditional_t<
            std::is_floating_point_v<value_type>,        real<val>,      std::conditional_t<
            std::is_same_v<value_type, ratio_impl>,      ratio<val>,     std::conditional_t<
            std::is_complex_v<value_type, complex_impl>, complex<val>,   void>;
    }
}
template<auto val>
using wrap = detali::wrap<val>::type;
```

这里我修改了 `ratio` 和 `complex` 的定义，让其非类型模板参数直接是我们定义的 `ratio_impl` 和 `complex_impl`，这样就和其它基本类型的模板参数列表保持一致。不过要是这样就不能用 `std::pair` 作为底层的存储模型了（因为它的一些实现用到了 `private` 继承）：

```cpp
struct ratio_impl {
    // similar definition as we gave before
    int64_t num, denom;
};

template<ratio_impl val>
struct ratio {
    static constexpr auto value = val;  
};
```

### 列表结构

**Meta** 中的列表结构是指所有模板参数列表为：`template<typename...>` 的模板类。在 **Meta** 中很多地方对列表的要求只需要这一点。我们将以 `tuple` 为例子进行讲解：

```cpp
template<typename... Ts> struct tuple;
```

`tuple` 中可以存放 *任何* 实体，包括 **Meta** 中的变量、类型或函数，因此它是默认的列表结构。我们不像 **Haskell** 中区分 **列表（List）**和 **元组（Tuple）**，因为它们在模板元编程中的行为高度相似，我也就利用了这一点。

```cpp
using tup1e = tuple<integer<10>, std::string, map>;
```

列表结构可以进行一些通用的操作，其中有一些是高阶函数，我们会在后面详细说明。下面是一个不完全的介绍：

- `head`：取列表的第一个元素。
- `tail`：取除去第一个元素的列表。
- `init`：取除去最后一个元素的列表。
- `tail`：取列表的最后一个元素。
- `cons`：将一个元素置于列表的头部，形成新的列表。
- `size`：求列表的长度。这个可以通过 **C++** 内置的 `sizeof...` 运算符轻松得到。
- `nth`：求列表的某一个元素。
- `take`：取列表的前序元素（得到一个列表）。
- `drop`：取除去列表前序元素的列表。
- `split`：以列表中满足特定条件的元素为分隔，返回一系列列表的列表。
- `filter`：取列表中满足特定条件的子序列。这是一个高阶函数，其中的条件是一个返回 `boolean` 的函数。
- `map`：将列表每个元素经过映射后得到新的列表。这是一个高阶函数，其中的映射是一个一元函数。

这些函数可以实现为模板类，通过偏特化（模式匹配）来处理所有情况，让我们以 `cons` 为例：

```cpp
namespace detail {
    template<typename Elem, typename List> struct cons;

    template<typename Head, typename... Tail>
    struct cons<Head, tuple<Tail...>> {
        using type = tuple<Head, Tail...>;
    };
}

template<typename Elem, typename List>
using cons = detail<Elem, List>::type;
```

这里看似没有必要偏特化，但是为了之后的易扩展性（比如 `tuple` 以外的结构也需要用 `cons`），这里一定是要这样写的。不过正如我之前所说，`tuple` 相比其它任何的列表结构没有什么特殊性，所以这里应该将列表类型再做一次抽象，将它提升到偏特化的模板参数里：

```cpp
namespace detail {
    template<typename Elem, typename List> struct cons;
    
    template<typename Head, template<typename...> class List, typename... Tail>
    struct cons<Head, List<Tail...>> {
        using type = List<Head, Tail...>;
    };
}

template<typename Elem, typename List>
using cons = detail<Elem, List>::type;
```

需要注意我们 *没有* 修改这个模板的主要声明（也就是最上面那个声明），`cons` 依然通过一个元素和一个列表来调用。

这个实现已经可以应付一些情况了，我目前已经写出来的就是这样的一些码。不过针对一些 **Haskell** 的应用，这种形式不足以拓展：

- 高阶函数：我们上面定义的（如 `cons`）都是函数，因此它们理应可以作为一些高阶函数的参数；类似地，高阶函数当然也可以成为其它高阶函数的参数。此时如果还在意这些函数的签名（也就是它们的模板参数），我们就得非常痛苦地进行偏特化（我感觉这是不可能的）。
- 部分应用：**Haskell** 支持的部分应用可以让一个函数在参数不足够时返回一个新的函数（比如 `add 1` 会返回 `\x -> 1 + x`），此时函数的参数再次成为累赘。这点和上一点实际上是一致的。

所以一个解决方法是让所有函数变成 *非模板* 类，然后其函数调用通过内部模板类 `apply` 执行，其默认为无限个参数。我们依然用 `cons` 作为例子：

```cpp
namespace detail {
    struct cons {
        template<typename...> struct apply;
        
        template<typename Head, template<typename...> class List, typename... Tail>
        struct apply<Head, List<Tail...>> {
            using type = List<Head, Tail...>;
        };
    };
}

template<typename... Ts>
using cons = detail::cons::apply<Ts...>::type;
```

这种实现对于高阶函数和部分应用不可不谓福音。同时，函数重载也成为可能（尽管这在 **Haskell** 中是不允许的），因为我们可以随时对 `apply` 进行偏特化。这可能会对 **Meta** 的类型系统产生不小损伤，但我决定之后再考虑类型，现在首要的是做出一个（弱类型的）函数式模板元库。

### 函数

我们可以像前面那样定义函数：

```cpp
struct fibonacci_impl {
    template<typename...> struct apply;
    
    template<>
    struct apply<integer<0>> {
        using type = integer<0>;
    };
    
    template<>
    struct apply<integer<1>> {
        using type = integer<1>;
    };
    
    template<int64_t i>
    struct apply<integer<i>> {
        using type = integer<fibonacci_impl<integer<i - 1>>::type::value + 
            fibonacci_impl<integer<i - 2>>::type::value>;
    };
};

template<typename... Ts>
using fibonacci = fibonacci_impl<Ts...>::type;
```

我们也可以利用 lambda 表达式定义函数。这就要求我们定义一个能够像函数一样调用的对象。这理论上并不难：

```cpp
namespace arguments {
    template<int64_t i> struct _;
}

template<typename... Args> 
struct lambda {
    template<typename...> struct apply;
};

struct fibonacci {
    
};
```

