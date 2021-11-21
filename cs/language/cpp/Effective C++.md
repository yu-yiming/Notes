# Effective C++ 笔记

[TOC]

本篇是 *Effective C++ (Scott Meyers)* 的阅读笔记，会遵照原文进行分章。

## 让自己熟悉 **C++**

### 将 **C++** 视为一个语言联邦

首先，**C++** *不是* 带类的 **C**！在语言设计的早期，它确实只有 **C** 的内容以及类相关的新特性。但是在数十年的发展后（截至 2021 年），**C++** 已经焕然一新，支持多种编程范式，这使得其表达力非常强大。但是不可否认，不同风格的 **C++** 可能看起来差异非常大，仿佛两种语言一样。因此这里的提议是将 **C++** 看作一个语言联邦。这并不是说要分裂 **C++**，其语言设计的内核依然是统一的，但是在不同“子语言”中转换时，要做好改变编程策略的准备。我们将 **C++** 主要分为下面四个子语言：

- **C**：毫无疑问 **C++** 的语言核心仍然是 **过程的（Procedural）**，在任何时候你都可以将它写成 **C** 的风格。这并不是说 **C** 是 **C++** 的子集，两者经过多年发展后显然已经分道扬镳；但是如果取出 **C++** 最基础的语言架构，比如 **块（Block）**、**语句（Statement）**、**预处理器（Preprocessor）**、**指针（Pointer）** 等都是沿袭自 **C** 并在 **C++** 中依然大量使用的特性。
- 面向对象的 **C++**：这也是对 **C++** 的刻板印象，“带类的 **C**”为了适用于 **面向对象编程（Object-Oriented Programming, OOP）** 而提供的特性，如 **类（Class）**、**继承（Inheritance）**、**子类型多态（Subtype Polymorphism）** 等。这也是 **C++** 在高速发展年间最被人熟知的部分。如同其它 OOP 语言一样，我们对 **C++** 也应该践行一些通用的 OOP 思想。
- 模版 **C++**：也即 **C++** 对 **范型编程（Generic Programming）** 的支持，其威力极为强大。这部分对于普通程序员来说可能不太熟悉，后文中多数模版相关的条款都是惟模版适用的。
- **STL** **C++**：这里的 **STL** 是指标准模版库，提供了 **容器（Container）**、**迭代器（Iterator）**、**算法（Algorithm）** 等相当有用的工具并需要遵守一定的使用思路。

以函数参数传递方式而言，**C** 风格的 **C++** 多数都是通过值来传递；但面向对象的 **C++** 会毫不犹豫地偏向传入常引用，以避免对象构建与销毁的巨大开销；模版 **C++** 由于不知道传入的类型，多数情况也只能选择常引用；**STL** 中则存在一些同样适合以值传递的类型，比如迭代器。

### 优先使用 `const`、`enum`、`inline` 而非 `#define`

这个条款在新的 **C++** 程序员中可能已经成为了常识，不过还是有必要提一下。简单来说，**C++** 的宏 `#define` 是一个简单粗暴的文本替换，可以用来模仿变量或函数的行为：

```cpp
#define X 1.23
#define F(x) ((x) + 1)		// 函数形式的宏需要谨慎地加上括号，以防止运算符优先级的冲突
int main() {
    std::cout << F(X);		// 等价于 std::cout << (1.23 + 1)
    return 0;
}
```

然而，这也意味着这些名字并不是标识符；编译时它们会被首先预处理为定义处的展开式，出现报错会令人不明所以。同时，类型的缺失也会容易导致误用（鬼知道上面的 `F` 想要一个什么样的东西）。更恶心的是，如果不经过 `#undef`，宏会污染之后当前文件下所有同样的名字。

一个解决方案是使用 `const` 修饰变量：

```cpp
int const x = 10;
char const* const str = "abc";
class MyClass {
public:
    static int const size = 42;
private:
    int arr[size];
};
```

需要注意的是，上面的 `MyClass::size` 在类中给出了“定义”，但实际上这种情况下它不会被分配任何内存（也就不允许对其求址了）。所有其出现的地方都会被 `42` 代替，原理和 `#define` 颇为相似，但它更加好用，一方面是确定的类型和标识符信息，还有就是它完美地局限在了 `MyClass` 这个类中。如果希望允许对 `MyClass::size` 求址，可以在源文件中提供它的定义：

