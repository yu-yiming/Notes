# Sugar C++

本篇是精简并扩充 **C++** 的一个尝试。众所周知 **C++** 中存在大量历史相关的别扭设计，无论是语言上还是库上的设计都需要谨小慎微。尽管委员会（尤其是近几年）作出了很大的努力，接纳了不少现代的设计，但在一些相对激进的设计上还是不得不维持保守。同时由于精力有限，一些很有帮助，却没有深刻意义的提案都被推却了。

我这里给出一个贴近现代 **C++** 语法风格的语言提案，对 **C++** 现有的诸多设计进行个人向的改良。让我们从 Hello World 程序开始：

```cpp
import prelude;

auto main = () -> {
    std::print("Hello World!");
    return 0;
}
```

这里，我们采用 **C++20** 起支持的模块导入语句，将 `main` 声明为一个函数指针，指向等号右侧的闭包。我采用了 **C++11** 起支持的 `auto` 占位符，因此不需要显示写出 `main` 的类型（`int (*)()`）；同时 lambda 表达式比起 **C++** 中的更为清晰，只需简单对比就能看出：

```cpp
// C++11
auto foo = []() { return 42; };
// Sugar C++
auto foo = () -> 42;
```

我决定将捕获列表的形式以另一种方式提供出来，这里暂时不讨论。

[TOC]



## 变量声明与定义

**C++** 的变量声明沿自 **C**，采用类似下面的形式：

```
[some-specifiers] [declarator], [more-declarators];
```

看起来很简单是不是，但是要把下面这些全部囊括：

- `static const int*& x;`
- `char (*const arr)[3], ch;`
- `inline void* (*foo)(int (*)() arg, ...);`
- `std::vector<int>::const_iterator it;`

不难想象其中规则的复杂程度。不过好在，**C++11** 引入了类型占位符 `auto`，其解决了多数情况下声明中的信息冗余：

```cpp
int const x = 42;
void foo(int (*func)(), ...);
std::vector<int> vec;

int main() {
    static auto& r = x;
    auto fooptr = &foo;
    auto it = vec.cbegin();
    return 0;
}
```

**Sugar C++** 中沿用这种便捷的写法，只不过其默认修饰符不同：我们假定对所有变量都采取 `const` 修饰符：

```cpp
auto main = () -> {
    auto x = 42;
    auto p = &x;
    auto r = $x;		// Sugar C++ 中通过 $ 一元运算符得到变量的引用
    x = 24;				// 错误，x 不是可修改的变量
    p = &x + 1;			// 错误，p 不是可修改的变量
    r = 24;				// 错误，r 引用的变量不是可修改的变量
    return 0;
}
```

如果不需要顶部默认的 `const` 修饰符，可以使用符号 `~`：

```cpp
auto main = () -> {
    auto x = ~ 42;
    x = 24;
    return 0;
}
```

注意，**Sugar C++** 中的引用和 **C++** 中依然一致，其只能是 `const` 的。因此 `~` 对其没有效果（或者理应报错）。不过我们可以让它引用的东西可修改，前提是该对象本身可以修改：

```cpp
auto main = () -> {
    auto x = ~ 42;
    auto y = 42;
    auto r = $~ x;
    auto s = $~ y;		// 错误，s 引用的变量不是可修改的变量
    return 0;
}
```

和 **C++** 不同，**Sugar C++** 中并不要求 `auto` 声明语句中的变量拥有相同的（类型等）限定符，`auto` 在此的作用比占位符更加随意：

```cpp
auto main = () -> {
    auto i = 42, s = "abc", p = &i;
    return 0;
}
```

至此，我们了解了在有初始值的情况下如何声明一个变量，那么如果没有初始值，也即默认初始化的情形呢？为了形式上的统一，我并不打算采纳 **C++** 的原始方案（就是 `int x;` 这样的）。使用冒号是一个不错的选择：

```cpp
auto x : int;
auto y : [char];
auto z : int -> char;
```

可以看到我们使用了和 **C++** 迥异的类型组合方式：通过 `[]` 表示数组类型，而 `->` 表示函数类型。我们有必要详细说明这一点：

