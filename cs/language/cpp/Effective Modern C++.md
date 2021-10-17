# Effective Modern C++ 笔记

[TOC]

这篇是对 *Effective Modern C++ (Scott Meyers)* 的阅读笔记，其对 **C++11** 之后 **C++** 语言的主要编码思路做出了建设性的建议。我将严格按照原书的章节分化，整理罗列出个人在意的知识点并给出自己的心得。

## 类别推导

### 模版类型推导

**C++** 的类型推导早在 **C++98** 就已经运用于模版了，程序员不需要显式给出模版函数的模版参数类型，只要能从所给的参数中推导出它们：

```cpp
template<typename T>
void print(T arg) {
    std::cout << arg;
}
int main() {
    print(1);		// 调用 print<int>(1)
    print(1.0);		// 调用 print<double>(1.0)
 	return 0;   
}
```

上面中 `print` 两次实例化出来的函数没有任何歧义，都能够从参数推导类型。这是一个相当方便的设定。不过，当形参存在修饰符（比如引用修饰符）时，情况会变得有些复杂：

1. 模版函数的形参是一个指针或引用（但不是转发引用）：

   这种情形的类别推导是最直接的，将最外层的引用褪去后，剩下的类型会成为推导的类型：

   ```cpp
   template<typename T>
   void f(T& param);
   template<typename T>
   void g(T* param);
   
   int main() {
       int x = 42;
       int const cx = x;
       int const& rcx = x;
       int&& rrx = 42;
       
       f(x);			// 实例化了 f<int>
       f(cx);			// 实例化了 f<int const>
       f(rcx);			// 实例化了 f<int const>
       f(rrx);			// 实例化了 f<int>
      	
       int* px = &x;
       int const* pcx = &x;
       int const*& rpcx = pcx;
       
       g(px);			// 实例化了 g<int>
       g(pcx);			// 实例化了 g<int const>
       g(rpcx);		// 实例化了 g<int const>
    	return 0;   
   }
   ```

   可以看到内层的 `const` 得到了保留。这是非常符合直觉的类型推导方式。

2. 模版函数的形参是一个转发引用：

   转发引用的特别之处在于它可以区别对待左值和右值。如果将左值传给模版函数的转发引用形参，会实例化一个左值引用，参数也同样变成一个左值引用（这略有些奇异）：

   ```cpp
   template<typename T>
   void f(T&& param);		// 转发引用和右值引用语法相同，但是在带有模版参数时都视为转发引用
   
   int main() {
       int x = 42;
       int const cx = x;
       int const& rcx = x;
       int&& rrx = 42;
       
       f(x);			// 实例化了 f<int&>，参数类型是 int&
       f(cx);			// 实例化了 f<int const&>，参数类型是 int const&
       f(rcx);			// 实例化了 f<int const&>，参数类型是 int const&
       f(rrx);			// 实例化了 f<int&>，参数类型是 int&；这里需要注意的就是右值引用是左值（因为所有引用都是左值）
       f(42);			// 实例化了 f<int>，参数类型是 int&&
    	return 0;   
   }
   ```

   这是唯一一种能在模版推导中得到引用类型的情形。

3. 模版函数的形参不存在任何修饰符：

   此时所有外层的修饰符都会被忽视（先褪去一层引用修饰符，再褪去一层 `const` 及 `volatile` 修饰符）：

   ```cpp
   template<typename T>
   void f(T param);
   
   int main() {
       int x = 42;
       int const cx = x;
       int const& rcx = x;
       int&& rrx = 42;
       int const* const cpcx = &cx;
       
       f(x);			// 实例化了 f<int>
       f(cx);			// 实例化了 f<int>
       f(rcx);			// 实例化了 f<int>
       f(rrx);			// 实例化了 f<int>
       f(cpcx);		// 实例化了 f<int const*>
    	return 0;   
   }
   ```

最后让我们看看两个自带 **退化（Decay）** 的类型：数组和函数。当它们传入上面第三种形参类型时会分别退化为指向首个元素的指针以及指向其自己的函数指针，此外的情形则能够被推导为数组引用及函数引用：

```cpp
template<typename T>
void f(T& param);
template<typename T>
void g(T param);

int main() {
    int arr[3] = { 1, 2, 3 };
    void foo();
    
    f(arr);				// 实例化了 f<int [3]>，参数类型是 int (&)[3]
    f(foo);				// 实例化了 f<void ()>，参数类型是 void (*)()
    g(arr);				// 实例化了 f<int*>
    g(foo);				// 实例化了 f<void (*)()>
    return 0;
}
```

### `auto` 类型推导

**C++11** 引入了 `auto` 关键字的全新含义（此前则是继承 **C** 语言中毫无存在感的自动变量限定符），让类型推导能够出现在各种语句当中。总体来讲，`auto` 类型推导的规则和模版类型推导一致，左侧对等于模版函数的形参声明，右侧对等于模版函数的实参。

```cpp
int main() {
    auto x = 10;			// x 被推断为 int
    auto const cx = 42;		// cx 被推断为 int const
    
    auto& rx = x;			// rx 被推断为 int&
    auto& rcx = cx;			// rcx 被推断为 int const&
    auto const& rcx2 = x;	// rcx2 被推断为 int const& 
    
    auto&& rx2 = cx;		// rx2 被推断为 int const&
    auto&& rrx = 42;		// rrx 被推断为 int&&
    auto&& rx3 = rrx;		// rx3 被推断为 int&
    
    int arr[3] = { 1, 2, 3 };
    void foo();
    auto a = arr;			// a 被推断为 int*
    auto* a = arr;			// a 被推断为 int*
    auto& a = arr;			// a 被推断为 int (&)[3]
    auto f = foo;			// f 被推断为 void (*)()
    auto* f = foo;			// f 被推断为 void (*)()
    auto& f = foo;			// f 被推断为 void (&)()
    return 0;
}
```

`auto` 机理上唯一有别于模版类型推导的就是对于大括号的列表的类型推断：

```cpp
int main() {
    int x1 = 10;			// x1 被声明为 int
    int x2(10);				// x2 被声明为 int
    int x3{10};				// x3 被声明为 int
    int x4 = { 10 };		// x4 被声明为 int
    auto y1 = 10;			// y1 被推断为 int
    auto y2(10);			// y2 被推断为 int
    auto y3{10};			// y3 被推断为 std::initializer_list<int>
    auto y4 = { 10 };		// y4 被推断为 std::initializer_list<int>
 	return 0;   
}
```

这实际上还是一个对模版推导的补完；如果对模版函数中传入大括号的列表，是不能自动推断出其类型的。而 `auto` 可以根据列表的元素类型推断为例如 `std::initializer_list<int>` 的类型。只不过这样就没发让 `auto` 初始化的类型和显式声明类型的初始化的类型完全一致了，需要注意这一点。

**C++11** 为了统一变量初始化的语句而引入了 `{}` （列表初始化）这个机制，却又因为对同时引入的 `std::initializer_list<>` 的“过多偏爱”造成一些意想不到的情况。我们后面可能还会提到这一点。

