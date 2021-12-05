# C++ Tips

这里记录了我在实际写码中学习到的一些 **C++** 要点，其顺序没有多少关系。每一条都只反映当时我的一些实验和反思，不一定是正确或最优的选择。通常我也会查阅网上的相关建议，如果有必要会附在每一条后面。

[TOC]

## 仿函数的传递方式（2021/12/02）

之前不记得是在哪里学到的，将仿函数（比如标准库种经常出现的 `Compare`、`Predicate` 模板类型）通过转发引用方式传递，但今天写码的时候突然有点迷惑：通常我也不需要转发（`std::forward`）这个仿函数，为什么还要这样传进来呢？我看了看标准库中的传递方式，比如 `std::sort` 的接口如下：

```cpp
template<class RandomIt, class Compare>
void sort(RandomIt first, RandomIt last, Compare comp);
```

这里是用值传递的，显然这是最保险的方式，因为值传递可以包揽所有情形：首先对于最常见的，使用 lambda 表达式或用标准库中 `std::less` 等在调用处当场构建对象的时候，作为纯右值它会移动传递到 `sort` 函数中；如果是一个左值那就只能复制进来了，对于一些复杂的仿函数这可能有点让人膈应。

显然我们不应该以左值引用传递，因为这就直接封死了 lambda 表达式。那么常量左值引用（`const&`）如何呢？一开始我差点确信这个没什么问题了，因为常量左值引用一向作为万金油的参数传递方式：因为还有不少不允许复制操作的类型，但是没有任何类型不允许引用。刚改一两行码，突然意识到这样产生的问题：如果仿函数是通过 `const` 传进来的，那就不应该允许修改捕获变量了，这样对于声明为 `mutable` 的闭包可能会编译不过：

```cpp
template<typename F>
void foo(F const& f) {
    f();
}

int main() {
    int i = 42;
    foo([i] mutable { return ++i; });		// 错误，mutable 的闭包，其 operator() 没有被声明为 const
    return 0;
}
```

于是回到开头的解法，既然常量左值引用不合适，我们或许应该试试转发引用，因为它一样可以接受任何传递形式的参数。下面我做了一个小实验，来看看仿函数类型通过转发引用传递时是怎么选择的：

```cpp
template<typename F>
void foo(F&& f) {
    std::cout << f();
}
struct Functor {
	Functor() = default;
    Functor(Functor const& other) {
        std::cout << "copied" << '\n';
    }
    Functor(Functor&& other) {
        std::cout << "moved" << '\n';
    }
    auto operator ()() const {
        return 42;
    }
};

int main() {
    Functor f;
    foo(f);					// 传递左值引用
    foo(Functor{});			// 传递右值引用
    foo(std::move(f));		// 传递右值引用
    return 0;
}
```

答案其实还挺出乎我意料的（因为我此前对转发引用的理解有问题），三种情况都会静悄悄地输出 `42` ，仿佛没有产生任何构造操作（我原来以为在通过右值引用传递时会进行一次移动构造，这简直误会大了）。事实上，无论是通过左值还是右值引用传递，它们都是 *引用*，因此不会发生任何构造。之所以会产生它们会发生移动或复制操作的错觉是因为通常在函数中会出现类似于：

```cpp
auto new_obj = std::forward<T>(elem);
```

或者：

```cpp
auto new_obj = std::move(elem);
```

的用法。当右值引用出现在初始化语句中时，*的确* 会发生移动构造，同理左值引用在初始化语句中也会导致复制构造，因为此时确实需要构造一个对象。

回到之前的例子，毫无疑问转发引用的效果是最好的，因为无论是左值还是右值，它都能低成本地传递到函数当中。

既然谈到了仿函数，这里讲一个题外话。通常来讲我们认为闭包类型和手动定义的仿函数是等价的，但通过这次的例子我们发现了 lambda 的局限性：它默认是 `const` 的，但当声明为 `mutable` 时它不是将捕获变量声明为 `mutable`，而是去除 `operator ()` 函数的 `const` 修饰，这就带来一定的局限性。作为对比，手动定义的仿函数是可以通过 `const&` 传递的：

```cpp
template<typename F>
void foo(F const& f) {
    std::cout << f() << '\n';
}
class Functor {
public:
    Functor(int i, int& r) : m_i(i) {}
    auto operator ()() const {
        return i += 10;
    }
private:
    mutable int m_i;
};

int main() {
    Functor f(10);
    foo(f);				// 输出 20
    foo(f);				// 输出 30
    return 0;
}
```

引用捕获的变量没法通过 `mutable` 标记，但它们本来就可以随意修改（**C++** 中的引用本身不能修改，即所有引用类型 `T&` 都似乎默认是 `T& const`，虽然对引用的修饰符是非法的；但引用所指的对象可以随意修改），即使闭包被默认声明为 `const` 的。这其实某种角度也反映了 **C++** 中 `const` 的局部性，无论是指针还是引用都无法通过内置语法将其常量性传递到更深层。一个解决方案是 `std::propagate_const`，它能够在对象被声明为 `const` 时，令使用这个模板的类指针对象指向内容也被 `const` 修饰：