- `[type]` 类型的变量是 `type` 的数组类型，其约等于 **C++** 中的 `type[]`。不过，**Sugar C++** 中的数组长度并不记入数组对象的类型当中，其更像是 **C++** 中指向数组的指针。
- `input_t -> output_t` 类型的变量是接收 `input_t` 类型，输出 `output_t` 类型的函数指针。和 **C++** 一样，函数依然不是一等公民，但我们可以像使用函数一样使用函数指针。 
- `type_1 | type_2` 类型的变量是一个 Sum Type，可以认为和 `std::variant<type_1, type_2>` 类似。
- `type_1 & type_2` 类型的变量是一个 Intersection Type，**C++** 中没有同样的类型支持，不过可以通过 Concept 实现类似的功能。
- `(type_1, type_2)` 类型的变量是一个 Product Type，可以认为和 `std::tuple<type_1, type_2>` 类似，但可以类似 **Haskell** 通过 `,` 运算符构建和模式匹配。
- `[:type:]` 类型的变量是 `type` 的列表类型，其约等于 **C++** 中的 `std::list<type>`，但可以类似 **Haskell** 通过 `:` 运算符构建和模式匹配。
- `&type` 类型的变量是 `type` 的指针类型，其等价于 **C++** 中的 `type*`，可以通过 `&` 运算符构建和模式匹配。
- `$type` 类型的变量是 `type` 的引用类型，其等价于 **C++** 中的 `type&`，它没有相应的匹配模式。
- `>>type` 类型的变量是 `type` 的右值引用类型，其等价于 `C++` 中的 `type&&`，它没有相应的匹配形式。

上面列出的 Sum Type 和 Product Type 都可以进行长连接，并通过括号进行分组。比如：

```cpp
(int, float | bool | (int | (std::string, [char])))
```

就是一个复杂的复合类型。函数类型虽然可以进行长连接，但它是柯里化的实现，行为和 **C++** 中普通的函数不同。比如：

```cpp
char foo(int i, double d);
```

在 **Sugar C++** 中等价于：

```cpp
auto foo : (int, double) -> char
```

而下面的写法：

```cpp
auto foo : int -> double -> char
```

约等于 **C++** 中的这种写法：

```cpp
char (*( (*foo)(int) ))(double);
```

或者写成下面这样可能更好懂一些（**C++11**）：

```cpp
auto foo(int) -> char (*)(double);
```

### 初始化

**C++** 中的初始化就是一团乱麻。**Sugar C++** 希望改善这一点。我们只允许两种形式的初始化，那就是：

```
[specifiers] auto [var-name] = [ctor]([arg-list]);
[specifiers] auto [var-name] = [expression]
```

其中后者等价于调用只有一个参数的构造函数。什么直接初始化、列表初始化、聚合初始化，都走一边去吧…… 所有的初始化必须写成上面两种形式之一，这也显得简单易懂。举个例子：

```cpp
std::vector<int> v1{1, 3};
std::vector<int> v2{1, 2, 3};
```

上面的两个 `std::vector<int>` 被分别初始化为 `[3]` 和 `[1, 2, 3]`，因为第一个是通过构造函数 `vector(ct, arg)` 构造的，而第二个是通过 `vector(init-list)` 构造的。在 **Sugar C++** 中，它们会被写成：

```cpp
auto v1 = vector<int>(1, 3);
auto v2 = [1, 2, 3];
```

非常直观清晰，也不容许任何歧义存在。

### 生命周期

**C++** 中对象的生命周期分为下面几种：

- 自动生命周期：受所在作用域控制。
- 全局生命周期：在程序运行过程中始终存在。在命名空间作用域的变量和被 `static` 修饰的变量拥有全局存储周期。
- 动态生命周期：受程序员手动控制。

**C++** 采用的 **RAII（Resource Acquisition Is Initialization）** 是控制对象生命周期的主要方式，其通过构造函数分配资源，再通过析构函数释放资源。**Sugar C++** 采用完全相同的习惯：

```cpp
auto Int = struct {
	ctor = (x : int) -> {
        this.x = new int(x);
    }
    dtor = () -> {
        delete this.x;
    }
    .x : &int
}

auto main = () -> {			// 全局生命周期
    auto x = 42;			// 自动生命周期
    auto p = new int(42);	// 动态生命周期，需要手动释放，这不是 RAII 风格的代码
    auto y = Int(42);		// 自动生命周期，虽然其成员变量是动态生命周期，但是受到 RAII 控制
    global auto z = 42;		// 全局生命周期
    delete p;
}
```

### 链接类型

**C++** 中变量的 **链接类型（Linkage）** 描述了它在同文件和不同文件的可视性：

