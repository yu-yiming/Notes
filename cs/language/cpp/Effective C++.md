# Effective C++ 笔记

[TOC]

本篇是 *Effective C++ (Scott Meyers)* 的阅读笔记，会遵照原文进行分章。阅读本篇的同学最好对 **C++** 已经有一定了解，知道变量、函数、类以及相关概念。

## 让自己熟悉 **C++**

### 1. 将 **C++** 视为一个语言联邦

首先，**C++** *不是* 带类的 **C**！在语言设计的早期，它确实只有 **C** 的内容以及类相关的新特性。但是在数十年的发展后（截至 2021 年），**C++** 已经焕然一新，支持多种编程范式，这使得其表达力非常强大。但是不可否认，不同风格的 **C++** 可能看起来差异非常大，仿佛两种语言一样。因此这里的提议是将 **C++** 看作一个语言联邦。这并不是说要分裂 **C++**，其语言设计的内核依然是统一的，但是在不同“子语言”中转换时，要做好改变编程策略的准备。我们将 **C++** 主要分为下面四个子语言：

- **C**：毫无疑问 **C++** 的语言核心仍然是 **过程的（Procedural）**，在任何时候你都可以将它写成 **C** 的风格。这并不是说 **C** 是 **C++** 的子集，两者经过多年发展后显然已经分道扬镳；但是如果取出 **C++** 最基础的语言架构，比如 **块（Block）**、**语句（Statement）**、**预处理器（Preprocessor）**、**指针（Pointer）** 等都是沿袭自 **C** 并在 **C++** 中依然大量使用的特性。
- 面向对象的 **C++**：这也是对 **C++** 的刻板印象，“带类的 **C**”为了适用于 **面向对象编程（Object-Oriented Programming, OOP）** 而提供的特性，如 **类（Class）**、**继承（Inheritance）**、**子类型多态（Subtype Polymorphism）** 等。这也是 **C++** 在高速发展年间最被人熟知的部分。如同其它 OOP 语言一样，我们对 **C++** 也应该践行一些通用的 OOP 思想。
- 模版 **C++**：也即 **C++** 对 **范型编程（Generic Programming）** 的支持，其威力极为强大。这部分对于普通程序员来说可能不太熟悉，后文中多数模版相关的条款都是惟模版适用的。
- **STL** **C++**：这里的 **STL** 是指标准模版库，提供了 **容器（Container）**、**迭代器（Iterator）**、**算法（Algorithm）** 等相当有用的工具并需要遵守一定的使用思路。

以函数参数传递方式而言，**C** 风格的 **C++** 多数都是通过值来传递；但面向对象的 **C++** 会毫不犹豫地偏向传入常引用，以避免对象构建与销毁的巨大开销；模版 **C++** 由于不知道传入的类型，多数情况也只能选择常引用；**STL** 中则存在一些同样适合以值传递的类型，比如迭代器。

### 2. 优先使用 `const`、`enum`、`inline` 而非 `#define`

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
    static inline int const size = 42;	// (C++17) 这个就相当于唯一的定义
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

### 3. 尽可能使用 `const`

`const` 大概是 **C++** 中比较特别的设计。它为一个变量提供了语义上“不允许修改”的约束，并作为变量类型的一部分，由编译器来强制实施。它实际上不是绝对的：我们可以通过 `const_cast`  轻松绕过一些约束，且声明为诸如 `T const&` 或 `T const*` 也不代表引用的对象真的是完全不允许改变的。因此，`const` 就好像一个约定一样，我们可以通过加上该修饰符让编译器帮助我们判断行为的正确性。

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

### 4. 确定对象在使用前已经被初始化

**C++** 这该死的初始化，到处都是问题。首先，在 *任何时候* 都要初始化你声明的变量。其实更准确来讲，是确保它在使用之前被初始化或赋值过，不过由于程序流的复杂性，有些时候我们并不能确保未被初始化的变量在使用前进行了赋值，所以请初始化！

至于原因，是源自 **C** 的遗留特性，即为了不影响运行期效率，默认 *不* 对内置类型（仅自动生命周期）进行初始化：