```cpp
// some_source.cpp
int const MyClass::size;		// 此处不需要再初始化了，因为在类中已经给出过一个定义
```

或者在 **C++17** 后，为成员变量使用 `inline` 限定符：

```cpp
class MyClass {
public:
    static inline int const size = 42;	// (C++17)
private:
    int arr[size];
};
```

在远古的编译器中，`const` 修饰并在类中初始化的成员变量并不能保证编译期求值，此时可以使用 `enum` 来完成相同效果（这个技术在 **C++98** 年代的模板元编程中非常普遍）：

```cpp
class MyClass {
public:
    enum { size = 42 };			// 这个可以确保在编译期求值，不过同样地，不能对 enum 求址
private:
    int arr[size];
};
```

不过，**C++11** 引入的 `constexpr` 以更清晰的语义完成了相同的事情：

```cpp
class MyClass {
public:
    static constexpr int size = 42;		// (C++11) 同时我们可以对 MyClass::size 求址
    									// （因为所有 constexpr 成员都是默认 inline 的）
private:
    int arr[size];
};
```

这应该是最佳的方案，即满足了常量性和编译期求值，也完全符合普通变量的特点。

之后来讲讲函数形式的宏。它最大的骗局就在于，其并不像函数那样对参数进行求值后再带入计算。考虑下面的例子：

```cpp
#define CALL_WITH_MAX(a, b) f((a) > (b) ? (a) : (b))
int main() {
    int a = 5, b = 0;
    CALL_WITH_MAX(++a, b);			// 展开就得到了 f((++a) > (b) ? (++a) : (b))
    CALL_WITH_MAX(++a, 5);
    return 0;
}
```

可以看到这里出乎意料地，我们将 `++a` 执行了两次（或一次），（这简直匪夷所思，递增次数居然和它们大小有关）。所以解决方法是使用模板内联函数：

```cpp
template<typename T>
inline void call_with_max(T const& a, T const& b) {
    f(a > b ? a : b);
}
```

**C++11** 之后也出现了更加使用的 `constexpr` 函数，其保证要求在编译期求值时在编译期调用：

```cpp
template<typename T>
constexpr void call_with_max(T const& a, T const& b) {	// (C++11)
    f(a > b ? a : b);
}
```

如果依然认为模板函数的语法过于复杂，可以在 **C++14** 后使用 **泛型 lambda（Generic Lambda）** 或 **C++20** 后的函数参数占位符特性简化：

```cpp
auto add_1 = [] (auto a, auto b) constexpr { return a + b; };	
										// (C++14)其中的 constexpr 限定符在 C++17 后才能使用
constexpr auto add_2(auto a, auto b) {  // (C++20)
    return a + b;
}
```

它们的本质都是模板函数。

### 尽可能使用 `const`

`const` 大概是 **C++**中比较特别的设计。它为一个变量提供了语义上“不允许修改”的约束，并作为变量类型的一部分，由编译器来强制实施。它实际上不是绝对的：我们可以通过 `const_cast`  轻松绕过一些约束，且声明为诸如 `T const&` 或 `T const*` 也不代表引用的对象真的是完全不允许改变的。因此，`const` 就好像一个约定一样，我们可以通过加上该修饰符让编译器帮助我们判断行为的正确性。

基本上，`const` 可以出现在任何变量声明的地方，下面是一些变量声明被 `const` 参与后的样子：

```cpp
int const x = 42;		// x 是 int const 类型的变量。需要注意，这里也可以写成 const int x = 42; 两者完全等价
char str[] = "abc";
char const* pcstr = str;			// pcstr 是一个指向 char const 的指针
char* const cpstr = str;			// cpstr 是一个指向 char 的常指针
char const* const cpcstr = str;		// cpcstr 是一个指向 char const 的常指针
```

从上面的例子可见，`const` 可以用来修饰指针或指针指向对象是否为常量，总体来讲规则是从右向左读：比如 `char const* const` 就是一个“常量的指针，指向常量的 `char` 类型”。由于最内层（指针最深处）的 `const` 允许写在存储类型的左侧（如 `const char`），需要特别注意。

`const` 最大的威力出现在函数相关的运用。比如可以通过返回常量来避免奇怪的问题：