- 无链接：仅在当前其定义所在作用域中可视。
- 内部链接：仅在当前编译单元中可视。在命名空间作用域被 `static` 修饰、被 `const` 修饰且非 `inline`、匿名命名空间中的变量等拥有内部链接（从这里其实可以看到 **C++** 的过度复杂性）。
- 外部链接：在其它编译单元中也可视。在命名空间作用于不被 `static` 修饰，或通过 `extern` 声明的变量拥有外部链接。
- 模块链接（**C++20**）：仅在模块单元中可视。

**Sugar C++** 中，不存在外部链接和模块链接的分别；所有命名空间作用域中的变量被 `extern` 修饰时是外部链接，被 `intern` 修饰为内部链接；此外均为无连接。默认情况下，任何命名空间中变量的链接都是内部链接；可以使用限定符标签（类似于 `public` 和 `private` 那样的用法）来改变接下来的默认链接性。

```cpp
extern auto x = 42;			// 外部链接
auto y = ~ 42;				// 内部链接

extern:
auto main = () -> {			// 外部链接
    auto x = 42;			// 无链接
    global auto y = 42;		// 无链接
    return 0;
}
namespace foo {				// 外部链接
    auto x = 42;			// 内部链接
extern:
    auto y = 42;			// 外部链接
}
intern:
namespace bar {				// 内部链接
    extern auto x = 42;		// 内部链接
}
```

这里我使用的只有 `extern` 和 `intern` 两个关键字，不需要考虑其是否被 `static`、`inline`、`const`、`constexpr` 等修饰，简化了很多。不过，由于这些关键字牵扯链接类型有深层的原因，我们不能简单一刀切，还是要讨论如何处理 **Sugar C++** 中这些限定符出现的场景：

- `inline` 在 **C++** 中拥有外部链接；不过我们依然可以在不同编译单元中定义多个 `inline` 的变量/函数。此时它们的定义应该完全相同，因为在编译过程中只有一个定义会留下来并作为唯一定义。不过，如果变量/函数被 `static inline` 修饰，它的行为就和 `static` 类似，只在当前编译单元中可视且允许不同编译单元拥有同名变量/函数的多个定义。因此在 **Sugar C++** 中，头文件中出现的 `inline` 变量应该被 `extern` 修饰（以确保只需一个定义），否则可以是 `extern` 或 `intern` 均可。
- `static` 在 **C++** 中既表示链接类型，也表示变量的存储类型（生命周期）。**Sugar C++** 中它没有这两种中的任一个意义（我们马上会看到它实际的意义），因此不会出现问题。
- `const` 在 **C++** 中是内部链接（和 **C** 的默认行为不同），这是因为 `const` 的编译期可知特性让其倾向于在头文件中出现；彼时没有 `inline` 变量（类似于 `inline` 函数）的概念，因此只能将 `const` 默认为内部链接，以防止破坏 **ODR**。**Sugar C++** 中不区分变量 `const` 与否对链接性的影响。头文件中出现的定义将被自动修饰为 `inline`。

## 基本类型

**C++** 的基本类型包括布尔类型、整数、浮点数、指针、引用、数组等。不过由于历史原因，整数类型的使用不是很令人愉快：`int` 可能是 2 字节或 4 字节，`long` 可能是 4 字节或 8 字节。此外，`char` 可能是有符号或无符号，`unsigned`、`short`、`long` 这些相对随意的命名也会让人摸不着头脑。在许多其它语言中（如 **Rust**），整数类型都用非常简单清晰的 `i32`、`ui64` 等方式记录。我考虑在 **Sugar C++** 中引入类似的记号：

- `i[num]`：`num` 位有符号整型。这里的 `num` 只能是 8、16、32、64、128 之一。
- `ui[num]`：`num` 位无符号整型。这里的 `num` 只能是 8、16、32、64、128 之一。
- `c[num]`：`num` 位无符号字符类型。这里的 `num` 只能是 8、16、32 之一。

此外，为了依然贴合传统的用法，引入下面这些类型：

- `int`：等价于 32 位有符号整型。
- `uint`：等价于 32 位无符号整型。
- `size_t`：等价于当前机器中最大的无符号整型（这和 `std::size_t` 定义是一样的）。
- `ssize_t`：等价于当前及其中最大的有符号整型。

浮点类型则只有下面几种：