### `decltype`

`decltype` 是 **C++11** 引入的新关键字，它能够接收一个表达式，然后返回一个具体的类型，其中也反应了某种类型推导机制。令人沮丧的是，这种机制和我们之前提到的模版推导和 `auto` 推导都有所不同。

首先让我们看看普通的情形：

```cpp
int const cx = 42;			// decltype(cx) 是 int const
void foo(int const& x);		// decltype(foo) 是 void foo (int const&)
struct Point {
    int x, y;				// decltype(Point::x) 是 int
} p;						// decltype(p) 是 Point
std::vector<int> v(10);		// decltype(v) 是 std::vector<int>
							// decltype(v[0]) 是 int&
```

似乎没有什么不寻常的事情？上面所有类型推导都十分规矩，甚至有点过于规矩，比如函数没有退化为指针、引用没有被褪去。这种忠实于表达式真实类型的性质让 `decltype` 一度成为比较流行的模版函数返回值类型的推断：

```cpp
// 下面这种函数声明语法是 C++11 引入的，可以用 auto 作为返回类型占用符，然后将真正的返回类型用 -> 结构显式给出
// 在使用这种尾序语法时，可以利用 decltype 判断某个表达式的类型。需要注意表达式中不能出现没有引入的标识符
template<typename Container, typename Index>
auto auth_and_access(Container& c, Index i) -> decltype(c[i]) {
    authenticate_user();
    return c[i];
}
```

**C++14** 起，我们不需要显式给出尾序的返回类型，编译器可以根据 `return` 语句来判断返回类型：

```cpp
template<typename Container, typename Index>
auto auth_and_access(Container& c, Index i) {
    authenticate_user();
    return c[i];
}
```

呃，不过需要指出，这种情况下会使用 `auto` 的类型推导规则。而我们之前认识到它会褪去一层引用，这就和我们的初衷不符了。这也是为什么 **C++14** 同时引入了新的尾序函数声明的占位符 `decltype(auto)`。用它代替 `auto` 可以得到和之前一样的结果：

```cpp
template<typename Container, typename Index>
decltype(auto) auth_and_access(Conatiner& c, Index i) {
 	authenticate_user();
    return c[i];
}
```

实际上，`decltype(auto)` 可以用在所有 `auto` 能够使用的地方，也就是可以直接借用它来在任何地方进行类型推导：

```cpp
int main() {
	decltype(auto) x = 10;			// x 被推断为 int（等同于 decltype(10)）
    decltype(auto) const cx = 10;	// 错误，decltype(auto) 四周不能接其它修饰符了
    int const c = x;
    decltype(auto) cx = c;			// x 被推断为 int const
    int arr[3] = { 1, 2, 3 };
    decltype(auto) rx = arr[0];		// rx 被推断为 int&
    int&& temp = 42;
    decltype(auto) rrx = temp;		// rrx 被推断为 int&&
 	return 0;   
}
```

现在要开始讲述 `decltype` 比较神奇的特性。我们在前面的例子中看到，对一个变量进行 `decltype` 时，总是能够得到其本身的类型（尽管它是一个左值）；但对比如 `arr[k]` 进行 `delctype` 就会得到一个左值引用。这是因为 `decltype` 规定对于所有非平凡的左值表达式，都保证产生一个左值引用。因此 `decltype(x)` 是 `x` 的真实类型，而 `decltype((x))` 就可能会加一个引用符号。这个特点适用于所有 `dectlype` 类型推导规则出现的场景，包括 `decltype(auto)`。

### 如何查看类型推导结果

用 **Visual Studio** 就对了！这一节就这一点最重要，可以翻到下一章了。

开个玩笑，虽然宇宙第一 **IDE** 不见得是吹的，但是我们也要需要一些更轻量且巧妙的方法来获得类型推导结果。

第一种是借用编译器的报错，当模版实例化出现错误时，编译器会给出实例化相关的信息：

```cpp
// 只给出类的声明，实例化是会报错
template<typename T>
class TypeDisplay;

int main() {
    int x = 10;
    TypeDisplay<decltype(x)> type1;
    TypeDisplay<decltype((x))> type2;
    return 0;
}
```

也可以通过古老的 `typeid` 运算符在运行时得到类型信息。`typeid` 会接受一个值或类型并返回 `std::type_info` 对象，调用其 `name` 方法就能得到一段表示其类型的唯一字符串。只不过不同编译器产生的字符串可能大不相同，且不是那么可读：

```cpp
int main() {
    int x = 10;
    int const cx = x;
    int* px = &x;
    int& rx = x;
    int const* const cpcx = &cx;
    struct Temp {} t;
	std::cout << typeid(x).name() << '\n';		// 可能输出 i。这大体是指代 int
    std::cout << typeid(cx).name() << '\n';		// 可能输出 i。所以最外层的 const 被褪去了
    std::cout << typeid(px).name() << '\n'; 	// 可能输出 Pi。可能是 Pointer to int 的意思
    std::cout << typeid(rx).name() << '\n';		// 可能输出 i。所以最外层的引用也被褪去了
    std::cout << typeid(cpcx).name() << '\n';	// 可能输出 PKi。呃，可能是 Pointer to K(C)onst int 的意思
    std::cout << typeid(t).name() << '\n';		// 可能输出 Z4mainE4Temp。这是什么？？
	return 0;
}
```

可以发现 `typeid` 的行为和 `auto` 类型推导非常相似，但可读性令人堪忧。一个替代品是 **Boost** 库的 `type_index.hpp` 头文件，其中包含了一个不会褪去 `const` 等修饰符的 `type_id_with_cvr<>` 函数模版。

## `auto`

### 优先使用 `auto` 而非显式类别声明

让我们看看没有 `auto` 的 **C++11** 会出现什么样的代码：

```cpp
template<typename F, typename T>
void apply(F f, T t) {
 	f(t);   
}

int main() {
    int x;			// 呃，定义了一个 int 变量，却没有初始化它，它也许是一个混乱的值
    std::function<void (int)> f = [] (int x) { std::cout << x; };	// 可以使用 std::function<> 来绑定 lambda
    apply(f);
    return 0;
}

template<typename It>
void foo(It begin, It end) {
 	while (begin != end) {
     	typename std::iterator_traits<It>::value_type curr = *begin;	// 呃，这真的太长了
        // 省略具体操作
    }
}
```

现在引入了 `auto`，上面的代码都会变得更好：声明语句变短了，且强制其显式初始化；也不需要再用缓慢低效的 `std::function<>`。

```cpp
int main() {
 	auto x;			// 错误，auto 类型推导必须初始化变量
    auto x = 10;	// 没有问题
    auto f = [] (int x) { std::cout << x; };		// 将 lambda 绑定于一个变量。至于其类型，我们不需要关心
}
```

从 **C++14** 起，我们甚至可以将函数和 lambda 的参数类型声明为 `auto`，此时其行为非常像一个模版函数：