```cpp
class Rational {
public:
    friend Rational const operator *(Rational const& lhs, Rational const& rhs);
	// 省略定义
};
int main() {
    Rational a{}, b{}, c{};		// (C++11) 列表初始化
    // 省略细节
    a * b = c;					// 如果不将 operator * 返回值声明为 const，这是完全合法（且没有意义的）
    if (a * b = c) {			// 如果 Rational 可以隐式转换为 bool 类型，这里就可以产生难以预测的结果
        						// 我们可能本来想要比较 a * b 和 c，但一个笔误就会产生静默的错误行为
        // 省略细节
    }
}
```

值得注意的是，对于内置类型，上面出现的 `a * b = c` 式子压根就不会通过编译，虽然这涉及了编译器的行为，但也提示我们应该利用 `const` 杜绝这样的代码通过编译。

此外，`const` 用于成员函数可以修饰隐式传递的 `this` 指针，而这可以作为重载的条件。

```cpp
class Vector {
public:
    int& operator [](int idx) {
        return m_data[idx];
    }
    int const& operator [](int idx) const {	// 将 this 以 Vector const* 形式传递，此时成员函数中不允许修改成员变量
        return m_data[idx];
    }
    // 省略定义
private:
    int* m_data;
};
int main() {
    Vector v1{};
    Vector const v2{};
    // 省略细节
    std::cout << v1[0];			// 调用 Vector::operator [](int)
    std::cout << v2[0];			// 调用 Vector::operator [](int) const
    return 0;
}
```

事实上 **C++** 的很多成员函数都如此定义了常量和非常量版本。这引发了一个问题：它们的定义大多数时候基本一致，这样不会造成代码膨胀么？确实如此，因此我们倾向于只定义常量的版本，然后使用强制类型转换处理非常量版本：

```cpp
class Vector {
public:
    int const& operator [](int idx) const {
        return m_data[idx];
    }
    int operator [](int idx) {
        return const_cast<char&>(static_cast<Vector const&>(*this)[idx]);
    }
}
```

上面首先使用 `static_cast` 进行了安全的从非常量到常量的类型转换，随后再通过 `const_cast` 将结果的 `const` 擦除。需要注意，我们不应该反过来实现（即让常量版本调用非常量版本的任何成员函数），这是因为 `const` 约定不会对任何成员的逻辑状态进行修改，因此现在的调用方式总是安全的，而反过来可能会出现问题（更多的是迷惑性）。

此外，**C++** 也为 `const` 成员函数开了一扇门。只要将成员变量声明为 `mutable` 的，就可以在 `const` 的成员函数中对它进行修改。

### 确定对象在使用前已经被初始化

**C++** 这该死的初始化，到处都是问题。首先，在 *任何时候* 都要初始化你声明的变量。其实更准确来讲，是确保它在使用之前被初始化或赋值过，不过由于程序流的复杂性，有些时候我们并不能确保未被初始化的变量在使用前进行了赋值，所以请初始化！

至于原因，是源自 **C** 的遗留特性，即为了不影响运行期效率，默认 *不* 对内置类型（仅自动生命周期）进行初始化：

```cpp
int x;				// 全局变量会被初始化，因为它存在 BSS 段
static int y;		// 静态变量（全局与否）会被初始化，它也存在 BSS 段
int main() {
    int a;			// 没有初始化！x 在运行时可能是任何值
    int b, c = 10;	// 只有 c 被初始化为 10，b 没有被初始化
    if (a + b == 0) {	// 这里就产生了 UB（未定义行为），因为使用了未初始化的变量
        // 省略喜结
    }
    return 0;
}
```

还有一个促使我们初始化变量的场景，那就是构造函数：

```cpp
class Foo {
public:
    Foo(std::string& str, int x) {
        m_str = str;				// 错误！m_str 没有经过初始化（这里是一个赋值）
        m_x = x;					// 错误！m_x 不能被赋值
    }
private:
    std::string& m_str;				// 必须初始化引用
    int const m_x;					// 常量一经初始化，就不能再修改
};
```

此时应该使用成员初始化列表来对成员变量初始化：

```cpp
class Foo {
public:
    Foo(std::string& str, int x)
        : m_str(str), m_x(x) {}		// 此处进行的是初始化，而非赋值，因此非常适合这里的两个成员变量
private:
    std::string& m_str;
    int const m_x;
};
```

即使不是引用或常量，也推荐使用成员初始化列表，因为单独一次初始化总比默认初始化后再进行赋值更高效（更别说内置类型 *没法* 默认初始化了）。不过一些只需要默认初始化，或需要比较复杂设置的变量，我们也不需强求它们在成员初始化列表中初始化。