```cpp
int x;				// 全局变量会被初始化，因为它存在 BSS 段
static int y;		// 静态变量（全局与否）会被初始化，它也存在 BSS 段
int main() {
    int a;			// 没有初始化！x 在运行时可能是任何值
    int b, c = 10;	// 只有 c 被初始化为 10，b 没有被初始化
    if (a + b == 0) {	// 这里就产生了 UB（未定义行为），因为使用了未初始化的变量
        // 省略细节
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

这样单纯的函数（仅包含变量声明和 `return` 语句）会成为 `inline` 优化的青睐目标，因此无需担忧函数调用的开销。

## 构造函数/析构函数/赋值运算符

### 5. 了解 **C++** 默默编写并调用哪些函数

**C++** 会为没有编写一些 **特殊成员函数（Special Member Function）** 的类型进行这些函数的默认实现。详细请参考 *Effective Modern C++* 的条款 17，这里只提一下一种特别的情况：

```cpp
class Foo {
public:
    Foo(std::string& str, int x)
        : m_str(str), m_x(x) {}
private:
    std::string& m_str;
    int const m_x;
};
int main() {
    Foo f1("abc", 10), f2("def", 42);
    f1 = f2;				// 错误！编译器拒绝生成复制赋值操作。这是因为 C++ 不允许改变引用和常量
    return 0;
}
```

### 6. 如果不想使用编译器自动生成的函数，应该明确禁止它

如果不希望编译器自动生成特殊成员函数，比如复制相关操作，可以使用 `= delete` 作为其定义，详情参考 *Effective Modern C++* 的条款 11（事实上那里直接将本书这里提供的方法给否定了）。不过对于不支持 **C++11** 的情形，下面的 `Uncopyable` 类或许有些帮助：

```cpp
class Uncopyable {
protected:
    Uncopyable() {}
    ~Uncopyable() {}
private:
    Uncopyable(Uncopyable const&);			// 通过将复制操作声明为 private 而阻止任何复制
    Uncopyable& operator =(Uncopyable const&);
};
class MyClass : private Uncopyable {
    // 省略定义
};
```

上面只要我们不为 `MyClass` 定义复制操作，编译器就会尝试自动定义，并在其中首先默认调用基类的复制操作，而这会产生编译器错误（`private` 成员函数无法访问）。

### 7. 为多态基类声明 `virtual` 析构函数

简而言之，如果希望一个类型被继承，那么 *一定* 要将基类的类型声明为 `virtual`。这是因为，一个指向派生类的基类指针被释放时，只会调用基类的析构函数。如果不声明为 `virtual`，派生类中声明的成员变量就不能正常释放。

不过，对于不可能被继承的类型，我们也不该无脑加上 `virtual` 限定符，这是因为虚函数是有代价的：每个对象都会配套一个 **虚表指针（Virtual Table Pointer）**，其指向一个函数指针构成的数组。在对象调用某一个虚函数时，实际调用的成员函数取决于虚表中相应位置存储的函数指针。这个表无疑会占用很多的内存。如果本来是一个简单的类型，比如图形引擎常用的 `Point` 类型，本身可能只是由两个 `float` 组成的轻量结构，但是加上了 `virtual` 修饰后，就会无端多出一个指针的大小：

```cpp
struct Point {
    Point(float x_, float y_);
    ~Point();
    float x, y;   
};
// 下面是声明 virtual 函数后，类中的结构
struct PointVirtual {
    Point(float x_, float y_);
    virtual ~Point();
    float x, y;
    vptr_t vptr;		// 这里的 vptr_t 指向一系列函数指针
};
```

此外，继承其它的类型也可能让自己的函数变为 `virtual` 的，比如由于 `std::string` 有一个虚的析构函数，继承自 `std::string` 会导致派生类的虚构函数自动变为 `virtual` 的（无论你是否显式声明）。

### 8. 不要让析构函数抛出异常

析构函数是用来释放资源的。在抛出异常时，**C++** 标准保证所有成员的析构函数都会被执行，即使出现异常，也会先回滚栈，并令其中的对象逐次调用析构函数。但是，如果在析构这些对象时再次出现异常（真巧，它们的析构函数也没有遵守这个条款），事情会变得复杂。简单来说，**C++** 此时无法合适地处理这些异常。如果想要详细的例子，可以看下面的代码：

```cpp
struct OuterException : std::exception{};
struct InnerException : std::exception{};
 
struct LegalThrowingDtor 
{
    ~LegalThrowingDtor() /*noexcept(false)*/ {
        assert( std::uncaught_exception() ); /*5*/
        throw InnerException();
    }
};
 
struct InnerWorld
{
    ~InnerWorld() {
        try {
            LegalThrowingDtor obj; /*4*/
        }
        catch( OuterException const& ) { /*3*/
            assert( false );
        }
        catch( InnerException const & ) { 
            // swallow
        }
    }
};
 
