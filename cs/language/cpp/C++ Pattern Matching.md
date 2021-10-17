# C++ 模式匹配

[TOC]

**C++** 的模式匹配在很长一段时间是缺失的，先不提 **Haskell** 那样强大且随处可用的，类似 **Java** 那样支持字符串匹配的功能都没有，只能用 `switch` 语句来匹配整型值。模版参数列表确实提供了某种程度上的模式匹配，但是直到 **C++20** 的 **概念（Concept）** 被支持前都不能说非常易用，且其模式匹配仅限于编译期（对浮点数 **非类型模版参数（Non-Type Template Parameter, NTTP）** 则是直到 **C++20** 才支持）。因此，非常有必要引入一系列特性来弥补这些。

不过，比起重新创建一个语法体系（这在 **C++** 中并不少见），我们更希望能够接纳已有的语法，这就是 **C++17** 中引入的 **结构化绑定（Structured Binding）**。可以说，结构化绑定就是 **C++** 标准向模式匹配迈出的一大步：

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