这里需要特别强调一点，成员初始化列表中，*总是* 先初始化基类的变量，再初始化派生类的变量；同时，每个类中 *总是* 以声明顺序初始化各个成员，与其在成员初始化列表中的顺序 *无关*。这可能有些令人沮丧，因为我们不得不额外注意成员的声明顺序。

最后一个初始化相关的问题涉及全局变量。此处的全局变量指的是定义在文件作用域、命名空间中的全局变量，或定义在类中的静态变量。由于 **C++** 中并不规定它们的初始化顺序（事实上可能根本没法设计出真正可靠的初始化顺序规则），因此出现相互依赖时可能会产生严重问题：

```cpp
// file_system.hpp
class FileSystem {
public:
    size_t num_disks() const;
    // 省略定义
};
extern FileSystem tfs;			// 全局变量声明
```

````cpp
// directory.cpp
#include "file_system.hpp"
class Directory {
public:
    Directory( /* 省略细节 */) {
        size_t disks = tfs.num_disks();		// 可能会出现问题！此处 tfs 有可能没有经过初始化
        // 省略定义
    }
    // 省略定义
};
````

我们可以通过将全局变量作为函数中的静态变量调取。由于 **C++** 保证局部的静态变量一定会在它使用前进行初始化，我们可以确保安全：

```cpp
// file_system.hpp
FileSystem& tfs();
```

```cpp
// file_system.cpp
FileSystem& tfs() {
    static FileSystem fs;
    return fs;
}
```

这样单纯的函数（仅包含变量声明和 `return` 语句）也成为了 `inline` 优化的青睐目标，因此无需担忧函数调用的开销。

## 构造函数/析构函数/赋值运算符

### 了解 **C++** 默默编写并调用哪些函数

**C++** 会为没有编写一些 **特殊成员函数（Special Member Function）** 的类型进行这些函数的默认实现。详细请参考 *Effective Modern C++* 的条款 17，这里不再赘述。

### 如果不想使用编译器自动生成的函数，应该明确禁止它

### 为多态基类声明 `virtual` 析构函数

### 不要让析构函数抛出异常

### 不要在构造和析构函数中使用 `virtual` 函数

### 让 `operator =` 函数返回一个对 `*this` 的引用

### 在 `operator =` 中处理自我赋值

### 复制对象中的每一个成分

## 资源管理

### 以对象管理资源

### 在资源管理类中小心复制行为

### 在资源管理类中提供对原始资源的访问

### 成对使用 `new` 和 `delete` 时使用相同的形式

### 以独立语句将 `new` 得到的对象存入智能指针中

## 设计与声明

### 让接口容易被正确使用

### 将类设计得像一个类型

### 优先以常引用传递而非以值传递

### 不要返回引用类型

### 将成员变量声明为 `private`

### 优先使用非成员函数和非友元函数而非成员函数

### 如果所有参数都需要类型转换，请使用非成员函数

### 考虑写一个不抛出异常的 `swap` 函数

## 实现

### 尽可能延后变量定义出现的地方

### 尽可能减少类型转换

### 避免返回对象内部的“句柄”

### 为异常安全而努力

### 彻底了解内联

### 将文件间的编译依赖关系降到最低

## 继承与面向对象设计

### 确保 `public` 继承体现了 is-a 的关系

### 避免名称覆盖

### 区分对接口和对实现的继承

### 考虑 `virtual` 函数以外的其它选择

### 绝不重新定义继承的非 `virtual` 函数

### 绝不重新定义继承的默认参数值

### 通过组合体现 has-a 关系

### 审慎地使用 `private` 继承

### 审慎地使用多重继承

## 模版和范型编程

### 了解隐式接口和编译器多态

### 了解 `typename` 的双重含义

### 了解如何访问模版化的基类中的名称

### 将与模版参数无关的代码抽出模版

### 通过模版成员函数接受所有兼容类型

### 需要类型转换时在模版中定义非成员函数

### 使用特征类型表现类型信息

### 认识模版元编程

## 自定义 `new` 和 `delete`

### 了解 new-handler 的行为

### 了解替换 `new` 和 `delete` 的合理时机

### 编写 `new` 和 `delete` 时遵守约定

### 如果写了 placement-new，也要写 place-delete

## 杂项

### 注意编译器的警告

### 熟悉包括 **TR1** 在内的标注库

### 熟悉 **Boost**