int main() 
try {
    InnerWorld innerWorld;
    throw OuterException(); /*1*/
}
catch( OuterException const& ) { /*2*/
    assert( !std::uncaught_exception() );
    return 0;
}
```

此外，自从 **C++11** 引入 `noexcept` 关键字（以及相应的异常规格）后，所有的析构函数都默认声明为 `noexcept` 的（只要其基类以及所有成员的析构函数都是 `noexcept` 的），因此如果需要特别照顾某个析构函数令其能够抛出异常，请使用 `noexcept(false)`。

### 9. 不要在构造和析构函数中使用 `virtual` 函数

这个条款所述内容的原因很简单，所有派生类的构建都是在基类之后，而析构在基类之前，因此会以“基类构造->派生类构造->派生类析构->基类析构”的顺序进行派生类对象的生命周期。因此，在构造和析构基类的部分时，这个对象“无法意识到”自己是派生类型，因此即是调用 `virtual` 函数，也只会执行当前类型（基类）的版本。这样的设计有相应的好处，因为无论是构造还是析构时，派生类的成员变量都不存在，此时作为派生类对象的任何操作都是未定义行为。

这样的行为即使不在构造和析构函数中也同样会被执行。比如构造函数调用了一个“会调用 `virtual` 函数”的函数，此时程序依然会调用基类的版本。

如果基类声明的是纯虚函数，上面的行为会产生运行错误。

### 10. 让 `operator =` 函数返回一个对 `*this` 的引用

标准中对 `operator =` 的返回类型并没有要求（事实上对参数类型也没有要求），因此我们甚至可以写出下面这样的代码：

```cpp
struct Foo {
    int operator =(Foo const& other) {
        return 0;
    }
};
```

这合法但是令人非常迷惑。通常我们会返回 `*this`，以和内置类型的行为保持一致：

```cpp
int main() {
    int i = 0, j = 1, k = 2;
    (i = j) = k;				// 等价于 i = j，然后执行 i = k，所以第一次返回的是
    std::cout << i << j << k;
    return 0;
}
```

### 11. 在 `operator =` 中处理自我赋值

自我赋值是类似于 `x = x` 的情形。看起来不太可能，但是 **C++** 中盛行的引用让这种情况变得常见。比如 `*px = *py` 或 `arr[i] = arr[j]` 时，我们无法保证这是不是在进行自我赋值。一个功能正确的自我赋值不应该搞砸任何事情。但是在需要资源管理的类型中：

```cpp
struct Foo {
    Foo& operator(Foo const& other) {
        delete[] resrc;
        resrc = new int[other.size];
        std::copy(other.resrc, other.resrc + other.size, resrc);
        size = other.size;
    }
    int resrc[]; 
    int size;
};
```

上面的例子中，如果我们将一个 `Foo` 对象赋值给了自己，由于其中我们一上来就 `delete[]` 了 `resrc`，之后所有的访问操作都是无效的。解决方法是在一开始判断 `this` 是否与 `&other` 相等，此时直接返回 `*this` 即可。

### 12. 复制对象中的每一个成分

**C++** 编译器毕竟不是保姆，如果我们实现复制操作时忘记复制其中一个成员，它不会做任何警告（毕竟这完全是合法的行为）。你大概会觉得，怎么可能忘记复制呢？实际上在实现派生类的复制操作时，可能会忘记调用基类的复制构造或复制赋值函数：

```cpp
struct Base {
    Base(Base const& other) = default;
    Base& operator =(Base const& other) = default;
    int i;
};

