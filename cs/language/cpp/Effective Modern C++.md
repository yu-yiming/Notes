# Effective Modern C++ 笔记

[TOC]

这篇是对 *Effective Modern C++ (Scott Meyers)* 的阅读笔记，其对 **C++11** 之后 **C++** 语言的主要编码思路做出了建设性的建议。我将严格按照原书的章节分化，整理罗列出个人在意的知识点并给出自己的心得。其中很多语言组织方式和代码改编自原书。

## 类别推导

### 1. 模版类型推导

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

### 2. `auto` 类型推导

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

### 3. `decltype`

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

### 4. 如何查看类型推导结果

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

### 5. 优先使用 `auto` 而非显式类别声明

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

### 6. 当 `auto` 推导的类型不符合要求时，使用显式类型声明

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

### 7. 在创建对象时区分 `()` 和 `{}`

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

### 8. 优先选用 `nullptr` 而非 `0` 或 `NULL`

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

### 9. 优先使用 `using` 而非 `typedef`

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

### 10. 优先选用限定作用域的枚举类型

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

### 11. 优先使用删除函数，而非 `private` 未定义函数

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

### 12. 为重写的函数添加 `override` 声明

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

### 13. 优先使用 `const_iterator`，而非 `iterator`

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

### 14. 只要函数不会产生异常，就为其加上 `noexcept` 声明

**C++** 关于异常的处理在 **C++11** 发生了翻天覆地的变化。此前使用的是 **动态异常规格（Dynamic Exception Specification）**，即在函数参数列表后给出 `throw()` 或 `throw(...)`，括号中包含所有可能被抛出的类型；如果没有给出 `throw`，则默认可以抛出所有类型。一个函数不能抛出其声明处没有给出的类型：

```cpp
void foo(int i) throw(int, char*) {	// 老实说，这是一个挺恶心的设计
    if (i > 10) {
        throw 10;
    }
    throw 1.0;		// 错误，在声明中没有允许抛出浮点类型
}
```

动态异常规格还可以出现在函数参数中，因此它非常像是函数签名的一部分（但实际上并不是，它不能出现在 `typedef` 语句中）。但是这个语法会让人非常疲于使用，一旦抛出的类型需要更新，可能会要求客户代码也要进行更改。我们发现，其实抛出什么类型的异常并不重要，重要的是是否会抛出异常。这就引出了 `noexcept` 的概念。

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

### 15. 只要有可能使用 `constexpr` 就使用它

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

### 16. 保证 `const` 成员函数的线程安全性

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

### 17. 理解特殊成员函数的生成机制

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

- 由于数组的退化机制，我们不知道裸指针变量究竟指向单个对象还是一个数组。
- 我们不知道裸指针是否拥有一个对象，也即在使用之后是否需要释放其指向的内存。
- 我们不知道如何合适地释放裸指针指向的内存，比如是直接使用 `delete` 运算符，或 `delete[]` 运算符，还是使用什么函数或成员函数。
- 我们无法确保一个裸指针指向的内存只被释放一次，因为可能有多个指向这段内存的裸指针。这点和第二条原因是类似的。
- 我们不知道一个裸指针是否是 **空悬指针（Dangling Pointer）** ，也即不指向任何有效内存空间的指针。

这些原因导致了难以计数的内存问题，造成了大量的损失。与 **C** 语言不同，在 **C++** 中并不需要无时不刻与裸指针打交道且保证最高的性能，因此我们有必要设计一种安全使用指针的模式，其足够小巧，在保证在多数场景中不损耗性能的同时确保内存安全。**C++** 标准中存在四种智能指针：`std::auto_ptr`、`std::shared_ptr`、`std::unique_ptr`、`std::weak_ptr`。其中 `std::auto_ptr` 在 **C++17** 后移除了标准库，`std::unique_ptr` 是它的上位替代。

### 18. 使用 `std::unique_ptr` 管理专属所有权的资源

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

### 19. 使用 `std::shared_ptr` 管理具备共享所有权的资源

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

### 20. 对于类似 `std::shared_ptr` 但有可能空悬的指针使用 `std::weak_ptr`



### 21. 优先选用 `std::make_unique` 和  `std::make_shared`，而非直接使用 `new`

我们推荐使用 **C++11** 引入的 `std::make_shared` 和 **C++14** 引入的 `std::make_unique` 来构建，这首先是因为他们更简洁：

```cpp
auto sp_1(std::make_unique<int>());
std::unique_ptr<int> sp_2(new int);
```

上面第一种情形我们采用了 `auto` 的声明形式，其中只显示写出了 *一次* 存储的类型。如果将 `int` 换为其它啰嗦的类型名称，其区别会更加显著，且在代码进行修改时更加麻烦。

另一个重要的原因是，一些情况下使用 `new` 语句构建智能指针会造成内存泄露。看下面这个例子：

```cpp
void foo(std::shared_ptr<int> sp, int val);
int bar();
int main() {
    foo(new int, bar());
    return 0;
}
```