```cpp
auto inc = [] (auto x) { return x + 1; };
// 下面的模版函数和上面的 lambda 行为基本一致
template<typename T>
auto inc_templ(T x) {
    return x + 1;
}
```

`auto` 在大多数情况下都能大幅度减少检查类型正确性的工作，且能避免一些看似理所当然实则错误的想法，比如下面这个例子；

```cpp
int main() {
    std::unordered_map<std::string, iknt> m;
    // 需要注意 unordered_map 中存储的实际上是 std::pair<std::string const, int>
    // 因此这里会发生 std::string 的复制，非常浪费资源
    for (std::pair<std::string, int> const& p : m) {
        // 省略操作
    }
    // 这里 p 自动推断为 std::pair<std::string const, int> const&，就没有任何问题了
    // 最主要非常简洁
    for (auto const& p : m) {
     	// 省略操作   
    }
 	return 0;   
}
```

### 当 `auto` 推导的类型不符合要求时，使用显式类型声明

书中举例是代理结构造成的类型推断失误，我们可以用一个比较抽象的例子讲述：

```cpp
class Real {
    // 省略定义
};
class Proxy {
    Proxy& method1() { /* 省略定义 */ }
    Proxy& method2() { /* 省略定义 */ }
    // 这是为了简便的隐式类型转换
  	operator Real() const {
        // 省略定义
    }
};

void foo(Real const& r) {
 	// 省略定义   
}
int main() {
    // 利用 Proxy 类型进行运算，但是结果会返回给一个 Real 类型的变量
    Real result1 = Proxy().method1().method2();
    // 然而如果使用 auto，结果并不会隐式转换为 Real 类型，这可能会引发后面的严重错误
    auto result2 = Proxy().method1().method2();
    foo(result2);		// 类型错误
    // 当然我们可以利用强制类型转换来得到需要的类型
    auto result3 = static_cast<Real>(Proxy().method1().method2());
    return 0;
}
```

## 转向现代 **C++**

### 在创建对象时区分 `()` 和 `{}`

在 **C++** 中，以复杂而臭名昭著的特性中，变量的初始化算是需要最早接触的之一了。普遍情况下，一个变量有五种初始化的格式：

- 只给出类型和标识符，即 `type var;` 的形式
- 使用圆括号，即 `type var(init);` 的形式
- 使用等号，即 `type var = init;` 的形式
- 使用大括号，即 `type var{init};` 的形式
- 使用等号和大括号，即 `type var = { init };` 的形式

其中后面两种作为广泛的初始化语句在 **C++11** 前并不存在，最后一种在此前仅用于数组和结构体变量初始化。这里面比较令人迷惑的就是不带初始值的初始化和利用等号的初始化格式，前者会让新手认为是一个变量声明，后者会被误解为一个赋值操作。实际上，前者对变量进行默认初始化，而后者等同于圆括号的形式。

圆括号的初始化格式看似很美好，却依然存在一个问题：`type var();` 会被视作一个函数的声明而非一个变量的默认初始化；另一方面， `type var;` 无法预测一些内置类型的初始值。因此我们需要一个保证语法一致，且能够对任意类型进行默认初始化或零初始化的语法，这也就是大括号的引入由来。

```cpp
struct MyClass {   
   	int x(0);		// 错误，不能这样初始化成员
    int y = 0;		// 没问题
    int z{0};		// 没问题
    int w = {42};	// 没问题
}

int main() {
 	int x{};		// 将 x 初始化为 0，等价于 int x = int();
    int y{42};		// 将 y 初始化为 42，等价于 int y(42);
    int z = { 10 };	// 将 z 初始化为 10，等价于 int z = 42
    // 相比之下：
    int f();		// 声明了一个函数 f
    int g(42);
    int h = 10;
}
```

因此大括号是最“通用”的初始化方式，它能够融合 **C++11** 以前的所有初始化形式，并 *基本* 达到相同的结果。为什么强调是基本，因为 **C++11** 引入了 `std::initializer_list<>` 模版，并为其开了后门：所有大括号包含的列表都会以更高优先级被视作一个 `std::initializer_list<>` ，这个优先级。这就造成了下面这种微妙的区别：

```cpp
struct MyClass {
  	MyClass(int i, double d) {}
    MyClass(std::initializer_list<double> list) {}
};
int main() {
    std::vector<int> v1 = { 1, 2, 3 };	// 调用 std::vector<int>(std::initializer_list<int>)
    std::vector<int> v2(3, 0);			// 调用 std::vector<int>(size_type, int)
    std::vector<int> v3{3, 0};			// 调用 std::vector<int>(std::initializer_list<int>)
    
    MyClass mc1{1, 2.0};				// 调用 MyClass(std::initializer_list<double>)
    MyClass mc2{1, 2.0lf};		
    	// 错误，其认定调用 MyClass(std::initializer_list<double>)，但这里传入的 long double 不能隐式类型转换
    return 0;
}
```

简单解释这个性质，就是大括号初始化在没有定义 `std::initializer_list<>` 为参数的构造函数时，等同于用圆括号的直接初始化，否则只要大括号内的所有对象类型都能被显式转换为指定的类型，就会被当作 `std::initializer_list<>` 处理（且如果不能够隐式类型转换，还会产生错误。这点其实特别离谱），需要特别注意。