struct Derived : Base {
    Derived(Derived const& other) : Derived(other) {	// 调用复制构造函数
        ptr = new int(0);
    }
    Base& operator =(Derived const& other) {
        Base::operator =(other);						// 调用复制赋值函数
        *ptr = *other.ptr;
        return *this;
    }
    int* ptr;
}
```

## 资源管理

### 13. 以对象管理资源

这部分是 **C++** 从始至终的主要程序设计思想之一，也即 **资源获取即是初始化（Resource Acquisition Is Initialization, RAII）**。这个拗口的名词就是下面这个思想的抽象：

```cpp
int main() {
    acquire_resource();		// 获取资源
    do_work();				// 利用资源进行工作
    release_resource();		// 释放资源
    return 0;
}
```

在 **C** 语言中，上面的内容可能以下面的形式展现：

```cpp
int main() {
    int* i = malloc(sizeof(int));
    printf("%d", i);
    free(i);
    return 0;
}
```

**RAII** 则是将资源的分配和释放移动到对象的构造函数和析构函数中：

```cpp
struct Resource {
    Resource(int val) : ptr(new int(val)) {}
    ~Resource() {
        delete ptr;
    }
    int* ptr;
};
int main() {
    Resource resrc(10);		// 初始化变量时获取资源
    return 0;
}							// 对象析构时释放资源
```

**C++11** 之后在标准库中提供了三种智能指针 `std::unique_ptr`、`std::shared_ptr`、`std::weak_ptr` 以利用 **RAII** 管理资源（它们等价于一个指针）。具体的内容可以参考 *Effective Modern C++*。本书中这一条款由于时代局限，只提到了其时远不够完善的 `std::auto_ptr`（在 **C++17** 中弃用了）；由于 **C++11** 之前没有移动语义，我们没法方便地限定资源的所有权以及进行所有权转移，具体内容也可以参考 *Effective Modern C++*。

### 14. 在资源管理类中小心复制行为

使用 **RAII** 的类型时，我们需要确定其复制的含义。通常有下面这些实现形式：

- 禁止复制。比如 `std::mutex`，因为对其复制并不合理。可以使用 `= delete`（**C++11**）或将复制操作声明为 `private`。
- 引用计数。在 **RAII** 类型中设置引用计数指针，然后每次复制时只进行浅拷贝，然后将计数加一。每次析构对象时，将计数减一，仅当引用计数为零时才释放资源。这就是 `std::shared_ptr` 的基本原理。
- 深拷贝。这也是最简单粗暴的方法，有些时候比较低效。
- 转移所有权。本质上就是浅拷贝并将原来的对象无效化。**C++11** 的移动语义为此提供了一些便利。

### 15. 在资源管理类中提供对原始资源的访问

由于需要使用大量历史代码以及 **C** 代码，即使我们使用 **RAII** 也依然需要提供方式暴露出内部的资源（当然，能避免则避免）。比如，`std::vector` 提供了 `data` 方法以得到底层的动态数组；`std::string` 提供了 `c_str` 方法已得到一个空字符结尾的 **C** 风格字符串。不过，最好不要图方便声明一个隐式转换（事实上，隐式转换总是伴随风险的）。

### 16. 成对使用 `new` 和 `delete` 时使用相同的形式

这项条款可以几乎用一句话概括：`new` 的对象用 `delete` 释放；`new[]` 的对象用 `delete[]` 释放。如果混用的话就会产生未定义行为。你也许会觉得这轻而易举，但看下面这个例子：

```cpp
using MyArray = int[3];
int main() {
    MyArray arr = new MyArray;    // 这里等同于 new int[3]，因此使用的是 new[] 运算符
    delete arr;					// 未定义行为！
    delete[] arr;				// 正确行为
    return 0;
}
```

不过通常来讲 **C++** 不需要 `new[]` 和 `delete[]`，因为很多需要动态分配数组的结构已经在标准库中实现了，比如 `std::vector`、`std::string` 等。

### 17. 以独立语句将 `new` 得到的对象存入智能指针中

这个内容和 *Effective Modern C++* 中相关内容雷同。

## 设计与声明

### 18. 让接口容易被正确使用

**C++** 中存在大量的接口，如函数、类、模板等。为了让用户能够尽量避免误用，我们需要好好考虑函数签名、类方法和模板参数的设计。下面时一个 `Date` 类型的构造函数接口：

```cpp
Date(int month, int day, int year);
```

乍一看没有什么问题，毕竟这三个量都可以用 `int` 来表示。但是注意：不同国家地区的用户对日期的表示习惯不同。有些用户可能会尝试这样初始化：

```cpp
Date d(2021, 12, 21);
```

呃，这样问题就大了，似乎 *没有一个* 参数是被正确传递的。那么是否可以通过声明别名来指定类型呢？

```cpp
using MonthType = int;
using DayType = int;
using YearType = int;
Date(MonthType, DayType day, YearType year);
```

但是很可惜，这和之前的情况没有任何区别，因为 **C++** 的别名并不是一个严格的类型。所以一个合适的方法是将它们包装成类型：

```cpp
// 这里只列出 Month 作为例子
struct Month {
    // 声明为 explicit 防止隐式转换
    explicit Month(int m) : val(m) {}
    int val;
};
```

此时构造 `Date` 就只能用下面的形式了：

```cpp
Date d(Month(12), Day(30), Year(2000));
```

对于用户输入数据合法性的保证，一方面可以在构造函数中检查，也可以将正确的信息“枚举”出来。当然这仅限于合理的情形。比如 `Month` 中合法的值仅有 12 个，所以可以这样定义：

```cpp
struct Month {
    static Month Jan() { return Month(1); }
    static Month Feb() { return Month(2); }
    // 省略其它的月份
private:
    explicit Month(int m) : val(m) {}
    int val;
};
```

此时可以像下面这样构造 `Date`：

```cpp
Date d(Mont::Jan(), Day(20), Year(2000));
```

对于概念类似的方法，建议起相同的名字。比如 **STL** 中的大多数容器都提供了 `size` 方法，这样拿到任何一个容器都知道这个名字对应着容器大小的方法。

最后，如果可以返回智能指针，就尽量不要返回管理资源的裸指针。比如当用户得到 `get_resource` 返回的裸指针后，可能不清楚这个指针应该通过什么方式释放（是 `delete`，是 `delete[]`，还是某个函数，比如 `free_resource`？）而且在一些平台上，如果指针在一个 **DLL** 中 `new`，却在另一个 **DLL** 中 `delete` 会引发运行时错误。智能指针不会遇到这两种问题。首先它会托管资源，在到生命周期结束是会自动释放资源；其次它在创建时会指定一个删除器，即使在另一个 **DLL** 中释放也会使用原来的删除器。关于删除器的用法可以参考 *Effective Modern C++*。

### 19. 将类设计得像一个类型

这似乎是一句废话。但是“像一个类型”可不是很简单就能做到的。作者建议审视下面这些要点：

- 如何创建和销毁这个类型的对象？这涉及对构造和析构函数的编写。当然也可以重载 `new`、`delete`、`new[]` 和 `delete[]` 运算符。
- 初始化和赋值应该有什么差别？这两个是不同的操作，需要额外注意。
- 该类型对象的复制语义是？也即其复制操作（复制构造与复制赋值）的行为是什么。
- 该类型对象的合法值是？这决定了许多成员函数的前置和后置约束。当这些约束不被满足时，可能需要一些错误处理方法，如状态码或异常。
- 该类型的继承图？这个类如果需要继承其他类，就应该重写它的纯虚函数，并注意初始化与赋值时的操作
- 该类型需要什么类型转换函数？如果需要将此类型转换为 `T`，就需要定义 `operator T`。如果不允许隐式转换，还要将其声明为 `explicit`。
- 该类型需要哪些操作？这涉及成员函数和运算符重载的定义。
- 谁可以访问该类型的成员？这涉及成员的访问修饰符以及友元的声明。
- 该类型有哪些“未声明接口”？这指的是方法的前后约束。当执行函数前后出现不合法的状态时，应该考虑如何处理。
- 该类型足够特定么？如果需要一个一般化的类型，我们应该定义一个模板类。
- 该类型真的必要么？有时比起涉及派生类，多设计一个函数就可以解决问题。

这些问题有时候没有唯一正确的答案，这也是为什么根据实际情况设计出高效好用的类型并不容易。

### 20. 优先以常引用传递而非以值传递

首先最直观的一点是，对于非内置类型，值传递通常会涉及复制构造操作以及析构操作（在 **C++11** 之后的移动语义让一些情况变得复杂，但是通常还是会涉及这些操作），对于一些类型来说代价高昂。因此传递诸如 `std::string`、`std::vector` 等对象时首选的方式就是常引用传递。

除了性能考量外，值传递并不能反映子类型多态。看下面的例子：

```cpp
struct Base {
    virtual void show() {
        std::cout << "Base";
    }
};
struct Derived : Base {
    void show() override {
        std::cout << "Derived";
    }
};
void foo(Base b) {
    b.show();
}
int main() {
    Derived d{};
    foo(d);			// 输出 Base
    return 0;
}
```

派生类对象传入 `foo` 函数时，构造了一个 `Base` 类型的对象，因此调用 `show` 方法时必定调用基类的定义。因此如果需要用到子类型多态的函数，操纵的一定是指针或者引用（因为此时不会出现复制构造）。

需要指出的一个误区是，有些人认为只要类型的大小足够小，就适合以值方式传递。这并不准确。很多 **RAII** 的类型的复制操作不仅要复制它本身，还要复制它管理的资源（比如 `std::vector`），这可能会代价高昂。

### 21. 不要返回引用类型

上一条可能会让你醍醐灌顶，坚定地追求传递引用而非值。但这对于函数的返回值而言并不理想。引用的对象该如何构造成为了巨大的问题。首先它不可能是一个自动存储周期的变量，因为函数返回就离开了当前作用域，这个变量会失效：

```cpp
std::string& foo() {
    std::string s = "abc";
    return s;
}					// s 到这里就失效了，返回的是一个非法的引用
```

一个显然的解决办法似乎是使用 `new` 运算符，这样对象就拥有动态存储周期了：

```cpp
std::string& foo() {
    std::string* ptr = new std::string("abc");
    return *ptr;
}
```

但是另一个问题接踵而来：谁应该负责释放这块内存？我想一定是用户。但是考虑下面这个例子：

```cpp
int main() {
    std::string s = foo() + "def";	// 呃，似乎我们把 foo() 返回的引用给弄丢了。内存泄露 get
    return 0;
}
```

合适的使用方式如下：

```cpp
int main() {
    std::string& ref = foo();
    ref += "def";
    delete &ref;
    return 0;
}
```

但这也未免太麻烦了，而且对用户来说，这种不对称的 `new`、`delete` 用法是一种折磨（确切来说，就应该尽量避免 `new` 和 `delete`）。这里有些机智的朋友可能会想到静态存储周期，它也能保证返回引用的有效性，而且还不需要 `delete`：

```cpp
std::string& foo(char const* suffix) {
    static std::string str = "abc";
    return str += suffix;
}
```

但是这次问题一样严重。先不提静态存储周期导致的线程安全问题，看看下面这段代码：

```cpp
int main() {
    if (foo("xxx") == foo("yyy")) {		// 这个表达式永远为 true
        std::cout << "Suffixes match";
    }
    return 0;
}
```

由于 `operator ==` 总是在 `foo` 执行两次后执行，我们唯一的静态变量 `str` 总是最后一次执行的值，因此两者比较恒等。

至此，我们大概已经阐明为何返回引用是一个坏主意。如果我们确实需要构造一个新对象，那就大胆地返回它的值：

```cpp
std::string foo() {
    return "foo";
}
int main() {
	std::string str = foo();
    return 0;
}
```

事实上，**C++** 的 **返回值优化（Return Value Optimization, RVO）** 和 **C++17** 后标准引入的 **复制省略（Copy Elision）** 机制保证返回值的整个过程最优化。上面的例子中，只会出现 *一次* `std::string` 构造。

### 22. 将成员变量声明为 `private`

简单而言，`private` 强调了类的封装性，让所有成员的访问取决于 *getter* 和 *setter*。一方面可以调整成员的可读性和可写性，另一方面在微调类的行为的时候，用户无需知道其中的细节，且可以确保所有开放的接口（`public` 成员函数）都是不变的含义。**C#** 的属性或许是一个更优雅的方案，但是 **C++** 目前的方式并无问题。`protected` 从某种角度讲和 `public` 非常类似，因此作者认为它的封装性一样差。

### 23. 优先使用非成员函数和非友元函数而非成员函数

成员函数是类型很重要的接口，但有时我们需要将一些接口组合起来形成新的成员函数。此时我们不一定需要定义成员函数，而只需将其定义在全局作用域或命名空间中：

```cpp
class MyClass {
public:
    void do_a();
    void do_b();
    void do_c();
    // 省略类实现
};
namespace MyClassUtility {
    void do_all(MyClass& mc) {
        do_a(); do_b(); do_c();
    }
}
```

比起实现一个成员函数或友元函数，这样做的优势是不需要修改任何原有代码。现实中，上面的 `MyClass` 和 `do_all` 通常在不同文件中定义，因此无需修改此前的文件就可以增加类的功能。此外，由于用户可能并不需要所有这样的功能，如果一股脑儿 `#include` 进来会造成编译负担（目前头文件依然是主流）。此时可以将不同的功能分开在不同文件中定义，这样用户就可以用什么再 `#include` 什么了。