- `f32`：单精度浮点数类型，等价于 `float`。
- `f64`：双精度浮点数类型，等价于 `double`。
- `f128`：四精度浮点数类型，等价于 `long double`。

当然，还不能忘记一些简单但重要的类型：

- `bool`：布尔类型，只可能是 `true` 或 `false`。**C++** 中它的实现一般是一个整数；**Sugar C++** 中将其实现为一个枚举。
- `null_t`：空类型，可以认为类似于 `std::nullptr_t`。

### 指针和引用

**C++** 中指针和引用占有特殊的地位，不过两者的一些性质不太易用：

- 指针不一定是合法的：它可能是空指针，也可能指向未分配的内存。
- 引用没有默认值，虽然 `std::optional<std::reference_wrapper<type>>` （**C++17**）是一个解决方案，但未免太麻烦了点。

因此我将两者进行了魔改。指针的可能状态只有两种：`&data` 或 `null`。这里的 `data` 一定是合法的内存：

```cpp
auto test_pointer = (ptr : &int) -> {
    inspect (ptr) {
        &data -> {
            std::print("The data is {}", data);
        }
        null -> {
            std::print("No valid data!");
        }
    }
}

auto main = () -> {
    auto x = 42;
    auto p1 : &int, p2 : &int, p3 : &int;
    p1 = &x;				// 指向栈内存。
    if (true) {
        auto tmp = 42;
        p2 = &tmp;			// 一旦在分支中进行这样的赋值，p2 的作用域会降级到当前作用域中。
    }						// 作用域结束时，会尝试对 p2 进行内存释放，但其指向栈内存，因此无果，将其设置为 null
    p3 = new int(42);		// 指向堆内存。
    test_pointer(p1);		// 输出 The data is 42
    test_pointer(p2);		// 输出 No valid data!
    test_pointer(p3);		// 输出 The data is 42
}							// 作用域结束时，会尝试对 p1, p2, p3 进行内存释放
```

可以看到，所有指针在其绑定对象的作用域结尾处都会被释放；如果指针在嵌套的作用域中被赋值，则无论其是否一定执行到该作用域，它都会在此作用域结尾处释放并置为 `null`。不过这样我们应该如何从作用域中将内容传递出去呢？

第一种方式是用引用。引用相比指针的一大好处在于，它的作用域无法被更改：

```cpp
auto main = () -> {
    auto x = 42;
    auto r = $x;
    if (true) {
        auto tmp = 42;
        r = tmp;
    }
}
```

第二种方式是利用模式匹配。实际上，模式匹配 `&data` 中的 `data` 就是引用类型，这和 **C++** 中是完全一样的：

```cpp
auto main = () -> {
    auto x = 42;
    auto p = &x;
    if (true) {
        auto tmp = 42;
        inspect (p) {
            &data -> {
                data = tmp;
            }
            null -> {}
        }
    }
}
```

至此大概也能看出来，我废弃了 **C++** 中的解引用运算符，必须通过模式匹配才能得到其中的值。当然，可以通过判空运算符 `?` 来简化这一过程：

```cpp
auto main = () -> {
    auto x = 42;
    auto p = &x;
    if (true) {
        auto tmp = 42;
        p? = tmp;			// 仅当 p 不为 null 时，将其指向内存赋值为 tmp
    }
}
```

 这里的 ? 是一个后置单元运算符，返回 p 的引用和一个布尔值。赋值运算符对 `($type, bool), type` 进行了重载，实现约等于：

```cpp
[[overload]]
operator (=) = (res : ($t, bool), val : t) -> {
    if (res[1]) {
        res[0] = val;
    }
    return &res;
}
```

其中函数参数列表中的 `t` 是一个泛型，我们会在后面详细介绍。

第三种方式是借用 **RAII**。我们在前面也看到了，构造函数中的 `new` 表达式是需要匹配一个析沟函数中的 `delete` 表达式的。这是因为构造函数的作用域结尾 **不会** 对任何指针进行释放。这可以说是一个“漏洞”，但也是因为我不想将 **Sugar C++** 设置得太过拘束。我们只需要确保构造函数中的资源不存在安全问题，就基本可以确定程序的内存安全了：