但是，如果是空的大括号，会被编译器理解为默认初始化，而非空的 `std::initializer_list`<>`：

```cpp
struct MyClass {
    MyClass() {
        std::cout << "Default initialization";
    }
    MyClass(std::initializer_list<int> list) {
        std::cout << "List initialization";
    }
};
int main() {
	MyClass mc1{};			// 调用 MyClass()
    MyClass mc2 = {};		// 调用 MyClass()
    MyClass mc3({});		// 调用 MyClass(std::initializer_list<int>)
    MyClass mc4{{}};		// 调用 MyClass(std::initializer_list<int>)
    MyClass mc5 = { {} };	// 调用 MyClass(std::initializer_list<int>)
    return 0;
}
```

呜哇，这还真的是令人头疼。明明引入了一个看似能够统合所有风格的初始化方法，却因为对 `std::initializer_list<>` 的偏爱而产生难以预知的问题；默认初始化的情形也显得前后不一致。个人使用的风格是，仅在默认初始化以及使用 `std::initializer_list<>` 作为参数时才会使用大括号，除此之外只使用小括号的直接初始化。

```cpp
int main() {
    std::ostringstream oss{};			// 默认初始化
    std::vector<int> v1(3, 10);			// 直接初始化，将向量初始化为 3 个 10
    std::vector<int> v2 = { 1, 2, 3 };	// 列表初始化
 	return 0;   
}
```

这样能够避免任何歧义，且确保所有变量正确地初始化。

### 优先选用 `nullptr` 而非 `0` 或 `NULL`

**C++11** 之前沿用了 **C** 语言的 `NULL`，其定义是基于实现的，但通常是一个整型。这会导致存在参数为整型和指针类型的函数重载时，会优先调用整型参数的函数，这就和预期不符了。相比之下，`nullptr` 是独特的  `std::nullptr_t` 类型，其被设计为能够自动转换为任何指针类型：

```cpp
void foo(long long l):
void foo(void* ptr);
void bar(std::unique_ptr<int> ptr);
int main() {
    foo(NULL);		// 可能会调用 foo(long long)
    foo(nullptr);	// 一定会调用 foo(void*)
    bar(NULL);		// 错误，NULL 不是一个指针类型
    bar(nullptr);	// 没有问题
    return 0;
}
```

### 优先使用 `using` 而非 `typedef`

`using` 是 **C++11** 引入的别名声明关键字，它可以将某个标识符声明为一个类型。相比继承自 **C** 语言的 `typedef` 语句，`using` 在语法上更加可读，比如下面这几个例子：

```cpp
// 声明了 func_type1 类型
typedef void (*func_type1)(int, double);
// 声明了 func_type2 类型
using func_type2 = void (*)(int, double);
```

`using` 语句中等号的左侧是新声明的标识符，而右侧是想要声明为别名的类型，相比 `typedef` 要清楚很多（虽然依然很奇怪）。

除了这点，可以定义模版 `using` 别名，而 `typedef` 就不行：

```cpp
template<typename T>
using vector_iterator1 = typename std::vector<T>::iterator;
// 如果使用 typedef 只能这样写
template<typename T>
struct vector_iterator2 {
  	typedef std::vector<T>::iterator type;
};
int main() {
 	vector_iterator1 it1{};
    // 此时必须要使用 typename 关键字，因为所有类型中定义的类型及别名都需要 typename 才能判断为类型
    typename vector_iterator2::type it2{};
}
```

关于啰嗦的 `typename`，**C++14** 对 `<type_traits>` 中的一些模版如 `std::remove_reference<>` 定义了 `using` 别名，能够省略一些冗余：

```cpp
namespace std {
template<typename T>
using remove_reference_t = typename remove_reference<T>::type;
}
int main() {
 	typename std::remove_reference<int&>::type i{};		// C++14 前的写法
    std::remove_reference_t<int&> j{};					// C++14 之后的写法
}
```

这显著减少了无意义的代码量。

### 优先选用限定作用域的枚举类型

**C++** 继承了 **C** 语言的无作用域枚举，也即枚举中的所有标识符都仿佛定义在枚举类型定义所在的作用域中。这会造成命名空间污染：

```cpp
enum Color { black, white, red };
void foo(int i);
int main() {
    auto white = false;			// 错误，双重定义
    foo(white);					// 可以通过，因为无作用域枚举类型可以隐式转换为其它类型
 	return 0;   
}
```

这并不符合 **C++** 设计命名空间以避免标识符污染，以及避免隐式类型转换的思想。于是在 **C++11** 中引入了 `enum class` 的概念：

```cpp
enum class Color { black, white, red };
void foo(int i);
int main() {
	auto white = Color::white;		// 没有问题 
    foo(white);						// 错误，类型不一致
    foo(static_cast<int>(white));	// 枚举类型依然可以转换为其它整数类型，这是因为它本质上就是一个整型
}
```

每个枚举类型本质都是一个整型，我们可以用类似于类型继承的语法来显式指定其本质的类型。通过 `std::underlying_type<>` 能得到枚举类型的本质类型。

有时候会感觉限定作用域的枚举类型过于啰嗦，这个时候可以写一个帮助函数：

```cpp
template<typename E>
constexpr typename std::underlying_type<E>::type toUType(E enumerator) noexcept {
 	return static_cast<typename std::underlying_type<E>::type>(enumerator);   
}
enum class Color { black, white, red };
void foo(int i);
int main() {
 	foo(toUType(Color::white));
    return 0;
}
```

### 优先使用删除函数，而非 `private` 未定义函数

通常，如果我们不希望用户调用某个函数（或成员函数），只需要不声明它即可。然而， **C++** 有时会自动生成一些成员函数，他们是下面这些，也合称为 **特殊成员函数（Special Member Function）**：

- 默认构造函数，即零参数构造函数。当没有定义任何构造函数时，这个成员函数会被自动定义为对所有成员的默认构造
- 析构函数。当没有定义析构函数时，这个成员函数会被自动定义
- 复制构造函数。当没有定义复制构造函数和移动构造函数时，这个成员函数会被自动定义为对所有成员的复制构造
- 复制赋值运算符。当没有定义复制赋值运算符和移动赋值运算符时，这个成员函数会被自动定义为对所有成员的复制赋值
- 移动构造函数。当没有定义复制操作、移动操作及析构函数时，这个成员函数会被自动定义为对所有成员的移动构造
- 移动赋值运算符。当没有定义复制操作、移动操作及析构函数时，这个成员函数会被自动定义为对所有成员的移动赋值

我们会在后面的条款中再提到它们自动生成的规律。但是现在我们希望避免它们自动生成这样的成员函数，比如我们就不希望某个类型的变量能够被复制（很快我们就会看到一个例子）。如果我们不定义复制构造函数，编译器会为我们自动生成一个，这可不中。在 **C++98** 及以前，我们可以通过将这样的成员函数显式声明为 `private` 的，且不去定义它们。这样的好处在于，用户端不可能调用这些函数，而其它成员函数或友元错误地使用它们的话，也会产生未定义的错误。

**C++11** 引入了 `= delete` 代替函数体的函数定义方式，表明当前函数不能被调用。这能够更加清晰地展示出类的接口，也能在其它成员函数或友元错误调用的情形的报错提前到编译阶段。

或许你对 `= delete` 用于上面的情形早就有所耳闻，但令人吃惊（但是意料之中）的是它同样也可以用于普通的函数上：

```cpp
void test(int i) {
 	// 省略定义   
}
void test(bool b) = delete;
void test(char c) = delete;
int main() {
 	test(10);  		// 调用 test(int)
    test(true);		// 调用 test(bool)，但其定义为 = delete，因此发生错误
    test('c');		// 调用 test(char)，但其定义为 = delete，因此发生错误
    return 0;
}
```

通过 `= delete` 语法我们可以禁止函数调用处的隐式类型转换。如果用于模版函数，则可以禁止特定类型的模版函数实例化：

```cpp
template<typename T>
void process_pointer(T* ptr);
template<>
void process_pointer(void*) = delete;
template<>
void process_pointer(char*) = delete;
```

### 为重写的函数添加 `override` 声明

**C++** 的类继承和重写机制我们已经很熟悉了，一个成员函数可以被派生类的成员函数重写，只要满足下面的条件：

- 基类的成员函数被 `virtual` 修饰，或其重写了某个基类的成员函数
- 基类的成员函数和子类的成员函数拥有相同的签名，签名包含了：函数参数列表、函数修饰符（`const`、`&` 等）
- 基类的成员函数返回值需要和子类的成员函数相同或有协变关系（基类指针和子类指针的关系）

由于重写所需的限定符只有在基类成员函数上的 `virtual`（如果这个成员函数本身也进行了重写，甚至可以省略这个限定符），当我们对子类成员函数的声明存在错误的时候，其不容易被发现，因为它可能会很自然地被当作一个新的成员函数；如果我们甚至忘记为基类成员函数使用 `virtual` 关键字，拥有同样签名的子类成员函数会隐藏基类的成员函数。因此，需要一个提示编译器子类重写的关键字，这就是 `override`。

```cpp
struct Base {
	virtual void mf1() const;
    virtual void mf2(int x);
    virtual void mf3() &;
    void mf4();
};
struct Derived {
  	virtual void mf1() const override;	// 这里 virtual 并不是必须的
    void mf2(unsigned x) override;		// 错误，并不能构成重写，函数参数类型不同
    void mf3() && override;				// 错误，并不能构成重写，函数限定符不同
    void mf4() override;				// 错误，并不能构成重写，没有找到基类虚函数
};
```

`override` 是一个上下文关键字，它可以成为一个合法的标识符，当且仅当在函数声明中的特定位置才会被理解为上文中介绍的含义。编译器会对所有用 `override` 修饰符的函数进行特殊检查，如果找不到其重写的基类函数，就会产生错误。 `override` 并不是必须添加的，只作为提示编译检查的标签。

### 优先使用 `const_iterator`，而非 `iterator`

这个条款沿袭了 **C++** 一直贯彻的代码思想，那就是能使用 `const` 的地方一律使用 `const`。

`iterator` 和 `const_iterator` 都是 **C++** 标准库中容器给出的类型，其行为类似于指针和指向常数的指针。在 **C++98** 及之前的标准中，似乎没有什么 `const_iterator` 能大展身手的地方，因为标准库的接口大多只支持 `iterator`，甚至不提供与 `begin` 和 `end` 对等的 `cbegin` 和 `cend`，这让 `const_iterator` 无论是从构造到使用都举步维艰。

从 **C++11** 开始，这些都得到了改善。我们可以通过成员函数 `cbegin` 和 `cend` 构造 `const_iterator`，且所有标准库中所有指示位置的迭代器类型都是 `const_iterator`。不过对于一些没有实现 `cbegin` 和 `cend` （但要确保实现了 `begin` 和 `end`）的类型，我们可以使用标准库的 `std::cbegin` 和 `std::cend` 函数（**C++14**）。这在进行模版编程，接受一个任意的容器类型时会很好用：

```cpp
template<class C, typename V>
void find_insert(C& cont, V const& target, V const& val) {
    auto it = std::find(std::cbegin(cont), std::cend(cont), target);
    container.insert(it, val);
}
```

即使在 **C++11** 中，我们也可以自己动手实现作为普通函数的 `cbegin` 和 `cend`：

```cpp
template<typename C>
auto cbegin(C const& c) -> decltype(std::begin(c)) {
 	return std::begin(c);	// std::begin（C++11）作用于引用常容器可以得到 const_iterator 类型   
}
```

值得一提的是，上面这段函数对数组也是有效的，这是因为 `std::begin` 和 `std::end` 对数组类型进行的特化，返回一个指针。

### 只要函数不会产生异常，就为其加上 `noexcept` 声明

**C++** 关于异常的处理在 **C++11** 发生了翻天覆地的变化。此前使用的是 **动态异常申明（Dynamic Exception Specification）**，即在函数参数列表后给出 `throw()` 或 `throw(...)`，括号中包含所有可能被抛出的类型；如果没有给出 `throw`，则默认可以抛出所有类型。一个函数不能抛出其声明处没有给出的类型：

```cpp
void foo(int i) throw(int, char*) {	// 老实说，这是一个挺恶心的设计
    if (i > 10) {
        throw 10;
    }
    throw 1.0;		// 错误，在声明中没有允许抛出浮点类型
}
```

动态异常申明还可以出现在函数参数中，因此它非常像是函数签名的一部分（但实际上并不是，它不能出现在 `typedef` 语句中）。但是这个语法会让人非常疲于使用，一旦抛出的类型需要更新，可能会要求客户代码也要进行更改。我们发现，其实抛出什么类型的异常并不重要，重要的是是否会抛出异常。这就引出了 `noexcept` 的概念。

**C++11** 开始，可以通过 `noexcept` 来声明一个函数不会产生异常，下面的代码在语义上是一致的：

```cpp
void f() throw();			// 在 C++20 之前有效
void g() noexcept;			// 在 C++11 之后有效
```

 `noexcept` 对 `throw()` 有一个优势存在，前者在运行期有异常产生时，一定会将栈展开到调用处，进行一些操作后终止程序；后者则不一定进行栈展开。这样的差别使得优化器不需要对声明为 `noexcept` 的函数的栈保持可展开状态，也不需要在异常产生时对函数中所有对象以其构造顺序进行析构。换句话说 `noexcept` 声明的函数给予其更高的信任，效率会有一定提升。

`noexcept` 有一个变体 `noexcept(expr)`，它会在编译时检查 `expr` 是否能得到 `true`，而另外还有一个 `noexcept(op)` 表达式，其可以在编译期检查某个表达式 `op` 是否可能产生异常，并返回其对应的布尔值。因此会产生下面这样的代码：

```cpp
// 下面这个对数组 swap 的函数在 swap 其中元素不会发生错误时，就是 noexcept 的
template<typename T, size_t N>
void swap(T (&a)[N], T (&b)[N]) noexcept(noexcept(swap(*a, *b)));