**C++** 标准库高度遵守了这样的原则，在一些头文件，如 `<algorithm>` 中定义了通用的模板算法。除非需要针对特定类型进行优化而调用它的成员函数，使用命名空间中的函数更加通用。

### 24. 如果所有参数都需要类型转换，请使用非成员函数

隐式转换有时会显著简化一些工作，比如假设我们定义一个 `Rational` 类：

```cpp
class Rational {
public:
    Rational(int numerator = 0, int denominator = 1);
    int numerator() const;
    int denominator() const;
    Rational const operator *(Rational const& rhs);
    // 省略类实现
};
```

此时进行乘法操作时会出现微妙的“不可交换性”：

```cpp
int main() {
    Rational a(1, 8);
    a = a * 3;			// 没问题，3 被隐式转换为 Rational 类型
    a = 3 * a;			// 错误！这相当于 operator *(3, a)，但我们没有定义这个函数
    return 0;
}
```

因此我们应该用非成员函数的方式重载运算符：

```cpp
Rational const operator *(Rational const& lhs, Rational const& rhs);
```

### 25. 考虑写一个不抛出异常的 `swap` 函数

**C++** 标准库中有一个默认的 `swap` 函数，在 **C++11** 以前定义大致如下：

```cpp
namespace std {
    template<typename T>
    void swap(T& a, T& b) {
        auto tmp = a;
        a = b;
        b = tmp;
    }
}
```