```cpp
auto main = () -> {
    auto Temp = struct {
        ctor = (int i) -> {
            this.data = new int(i);
        }
        dtor = () -> {
            delete this.data;
        }
        operator (=) = (this, int val) -> {
            this.dtor();
            this.ctor(val);
        }
        .data : &int;
    }
    auto x = 42;
    auto p = Temp(x);
    if (true) {
        auto tmp = 42;
        p = tmp;
    }
}
```

### 数组

**C++** 中的原生数组相当难用，其特性大多来自于 **C**。

```cpp
int main() {
    int arr_1[3] = { 1, 2, 3 };
    arr_1 = { 4, 5, 6 };			// 错误！数组不允许赋值。
    auto arr_2 = arr_1;				// 数组会在绝大多数表达式中退化为指针，从而丢失类型信息。
    std::cout << arr_1[3];			// 没有越界检查，可能会出问题。
    return 0;
}
```

**Sugar C++** 中将数组实现为 `std::vector` 那样的动态数组。它拥有原生的一系列成员函数供调用：

```cpp
auto main = () -> {
    auto arr_1 = [1, 2, 3];
    arr_1 = [4, 5, 6];
    auto arr_2 = arr_1;				// 复制了一个数组出来，arr_2 还是数组。
    arr_2.push_back(7);
    arr_2.assign([1, 2, 3, 4]);		// assign 方法可以复用已有的空间。
    for elem in arr_1 {				// range-for 是一个语法糖，后面会详细介绍。
        std::print("Element {}\n", elem);
    }
}
```

让我列举其中的一些：

- `ctor(args...)`：构造函数，通过列举元素构建。
- `ctor(arr: std::array<t, n>)`：构造函数，通过一个定长数组构建。顺带一提，`[...]` 结构的类型的实现就是 `std::array`，我称其为 **初始化数组**。
- `ctor(other)`：复制构造函数，通过另一个数组构建。
- `operator =(arr : std::array<t, n>)`：赋值函数，通过一个初始化数组重新分配内存并构建数组。
- `operator =(other)`：赋值函数，通过另一个数组重新分配内存并构建数组。
- `size(this)`：返回数组长度。
- `capacity(this)`：返回数组容量。
- `is_empty(this)`：返回数组是否为空。**C++** 中命名为 `empty` 着实令人迷惑。
- `operator [](this, idx)`：返回指定位置的元素；默认进行下标检查。如果不需要下标检查可以在调用处添加属性 `[[no_boundary_check]]`。
- `push_back(this, val)`：在数组最后添加一个元素。
- `pop_back(this)`：在数组最后删除一个元素。
- `assign(args...)`：用列举元素重构当前的数组，会尝试利用已经分配的内存。
- `assign(arr : std::array<t, n>)`：用初始化数组重构当前的数组，会尝试利用已经分配的内存。
- `assign(other)`：用另一个数组重构当前的数组，会尝试利用已经分配的内存。

### 元组

**C++** 中的元组是在 **C++11** 才支持的库类型 `std::tuple`，且使用比较麻烦：

```cpp
int main() {
    auto tup_1 = std::tuple(1, "abc", 2.0);
    std::cout << std::get<2>(tup_1) << '\n';	// 输出 abc
    std::apply([](auto const& elem) { std::cout << elem; }, tup_1);	// (C++17) 逐个打印 tup_1
}
```

由于不是内置的结构，**C++** 提供的接口也不易于使用，`std::tuple` 相关的操作动辄需要模板元相关的操作，新手不友好且不算美观。**Sugar C++** 中对元组提供了原生支持，它是通过 `,` 运算符得到的结构类型。比如 `1, 2` 就是一个元组。元组允许通过 `()` 来划分边界，这实际上也是 **Python** 中的写法：

```cpp
auto main = () -> {
    auto tup_1 = 1, "abc", 2.0;
    auto tup_2 = (42, "def");
    auto a = tup_1, tup_2;			// a = ((1, "abc", 2.0), (42, "def"))
    auto b = tup_1, c = tup_2;		// b 和 c 是定义的两个变量
    return 0;
}
```

和数组类似，元组有一系列的成员函数：

- `ctor(args...)`：构造函数。
- `ctor(other)`：

### 列表

**C++** 中的列表可以使用 `std::list`，不过它只是一个普通的 STL 容器，原生的支持很少。**Sugar C++** 中借鉴了 **Haskell** 中列表的构建方式，采用 `::` 运算符构建列表：

```cpp
auto main = () -> {
    auto list_1 = 1 :: 2 :: [];
    auto list_2 : [:int:] = [1, 2, 3];
}
```