**C++** 中，函数调用时参数的求值顺序是不确定的（参考 [cppreference](https://en.cppreference.com/w/cpp/language/eval_order)），上面对 `foo` 的调用存在下面三个步骤：

- 在堆上分配 `int` 类型的内存，得到一个堆指针。
- 将堆指针作为参数构造 `std::shared_ptr`。
- 调用并得到 `bar()` 的返回值。

事实上，第三条可能在第一和第二条之间执行，如果它抛出了异常，第一条产生的堆内存将无法得到释放，因而内存泄露。如果我们坚持使用 `std::make_shared`，就能保证智能指针的构建不会被其它求值的异常干扰；即使 `bar()` 在之后求值且出现异常，我们也可以借用智能指针的析构函数释放内存。

另外，由于 `std::shared_ptr` 内部会设置 **单块（Single Chunk）**来同时存储对象和控制块，因此只 `std::make_shared` 需要一次内存分配，而 `new` 语句需要在分配内存后再次为控制块分配内存来构建智能指针，这在性能上就有一定的损失。

不过（意料之中地），`new` 能够做到这两个函数办不到的事情。正如我们前面介绍的，智能指针的构造函数允许自定义释放规则，但是 `std::make_shared` 就不行。此外，如果需要用初始化参数列表（即大括号）来创建元素，我们不能直接这样写：

```cpp
auto sp = std::make_shared<std::vector<int>>({ 1, 2, 3 });
```

这是因为在 **C++** 中，形如 `{1, 2, 3}` 的结构 *没有* 类型（或者可以说是魔法类型，参考 lambda），尽管在 `auto` 类型推到中被它赋值的对象会被认为是 `std::initializer_list`，但它独自出现的时候我们需要注意。`std::make_shared` 会将所有参数完美转发到指定类型的构造器中，因此 `{1, 2, 3}` 也“完美”地准备进入构造器，然而它的类型无法被识别，便发生了错误。一个简单的解决方法是在调用 `std::make_shared` 之前利用一次 `auto` 类型推导：

```cpp
auto list = { 1, 2, 3 };
auto sp = std::make_shared<std::vector<int>>(list);
```

或者是在调用时用 `std::initializer_list` 的构造函数（实际上是魔法）将 `{1, 2, 3}` 转化：

```cpp
auto sp = std::make_shared<std::vector<int>>(std::initializer_list<int>{1, 2, 3});
```

当然，此时相对简洁的写法是使用我们之前不提倡的 `new` 语句：

```cpp
std::shared_ptr<std::vector<int>> sp(new std::vector<int>{1, 2, 3});
```

还有两个比较罕见的情形也不建议使用 `std::make_shared`：

- 资源类重载了 `operator new` 和 `operator delete` 时。我们重载这些运算符通常是为了精准控制内存块的大小。由于 `std::make_shared` 中同时分配了控制块，因此不能如愿。
- 资源类分配非常大的一块内存时，使用 `std::make_shared` 可能会导致内存无法及时释放。原因依然是该函数的内存申请策略。由于资源的内存和控制块内存是一起申请的，因此在有 `std::weak_ptr` 指向该内存时，即使引用计数已经清零，控制块中的弱计数依然没有清零，此时控制块内存不能释放，资源内存也就一并不能释放。如果使用 `new` 语句构造智能指针，引用计数清零的时候内存块就会被释放，控制块则在最后一个 `std::weak_ptr` 析构后被释放，内存使用更加有效率。

上面这两种情况皆是因为 `std::make_shared` 对内存分配上的优化导致的，所以对 `std::make_unique` 并不适用。

最后，让我们对最常见的不得不使用 `new` 语句构建智能指针的情况给出建议，即需要提供自定义内存释放规则时。我们已经提到，如果在函数调用时直接用 `new` 语句隐式转换为智能指针，会因为参数求值顺序的不确定而导致内存泄露，因此我们推荐将智能指针的构造分离出来：

```cpp
void foo(std::shared_ptr<int> sp, int val);
void del(int* ptr);
int bar();
int main() {
    std::shared_ptr<int> sp(new int, del);
    foo(sp, bar());
    return 0;
}
```

不过由于这里函数调用处传了一个左值，会进行复制操作，其中涉及的引用计数增加是一个原子操作，比较费时。如果想要优化这一点，可以将其转换为右值引用：

```cpp
foo(std::move(sp), bar());
```

这是这种情况的最优解。

### 22. 使用 `Pimpl` 习惯用法时，将特殊成员函数的定义放到实现文件中

我们先简单介绍一下 `Pimpl` 的含义。`Pimpl` 即 **指向实现的指针（Pointer to Implementation）**，也即在成员中使用指针而非普通类型。可以参考下面的例子：

```cpp
class MyClass {
public:
    MyClass();
    // 省略其它声明
private:
    std::string name;
    std::vector<double> data;
    SomeClass sc;
};
```

上面的代码为了编译，我们需要引入 `<string>`、`<vector>` 以及定义了 `SomeClass` 的头文件，如果它们发生任何修改，整个文件都需要重新编译，这是比较费时的。因此 `Pimpl` 方法使用非完整类型作为成员类型：

```cpp
class MyClass {
public:
    MyClass();
    ~MyClass();
    // 省略其它声明
private:
    struct Impl;
    Impl* p_impl;
};
```

这里的 `Impl*` 就是一个非完整类型，我们不能对它进行解引用操作（包括使用任何成员或成员函数），具体的操作均放到源文件中：

```cpp
#include "myclass.h"
#include "someclass.h"
#include <string>
#include <vector>
struct MyClass::Impl {
    std::string name;
    std::vector<double> data;
    SomeClass sc;
};
MyClass::MyClass() : p_impl(new Impl) {}
MyClass::~MyClass() {
    delete p_impl;
}
```

以上就是有点意思的程序设计思路，它将对其它头文件的以来从头文件转移到了源文件。不过，这里的所有语法都是 **C++98** 的，裸指针、`new` 与 `delete` 运算符、析构函数…… 显然我们现在有能力改进它，这里正是 `std::unique_ptr` 可以大显身手的地方：

```cpp
// myclass.h
struct MyClass {
public:
    MyClass();
    // 省略其它声明
private:
    std::unique_ptr<Impl> p_impl;
};
```

```cpp
// myclass.cpp
// 省略头文件
struct MyClass::Impl {
    std::string name;
    std::vector<double> data;
    SomeClass sc;
};
MyClass::MyClass() : p_impl(std::make_unique<Impl>()) {}
```

很遗憾，上面的代码不能让客户代码通过编译。它的原因比较难以想到。由于我们没有给出析构函数，编译器会为我们自动生成一个，其中 `std::unique_ptr` 对象会在析构时使用 `delete` 运算符；在此之前，通常会用 `static_assert` 语句检查指针类型是否为完整类型，此时就产生了编译期错误。因此解决方案是给出一个空的析构器定义，或在头文件中声明，并 *在源文件中* 使用 `= default` 定义以阻止编译器自动生成。

类似地，编译器自动生成的移动构造和移动赋值函数也需要指针类型为完整类型，因此为了防止编译期错误，我们依然可以在头文件中写下声明，并在源文件中用 `= default` 定义它们。下面是一块详细的代码：

```cpp
// myclass.h
class MyClass {
public:
    MyClass();
    MyClass(MyClass&& other);
    MyClass(MyClass const& other);
    ~MyClass();
    
    MyClass& operator =(MyClass const& rhs);
    MyClass& operator =(MyClass&& rhs);
private:
    struct Impl;
    std::unique_ptr<Impl> p_impl;
};
```

```cpp
// myclass.cpp
// 省略头文件
struct MyClass::Impl {
    std::string name;
    std::vector<double> data;
    SomeClass sc;
};
MyClass::MyClass() : p_impl(std::make_unique<Impl>()) {}
MyClass::MyClass(MyClass&& other) = default;
MyClass::MyClass(MyClass const& other) : p_impl(std::make_unique<Impl>(*other.p_impl)) {}
MyClass::~MyClass() = default;

MyClass& MyClass::operator =(MyClass&& rhs) = default;
MyClass& MyClass::operator =(MyClass const& rhs) {
    *p_impl = *rhs.impl;
    return *this;
}
```

有意思的是，如果使用 `std::shared_ptr`，就完全无需在意前面这个问题。没有编写析构函数时，编译器会自动生成移动操作。这其实也反映了两种智能指针的设计区别：`std::unique_ptr` 更加小巧高效，将删除器类型作为智能指针类型中的一部分，因此如果要利用编译器自动生成的特殊成员函数，就要求指向的类型是完整类型；`std::shared_ptr` 相对笨重，它不将删除器类型作为智能指针类型的一部分，自动生成的特殊成员函数中不要求指向类型是完整类型。

## 右值引用、移动语义和完美转发

**右值引用（Rvalue Reference）** 是 **C++** 对左值右值体系的重要补充。过去，我们有能够绑定左值的左值引用、能够绑定左值或右值的左值常引用（不过不能通过这个引用修改值），但居然没有一个绑定右值且对其修改的引用。因此 **C++11** 引入了右值引用，它只能绑定一个右值，除此之外它和左值引用非常类似（包括它是一个左值）。我们使用 `&&` 限定符来声明一个右值引用。

**移动语义（Move Semantics）** 是 **C++11** 引入的一个重要的语言特性。移动操作是对对象的一种轻量的构造模式，以替换代价高昂的复制操作。移动语义（对比值语义）使得对象能够通过移动方式进行传递（对比复制）。一些特殊的类型甚至禁止了复制操作，而只允许移动，如 `std::unique_ptr`、`std::future`、`std::thread` 等。

**完美转发（Perfect Forwarding）** 是对模板编程的一个补充特性，它会将任意类型的参数原封不动地传递到其它函数中（即不会发生退化、引用消失、修饰符消失等）。

上面的后两点和右值引用有着千丝万缕的关系，我们很快就会认识到这一点。

### 23. 理解 `std::move` 和 `std::forward`

从字面理解，库函数 `std::move` 用来移动对象，而 `std::forward` 用来转发对象。可惜事实上它们只是一个类型转换函数（某种程度上它们只是包装了 `static_cast`)。究竟为什么一个类型转换可以让它们产生特殊的效果呢，我们马上揭晓。