不过这种实现真的太过低效了。很多类型中的指针成员可以用更加高效的方式交换，即浅拷贝。从 **C++11** 开始，如果类型 `T` 可以进行移动操作，则会以下列方式进行 `swap`：

```cpp
template<typename T>
void swap(T& a, T& b) {
    auto tmp = std::move(a);
    a = std::move(b);
    b = std::move(tmp);
}
```

不过，我们可能会有需要自己定义 `swap` 的情形。一般情况下可以尝试特化 `swap` 模板函数：

```cpp
namespace std {
    template<>
    void swap<MyClass>(MyClass& mc_1, MyClass& mc_2) {
        using std::swap;				// 这里的 using 有妙用，稍后会讲到
        swap(mc_1.member, mc_2.member);
    }
}
```

稍等，如果是一个合理设计的类，`swap` 是没有办法访问 `MyClass::member` 的吧？所以上面的代码不能通过编译。一个通常的解决方法是为自己的类型写一个 `swap` 成员函数，然后在 `std` 中特化它：

```cpp
class MyClass {
public:
    void swap(MyClass const& other) {
        using std::swap;
        swap(this->member, other.member);
    }
    // 省略类定义
}
namespace std {
    template<>
    void swap<MyClass>(MyClass& mc_1, MyClass& mc_2) {
        mc_1.swap(mc_2);
    }
}
```

不幸的是，由于 **C++** 不支持函数模板偏特化，当我们想要为模板类定制 `std::swap` 时不能如愿。虽然通过重载依然可以达到效果，但是标准从原则上不允许向 `std` 命名空间添加任何新的东西（函数、类等），但当然，模板特化不在此列。

这时我们只好沮丧地将函数 `swap` 定义在自己的命名空间中了，比如说叫做 `my_namespace`。这个时候我们就可以通过 `my_namespace::swap` 的方式交换两个 `MyClass` 的对象。不过，对于用户来说，它碰到一个类型时，应该用什么方法调用 `swap` 呢？

```cpp
int main() {
    MyClass mc_1{}, mc_2{};
    std::swap(mc_1, mc_2);
    whatever::path::to::my::namespace::swap(mc_1, mc_2);
    return 0;
}
```

上面的第一种使用了 `std::swap`，但是说不定库作者没有实现这个类型的特化，这样调用的就是低效的默认版本。第二种使用了一个（可能）冗长的名字显然也不合适，因为说不定库作者实现了 `std::swap` 的偏特化而并没有在库所在命名空间中重载 `swap`。呃，其实可以通过勤奋地查文档完全解决这个问题。不过有一个更简单的方法：

```cpp
int main() {
    MyClass mc_1{}, mc_2{};
    using std::swap;			// 将 std::swap 引入当前函数、类或命名空间中
    						   // 有效的范围比较微妙，而且 using 本身不像是顺序执行的语句，它可以出现在任何地方，对当前区域都有效。
    swap(mc_1, mc_2);			// 编译器会选择 *最合适的* swap
    return 0;
}
```

**C++** 标准规定，对于没有使用 `::` 符号的标识符，会使用 **名称查找规则（Name Lookup Rules）** 在引入到当前作用域的所有名字（包括本层及外层命名空间所有已经声明的标识符，和参数中类型定义所在的命名空间的标识符）中查找 *最合适的* 一个。这里如果在库作者的命名空间中存在对该类型重载的 `swap`，显然它是最合适的。否则，最合适的就是 `std` 中的特化版本，再其次才是 `std` 中的通用版本。

由于 `swap` 在标准库以及许多第三方库中处处可见且实现地点多样，在 `swap` 之前使用 `using std::swap` 已经成为了惯用法。

最后，成员版本的 `swap` 最好声明为 `noexcept`，因为标准库中的许多类型都依赖此 `swap` 的无异常特性来提供强异常安全性。我们会在后面提到这一点。

## 实现

### 26. 尽可能延后变量定义出现的地方

由于定义变量会有一次构造成一次析构的成本，因此建议在需要使用之前不要构造（因为有时由于分支或异常，使用该变量的语句不一定会执行）。不过对于循环，最好还是将变量定义在外面，否则循环的每一轮都是一次构造和一次析构。

### 27. 尽可能减少类型转换

**C++** 支持下面诸多转型语法：