template<typename T1, typename T2>
struct Pair {
	T1 first;
    T2 second;
    // 下面这个对 Pair 进行 swap 的函数要确保对于其任一个成员的 swap 都是 noexcept 的
    void swap(Pair& other) 
        noexcept(noexcept(std::swap(first, other.first)) && noexcept(std::swap(second, other.second));
};
```

`noexcept` 是函数接口的组成部分，在 **C++17** 后甚至成为了函数类型的一部分；然而， `noexcept` 函数可以调用非 `noexcept` 的函数，一旦抛出异常，程序会直接调用 `std::terminate` 终止程序。因此我们要特别小心地使用 `noexcept` 关键字，不要在不必要的地方使用 `noexcept`，甚至为了 `noexcept` 而修改代码逻辑（比如用状态码代替异常），这会增添项目的不稳定性。但一旦确定这个函数不会产生异常且（更难判断地）不会调用任何可能产生异常的函数时，请务必使用 `noexcept` 。

有一种情形需要特别说明，就是当函数的异常性质取决于函数参数时，我们称此为 **窄契约（Narrow Contract）**，区别于没有前置条件的 **宽契约（Wide Contract）** 函数。窄契约函数也并不适用于 `noexcept`，比如 `std::vector` 的 `push_back` 在超过 `capacity` 时会产生异常，这在编译期是无法确定的，因此虽然大多数情况它没有问题，也不能声明为 `noexcept`。

最后，析构函数（以及类似特征的 `operator delete` 等）都隐式带有 `noexcept` 声明，这是因为资源释放时抛出异常并不是一个值得借鉴的代码风格，因此标准对此作出了强制。当然，你也可以显式将它们声明为 `noexcept(false)`，不过这很罕见。

### 只要有可能使用 `constexpr` 就使用它

`constexpr` 不可不谓是 **C++11** 引入的最重要特性之一，其在任何需要产生编译期常量时进行编译期的初始化及函数调用。作为变量声明的限定符时，其行为和 `const` 修饰符有点类似：

```cpp
constexpr int size = 10;
size = 20;					// 错误，constexpr 变量是 const 的，在初始化后不能够修改
constexpr int x;			// 错误，constexpr 变量必须初始化
std::array<int, size> arr;	// 没有问题，因为 constexpr 可以在编译期对 size 进行求值
```

相比之下，`const` 并不要求编译期的求值（考虑函数中的 `const` 参数、返回值以及类中定义的 `const` 成员），也没有办法作为模版非类型参数。`constexpr` 函数的行为则取决于上下文：

```cpp
// constexpr 函数会根据情况判断是否在编译期执行，它并不代表其返回一个常变量
constexpr int add(int a, int b) {
 	return a + b;   
}
constexpr int sum = add(1, 2);		// 编译期执行
extern int unknown;
int result = add(unknown, 42);		// 运行期执行
int x = add(1, 2);					// 可能在编译期，也可能在运行期执行
std::array<int, add(1, 2)> arr;		// 编译期运行
```

在 **C++14** 前，`constexpr` 函数中只允许出现一个语句，也即 `return` 语句，这也意味着返回类型为 `void` 的函数不能声明为 `constexpr`。虽然我们可以通过三目运算符和递归来达到大部分想要做的事，但是毫无疑问 **C++14** 之后，可以写出表达力更强的 `constexpr` 函数了。`constexpr` 函数的概念同样可以拓展到成员函数和构造函数，这使得自定义类型也能拥有编译期构造与操作的能力：

```cpp
struct Point {
  	constexpr Point(float x_, float y_) : x(x_), y(y_) {}
    // （C++14）在之前成员函数被声明为 constexpr，则自动被 const 修饰符修饰，但实际上这是不合理的
    constexpr Point& negate() {
     	x = -x;
        y = -y;
        return *this;
    }
    float x, y;
};
constexpr float inner_product(Point const& p1, Point const& p2) {
 	return p1.x * p2.x + p1.y * p2.y;
}
int main() {
    // result 在编译期就能得到 0.0f 的结果
    constexpr auto result = inner_product(Point(1.0f, 2.0f).negate(), Point(2.0f, -1.0f));
    return 0;
}
```

从这些例子中可以看到，`constexpr` 为函数提供了更广的运用场景，且在不需要编译期求值的情况下也不会产生问题。因此我们建议在所有能够使用 `constexpr` 的情况下使用它。事实上，从 **C++14** 起，标准库中大量的函数和成员函数都在向 `constexpr` 进化，**C++20** 标准更是让一些容器类型都变为可以编译期构造的。这是一个很有用的突破。

### 保证 `const` 成员函数的线程安全性

在 **C++11** 之前，**C++** 尚未提供对多线程的库支持（通常需要使用 **C** 语言的库，如 `<pthread.h>` 才能编写多线程代码），因此线程安全问题并没有那么热门。现在 **C++** 的代码显然需要考虑多线程情况下的问题。通常来讲，一个 `const` 的语义确保变量在初始化之后不会发生变化，但 **C++** 的 `mutable` 关键字为其敲开了一个小口：

```cpp
class Polynomial {
public:
    using RootsType = std::vector<double>;
    // 这个函数设置为 const 因为其多数时候都不会改变当前对象
    RootsType roots() const {
     	if (!calculated) {
         	// 省略定义
            calculated = true;
        }
        return root_vals;
    }
private:
    mutable bool calculated = false;	// 声明为 mutable，则其可以在 const 函数中被修改
    mutable RootsType root_vals{};
};
```

因此，这个 `const` 函数并不能保证得到稳定的值，在不同线程中调用这个函数时，会出现 **数据竞争（Data Race）**，导致未定义行为。解决它的办法很简单，可以使用 `std::mutex`：

```cpp
class Polynomial {
public:
    using RootsType = std::vector<double>;
    RootsType roots() const {
     	std::lock_guard<std::mutex> lg(m);	// 这会保证在当前作用域中的代码同时只被一个线程访问
        if (!calculated) {
         	// 省略定义
            calculated = true;
        }
        return root_vals;
    }
private:
    mutable std::mutex m{};
    mutable bool calculated = false;
    mutable RootsType root_vals{};
};
```

如果会产生数据竞争的变量只是基本类型，比如整型、也可以使用更加轻量化的 `std::atomic`，其保证该变量同时只能被一个线程访问：

```cpp
class Point {
public:
	double distance_from_origin() const noexcept {
     	++count;			// 这个语句同时只能有一个线程访问，所以是安全的
        return std::sqrt((x * x) + (y * y));
    }
private:
    mutable std::atomic<unsigned> count = 0;
    double x, y;
};
```

需要注意 `std::mutex` 与 `std::atomic` 均是不能复制的类型（复制构造函数定义为 `= delete`），因此包含它们的类自动生成的复制构造函数是不能调用的。

不过 `std::atomic` 致命的的问题在于其不能应用于需要多个变量进行原子操作的情形，因为无法保证多个语句是只允许单独线程的：

```cpp
std::atomic<int> x = 0;
std::atomic<bool> b = false;
int foo() {
 	if (!b) {
     	b = true;
        // 此时 b 已经使用完毕，另一个线程可能会拿到掌握权，然后直接在这个函数开头跳出判断，直接返回 x，而不是预料中的 42
        x = 42;
    }
    return x;
}
```

此时最好使用一开始提出的 `std::mutex` 方案。

### 理解特殊成员函数的生成机制

我们在 [前面的小节](# 优先使用删除函数，而非 `private` 未定义函数) 中提到了一些自动生成的成员函数。如果不给出任何构造函数及赋值运算符重载，则它们六个函数都会被自动定义。但是在下列情形中，其中的一个或多个函数不会产生自动定义，可以对比前文中的产生自动定义的条件来理解：

- 如果显式给出了任意一个特殊成员函数的声明，那么这个特定的成员函数不会再自动生成（满足唯一定义定律）
- 如果显式给出了任意一个构造函数的声明，那么默认构造函数不会再自动生成
- 如果显式给出了任意一个复制操作（复制构造函数或复制赋值运算符）或析构函数的声明，那么移动构造函数与移动赋值运算符不会再自动生成
- 如果显式给出了任意一个移动操作（移动构造函数或移动赋值运算符）的声明，那么赋值构造函数和复制赋值运算符会被废除，也即定义为 `= delete`。

上面的条件看起来没有任何规律，但是背后有一套隐含的逻辑。默认构造函数对应着不提供初始值的变量定义，其可能存在未定义行为（比如内置的一些类型，如 `int`），因此一旦主动声明了构造函数，它就不需要提供默认构造函数了。复制操作和 **C++98** 时代的 **大三律（Rule of Three）** 相关，即复制构造函数、复制赋值运算符、析构函数中如果需要显式定义任一个，其它两个也应该显式定义。这是因为复制操作的默认行为是变量的浅拷贝，析构函数的默认行为是不做任何事，如果需要自己定义就默认需要进行内存管理。为了保持行为一致，这三个函数需要统一进行定义。此外，当复制操作需要显式定义时，可以认为移动操作的默认行为并不适用，因此不会自动生成。另一方面，当移动操作需要显式定义时，会认为复制操作是被禁止的（事实上，这常见于很多结构），于是将复制操作默认定义为 `= delete`。

如果依然需要特殊成员函数的自动生成定义，可以使用 `= default` 代替函数体：

```cpp
class Base {
public:
    virtual ~Base() = default;
    Base(Base const&) = default;
    Base& operator =(Base const&) = default;
    Base(Base&&) = default;
    Base& operator =(Base&&) = default;
};
```

这里假想了一个需要被重写的基类，此时析构函数需要显式声明为 `virtual` 的。根据大三律，复制操作也需要显式定义。此时为了移动操作也能被默认定义，需要显式给出。这样，我们为了兼容重写，不得不把本来能够全部自动生成的成员函数显式给出定义（`= default`）。看起来有些麻烦，但我建议即使在不需要被重写的类型中，也尽量不依赖于自动生成的特殊成员函数，否则比如突然需要显式定义某个成员函数时，其它自动生成的成员函数定义可能会消失。

最后需要提及一个特别的情况，那就是模版构造函数和模版赋值函数。它们在形式上和特殊成员函数是一样的，但是它们 *并不是* 特殊成员函数，也即其不参与我们上文中提及的自动生成机制。比如 `Foo(Foo const&)` 会被认为和 `Foo<Foo>(Foo const&)` 是两个函数，且前者有更高的优先级。这样导致模版成员函数总是会被自动生成（或手动定义）的特殊成员函数“压制”。我们会在后面的条款中介绍其解决方法。

## 智能指针

**智能指针（Smart Pointer）** 是 **C++11** 中引入的最重要库支持之一（虽然在 **C++98** 中就有 `std::auto_ptr`，但其功能非常不完全），它对 **C** 语言的令人又爱又恨的指针特性进行了革命式的改进。为了区别于智能指针，我们之后会将继承自 **C** 的指针称为 **裸指针（Raw Pointer）**。裸指针活跃于 **C** 和 **C++** 的这些年中，对于它的批评从未停歇过（我们将主要采用 **C++** 的习惯和术语）：

- 由于数组的退化机制，我们不知道裸指针变量究竟指向单个对象还是一个数组
- 我们不知道裸指针是否拥有一个对象，也即在使用之后是否需要释放其指向的内存
- 我们不知道如何合适地释放裸指针指向的内存，比如是直接使用 `delete` 运算符，或 `delete[]` 运算符，还是使用什么函数或成员函数
- 我们无法确保一个裸指针指向的内存只被释放一次，因为可能有多个指向这段内存的裸指针。这点和第二条原因是类似的
- 我们不知道一个裸指针是否是 **空悬指针（Dangling Pointer）** ，也即不指向任何有效内存空间的指针

这些原因导致了难以计数的内存问题，造成了大量的损失。与 **C** 语言不同，在 **C++** 中并不需要无时不刻与裸指针打交道且保证最高的性能，因此我们有必要设计一种安全使用指针的模式，其足够小巧，在保证在多数场景中不损耗性能的同时确保内存安全。**C++** 标准中存在四种智能指针：`std::auto_ptr`、`std::shared_ptr`、`std::unique_ptr`、`std::weak_ptr`。其中 `std::auto_ptr` 在 **C++17** 后移除了标准库，`std::unique_ptr` 是它的上位替代。

### 使用 `std::unique_ptr` 管理专属所有权的资源

绝大多数情况下，我们都可以用 `std::unique_ptr` 代替一个裸指针；它们占用的空间一样，拥有几乎一样的操作模型，换句话说，`std::unique_ptr` 不会带来显著的性能负担。它的特别之处在于，其独有一块内存空间的所有权，你无法复制一个 `std::unique_ptr`，不过可以移动它（转移所有权）：

```cpp
int main() {
 	int* ptr = new int(10);
    int* borrow = ptr;		// 现在 borrow 和 ptr 共有对一段内存的所有权，这从某种角度讲是指针问题的根源
    std::unique_ptr<int> uptr = new int(10);	// uptr 拥有对一段内存的唯一控制
    std::cout << *uptr;		// 输出 10
    auto temp = uptr;		// 错误，std::unique_ptr<> 不能被复制
    auto vptr = std::move(uptr);		// 为 uptr 赋予移动语义来移动构造 vptr，现在 vptr 拥有对这段内存的唯一控制
    std::cout << (uptr == nullptr);		// 输出 1，失去所有权的 std::unique_ptr<> 会被自动置为 nullptr
    vptr.reset();			// 通过 reset 函数来释放其指向的空间，然后变为 nullptr
    return 0;
}
```

我们也可以指定智能指针的空间释放规则，这实际上类似于析构函数的作用。它在智能指针生命周期完结时调用。其默认为 `delete` 运算符。此时 `std::unique_ptr` 占用的空间会变大。

```cpp
int main() {
    {
        auto my_deleter = [] (int* ptr) { delete ptr; std::cout << "Deleter called"; };
        // 这里需要显式给出 deleter 的类型，也即是 lambda 类型
        std::unique_ptr<int, decltype(my_deleter)> uptr(new int(10), my_deleter);
    }	// 在此 uptr 被析构，上面构造时传入的函数（lambda）会被调用
 	return 0;   
}
```

如果利用 `auto` 关键字，以及 **C++14** 的 `std::make_unique` 函数，我们可以用一致的语法定义一个 `std::unique_ptr`：

```cpp
int main() {
    auto uptr = std::make_unique<int>(10);
    //
    auto vptr = std::make_unique<int, void (*)(int*)>(
        10, [](int* ptr) { delete ptr; std::cout << "Deleter called"; });
 	return 0;   
}
```

### 使用 `std::shared_ptr` 管理具备共享所有权的资源

`std::unique_ptr` 很好用，但是所有的资源都是独占的。我们希望能够设计一种既能对多个变量开放资源权限，且同时能够在资源不需要时自动回收的模型。这就是 `std::shared_ptr`，所有共享资源的变量同时持有指向资源的指针以及指向引用计数的指针。每当有新的变量加入时，引用计数会加一；反之当变量被析构时，引用计数会减一。引用计数归零则代表这个资源不再被任何变量拥有，随后可以放心释放它。`std::shared_ptr` 在复制赋值运算时，会减少等号左侧智能指针的引用计数，然后将右侧智能指针的引用计数加一，并共享资源给左侧的指针；如果是移动赋值，右侧指针的引用计数则 *不会* 加一。这种顺滑的设计使得我们可能会认为，`std::shared_ptr` 可以完全代替裸指针的使用场景了。其实不然，看看下面列出的问题：

- `std::shared_ptr` 大小是裸指针的两倍。这是因为它不仅存储了指向资源的指针，还有一个指向引用计数的指针。这对一些对存储空间敏感的场景是致命的 debuff。
- 引用计数必须动态分配。因为没有任何一个 `std::shared_ptr` 变量需要为引用计数负责，它一定会在堆上被初始化。额外的动态分配会造成性能损失。
- 引用计数的增减必须是原子操作。在并发场景下，不同线程中对引用计数的同时访问可能会导致意料外的资源析构。然而原子操作一般会慢于非原子操作。

和 `std::unique_ptr` 类似，我们可以显式给出 `std::shared_ptr` 资源释放的规则。不过与前者微妙的不同是，`std::shared_ptr` 并不将这个释放函数的类型视为模版参数，因此指向同一资源的不同智能指针可以拥有不同的释放逻辑。下面是一个例子：

```cpp
auto my_del = [](int* p) { do_something(p); delete p; };
std::unique_ptr<int, decltype(my_del)> upi(new int, my_del);
std::shared_ptr<int> spi_1(new int, my_del);	// 使用自定义的释放器
std::shared_ptr<int> spi_2(new int);			// 使用默认的释放器
std::vector<std::shared_ptr<int>> vspi = { spi_1, spi_2 };	// spi_1 和 spi_2 类型相同因此可以放在同一个容器中
```

此外，`std::shared_ptr` 中的资源释放器 *不会* 额外增加智能指针本身的存储占用 （`sizeof(std::shared_ptr<T>)` 总是 `2 * sizeof(T*)`），但会占用堆上的内存空间。这里让我们深入讲一下。此前我们所说的指向引用计数的指针，其实并不准确。在实现中指向的是一个堆上的控制块：

```cpp
namespace std {

template<typename T>
class shared_ptr {
public:
    shared_ptr(T* ptr);
    template<typename Del>
    shared_ptr(T* ptr, Del del);
    shared_ptr(shared_ptr<T> const& other);
private:
    struct control_block {
        size_t count;
        object deleter;		// 资源释放器经过了类型擦除，具体可能有很多手段
        // 其它信息
    };
    T* m_data; 
    control_block* m_control;
};
    
}
```

当我们使用裸指针构造 `std::shared_ptr` 时，会创建一个控制块，但并不会深拷贝这个裸指针：

```cpp
template<typename T>
std::shared_ptr<T>::shared_ptr(T* ptr) : m_data(ptr) {
    m_control = new control_block{ 0, Object([](T* p) { delete p; }) };
}
```

如果直接复制一个 `std::shared_ptr`，则不会创建新的控制块：

```cpp
template<typename T>
std::shared_ptr<T>::shared_ptr(std::shared_ptr<T> const& sp) 
    : m_data(sp.m_data), m_control(sp.m_control) {}
```

从 `std::unique_ptr` 复制到 `std::shared_ptr` 时，会将指针移动过来，创建一个控制块，并置空 `std::unique_ptr`：

```cpp
template<typename T, typename Del>
std::shared_ptr<T>::shared_ptr(std::unique_ptr<T, Del>&& up)
    : m_data(std::move(up.get())) {
    m_control = new control_block{ 0, Object(std::move(up.get_deleter())) };        
}
```

由于使用裸指针初始化智能指针并不会进行深拷贝，我们建议在初始化 `std::shared_ptr` 对象时直接使用 `new` 语句，而不是传入一个指针：

```cpp
int main() {
    auto p = new int;
    std::shared_ptr<int> spi_1(p);
    std::shared_ptr<int> spi_2(p);		// 危险！p 同时被两个控制块操控，会被释放两次。
    std::shared_ptr<int> spi_3(new int);
    std::shared_ptr<int> spi_3(new int);// 直接代入 new 表达式则不会有问题。
    return 0;
}
```

和上面类似的情形是利用 `this` 指针构建 `std::shared_ptr`，同样地，`this` 会在智能指针生命周期结束时析构 `this` 指向的对象，造成内存问题。为了解决 `this` 指针构建智能指针的问题，可以使用标准库中的模版 `std::enable_shared_from_this`：

```cpp
class MyClass : public std::enable_shared_from_this<MyClass> {
public:
    void foo() {
     	v.emplace_back(std::shared_from_this());	// 相当于利用 this 构建了一个智能指针。   
    }
private:
    std::vector<std::shared_ptr<MyClass>> v;
}
```

总之，`std::shared_ptr` 利用了控制块来管理资源，其中可能包含复杂的实现（如虚函数，这会带来一定成本）。不过，在不显示指定资源释放器时，这个成本并不明显。建议在设计时仔细考虑是否需要共享资源，如果可能并不需要，`std::unique_ptr` 能够提供更接近裸指针的效率，且可以随时转换为 `std::shared_ptr`。不过一旦定义了 `std::shared_ptr` 对象，就不能再变为 `std::unique_ptr` 了。

最后，在 **C++17** 之前，`std::shared_ptr<T[]>` 并未被支持，但从 `std::unique_ptr<T[]>` 到 `std::shared_ptr<T>` 的转换并未被编译器认知为错误，在一些情况下会发生错误，比如将 `std::shared_ptr<Derived>` 转换为 `std::shared_ptr<Base>` 在资源实际上是一个数组时（`Derived[]`）是危险的行为。**C++17** 之后提供了对 `std::shared_ptr<T[]>` 的支持，同时也禁止了 `std::unique_ptr<T[]>` 到 `std::shared_ptr<T>` 类型的转换。此处的建议是在 **C++17** 前避免使用 `std::shared_ptr` 管理数组。

### 对于类似 `std::shared_ptr` 但有可能空悬的指针使用 `std::weak_ptr`



### 优先选用 `std::make_unique` 和  `std::make_shared`，而非直接使用 `new`

### 使用 `Pimpl` 习惯用法时，将特殊成员函数的定义放到实现文件中

## 右值引用、移动语义和完美转发

### 理解 `std::move` 和 `std::forward`

### 区分转发引用和右值引用

### 针对右值引用实施 `std::move`，针对转发引用实施 `std::forward`

### 避免依赖转发引用类型进行重载

### 熟悉依赖转发引用类型进行重载的替代方案

### 理解引用折叠

### 假定移动操作不存在、成本高、未使用

### 熟悉完美转发的失败情形

## `lambda` 表达式

### 避免默认捕获形式

### 使用初始化捕获将对象引入闭包

### 对 `auto&&` 类型的形参使用 `decltype`、以 `std::forward`

### 优先选用 `lambda` 表达式，而非 `std::bind`

## 并发 API

### 优先选用基于任务而非基于线程的程序设计

### 如果异步是必要的，则指定 `std::launch::async`

### 使 `std::thread` 类型对象在所有路径均不可连结

### 对变化多端的线程句柄析构函数行为保持关注

### 考虑针对一次性事件通信使用以 `void` 为模版类型实参的期值

### 对并发使用 `std::atomic` 对特种内存使用 `volatile`

## 微调

### 针对可复制的形参，在移动成本低且一定会被复制的前提下，考虑将其按值传递

### 考虑置入而非插入