### 闭包

**C++11** 引入了 lambda 表达式和闭包，它们显然相当好用——除了 lambda 表达式有些啰嗦。**Sugar C++** 中不同的设置有下面几点：

- 取消了 **C++** 中必须添加的捕获列表 `[]`。但相应地，参数列表（即使没有参数）必须添加。
- 允许不使用块结构 `{}` 表示闭包体，前提是函数中只有一个语句。此时不需要 `return` 表达式。
- 自动以值方式捕获所有使用的外部变量。**C++** 中的引用捕获不再存在，但可以通过捕获指针方式做到同样的效果。
- 参数列表不需要指定类型（相当于默认为泛型 lambda 和缩略模板函数，且省略了 `auto`）

值的一提的是，**Sugar C++** 中不再存在 **C++** 中函数的概念，因为所有函数都通过 `auto f = <lambda>` 的形式定义，所以函数即是闭包，闭包即是函数。而且，所有闭包都能用标准的方式声明（和 **C++** 中的闭包不一样，它们有标准的类型）。

```cpp
auto half = (x) -> x / 2;		// 不需要块结构。
auto quarter = half <| half;	// 不需要捕获。<| 是有结合管道运算符

```



## 控制流

**Sugar C++** 继承了 **C++** 中大多数的控制流语句，且风格非常类似。比如 `if`、`while` 语句完全一致。

### `for` 语句

**Sugar C++** 在 `for` 语句上进行了较大改变：

```cpp
for pattern : pred of iterable {
	// 对 pattern 进行处理
}
```

其等价于：

```cpp
auto it = std::begin(iterable);
auto end = std::end(iterable);
auto temp : typeof(it);				// 相当于 decltype(it)
while ((temp = it++) != end) {
    inspect (std::deref(temp)) {
        pattern : pred -> {
            // 对 pattern 进行处理
        }
        _ -> {}
    }
}
```

这里的 `inspect` 语句进行模式匹配，我们很快就会介绍。从上面的展开代码可以发现，**Sugar C++** 中的 `for` 语句和 **C++** 中的 range `for` 非常类似，而且还支持模式匹配（**C++17** 起有类似模式匹配的结构化绑定，但是功能较弱）。下面是一个具体的例子：

```cpp
auto main = () -> {
    auto arr = [1, 2, 3, 4, 5, 6, 7];
    auto is_odd = x -> x % 2 == 0;
    for (i, num) : (_, is_odd) of std::enumerate(arr) {
        std::print("arr[{}] = {} is odd.\n", i, num);
    }
    return 0;
}
```

其大体等价于 **C++** 中的：

```cpp
int main() {
    std::vector arr = { 1, 2, 3, 4, 5, 6, 7 };
    auto is_odd = [](auto x) { return x % 2 == 0; };
    for (auto [i, num] : std::zip(std::views::iota(1), arr | std::ranges::views::filter(is_odd))) {
        std::cout << std::format("arr[{}] = {} is odd.\n", i, num);
    }
    return 0;
}
```

可以看到，从语法风格上没有特别大的变化，但是整体看起来简洁了不少。

### `break` 和 `continue` 语句

**C++** 中的 `break` 和 `continue` 语句是用来跳出当前循环和跳回循环开头的，只不过在一些情况下，我们希望能够指定某个循环（而不是强制最内部的循环）。**Sugar C++** 中将属性运用了起来，使用 `[[loop: <name>]]` 标签的循环会拥有一个名字，`break` 和 `continue` 语句可以显示指定这个名字：

```cpp
auto main = () -> {
    extern auto arr : [Int];
    [[loop: outer]]
    for i of range(10) {
        for j of range(10) {
            if (arr[i] == 42) {
                break outer;
            }
            else if (arr[j] == 42) {
                continue outer;
            }
        }
    }
    return 0;
}
```

### `inspect` 语句

**C++** 中仅有的模式匹配是结构化绑定，但它的功能还不完全（比如只能限定特定的结构，如模板偏特化了 `std::get` 和 `std::tuple_size` 的类型）。目前我知道的有两个模式匹配的提案：[P1371](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/p1371r3.pdf) 和 [P2392](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2021/p2392r1.pdf)，两者都相当有趣。它们都使用 `inspect` 用于对特定表达式模式匹配，因此我在 **Sugar C++** 中也沿用了。