- `(T) expr`：源自 **C** 的强制类型转换运算符 `(T)`。
- `T(expr)`：函数风格的类型转换，和 **C++** 中的构造函数语法一致。
- `static_cast<T>(expr)`：“相对安全”的强制类型转换，比如 `int` 和 `double` 之间、`unsigned` 和 `signed` 之间、各种指针和 `void*` 之间、基类和派生类之间、从非 `const` 到 `const` 等。这是最常用的类型转换。
- `const_cast<T>(expr)`：去除类型中的 `const` 修饰符。
- `dynamic_cast<T>(expr)`：安全向下类型转换。转换前会判断该类型在其继承图中是否能够转换为目标类型，如果不能则返回 `nullptr`。这是一个动态的类型转换，因此有一定的调用成本。
- `reinterpret_cast<T>(expr)`：特殊类型转换，不过对类型依然有一定要求。这个要求相当复杂，这里不作讲解了。这是最少用到的类型转换。

前两种在 **C++** 早期就有了，我们称其为 *旧式* 类型转换，它们可以模拟除了 `dynamic_cast` 以外任意的类型转换。通常我们建议少使用旧式类型转换（当然，构造函数并不是规避的用法），因为新式的含义更加清晰，便于编译器查验；同时语法风格比较显著，可以方便进行代码分析。

转型并不仅仅是编译器的检查工具，它有可能产生相应的汇编代码。比如 `static_cast<double>(42)` 显然需要什么汇编语句来进行类型转换。此外，基类和派生类的指针进行相互转换时，有时也需要加减一个偏移量。由于 **C++** 内存布局的复杂性（平台不兼容），将对象地址转换为 `char*` 然后逐字节访问是不太理性的（这和 **C** 形成一些反差），即使在一个平台上可行也不代表可以移植到另一平台。

需要注意的是，类型转换会返回一个临时的对象，因此可能会出现意料之外的复制操作：

```cpp
class Derived : public Base {
    void foo() override {
        static_cast<Base>(*this).foo();		// 这里调用 foo 的是类型转换产生的临时对象
        // 省略定义
    }
};
```

最好的方法当然是利用 **C++** 特有的语法：

```cpp
class Derived : public Base {
    void foo() override {
        Base::foo();
        // 省略定义
    }
};
```

`dynamic_cast` 会检测一个类型（的指针或引用）是否可以转换为其某个子类（的指针或引用）。当不能转换时，前一种情况会返回 `nullptr`，后一种情况会产生运行时错误：

```cpp
int main() {
    std::vector<Base*> base_ptrs{};
    // 加入一系列元素
    for (auto ptr : base_ptrs) {
        if (auto dptr = dynamic_cast<Derived*>(ptr)) {			// 只有转换成功时才会进行后续操作
            dptr->special_method_that_only_derived_class_has();
        }
    }
}
```

不过 `dynamic_cast` 通常通过一系列字符串比较来判断类型是否合适，是一个比较低效的操作。因此我们要尽可能避免大量使用 `dynamic_cast` 来做运行时类型判断。下面的代码可能会让人看得眼前一黑：

```cpp
for (auto ptr : base_ptrs) {
    if (auto dptr = dynamic_cast<Derived1*>(ptr)) { /* 省略实现 */ }
    else if (auto dptr = dynamic_cast<Derived2*>(ptr)) { /* 省略实现 */ }
    else if (auto dptr = dynamic_cast<Derived3*>(ptr)) { /* 省略实现 */}
    // 还有更多
}
```

显然我们应该使用例如虚函数的方式来避免这样的低效无趣代码。

### 28. 避免返回对象内部的“句柄”

这里提到的“句柄”是指指针、引用、迭代器等结构。简单来说，我们通过定义类来规定对象管理的资源。如果一个成员函数返回了指向资源的指针，就可能造成悬垂指针问题：在对象的生命周期结束后，这个指针指向的是已经释放的内存。此外，即使是有必要返回指针的情况（如参考[条款15]()），也应该返回指向 `const` 的指针，否则这与将成员声明为 `public` 几乎没什么不同（至于为何我们需要 `private` 而非 `public` 可以回顾[条款22]()）。

### 29. 为异常安全而努力

异常安全性可以体现为下面这几点：

- 不泄露任何资源：确保出错以后，资源（如堆内存、互斥锁等）依然能够释放。
- 不损坏任何数据：确保出错之后，操作之前的数据保持原样。

**C++** 中将提供一场安全保证的函数分为下面几种：

- 不抛出异常保证：保证不会抛出异常。通过 `noexcept` 修饰的函数并不意味着一定不抛出异常（而是在它们抛出异常时会终止程序）。
- 强异常保证：异常抛出后程序状态不改变。这意味着这个函数要么成功，要么（状态上）相当于从来没有调用过。
- 基本异常保证：异常抛出后程序状态是有效状态，但不保证其和函数调用前一致。

异常安全的代码必须要提供上列三种异常保证之一。

有一个比较通用的设计策略，称为 **复制交换（Copy and Swap）**，用以代替简单的赋值：

```cpp
void pure_copy(std::string const& in, std::string* out) {
    out = new std::string(in);			// 这里可能会产生异常，我们没有办法进行处理
}
void copy_swap(std::string const& in, std::string* out) {
    using std::swap;
    auto new_s = std::make_unique<std::string>(in);		// 如果这里出错，原来的数据不会被修改
    swap(out, new_s.release());
}
```