```cpp
struct Foo {
    void foo() {
        std::cout << "Foo::foo()" << '\n';
    }
    void foo() const {
        std::cout << "Foo::foo() const" << '\n';
    }
};
struct Bar {
    Bar() : ptr(std::make_unique<Foo>()) {}
    void bar() {
        std::cout << "Bar::bar()" << '\n';
        ptr->foo();
    }
    void bar() const {
        std::cout << "Bar::bar() const" << '\n';
        ptr->foo();
    }
    std::propagate_const<std::unique_ptr<Foo>> ptr;
};
int main() {
    Bar b1{};
    Bar const b2{};
    b1.bar();		// 输出 Bar::bar() 及 Foo::foo()
    b2.bar();		// 输出 Bar::bar() const 及 Foo::foo() const
    return 0;
}
```

如果不用 `std::propagate_const`，两次都只会调用 `Foo::foo()`。不过这个模板目前还在 **TS** 中，暂时不清楚什么时候合并到标准库中，但可以用 `std::experimental::propagate_const` 来调用。

扯远了，说到底闭包也不能用这个，因为这是用在类型声明上的，捕获列表中可能没法将普通指针/引用类型转换为这个类型。那么应该如何以常量引用/常量指针来捕获变量呢？在 **C++14** 后可以用带初始化的捕获：

```cpp
int main() {
    std::string str = "abc";
    auto f = [&s = static_cast<int const&>(str)] (auto i) { return s[i]; };
    return 0;
}
```

在 **C++17** 后，可以用更简洁的 `std::as_const`：

```cpp
int main() {
    std::string str = "abc";
    auto f = [&s = std::as_const(str)] (auto i) { return s[i]; };
    return 0;
}
```

参考资料：[Stack Overflow](https://stackoverflow.com/questions/3772867/lambda-capture-as-const-reference)

## Trait 定义成类比较好处理（2021/12/04）

众所周知，**C++** 处理模板中的成员（无论是成员变量、成员函数还是成员别名）时无法准确判断其性质，因为在不同实例中可能会有不同含义。因此我们需要两个 **消歧器（Disambiguator）** 来提示编译器某个成员是否为类型（`typename`）以及是否是模板（`template`），下面举两个简单的例子：

```cpp
template<typename T>
struct Foo {
    struct A {};
    using B = int;
    template<typename U>
    struct C {};
    template<typename U>
    U bar(int i, int j) {
        return U{};
    }
};
int main() {
    using AType = typename Foo<int>::Type;
    typename Foo<int>::A a{};
    typename Foo<int>::B b{};
    typename Foo<int>::template C<int> c{};
    auto x = Foo<int>{}.template bar<int>(1, 2);
    return 0;
}
```

以上都是比较夸张的情况。事实上从 **C++20** 起在声明变量以及别名时时不再需要写这些烦人的东西了（虽然在同时存在模板成员函数和普通成员函数时依然需要使用 `template` 关键字来强制指定调用函数），虽然自从 **C++11** 开始标准逐渐开始扩展消歧器的使用场景，比如下面这些迷惑的用法都是合法的：

```cpp
template<typename T>
struct Foo {};

int main() {
    typename ::Foo<int> f1;
    typename std::vector<int> v;
    ::template Foo<int> f2;
    std::template vector<int> v2;
    return 0;
}
```

我是感觉为了追求语法的普适性有点矫枉过正了。当然以 **C++** 的传奇历史说不定以后能够玩出什么花来，但目前来看这就是个毫无用处的语法。

回到正题，**C++** 标准库在设计 **特性（Traits）** 的时候通常会定义一个类，然后再给出一个别名用来省略啰嗦的 `typename`，比如说：

```cpp
template<typename T>
struct is_magic {
    using Type = /* 省略定义 */;
};
template<typename T>
using is_magic_t = typename is_magic<T>::Type;
```

这样能保证在任何情况下只要使用 `is_magic_t<T>` 就一定不需要使用 `typename`（因为 `is_magic_t` 是一个不内嵌在任何模板类中的别名，因此一定是一个类型）。标准库里有大量类似的用法，好不麻烦。

于是我为了节约精力，干脆直接这么写：

```cpp
template<typename T>
using is_magic_t = /* 省略定义 */;
```

这样做确实方便了，但是一旦需要改一些条件，需要用到模板元编程的一些技巧时，这就不好用了（因为模板别名无法偏特化）。我一开始是为了给一些通用成员别名写一个全局的别名，比如：

```cpp
template<typename C>
using ReferenceType = typename C::Reference;
```

不过由于我对成员别名的起名方式有别于标准库（全部首字母大写），标准库里的类到我这里会报错。于是我不得不加上一个判断，但由于我只写了别名，在定义时直接判断是不行的：

```cpp
template<typename C>
using ReferenceType = std::conditional_t<is_stl_v<C>, typename C::reference, typename C::Reference>;
// 这里的 is_stl_v<C> 是我定义的一个判断是否为 STL 容器的 Trait
```

上面会出现编译错误，因为标准库的类型 `C` 根本就没有 `Reference` 这个成员别名。通常的解决方法是：

```cpp
template<typename C, typename = is_stl<C>>
struct Reference;
template<typename C>
using ReferenceType = typename Reference::Type;

template<typename C>
struct Reference<C, std::true_type> {
    using Type = typename C::reference;
}
template<typename C>
struct Reference<C, std::false_type> {
    using Type = typename C::Reference;
}
```

可以看到真的费事了很多，但如果一开始我就把 Trait 的类定义写好，就能少改点东西（我有大概十个类似的 Trait）。