首先我们看看 `std::move` 的定义：

```cpp
template<typename T>
decltype(auto) move(T&& param) {
    return static_cast<std::remove_reference_t<T>&&>(param);
}
```

它将传入的参数强制类型转换为右值引用。根据引用折叠（见 [后文](# 理解引用折叠)），为防止左值引用依然是左值引用，我们在转换之前先移除了参数类型的引用。

需要注意的是，并非我们使用了 `std::move` 就能保证变量被移动传递（就类似于左值引用也不一定被引用传递）。考察下面的情形：

```cpp
int main() {
    std::string const str_1 = "abc";
    std::string str_2 = std::move(str_1);
    return 0;
}
```

我们知道 `std::string` 实现了移动构造函数，因此等式右边是一个右值引用时，理应移动构造左侧的变量…… 可惜这里情况有些微的差别。从之前给出的定义我们知道，`std::move` 并没有去除参数类型中的 `const` 修饰符，因此上面例子中，`str_2` 定义的等号右侧是 `std::string const&&` 类型的。和 `std::string const&` 类似，这是一个对常量的引用，由于移动操作默认是对非常量的引用，此处没法进行移动，因此依然调用了复制构造函数。

至于 `std::forward` 的用处，可以参考下面的例子：

```cpp
void foo(std::string const& lval);
void foo(std::string&& rval);
template<typename T>
void bar(T&& param) {
    foo(param);
}
```

上面的模板函数 `bar` 可以以合适的方式接收任意类型的参数（值传递或引用传递），即转发引用，我们很快就会介绍。不过一旦传入函数中后，`param` 始终是一个左值（回忆一下，右值引用也是一个左值），因此调用 `foo` 的时候就没法根据引用类型进入正确的函数。这时 `std::forward` 就大显身手了。它仅当传入类型此前是通过右值初始化的时候，才会将其转换为右值引用。判断参数是通过左值或右值初始化的奥秘在于模板参数 `T`，我们在[后续](# 理解引用折叠)会进行说明。

### 24. 区分转发引用和右值引用

**转发引用（Forwarding Reference）** 用来绑定任意类型的对象（包括值、左值引用和右值引用，无论其拥有什么样的修饰符），和右值引用使用完全一致的语法，但是其行为颇为不同。一般情况下，需要类型推导的地方出现的是转发引用，否则就是我们已经熟悉的右值引用。看下面的例子：

```cpp
char foo(int&& rval);				// 右值引用
auto&& var = foo(10);				// 转发引用
template<typename T>
void bar(T&& rval);					// 右值引用
template<typename T>
void baz(T&& param);				// 转发引用
```

转发引用同样也是引用，所以我们需要在其定义时初始化；事实上正是初始化决定了转发引用实际的引用类型。

```cpp
template<typename T>
void foo(T&& param);
int main() {
    int i = 10;
    foo(i);				// foo 中 param 是一个 int&
    foo(std::move(i));	// foo 中 param 是一个 int&&
    return 0;
}
```

前面我们说，需要类型推导的地方出现的通常是转发引用，这确实存在反例：

```cpp
template<typename T>
void foo(std::vector<T>&& v);		// 此处传递的是右值引用
template<typename T>
void bar(T const&& param);			// 此处传递的是右值常引用
```

同时，模板类中定义的成员函数，对于其已经实例化的模板类型，只会产生右值引用而非转发引用。这个道理是一样的，即只有类型推导出现的地方才可能有转发引用：

```cpp
template<typename T>
class MyClass {
    void foo(T&& rval);			// 右值引用
    template<typename... Args>
    void bar(Args... args);		// 转发引用
};
```

到现在为止我们都没有解释为什么转发引用有这样的功效。这是拜引用折叠所赐，我们会在[后文](# 理解引用折叠)详细说明。

### 25. 针对右值引用实施 `std::move`，针对转发引用实施 `std::forward`

此前我们已经介绍了 `std::move` 和 `std::forward` 的相似性，不过千万不要因此将它们随意混用。比如，一个转发引用被使用 `std::move`，而它恰巧是通过左值引用传入函数时，函数调用处的这个左值就会失效：

```cpp
class MyClass {
public:
    template<typename T>
    void set(T&& val) {
        val = std::move(val);
    }
private:
    std::string val;
};
int main() {
    std::string str = "abc";
    MyClass mc{};
    mc.set(str);		// 这步之后 str 就是一个不确定的值了
}
```

上面将成员函数 `set` 定义为模板或许有些争议，不过它确实比提供两个重载版本（常引用以及右值引用）形式更加高效。这是因为考虑调用 `mc.set("abc")` 的情形，此时字面量 `"abc"` 需要构造成一个 `std::string` 对象，然后再在成员函数中移动构造 `val`，随后析构临时构造的 `std::string` 对象。这显然要比上面的单独一次构造要昂贵。当然，也可以为 `char const*` 类型参数进行额外的重载，但考虑有多个需要重载参数的情形，这会显著增大源代码体积，违背了模板和转发引用被设计出来的初衷。

为了不让转发引用的变量作为右值引用被移动，我们应该在其最后一次出现时才使用 `std::forward`，这样能够保证其前面在参数传递时始终作为左值引用，直到最后一次才变成实际上的引用形式。如果过早地使用 `std::forward`，对于实际上为右值引用的转发引用就像无条件使用 `std::move` 一样危险。

如果是返回右值引用或转发引用的情形，此时也应该确保使用 `std::move` 和 `std::forward`。看看下面的例子：

```cpp
Matrix operator +(Matrix&& lhs, Matrix const& rhs) {
    lhs += rhs;
    return std::move(lhs);
}
```

（这大概是为数不多的返回引用的合理情形）此时使用 `std::move` 会高效地将移动进函数的 `Matrix` 对象再次移动出去。即使 `Matrix` 不巧地没有实现移动构造，编译器也会通过复制构造函数构建返回值。转发引用的情况则完全类似，比起无条件复制，我们当然喜欢合适情况下更加高效的移动构造。

不过上面的例子可能会带来一个错觉，那就是对于任何返回类型为值的函数，都应该通过 `std::move` （或 `std::forward`）来返回局部变量，如下所示：

```cpp
MyClass get_object() {
    MyClass mc{};
    // 省略函数内容
    return std::move(mc):
}
```

这样写似乎显然比 `return mc` 来得高效，因为它避免了复制构造…… 真的如此么？这段代码 *的确* 利用了移动语义高效地返回了局部变量，不过早在 **C++98** 就纳入标准的 **返回值优化（Return Value Optimization, RVO）** 比它更加高明。对于函数参数以外的和返回类型相同的局部变量，编译器可以将它们在一开始就构建在为返回值分配的内存上，这样返回时不需要任何的构造操作。相比之下，如果使用了 `std::move`，返回的是一个引用而非对象，因此不能触发 **RVO**。此外，即使在编译器不进行 **RVO** 的时候，标准也要求其将返回语句中的对象看作右值，因此等同于使用了 `std::move`。综上，理想的做法是直接以值返回局部变量，这样能够在多数情况下享有优化，其余情形也与使用 `std::move` 等同。

### 26. 避免依赖转发引用类型进行重载

我们之前已经认识到，使用转发引用可以完美地处理各种情形的参数（值、左值引用、右值引用），它显然是一个在一些情形替代重载的手段：

```cpp
static std::multiset<std::string> mset;
template<typename T>
void foo(T&& str) {
    mset.emplace(std::forward<T>(str));
}

int main() {
    std::string str = "abc";
    foo(str);					// 作为左值引用传递，在 foo 中通过复制构造函数构建 mset 中的新元素
    foo(std::string("abc"));	// 作为右值引用传递，在 foo 中通过移动构造函数构建 mset 中的新元素
    foo(std::move(str));		// 同上
    foo("abc");					// 同上
    return 0;
}
```

如果是原来的方式，对 `std::string const&` 和 `std::string&&` 两种情况进行重载，就非常不高效：

```cpp
static std::multiset<std::string> mset;
void foo(std::string const& str) {
    mset.emplace(str);			// 任何情况下都会进行复制构造
}
void foo(std::string&& str) {
    mset.emplace(str);			// 任何情况下都会进行复制构造！这简直是一个大骗局
}
int main() {
    std::string str = "abc";
    foo(str);					// 作为左值引用传递，在 foo 中通过复制构造函数构建 mset 中的新元素
    foo(std::string("abc"));	// 作为右值引用传递，在 foo 中通过复制构造函数构建 mset 中的新元素
    foo(std::move(str));		// 同上
    foo("abc");					// 首先通过构造函数构建临时的 std::string
    							// 通过右值引用传递，然后在 foo 中通过复制构造函数构建 mset 中的新元素
    return 0;
}
```

因此，我们应该积极使用转发引用以代替原来笨拙的重载。不过，考虑一个需要用另一种类型重载 `foo` 的情形：

```cpp
static std::multiset<std::string> mset;
std::string get_string(int idx);
template<typename T>
void foo(T&& str) {
    mset.emplace(std::foward<T>(str));
}
void foo(int idx) {
    mset.emplace(get_string(idx));		// 通过 get_string 函数得到一个字符串
}
```

这里在调用 `foo` 时，就会对这两种情况进行对比，以判断 `T&&` 还是 `int` 更符合参数类型。一些情况下结果可能会出人意料：

```cpp
int main() {
    foo(42);				// 调用 foo(int)，和预期一致。这是因为非模版函数在函数调用时有更高的优先级
    foo(42l);				// 调用了 foo(T&&)，这是一个精准匹配，而 foo(int) 需要对参数进行一次类型转换，优先度较低
    						// 这里实际上会产生编译期错误，因为调用 mset.emplace 时参数 long 无法顺利进行
    return 0;
}
```

因此当使用转发引用作为参数的函数时，要避免同时使用重载，因为转发引用会精准匹配任何类型，在一些情况下会“抢走”不属于它们的函数调用，有时这并不符合直觉。

如果对成员构造函数使用转发引用（甚至进行重载），会出现更加严重的问题。考虑下面这种情形：

```cpp
class MyClass {
public:
    template<typename T>
    explicit MyClass(T&& str) : m_val(std::fowrad<T>(str)) {}
    explicit MyClass(int idx) : m_val(get_string(idx)) {}
private:
    std::string m_val;
};
```

看起来似乎和普通函数的情形相同，但是不要忘记 **C++** 的特殊构造函数！`MyClass` 中没有给出任何复制或移动构造函数，因此编译器会自动生成 `MyClass(MyClass const&)` 和 `MyClass(MyClass&&)`。现在考虑下面的代码：

```cpp
int main() {
    MyClass mc_1("abc");
    auto mc_2(mc_1);		// 错误！这里调用了使用转发引用的构造函数，而非默认生成的 `MyClass(MyClass const&)`
    						// 此时没法将 MyClass 转换为 std::string 类型
    return 0;
}
```

这里出现预料以外情形的原因是，在模版实例化时，转发引用的构造函数可以变成 `explicit MyClass(MyClass&)` 的形式，比起复制构造函数的 `MyClass(MyClass const&)`，它不需要对输入类型添加 `const` 修饰符，因此是更佳的匹配。为了验证我们的说法，可以尝试对 `mc_1` 添加 `const` 修饰符，此时就能确保调用的是复制构造函数了（因为尽管转发引用的版本实例化后拥有相同等级的匹配度，但编译器会优先采用非模版成员函数）。

更加烦人的情况是在派生类中使用基类的构造函数，此时 *一定* 会调用转发引用的版本（甚至没有解决手段）：

```cpp
class MyDerivedClass : public MyClass {
public:
    MyDerivedClass(MyDerivedClass const& other) : MyClass(other) {}
    MyDerivedClass(MyDerivedClass&& other) : MyClass(other) {}
};
```

这里出现问题的原因是 `other` 的类型是派生类引用，和基类的复制/移动构造函数的匹配程度不及转发引用的构造函数。这让我们立刻处在一个两难的境地：如果不使用转发引用，那么重载时就会产生大量的重复代码；如果使用，该如何处理特殊情况（需要复制和移动）的函数调用呢？紧接着这个条框将进行详细说明。

### 27. 熟悉依赖转发引用类型进行重载的替代方案

对于上一节提到的，重载 `foo` 造成的问题，最简单的方法就是 *不要重载*。在很多流行的编程语言中根本不支持重载（比如 **Python** 和 **Rust** 等），它们用不同名称的函数以及范型机制解决了这个问题。对于 **C++** 来说也是一样的。执行完全类似的行为时（传递值、左值引用、右值引用），我们可以利用转发引用；其它的情形值得使用别的函数名称。比如需要 `int` 参数的 `foo` 函数可以命名为 `foo_by_index`。不过，构造函数的问题没法通过这个解决：它们的名字必须是相同的。

我们也可以完全忘记 **C++11** 的更新，使用 `T const&` 作为参数类型（这里的 `T` 是一个具体的类型）。不过这也就抛弃了转发引用带来的高效率。

另一种古老而容易被人忽略的方法是使用值传递（即使用 `T` 作为参数类型，`T` 是一个具体的类型），这样也能将不同重载版本“平等”地区分开来。或许这有悖 **C++** 标准出现以来流行的传递引用的高效形式，不过当我们确定对象在函数中 *一定* 会被复制时，一个合理的方式是将其在传入函数时就进行复制。我们会在后面的条款中详细分析这种策略的效率问题。

上面的三种方式某种程度上都是比较“简单”的解决方式，要么抛弃重载，要么抛弃转发引用。下面我们将介绍不抛弃其中任何一个的解决方案：

一种方式是通过判断将普通情况和传入特殊类型的情况进行区分（有点类似于手动重载），看看下面对 `foo` 函数的改良：

```cpp
template<typename T>
void foo_impl(T&& str, std::false_type) {
    // 此时是调用了字符串的版本
}
void fool_impl(int i, std::true_type) {
    // 此时是调用了整数的版本
}

template<typename T>
void foo(T&& val) {
    foo_impl(std::forward<T>(val), std::is_integral<std::remove_reference_t<T>>{});
}
```

上面的类型特质 `std::integral` 会通过整数类型（无论是 `int`、`short` 还是`long` 等）实例化为 `std::true_type`，其余情况则会被实例化为 `std::false_type`，这样我们就可以通过重载将不同情况完全区分开了。需要注意这里的 `std::remove_reference` 是必须的，因为进行完美转发时，左值会被推导为 `T&`（左值引用），在这里进行类型特质判断时会产生 `std::false_type`。

这种方法对转发引用的构造函数依然不起效果，因为编译器会生成特种函数，而我们没法保证编译器一定或一定不调用转发引用的构造函数或普通的复制/移动构造函数，因此使用额外传标签类型重载的方式并不好写。此时可以使用模版元编程的一大杀器，`std::enable_if` 和 **C++** 的 **代替失败不是错误（Substitution Failure Is Not An Error, SFINAE）** 机制来让转发引用的构造函数在一些情况下不能被实例化（从而选择复制/移动构造函数）：

```cpp
class MyClass {
public:
    template<typename T, typename = std::enable_if_t<!std::is_same_v<MyClass, std::decay_t<T>>>>>
    explicit MyClass(T&& val);
    // 省略类实现
};
```

这里的一连串令人望而却步的类型特质就是 **SFINAE** 的使用示例，如果不熟悉模版元编程的朋友，这里我可以简单介绍一下。`std::enable_if<expr>` 中的 `expr` 是一个编译器常量，当它在模版实例化时得到 `false` 时，当前的模版会实例化失败；由于 **SFINAE** 机制，此处编译器并不会报错，而是接着寻找其它能够完成匹配的函数。这样，我们就筛选出不允许使用转发引用构造的参数类型，并让它们通过其它方式（复制/移动）构造。上面除了用到 `std::is_same` 来判断两个类型是否完全相等，我们还用到 `std::decay` 来去除模版类型 `T` 所带的引用限定符和 `const`/`volatile` 修饰符（实际上它还将会数组退化成指针，但和我们此处的需求无关）。

这还没完，因为我们还得让派生类调用基类构造函数时，能够调用复制/移动构造函数而非转发引用的版本。此时可以利用类型特质 `std::is_base_of` 来判断。完整的代码如下：

```cpp
class MyClass {
public:
    template<
    	typename T,
        typename = std::enable_if_t<
            !std::is_base_of_v<MyClass, std::decay_t<T>> &&
            !std::is_integral_v<std::remove_reference_t<T>>>>
    explicit MyClass(T&& str) : m_val(std::forward<T>(str)) {}
    explicit MyClass(int idx) : m_val(get_string(idx)) {}
private:
    std::string m_val;
};
```

太酷了！这应该是最完美的版本了，无论是输入任何方式传递的字符串和整型，都能够完美地调用构造函数。同时派生类也可以毫无负担地调用基类的构造函数。呃，其实还有一个优化方向。上面的 `std::enable_if` 只是过滤了派生类和整型；我们没有拦截别的类型。这会导致其它的输入会产生编译期错误（而且很可能非常丑陋），比如你可以试试将 `char16_t []` 类型带进去。此时可以在构造函数定义中使用 `static_cast` 产生更好看的报错信息：

```cpp
static_assert(std::is_constructible_v<std::string, T>, "Parameter cannot construct a std::string");
```

不过前面的混沌报错依然会出现…… 然后跟着我们自定义的静态断言信息，只能说我们尽力了。

### 28. 理解引用折叠

本节我们将探索 **C++** 的 **引用折叠（Reference Collapse）**。它是指模版类型推导时，将多重引用修饰符折叠为单独的引用修饰符的行为。我们知道 **C++** 中引用并不是一个对象，因此不能声明引用的引用，比如 `auto& & rx = x` 是无效的。不过，转发引用出现时，参数的类型就好像是一个引用的引用：

```cpp
template<typename T>
void foo(T&& val);
template<typename T>
void bar(T const& val);
template<typename T>
void baz(T& val);

int main() {
    int i{};
    
    foo(42);		// T 被推导为 int，所以 val 是 int&& 类型
    bar(42);		// T 被推导为 int，所以 val 是 int const& 类型
    
    foo(i);			// T 被推导为 int&，所以 val 是 int& && 类型？实际上是 int& 类型
    bar(i);			// T 被推导为 int，所以 val 是 int const& 类型
    baz(i);			// T 被推导为 int，所以 val 是 int& 类型
    
    return 0;
}
```

显然 `foo(i)` 的情况下不应该是 `int& &&` 类型，而是 `int&` 类型。如果让我们暂时忽视 `T` 经过类型推导的结果，可以发现当函数型参是一个引用时，根据其是左值还是右值引用以及实参是左值还是右值有四种情况：

- `T&` 以及左值实参，此时 `T&` 是一个左值引用。
- `T const&` 以及右值实参，此时 `T const&` 是一个左值常引用。
- `T&&` 以及左值实参，此时 `T&&` 是一个左值引用。
- `T&&` 以及右值实参，此时 `T&&` 是一个右值引用。

可以看到只有型参和实参都是右值的情况下，函数参数才会变成右值引用。这也是 `std::forward` 生效的情形。让我们从它的简单版本的定义探究每种情况下究竟发生了什么：

```cpp
template<typename T>
T&& forward(std::remove_reference_t<T>& param) {
    return static_cast<T&&>(param);
}
```

对于传入一个左值的情形，`T` 会被推导成左值引用，它会导致下面的实例化结果（以 `std::string` 为例）：

```cpp
// 这不是合法的 C++ 代码，仅做演示
std::string& && forward(std::remove_reference_t<std::string&>& param) {
 	return static_cast<std::string& &&>(param);
}
```

经过引用折叠，我们得到的是：

```cpp
std::string& forward(std::string& param) {
    return static_cast<std::string&>(param);		// cast 了个寂寞
}
```

如果传入的是一个右值，`T` 便是一个光秃秃的类型，实例化结果如下：

```cpp
std::string&& forward(std::remove_reference_t<std::string>& param) {
    return static_cast<std::string&&>(param);   
}
```

这个甚至不需要引用折叠就可以得到：

```cpp
std::string&& forward(std::string& param) {
    return static_cast<std::string&&>(param);		// 和 std::move 完全一致，将结果作为右值引用返回
}
```

我们此前提到过，有类型推导存在时的右值引用大多是转发引用。因此，使用 `auto&&` 的情景都是转发引用：

```cpp
int main() {
    auto i = 10;
    
    auto&& x = i;		// 等同于 int& && x = i; 即 int& x = i;
    auto&& y = 42;		// 等同于 int&& y = 42;
    auto&& z = rand();	// 等同于 int&& z = rand();
    return 0;
}
```

除此之外，在声明别名和使用 `decltype` 判断类型时，也会发生引用折叠：

```cpp
template<typename T>
struct MyStruct {
    using RrefT = T&&;
    using DeclT = decltype(T&&);
};
int main() {
    MyStruct<int&> ms_1;	// RrefT 和 DeclT 都是 int& 类型
    MyStruct<int&&> ms_2;	// RrefT 和 DeclT 都是 int&& 类型
    return 0;
}
```

### 29. 假定移动操作不存在、成本高、未使用

本章中我们主要介绍了 **C++11** 引入的移动语义、右值引用和引申的转发引用。它们在不少应用场景都能提升效率，事实上也确实如此。不过我们也不应该“神化”这些新的特性。**C++98** 的标准库在 **C++11** 经过了彻底的翻修，以在移动操作更加高效时使用移动操作，不过有些第三方代码库不一定支持移动操作，或者有些类干脆禁止了移动操作（通过 `= delete` 定义），此时使用 `std::move` 等技巧的作用就非常有限。

此外，有些类型的设计方式根本不适合进行移动，比如 `std::array`：

```cpp
std::array<int, 100> arr_1;
auto arr_2 = std::move(arr_1);		// 没有什么用处，因为 std::array 将元素存储在对象当中，依然需要移动每一个元素
									// 对于 int 类型来说，移动就是复制。
```

相比之下，`std::vector` 的移动要高效得多（因为它本身只需要转移一个堆指针的所有权）：

```cpp
std::vector<int> vec_1;
auto vec_2 = std::move(vec_1);		// 将 vec_1.data_ptr 赋值给 vec_2.data_ptr
```

处于中间形态的 `std::string` 则在短字符串的情况下存储在栈上，否则会开辟堆上的空间存储。这样我们对于不同长度的字符串，移动相比复制的优势是不同的。如果处理的是短字符串，则移动并没有什么好处。此外，我们在[前文]()中也提到过，为了兼容 **C++98** 容器操作的一些强异常保证，一些底层复制操作只有在已知移动操作不会抛出异常时才会使用移动操作。因此，没有 `noexcept` 声明的移动在标准库一些地方仍会被强制使用复制操作代替。

总之，在以下场景中，**C++11** 的移动语义并没有什么用处：

- 没有实现移动操作：待移动的对象未能提供移动操作，此时移动就会变成复制操作。
- 移动并没有更快：由于对象的存储性质，移动并不比复制更快。
- 移动不可用：移动本可以发生的语境下，要求移动不能抛出异常，但是移动操作没有声明为 `noexcept`。

还有一个容易忽视的情形，当对象是一个左值时，如果我们没有使用 `std::move` 或 `std::forward`，它也不会进行移动操作。

### 30. 熟悉完美转发的失败情形

完美转发堪称 **C++11** 中最引人注目的特性之一。不过，特性繁多的 **C++** 中自然能找到让它无法正常工作的情形。，对于任意函数 `foo`，如果下面两个操作的行为不同，我们就称完美转发在此失败了：

```cpp
template<typename... Ts>
void fwd(Ts&&... params) {
    f(std::forward<Ts>(params)...);
}
int main() {
    f(/* 一些参数 */);
    fwd(/* 一些参数 */);		// 理论上这两个函数应该有完全相同的行为。
```

下面我们将列举诸多失败的情形：

- 大括号表达式，也即是初始化列表，比如 `{ 1, 2 ,3 }`：

  ```cpp
  void f(std::vector<int> const& v);
  int main() {
      f({ 1, 2, 3 });			// 没有问题，{ 1, 2, 3 } 会通过构造函数隐式转换为 std::vector<int>
      fwd({ 1, 2, 3 });		// 错误！{ 1, 2, 3 } 本身没有类型，因此此处转发引用无法进行类型推导
      auto list = { 1, 2, 3 };
      fwd(list);				// 没问题，因为 list 是 std::initializer_list<int>，可以隐式转换为 std::vector<int>
      return 0;
  }
  ```

- 用 `0` 或 `NULL` 作为空指针。此时会将其类型推导为整型，当然也就无法保持其想要的指针类型了。

- 使用仅有声明的 `static const` 成员变量。**C++** 中，对于 `static const` 的成员，编译器往往将其看作一个类似于宏的固定值。因此我们甚至不需要给它在源文件中提供定义，只需要在类定义中直接为其“初始化”即可：

  ```cpp
  struct Foo {
      static int const x = 42;	// 没有问题，所有 Foo::x 出现的地方都会被替换为 42
  };
  int main() {
      std::cout << Foo::x;
      return 0;
  }
  ```

  不过上面成立的前提是，没有任何对 `Foo:x` 的取址操作，彼时编译器 *必须* 为它分配内存地址，因此必须要在类外给出定义了。这里面，引用也可能产生取址操作（因为多数情况下引用都是通过指针实现的）：

  ```cpp
  struct Foo {
      static int const x;
  };
  int const Foo::x = 42;			// 需要在源文件中定义
  int main() {
      int& r = Foo::x;
      std::cout << r;
      return 0;
  }
  ```

  敏锐的朋友可能已经意识到这和转发引用的相关性了。如果我们将类中静态常量成员定义为前面的形式，那么对它们使用完美转发都会失效，否则会发生链接错误。这个的解决方法也很显然，就是尽量将声明为 `static const` 的成员在源文件中定义出来。

  在 **C++17** 之后，我们可以将成员声明为 `inline`，从而可以将它们在类定义中赋予初始值，并不再出现链接问题（可以类比自从 **C++**标准伊始就能定义在类定义中的函数默认为 `inline`）：

  ```cpp
  struct Foo {
      static inline int const x = 42;	// 现在也没有问题了
  };
  int main() {
      int& r = Foo::x;
      std::cout << r;
      return 0;
  }
  ```

- 重载的函数和模板。不考虑转发引用时，即使出现函数重载，以函数指针为参数的函数也能顺利找到正确的函数，这是因为其签名中类型是确定的：

  ```cpp
  void process(int);
  void process(int, char*);
  void foo(void (*proc)(int));
  int main() {
      foo(process);			// 没有问题
      return 0;
  }
  ```

  但是如果调用 `fwd(process)` 问题就很大了，因为此时编译器无法确定传入的究竟是哪一个 `process`。此处有几种解决方法：

  ```cpp
  using RealProcessType = void (*)(int);
  int main() {
      RealProcessType rp = process;
      
      fwd<RealProcessType>(process);
      fwd(static_cast<RealProcessType>(process));
      fwd(rp);
  }
  ```

  不过这样的方法显然需要你知道 `fwd` 函数中究竟调用了哪一种 `process`，而用户并不一定知道这一点。因此在使用完美转发的函数中要避免使用重载的函数来处理完美转发的变量。

- 最后，是一个 **C++** 中较为少用的特性，位域。一言以蔽之，**C++** 标准禁止位域中的成员被非常量引用绑定。因此为了调用完美转发函数，我们需要将其强制类型转换为一个整型，之后再传入：

  ```cpp
  struct IPv4Header {
      std::uin32_t version : 4,
      			 IHL : 4,
      			 DSCP : 6,
      			 ECN : 2,
      			 totalLength : 16;
  };
  int main() {
      IPV4Header h{};
      fwd(static_cast<uint16_t>(h.totalLength));
  }
  ```

## `lambda` 表达式

lambda 表达式让函数定义变得十分简单，我们可以在许多需要传入谓词的函数（如 **STL** 中的 `_if` 系列函数）中轻松地使用。其它常见地应用还有提供 `std::sort`、`std::map` 等允许自定义比较函数的情景。比起定义一个函数（**C++** 中的函数只能定义在文件作用域中）或定义一个仿函数（重载了 `operator ()` 的类型），lambda 表达式各种意义上都更加出色。

lambda 表达式可以创建一个运行期的对象，称为 **闭包（Closure）**，它的结构和仿函数基本一致。不过，闭包的类型是被编译器隐藏起来的，每个 lambda 表达式都会对应独特的闭包类型，尽管它们看起来可能是一模一样的。

```cpp
int main() {
    auto f = []() { return 0; };
    auto g = []() { return 0; };
    std::cout << std::is_same_v<decltype(f), decltype(g)>;	// 输出 0
    return 0;
}
```

闭包默认支持了复制构造，因此我们可以在初始化时让一个闭包等于另一个的拷贝，此时两者的类型是相同的。不过闭包将复制赋值函数定义为 `= delete`，因此闭包在初始化后就不可能更改了。**C++20** 之后，没有捕获的闭包允许进行复制赋值操作。

### 31. 避免默认捕获形式

**C++11** 中，闭包有两种捕获形式，值或引用。当在捕获列表中显示列出形如 `x` 或 `&y` 时，我们会将 `x` 或 `y` 的引用捕获到闭包中。如果直接写 `=` 或 `&` 则会默认将所有闭包中出现的自由变量捕获为值或引用。

捕获引用是有较大风险的。因为闭包以返回值或参数形式离开当前作用域后，其捕获的引用可能会失效：

```cpp
using FilterCont = std::vector<std::function<bool(int)>>;
static FilterContainer filters;
void add_divisor_filter() {
    auto x = some_value();
    auto y = some_value();
    auto divisor = get_divisor(x, y);
    filters.emplace_back([&](auto value) { return value % divisor == 0; });
}
```

上面捕获的 `divisor` 会在 `add_divisor_filter` 调用后失效，此后如果调用存储在 `filters` 里的闭包就会报错。这里如果坚持将所有捕获变量显式写出来，即 `[&divisor]` 的形式，或许能提醒我们这是一个局部变量。

或许，在确保捕获引用的生命周期长于闭包时，可以使用默认捕获的形式；不过为了避免此后发现这个闭包特别好用，从而复制粘贴到另一块代码处且忘记修改默认捕获，就可能造成问题。

你也许会认为，如果默认捕获值就不会有这样的问题。很可惜，让我们看看下面的例子：

```cpp
class DivisorFilter {
public:
    void add_filter() const {
        filters.emplace_back([=](auto value) { return value % divisor == 0; });
    }
private:
    int divisor;
};
```

理论上，lambda 只能捕获非静态局部变量，这里的 `divisor` 是一个成员变量，是如何被捕获的呢？答案是 `[=]` 会默认以引用方式捕获 `this`，所以闭包中就可以省略 `this` 调用 `divisor` 了。你或许已经意识到，这是一个巨大的隐患，因为这样闭包使用的合法性就要关联一个 `DivsiorFilter` 对象的声明周期。因此一个推荐的写法是：

```cpp
void DivisorFilter::add_filter() const {
    auto captured_divisor = divisor;
    filters.emplace_back([captured_divisor](auto val) { return value % capture_divisor == 0; });
}
```

在 **C++14** 之后，我们可以使用 **广义 lambda 捕获（Generalized Lambda Capture）** 来捕获这样的变量：

```cpp
void DivisorFilter::add_filter() const {
    filters.emplace_back([divisor = divisor](auto val) { return value % capture_divisor == 0; });
}
```

此外，`[=]` 还会给人以闭包和外界行为完全隔离的错觉。事实上，由于所有闭包都会默认以引用方式捕获静态变量，当外界修改了静态变量时，闭包内的行为也会发生变化。因此，一方面我们建议在闭包中慎重使用外部的静态变量；另一方面，默认捕获应该被杜绝。

### 32. 使用初始化捕获将对象引入闭包

我们知道可以用值或引用两种方式捕获变量；但是既然 **C++11** 引入了移动语义，那么是否能够通过右值引用来移动变量到 lambda 中呢？遗憾的是，这不能通过简单的类如 `[&&x]` 的方式做到。**C++14** 中提供的广义 lambda 捕获倒是可以完成相同的事情：

```cpp
int main() {
    auto up = std::make_unique<int>(10);
    auto f = [up = std::move(up)] { return *up; };	// 当然，可以直接在这里调用 std::make_unique
    return 0;
}
```

如果使用 **C++11** 的编译器，我们可以延续此前仿函数的定义：

```cpp
class CaptureByMove {
public:
    explicit CaptureByMove(std::unique_ptr<int>&& up)
        : m_up(std::move(up)) {}
    int operator ()() const {
        return *m_up;
    }
private:
    std::unique_ptr<int> m_up;
};
```

不过这样有点太啰嗦，如果希望简单一些，可以使用 `std::bind`，它会返回一个新的函数，其等价于 `std::bind` 的第一个参数提供的函数，但该函数的前几个参数由 `std::bind` 的后续参数提供。

```cpp
int main() {
    auto up = std::make_unique<int>(10);
    auto f = std::bind([](std::unique_ptr<int> const& up) { return *up; }, std::move(up));
    return 0;
}
```

细心的同学会发现，这里 `up` 是作为常引用传入的，这是因为闭包的实现会自动将 `operator ()` 声明为 `const`，因此所有捕获变量不能在其中修改，我们为了保持意义上的一致，就传入了常引用。如果需要修改捕获的参数，则需要将 lambda 声明为 `mutable`：

```cpp
int main() {
    auto f = [up = std::make_unique<int>(10)] mutable { *up += 42; return *up; };
    auto g = std::bind(
        [] (std::unique_ptr<int>& up) mutable { *up += 42; return *up; }, std::move(up));
    return 0;
}
```

### 33. 对 `auto&&` 类型的形参使用 `decltype` 以通过 `std::forward` 转发

**C++14** 引入了 **泛型 lambda（Generic Lambda）**，这无疑是对 lambda 表达能力的极大提升：

```cpp
int main() {
    auto id = [] (auto x) { return x; };
    return 0;
}
```

上面 `id` 的闭包类型等价于下面这个：

```cpp
class GenericLambda {
public:
    template<typename T>
    auto operator ()(T x) const {
        return x;
    }
};
```

不过，如果在需要知晓闭包参数中 `auto` 类型的地方，这就比使用模板函数时更加麻烦。比如如果我们的 lambda 中需要转发传入的参数，此时必须使用 `std::forward`，且参数类型需要指定出来。我们可以让 `decltype` 完成这个工作：

```cpp
int main() {
    auto f = [] (auto&& param) {
        return foo(std::foward<decltype(param)>(param));
    };
    auto g = [] (auto&&... params) {
        return bar(std::forward<decltype(param)>(param)...);
    };
    return 0;
}
```

如果还记得 `decltype` 规则的同学会发现一个问题，那就是它对右值引用会如实地给出其类型，这似乎和我们之前对 `std::forward` 的用法不一致（我们一律不使用任何引用限定符）。不过回忆一下 `std::forward` 的定义：

```cpp
template<typename T>
T&& forward(std::remove_reference_t<T>& param) {
    return static_cast<T&&>(param);
}
```

如果我们将 `T` 显式给出，比如说为 `int&&`，实例化后的结果应该是：

```cpp
// 下面不是合法的 C++ 代码，仅作为演示
int&& forward(int& param) {
    return static_cast<int&& &&>(param);
}
```

由于引用折叠的机制，我们最后依然会得到 `static_cast<int&&>(param)`，因此没有任何问题。

### 34. 优先选用 `lambda` 表达式，而非 `std::bind`

`std::bind` 是古老的 `std::bind1st` 与 `std::bind2nd` 在 **C++11** 时的自然进化。某种角度讲，它的表达力还算强：

```cpp
void foo(int x, double y, int k) {
    std::cout << x << y << k;
}
int main() {
    using namespace std::placeholders;		// 引入这个命名空间，才能使用下面的占位符 _1, _2 等
    auto f = std::bind(foo, _2, _1, 42);
    f(3.14, 10);		// 相当于调用 foo(10, 3.14, 42)，此处的 3.14 会绑定到 _1，10 会绑定到 _2
    return 0;
}
```

不过，由于占位符 `_1`、`_2` 等的使用过于灵活，同时 `std::bind` 返回的函数对参数毫无限制，这会产生一些奇怪的默许行为：

```cpp
void foo(int x) {
    std::cout << x;
}
int main() {
    using namespace std::placeholders;
    auto f = std::bind(foo, _3);
    f(1.23, "abc", 10);		// 没有问题，前两个参数会被完全省略
    return 0;
}
```

真正让 `std::bind` 陷入难堪的是重载引发的函数选择问题。如果需要绑定的函数有多个版本，我们就不得不提供它的类型：

```cpp
void foo(int i);
void foo(double x, double y);
int main() {
    using namespace std::placeholders;
    using FooType1 = void (*)(double, double);
    auto f = std::bind(static_cast<FooType1>(foo), _2);
    f(1.0, 2.0);
    return 0;
}
```

这简直过于繁琐（我们甚至还没开始抱怨每次使用都不得不加的 `using namespace`）。相比之下，lambda 表达式清晰而简洁，且能在闭包体中轻松地进行更加丰富的运算。比如需要一个判断某数是否在一个区间中的函数，用 lambda 表达式和 `std::bind` 的实现如下：

```cpp
auto lo = 0, hi = 20;
auto between_by_lambda = [lo, hi] (auto const& val) { return lo <= val && val <= hi; };
auto between_by_bind = std::bind(std::logical_and{},
                                 std::bind(std::less_equal{}, lo, _1),
                                 std::bind(std::less_equal{}, _1, hi));
```

这样的天差地别让 `std::bind` 几乎没有任何可取之处了。实际上，除了在 **C++11** 中补全闭包不能移动捕获变量的遗憾（参见[条款32](#32. 使用初始化捕获将对象引入闭包)），以及缺少类型检查间接促使的“多态性”（这一点在 **C++14** 后也被泛型 lambda 完全替代），我们应该避免使用 `std::bind`。

## 并发 API

谢天谢地，**C++11** 终于在语言层面上支持，且提供了一系列的标准库用于并发了。这样我们就可以在不同平台编写兼容的并发程序了。

### 35. 优先选用基于任务而非基于线程的程序设计

如果我们想要异步运行一个函数 `foo`，有两种基本方式，**基于线程（Thread-Based）** 或 **基于任务（Task-Based）**：

```cpp
int foo();
int main() {
    std::thread t(foo);				// 基于线程
    auto fut = std::async(foo);		// 基于任务
    return 0;
}
```

两者间一个比较大的区别是，我们没有合适的方法得到前一种执行后返回的值，而后一种可以通过 `get` 方法可以得到。更重要的是，如果 `foo` 抛出了异常，以线程方式异步时程序会直接终止（调用 `std::terminate`），而基于任务的方式可以访问到这个异常。此外，`std::thread` 需要程序员手动处理可能出现的线程耗尽、超订、负载均衡以及跨平台适配问题，而 `std::async` 由于其调用时“不一定真正创建新线程”的特性，可以灵活地处理线程耗尽以及超订问题。

### 36. 如果异步是必要的，则指定 `std::launch::async`

正如前面条款中所说，调用 `std::async` 时，函数可能不会立即异步运行；实际上它甚至可能不会通过异步方式来运行。运行的策略有下面两个：

- `std::launch::async`：函数必须以异步方式运行，即执行于另一线程上。
- `std::launch::deferred`: 函数只有在其返回的值调用 `get` 或 `wait` 时才调用，即讲函数调用推迟到某一个必要的时刻。采取该策略时，函数调用会阻塞当前线程，直到函数调用完毕为止。

如果不指定任何运行策略，`std::async` 采用的将是上面两种的或运算，也即两者中任一个。下面的例子中，两个式子执行的策略是一致的：

```cpp
auto fut_1 = std::async(f);
auto fut_2 = std::async(std::launch::async | std::launch::deferred, f);
```

默认情况下的灵活性也是为何条款35中我们推荐使用 `std::async` 的原因。

不过，模棱两可的执行策略会导致下面的问题：

- 无法预知函数是否和当前线程并发运行。
- 无法预知函数是否运行在与调用 `get` 或 `wait` 的线程不同的某线程上。
- 无法预知函数是否已经运行，因为无法保证程序的每一条控制流都调用了 `get` 或 `wait`。

因此，如果在函数中使用了 `thread_loacal` 变量，我们甚至不知道这是哪个线程的局部存储；此外，基于 `wait` 的循环可能会永远无法终止：

```cpp
using namespace std::literals;
void f() {
    std::this_thread::sleep_for(1s);
}
auto fut = std::async(f);		// 对于负载较高的情况，fut 可能永远被推迟
while (fut.wait_for(100ms) != std::future_status::ready) {
    // 省略内容
}
```

解决方法如下：

```cpp
auto fut = std::async(f);
if (fut.wait_for(0s) == std::future_status::deferred) {
    // 利用 wait 或 get 方法异步调用 f
}
else { // 未被推迟，则不可能死循环
    while (fut.wait_for(100ms) != std::future_status::readY) {
        // 省略内容
    }
}
```

作为总结，如果要用默认的运行策略使用 `std::async`，就要确保：

- 任务不需要与调用 `get` 或 `wait` 的线程并发执行。
- 读写任一个线程的 `thread_local` 变量都无碍。
- 或者保证对 `std::async` 返回值调用 `get` 或 `wait`，或者能接受任务从不执行。
- 考虑 `wait_for` 或 `wait_until` 会将任务推迟。

只要上面任一个条件不能满足，就应该确保任务以异步方式执行。我们可以轻松撰写一个函数自动使用 `std::launch::async` 策略：

```cpp
template<typename F, typename... Args>
inline auto force_async(F&& f, Args&&... args) {
    return std::async(std::launch::async, std::forward<F>(f), std::forward<Args>(args)...);
}
```

### 37. 使 `std::thread` 类型对象在所有路径均不可连结

`std::thread` 总处于两种状态之一，可以被 **连结（Join）**，或不可连结。可连结的 `std::thread` 通常是其底层线程处于阻塞或等待调度，或者是已经运行结束。相对地，不可连结的 `std::thread` 对象分为下面这些情况：

- `std::thread` 被默认构造。此时没有任何函数可以执行。
- 被移动的 `std::thread`，此时也没有任何函数可以执行。
- 已连结的 `std::thread`，此时这个对象不再对应已结束运行的底层线程。
- 已分离的 `std::thread`，因为分离操作会将该对象和其对应的底层线程分开。

### 38. 对变化多端的线程句柄析构函数行为保持关注

### 39. 考虑针对一次性事件通信使用以 `void` 为模版类型实参的期值

### 40. 对并发使用 `std::atomic` 对特种内存使用 `volatile`

## 微调

### 41. 针对可复制的形参，在移动成本低且一定会被复制的前提下，考虑将其按值传递

**C++** 的一些函数参数，本来就是用来构造对象的，比如下面这两个函数：

```cpp
class MyClass {
public:
    void add_name(std::string const& str) {
        m_names.push_back(str);
    }
    void add_name(std::string&& str) {
        n_names.push_back(std::move(str));
    }
    // 省略内容
};
```

这里虽然我们分成两种引用类型来接收 `std::string`，且效率上一定是最优的，但这重复的代码不免让我们心烦：明明是在做同一件事，却要写两个函数。如果参数数量更多，情况只会更糟……等等，我们不是有完美转发吗？

```cpp
class MyClass {
public:
    // 这里使用了 C++20 的 concept 特性，用来限定 T 的类型
    template<std::convertible_to<std::string> T>
    void add_name(T&& str) {
        m_names.push_back(std::forward<T>(str));
    }
    // 省略内容
};
```

不过，此时对于任何可以类型转换为 `std::string` 的类型，包括通过构造函数转换的，都会在调用时被实例化为不同类型。这毫无疑问会造成代码膨胀。唉，如果有什么方法可以避免代码膨胀，又能针对左值和右值分别进行复制和移动操作就好了……还真有，而且这是 **C++** 程序员很早就学到 *不要* 做的一件事，那就是将`std::string` 以值的方式传入函数：

```cpp
class MyClass {
public:
    // 对于左值和右值都没问题，前者会产生复制构造，后者会产生移动构造
    void add_name(std::string str) {
        m_names.push_back(std::move(str));	// 将参数再移动构造到 std::vector 当中
    }
}
```

如果把这个方法和开始的重载了两种引用的方式，会发现只多了一次移动操作。这样的额外成本我们完全可以接受！不过，还是有必要分析一下我们什么时候才需要考虑这样的方式：

- 只有可复制的类型才能像这样传递。
- 移动成本低廉时，额外的成本才能被接受。如果是类似于 `std::array` 这样高额移动成本的类型，多进行一次移动可能无法接受。
- 

### 42. 考虑置入而非插入