即使是比较复杂的结构，如果支持移动操作，则复制交换只是多了几次移动而已。这样就可以提供强异常保证。不过当函数一定会对全局状态产生改变（产生副作用）时，我们可能要退而求其次，只追求基本异常保证。

最后，请尽量贯彻程序的异常安全。因为即使只有一个函数不是异常安全的，那么整个程序都没有绝对的异常安全可言。

### 30. 彻底了解内联

内联最初用于内联函数。声明为 `inline` 的函数可能会被编译器优化为“内嵌”在调用处的代码，因此省去了函数调用以及栈的操作。

```cpp
inline int zero() {
    return 0;
}
int main() {
    std::cout << zero();		// 可能会被直接优化为 std::cout << 0;
    return 0;
}
```

这是一种类似于宏却比起安全可靠得多的实践形式。所有 **C++** 类中定义的任何函数（无论是成员函数、静态函数还是友元函数）都默认为内联函数：

```cpp
class MyClass {
public:
    int get() {					// 内联函数
        return i;
    }
    void set(int val);
    friend void swap(MyClass mc_1, MyClass mc_2) {	// 内联函数
        std::swap(mc_1.i, mc_2.i);
    }
private:
    int i;
};
inline void MyClass::set(int val) {	// 同样也是内联函数。inline 只需在函数定义处写出
    this->i = val;
}
```

内联函数的定义基本上只出现在头文件中，因为标准要求使用内联函数时需要能够看到它的定义。有趣的事情是，程序中不同的编译单元中允许出现同一个内联函数定义的多份复制；这实际上非常合理，因为内联函数一定出现在头文件中，因此 `#include` 这个头文件的多个源文件一定会得到同一个定义的多份复制。**C++17** 后支持和内联函数这些性质类似的内联变量。声明为 `inline` 的变量允许在头文件中被定义（也即在不同编译单元中出现多份复制）：

```cpp
struct MyClass {
    static inline int i = 20;		// 现在可以通过 inline 在类中定义 static 变量了
};
```

正如之前所说，`inline` 并不能保证内联，而是会看具体情况判断是否适于内联；就算不使用 `inline`，编译器也会视情况为我们内联函数。一个常见的不内联的情形是通过函数指针调用函数。此时编译器无从知晓这个指针指向对象，因此不可能进行内联操作。内联的主要缺点是造成代码膨胀。作者的建议是只对小型、频繁调用的函数使用 `inline`。

最后需要提及的是，**C++11** 引入的 `constexpr` 会自动将函数以及变量（**C++17**）声明为 `inline`。

### 31. 将文件间的编译依赖关系降到最低

编译依赖关系决定了修改文件后究竟有多少代码要经过重新编译。相信我，在 **Modular C++** 没有成为主流之前，我们毫无疑问应该注意编译效率（**C++** 以编译速度慢闻名）。因此，作为库开放的接口，头文件应该尽可能少（最好从来不要）进行修改，否则就会导致用户代码不得不重新编译。

但是我们有时候确实需要调整一些类型的定义。这里的建议是，只进行 **提前声明（Forward Declaration）**，并在需要定义的地方只使用这些类型的指针或引用：

```cpp
class ComponentA;
class ComponentB;
class ComponentC;
class ModuleImpl;
class Module {
public:
    Module(ComponentA const& ca, ComponentB const& cb, ComponentC const& cc);
    // 省略定义
private:
    std::unique_ptr<ModuleImpl> m_impl;		// 一个指向实现块的指针（Pimpl）
};
```



## 继承与面向对象设计

### 32. 确保 `public` 继承体现了 is-a 的关系

### 33. 避免名称覆盖

### 34. 区分对接口和对实现的继承

### 35. 考虑 `virtual` 函数以外的其它选择

### 36. 绝不重新定义继承的非 `virtual` 函数

### 37. 绝不重新定义继承的默认参数值

### 38. 通过组合体现 has-a 关系

### 39. 审慎地使用 `private` 继承

### 40. 审慎地使用多重继承

## 模版和范型编程

### 41. 了解隐式接口和编译器多态

### 42. 了解 `typename` 的双重含义

### 43. 了解如何访问模版化的基类中的名称

### 44. 将与模版参数无关的代码抽出模版

### 45. 通过模版成员函数接受所有兼容类型

### 46. 需要类型转换时在模版中定义非成员函数

### 47. 使用特征类型表现类型信息

### 48. 认识模版元编程

## 自定义 `new` 和 `delete`

**C++** 区别于许多其它现代高级语言的一点，就是其采用手动管理内存。相比 **Java** 或 **Python** 这些采用 **垃圾回收（Garbage Collection, GC）** 机制的语言，**C++** 程序员需要花更多精力来写出安全有效的代码。尤其是在多线程环境下，堆是一个可修改全局资源，因此会发生 **竞争条件（Race Condition）**，此时不同线程执行的顺序会影响到最终的结果。如果我们使用的函数没有为并发做好准备，内存相关操作可能会出现严重问题。

### 49. 了解 new-handler 的行为

### 50. 了解替换 `new` 和 `delete` 的合理时机

### 51. 编写 `new` 和 `delete` 时遵守约定

### 52. 如果写了 placement-new，也要写 place-delete

## 杂项

### 53. 注意编译器的警告

### 54. 熟悉包括 **TR1** 在内的标注库

### 55. 熟悉 **Boost**

