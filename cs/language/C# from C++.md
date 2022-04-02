# **C#** 学习 —— 从 **C++** 角度

[toc]

最近出于兴趣开始接触 **C#** 。为了更好地理解一些语法特征，我也会主动尝试对标 **C++** 中同等的功能，所以不由得会有两者间的比较；并且早就对它优于 **Java** 的语言特性有所耳闻，于是眼睛会更加叼一些。

参考书是《C#图解教程（第5版）》（下文会简称为《C#教程》），本笔记的所有章节内容都是根据原书分划，并以个人关注点为导向的东西，并不能反映原书的全部知识点。

## **C#** 和 **.NET** 框架

~~警告：本节存在大量英文缩写，可以酌情放松一些，但至少对它们有个眼熟。~~

**C#** 是为了 **.NET** 而生的，这从它的诞生历史就能看出。20 世纪末，**Windows** 平台的编程有好几个门派， **Win32 API**、**MFC（Microsoft Foundation Class）**，或 **COM（Component Object Model）**。但是它们或多或少都有不可忽视的问题。因此微软在 21 世纪初推出了跨平台、安全、采用行业标准通信协议的框架，允许代码将其作为运行环境。下面是 **.NET** 的一些重要组件：

- **CLR（Common Language Runtime）**：这是代码的执行环境，进行程序的内存管理、代码安全认证以及代码执行、线程和异常的管理。
- **BCL（Base Class Library）**：通用的一个大型类库，可以在程序中调用。
- 编程工具：包括“宇宙第一**IDE（Integrated Development Environment）**” **Visual Studio**、与 **.NET** 兼容的编译器、调试器、和一些 **Web** 开发工具，比如 **ASP.NET**。

现在，我们在编写代码（可能调用 **BCL**）后，通过编程工具进行编译和调试，最后运行在 **CLR** 上。这里的代码可以是任何与 **.NET** 兼容的语言，比如 **C#**、**Visual Basic .NET**、**F#**、**C++/CLI**（我不是很想承认这个是 **C++**，当然它本身并不是）等。这样的公共平台是非常难得的，因为编程者不需要再针对桌面端、移动端、**Web**等多个场景设计不同的代码；取而代之的，基本只需要面向 **.NET** 编程即可（脏活累活都交给框架实现者）。同时， **CLR** 中有一个内存管理机制，即闻名的 **GC（Garbage Collector）**，它会检查内存中不被引用的对象并回收。这能从内存管理的泥沼中解放程序员的双手。此外，同被 **.NET** 支持的语言之间可以互相调用模块，仿佛毫无隔阂。

从这里看，**.NET** 和 **Java** 语系的 **JVM（Java Virtual Machine）** 非常相似（只能说一模一样）。先不论这两者之间的优劣，至少 **.NET** 在多个方面能把前文提到的三个老大哥打趴下的，因此它也确实受到了不少开发者的欢迎。

和 **Java** 源码被编译为字节码类似，**.NET** 上的语言会被编译为 **CIL（Common Intermediate Language）** 组成的程序集。这段代码依然是平台无关的，并且携带了程序中的类型和安全信息。随后，在程序被调用时，再通过 **JIT（Just-In-Time）** 编译器将程序集分步编译为机器相关的代码，再执行。**JIT** 进行编译的过程全程是受到 **CLR** 管理的。

为了像前文提到那样让不同语言在 **.NET** 上做好兄弟，微软设计了 **CLI（Common Language Infrastructure）** 作为这些语言都要遵守的规范，其中包括 **CLR**、**CLS（Common Language Standard）**、**BCL**、**CTS（Common Type System）**、**CIL**，以及元数据定义和语义。目前对这些从字面理解尚浅，但是可以看到为了让迥异的多个语言友好相处是要费比较大功夫的。不过这些语言都是微软自己家的，调教起来阻力不大（**C++** 也被魔改成 **C++/CLI** 了）。

《C#教程》在本节末吹了一下 **Windows** 和历代推的跨平台的环境，并且用 **Stackoverflow** 的开发者调查报告表明 **C#** 的流行和受欢迎程度都超过更加流行的 **JavaScript**、**SQL** 和 **Java**。总之未来可期就对了。呃，我是不反对的。不过语言的流行程度我无法判断，语言的友好程度、趣味性和创造力我还是能有所体会的，那么就让我拭目以待吧！

## **C#** 和 **.NET Core**

总结就是移动端和 **Web** 侵食了桌面应用的不少份额，早期专注于桌面端的 **.NET** 可能自己打不太行，因而微软推出了衍生产品 **.NET Core** 来同样支持 **Web** 开发和 **Linux**、**MacOS**，并收购了 **Xamarin** 来支持移动端。现在局势可能有点复杂，但是总之 **C#** 在这些上面都能较好的支持，因此学 **C#** 就对了。

## **C#** 编程概述

**C#** 的程序主体结构是 **命名空间（Namespace）** 包含着 **类（Class）**，其中再包含着 **成员函数（Member Function）** 和 **成员（Member）**。程序从一个 `Main` 函数中开始执行。下面是一个你好世界的例程：

```csharp
using System;		// 相当于导入一个包
namespace MyNamespace {
    class MyProgram {		// 所有函数和变量都需要出现在类当中
        static void Main() {
            Console.WriteLine("Hello, world!");	// Console 是在 System 命名空间的类，WriteLine 是一个方法
        }
    }
}
```

《C#教程》中罗列的 **C#** 的关键字足足有 77 个之多，其中不少是我难以猜测作用的新面孔。除此之外还有 26 个 **上下文关键字（Context-Sensitive Keyword）**，真的相当之多。**C++** 中也有上下文关键字的概念，能够立刻想到的是诸如 `override` 和 `final` 这样 **C++11** 之后添加的，它们只有在特定位置才会被识别为关键字。

**C#** 中的字符串可以进行格式化输出：

```csharp
// 这里 {0}，{1} 是指代后面的替换值，这里 {0} 就是 a，{1} 就是 b
// 后面的替换值可以是任何类型的，格式化串中的替换值顺序可以是任意的且允许重复
Console.WriteLine("Two numbers: n1={0}, n2={1}", a, b);
// (C# 6.0) 如果替换的是变量，可以使用所谓的字符串插值，用字符串前缀 $，里面所有大括号中都会视作代码表达式
Console.WriteLine("My name is {name}, {age} and live in {city}{address}");
```

哎，**C++** 馋疯了。用 `std::ostream` 那点东西根本不舒服；否则就是用远古的 `printf`，但也远比不上 **C#** 这样灵活。**C++23** 将要纳入标准库的 `std::format<>` 也更加相似于 **C#** 传统的格式化方式（上面的第一种），并不能随便控制替换值顺序和重复出现，不过在其它的方面更加强大一点。

此外，格式化字符串对数字有更加细微的控制，可以在 `{}` 中使用冒号后加上格式说明符的方式指定数字的输出格式，比如 `{abc, F4}` 表示输出四位小数的浮点数。这里格式说明符太多，就不展开说明了。

## 类型、存储和变量

**C#** 有 16 种预定义类型，其中包含整数、浮点、布尔、字符等简单类型，以及 `object`、`string` 和 `dynamic` 三种非简单类型，其中 `object` 是所有其它类型的基类。除了用于动态语言编写程序集的 `dynamic` 外，其余的类型都能在 **.NET** 框架中找到类型对应。

**C#** 也对自定义类型进行了分类，包括 `class`、`struct`、`array`、`enum`、`delegate`（委托）和 `interface`（接口）。可以通过类型声明来创造自定义类型。除了数组和委托外，其它的结构中都可以声明成员。**C++** 和这个差别不太大，也包括 `class`、 `struct`、 `enum` 以及内置的数组类型；接口类似于 **C++** 中的抽象类；委托则类似于 **C++** 的函数指针。总之有进行类比的价值。

**C#** 将所有类型分为 **值类型** 和 **引用类型** 两种。前者只占用栈空间，而后者本质是一个栈上的指针指向堆上的内存空间。所有的内置简单类型、`struct` 和 `enum` 是值类型，此外都是引用类型。这点倒是和 **Java** 基本一致，但远不如 **C++** 灵活（我想让什么在栈上，它绝对不敢在堆上），不过权力越大责任也越大，为了 **GC** 还是放弃这点权力吧。

变量可以被分为 **局部变量**、**字段**、**参数** 以及 **数组元素**（这个分类怎么看怎么别扭）。其中局部变量是在函数内部定义的变量；字段是类中声明的变量，永远依附于一个类或者一个对象；参数是通过函数传递得到的变量，类似于局部变量；数组元素可能是局部变量也可能是类型的成员。

变量初始化的方式比较直接，即 `type varname = init_val`。不过 **C#** 继承了 **C++** 的“坏毛病”，对于局部变量并不会为其自动初始化。当然，访问一个未初始化的变量会产生异常。其余的情形，比如字段，都能够自动初始化，因此可以放心地使用。

## 类的基本概念

类是 **C#** 中极其常用的结构。下面是一个示例的 **C#** 类型定义：

```csharp
class MyClass {
    private int i = 10;		// private 和 C++ 中的含义一致，并且可以在类中初始化变量
    int j;					// class 中默认为 private，且可以自动初始化。此处为 0
    public string str;		// public 和 C++ 中的含义一致，此处初始化为 null（注意和 C++ 中初始化为 "" 不同）
}
class MyProgram {
	static void Main() {
     	MyClass mc = new MyClass();		// new 语句和 C++ 中的含义一致，不过 C# 的引用类型并不需要形如 * 的修饰符  
        mc.str = "abc";
        Console.WriteLine(mc.str);		// 输出 "abc"
    }
}
```

一提到 **Java** 和 **C#** 的访问修饰符就费劲儿，只要不是 `private`，所有变量或者函数前面都要写出来。相比之下 **C++** 的访问修饰标签简洁太多了。还有一个需要适应的，就是引用类型不需要指针修饰符 `*`，并且所有在对象上的操作都默认解引用过，这点比较省事但是有的时候显得模糊，比如“著名”的浅拷贝问题：

```csharp
class MyClass {
	private int i;
    public MyClass(int i_) : i(i_) {}	// 类初始化列表的用法和 C++ 一致
    public int get_i() { return i; }
}
class MyProgram {
 	static void Main() {
    	MyClass mc1 = new MyClass(10);
        MyClass mc2 = mc1;				// 这里是一个浅拷贝，只是对指针进行了初值
        mc2 = new MyClass(mc1.get_i());	// 这才是深拷贝，创建了新的对象并且将字段复制过去
    }
}
```

所以说等号的作用可能会造成一定迷惑。不过一个辅助判断方式就是只有出现 `new` 语句时才可能出现深拷贝。**C++** 在这点上分得很清楚：

```cpp
class MyClass {
private:
	int i;
public:
    MyClass(int i_) : i(i_) {}
    int get_i() { return i; }
};
int main() {
    MyClass* mc1 = new MyClass(10);		// 通过 new 表达式创造一个指向堆的指针
    MyClass* mc2 = mc1;					// 只复制了指针，因此没有新的 MyClass 对象
    mc2 = new MyClass(mc1->get_i());	// 这里才产生了新的对象，并且将新的指针赋值给 mc2
    delete mc1;
    delete mc2;
    return 0;
}
```

## 方法

这一部分和 **C++** 风格有些差异，需要提到的首先有两点：其一，**C#** 不支持变量隐藏，也就是在块结构内的变量不能和外面的变量拥有相同的名字；此外，在 **C# 7.0** 往后，可以在函数中定义函数，即局部函数（语法上更加丰富包容了，我其实一直不理解为啥 **C++** 不能允许局部函数）。

此外，**C#** 有千奇百怪的参数传递方式，这几乎让我改变对 **C#** 还算不错的第一印象：

### 参数传递

和 **C++** 一样，函数默认传递的都是值，即所谓 **值参数（Value Parameter）**，被传入的内容无论在函数中如何修改，是无法改变函数调用处的这个值的。但是因为存在引用类型这种东西，因此虽然引用本身没法改变，却可以改变引用的对象的值：

```csharp
class MyClass {
	public int val = 42;
}
class MyProgram {
 	void PassByValue(int a, MyClass mc) {	// 以值方式传入，即所有传入值都被浅拷贝了一遍
        a = 10;					// 不会影响调用处的的值
        mc.val = 10;			// mc 会被解引用，这里会影响调用处的值
        mc = new MyClass();		// mc 现在引用了一个新的对象，但是不会影响调用处的值
        mc.val = 20;			// mc 引用的新对象被修改了，但是没什么用
    }
    static void Main() {
        MyClass mc = new MyClass();
        int i = 42;
        PassByValue(i, mc);
        Console.WriteLine($"i = {i}, mc.val = {mc.val}");	// 输出 "i = 42, mc.val = 10"
    }
}
```

同时，**C#** 也支持通过引用传递，即所谓 **引用参数（Reference Parameter）**。这样，许多值类型就可以通过函数修改了：

```csharp
class MyClass {
 	public int val = 42;   
}
class MyProgram {
 	void PassByReference(ref int a , ref MyClass mc) {	// 用引用传递
    	a = 10;					// 因为是通过引用传递的，这里会影响调用处
        mc.val = 10;
        mc = new MyClass();		// 这里会影响引用本身！调用处的引用现在引用了新的对象
        mc.val = 20;
    }
    static void Main() {
     	MyClass mc = new MyClass();
        int i = 42;
        PassByReference(ref i, ref mc);		// 函数调用处也要用 ref 关键字传递，ref 后面不能是常量
        Console.WriteLine($"i = {i}, mc.val = {mc.val}");	// 输出 "i = 10, mc.val = 20"
    }
}
```

我们可以用一个 **C++** 程序来统一介绍这个特性：

```cpp
struct MyClass {
    int val;
}
int MyClass::val = 42;
void pass_by_value(int a, MyClass* mc) {
    a = 10;
    mc->val = 10;
    delete *mc;
    mc = new MyClass();
    mc->val = 20;
}
void pass_by_reference(int* a, MyClass** mc) {
    *a = 10;
    (*mc)->val = 10;
    delete *mc;
    (*mc) = new MyClass();
    (*mc)->val = 20;
}
int main() {
    int i = 42;
    MyClass* mc = new MyClass();
    pass_by_value(i, mc);
    // 丑哭了，为什么 C++ 格式化打印东西这么丑
    std::cout << "i = " << i << ", mc->val = " << mc->val << '\n';
    pass_by_reference(&i, &mc);
    std::cout << "i = " << i << ", mc->val = " << mc->val << '\n';
    delete mc;
    return 0;
}
```

接下来开始出现奇怪的参数类型了，首先是 **输出参数**。输出参数和引用参数基本一致，但是有几个额外检查：

- 需要是已经声明的变量
- 函数内在对输出参数进行赋值前不能被读取
- 函数在返回之前必须对所有输出参数进行赋值
- 调用函数处需要使用 `out` 修饰符

然后是 **输入参数**。其和引用参数基本一致，但是有一个额外要求：

- 函数内不能修改这个参数

```csharp
class MyClass {
 	public int val = 42;   
}
class MyProgram {
 	void PassInOut(out int sum, in int x, in int y) {
     	sum = x + y; 
    }
    void PassInOut2(out MyClass mc) {
		mc.val = 10;
    }
    static void Main() {
        int result;
        PassInOut(out result, 10, 42);
        Console.WriteLine($"result = {result}");	// 输出 "result = 52"
        PassInOut(out MyClass mc);					// (C# 7.0) 可以直接在调用处声明变量
        Console.WriteLine($"mc.val = {mc.val}");	// 输出 "mc.val = 10"
    }
}
```

我们可以这样总结 `ref`、`out`、`in` 三种参数传递：

| **参数类型**      | 在函数中的行为            | 传递方式                                                |
| ----------------- | ------------------------- | ------------------------------------------------------- |
| 引用参数（`ref`） | *可能* 在函数中修改它的值 | 在传递处使用 `ref` 关键字                               |
| 输出参数（`out`） | *必须* 在函数中修改它的值 | 在传递处使用 `out` 关键字，也可以直接在调用处声明该变量 |
| 输入参数（`in`）  | *不能* 在函数中修改它的值 | 传递处和值参数无异                                      |

从某种角度上，**C#** 这样的设计用一种强制的方式抽象出函数输入和多个函数输出的模式。如果从 **C++** 角度来看，`in` 参数就是常引用 `const &`，能够达到一模一样的效果（考虑到 **C#** 的自动解引用，实际上也可以是 `const *`。`out` 参数在 **C++** 中则不存在等价对应。**C++** 除了拥有 `const` 之外，实际上缺乏对变量赋值（初始化）的强制。

最后让我们介绍 **参数数组**，它可以接受多个相同类型的参数，但有下列限制：

- 参数列表中至多只能有一个参数数组
- 参数数组必须是最后一个参数

为参数数组类型的参数传递参数时，可以是一个数组，或者是任意数量的参数。需要注意的是，因为可以传入数组，而数组是一个引用类型，所以 `null` 也可以被传入。

```csharp
class MyProgram {
 	int SumInt(params int[] vals) {
        int sum = 0;
     	if (vals != null && vals.length != 0) {
			for (int i = 0; i < vals.length; ++i) {
            	sum += vals[i];
            }
        }
        return sum;
    }
    static void Main() {
        int sum1 = SumInt(1, 2, 3, 4, 5);	// sum1 = 15
        int[] arr = { 1, 2, 3, 4, 5 };
        int sum2 = SumInt(arr);				// sum2 = 15
        int sum3 = SumInt();				// sum3 = 0
        int sum4 = SumInt(null);			// sum4 = 0
    }
}
```

比起直接传递数组，参数数组语法上更加简洁，在需要不定个数的同类型参数时比较好用。**C++** 中可以使用（过时的）可变参数函数，不过这个局限太大了（首先就是根本不知道有多少参数）。也可以使用初始化列表类型（**C++11**)，但是在调用处需要显示写出大括号，有时可能还要给出里面元素的类型。最通用的是使用模版参数包（**C++11**），不过它对类型的控制不是很严格，可能需要 `constexpr-if`（**C++17**）或者概念（**C++20**）：

```cpp
int sum_int() {
    return 0;
}
template<typename First, typename... Rest>
int sum_int(First first, Rest... rest) {
    if constexpr (std::is_same_v<First, int>) {
        return first + sum_int(rest...);
    }
    return 0;
}
int main() {
    int sum1 = sum_int(1, 2, 3);		// sum1 = 6
    int sum2 = sum_int();				// sum2 = 0
    int sum3 = sum_int(1, "abc", 2, 3);	// sum3 = 6
    return 0;
}
```

如果需要强制输入的类型，可以使用模版非类型参数包：

```cpp
template<int... args> struct SumInt;
template<>
struct SumInt<> {
  	static constexpr int result = 0;  
};
template<int first, int... rest>
struct SumInt<first, rest...> {
  	static constexpr int result = first + SumInt<rest...>::result;  
};
int main() {
    int sum1 = SumInt<1, 2, 3>::result;	// sum1 = 6
    int sum2 = SumInt<>::result;		// sum2 = 0
 	return 0;   
}
```

不过用模版做这个确实有点大材小用（费劲儿），而且适用类型必须是合法的非类型模版参数，最重要的是需要编译期常量。因此还是前面一种方式比较合适。相比之下，**C#** 虽然引入了奇怪的关键字，但是相对更加优雅地解决了此类问题。

### 方法返回值

前面我们见到的函数返回值都是值返回值，其特点可以参考前面的值参数。也可以通过引用返回一个值：

```c#
class MyClass {
    private int val = 42;
    // 这个函数返回一个 ref 类型
    public ref int GetReference() {
        return ref val;							// 需要显式使用 ref 关键字
    }
    public void Display() {
        Console.WriteLine($"The value is {val}");
    }
}
class MyProgram {
    static void Main() {
        MyClass mc = new MyClass();
        mc.Display();							// 输出 "The value is 42"
        ref int rval = ref mc.GetReference();	// ref 类型变量左右都需要使用 ref 关键字
        rval = 10;
        mc.Display();							// 输出 "The value is 10"
    }
}
```

这里和 **C++** 的指针机制可以说是一模一样；通过这些使用 `ref` 的例子，能够发现使用在类型中的 `ref` 关键字等同于 **C++** 中的指针修饰符 `*`，而使用在表达式中的 `ref` 关键字等同于 **C++** 中的取地址运算符 `&`。结合前面的引用类型，我们可以将 `ref` 看作是显式的引用类型。此外，所有引用类型在使用时（不包括浅拷贝）都会自动解引用，因此才会出现上面奇怪的 `ref int rval = ref mc.GetReference()` 这样奇怪的东西。不过从 **C#** 本身的角度来看，倒是还算自洽。

```cpp
class MyClass {
public:
    int* get_reference() {
        return &val;
    }
    void display() {
        std::cout << "The value is " << val;
    }
private:
    int val;
};
int main() {
    MyClass mc = new MyClass();
    mc->display();
    int* rval = mc->get_reference();	// C++ 不会自动解引用（右侧的指针），因此这里正好赋值给左侧的指针
    *rval = 10;							// 需要解引用
    mc->display();
}
```

最后有几个注意事项：

- 不能返回空、常量、枚举、属性和指向只读位置指针的引用。简单概括就是 `ref` 只相关于可变的值，而属性（下一章会提到）本质是一个方法所以也不能返回（可能是其读写性质比较复杂因此这里没有进行适应）。所以说 `ref` 就等同于没有 `const` 修饰的指针，因此不能指向 `const` 类型。
- 不能返回局部变量，这一点和 **C++** 的返回局部引用是类似的。
- 如果返回引用的函数的返回语句没有使用 `ref` 关键字，那么只会返回一个值。这一点相当微妙，因为在调用处是无法意识到这一点的。
- `ref` 类型的变量参与函数调用时，如果不使用引用参数传递，依然只传递值（当然，这个值可能是引用类型）。这是因为 **C#** 对所有引用的使用都会自动解引用。

### 可选参数和命名参数

值参数可以使用可选参数特性，即提供一个默认值在调用时可以省略，对于引用类型则只能使用 `null` 作为默认值。比较奇怪的是 `ref` 不允许使用可选参数（这让我开始怀疑 `ref` 更类似于 **C++** 的引用类型）。此外，可选参数总出现在方法参数列表的最后（但在参数数组之前）。总体上和 **C++** 是一致的。

命名参数是指在调用处通过指定参数名称来提示调用的实际行为：

```c#
class MyProgram {
    public int calculate(int x, int y, int z) {
        return (x + y) * z;
    }
    static void Main() {
        int ans1 = calculate(y: 1, x: 2, z: 3);		// ans = 9
        int ans2 = calculate(z: 0, y: 2, x: 4);		// ans = 0
    }
}
```

**C++** 有生之年很难实现这个功能，因为 **C++** 允许且存在大量的函数声明，并且声明和定义分离也是主流的程序设计思路。此时如果声明和定义处参数名称不同，就不能确定命名参数的标准（最主要，定义是被隐藏的）。并且在已有的各个版本中，参数名称甚至都是可选的。所以命名参数可能没法实现了。

感谢于命名参数，调用存在可选参数的方法时不一定只能从后向前隐藏参数值了。我们可以显式给出特定参数的值，省略的则根据可选参数的默认值来进行调用。这一点非常方便。

## 深入理解类

章节开头，《C#教程》给出了 **C#** 中类中所有可能的成员：

- 数据成员：字段或常量。**C#** 的常量有点类似于 **C++** 的静态编译期常量，即使用 `static constexpr` 修饰的变量。
- 函数成员：方法、属性、构造函数、析构函数、运算符、索引以及事件。这里面除了属性、索引和事件外都是 **C++** 中也存在的概念。

基本所有成员都可以使用访问修饰符和 `static` 修饰符修饰（除了常量和索引器之外）。`static` 修饰的成员（静态成员）可以直接通过类名调用。此外也可以用 `using static` 声明来将静态成员引入当前的命名空间：

```c#
class MyClass {
    public static int val = 10;
}
using static MyClass.val;
class MyProgram {
    static void Main() {
        Console.WriteLine($"MyClass.val = {val}");	// 输出 10
    }
}
```

### 属性

**C#** 的属性看起来就像一个字段。不同的是后面会跟着可选的 `get` 与 `set` 方法：

```c#
class MyClass {
    private int _val;		// 这是实际上的字段
    public int val {		// 并不会分配新的空间
        set {				// set 方法会在为属性 val 赋值的时候调用
            _val = value;	// value 是一个上下文关键字，这里表示输入的参数
        }
        get {				// get 方法会在使用属性 val 的时候调用
            return _val;
        }
    }
}
```

属性可以随意地访问类中其它的字段，所以在实现上相当灵活。同时也可以简化上面这个平凡定义：

```c#
class MyClass {
    public int val { get; set; }	// 实现了默认的属性，这里会分配一个 int 的空间
}
```

当然，这样的情形与声明一个 `public` 字段无异。更多的情况下，我们会通过省略属性的 `get` 和 `set` 控制属性的可读性和可写性；同时也可以为它们加上 `private` 修饰符来控制其对外部的可读性和可写性：

```c#
class MyClass {
 	public int val { private get { return 10} }   // 只提供了对内部的可读性，不可写。
}
```

只读属性的好处在于什么呢？观察下面的例子：

```c#
class RightTriangle {
 	public double A;
    public double B;
    public double Hypotenuse {				// 这里斜边总是取决于另外两边，因此“不可写”，但是其表现得像一个普通的字段
     	get { return Math.sqrt(A*A + B*B) }   
    }
}
```

如果在 **C++** 中，就只能将 `hypotenuse` 设置为一个函数了，这样在调用的时候就显得没有那么好看。我也一直想要让零参数的函数能够省略括号（然后强制函数指针使用取地址运算符就不会有歧义了），可惜这个改变不太可能。我个人对 **C#** 的属性在控制读写的功能上取保守态度，可能是因为习惯了 **C++** 中修饰符成为类型一部分的风格，**C#** 属性的可读写性不能从它的类型中看出来。不过 **IDE** 或许能够告诉我们它实现了怎样的 `get` 和 `set` 方法，这样就没有什么问题。

静态属性没有什么特别的，和静态字段大体相似。

### 构造函数和初始化

**C#** 的构造函数非常类似于 **C++** ，也支持构造函数初始化列表的语法。不过 **C#** 最令人羡慕的是静态构造函数，在任何类实例被创造之前以及静态成员被引用之前就会调用静态构造函数。这样就能保证类中静态成员的初始化以及让其遵守特定的顺序。在 **C++** 的话，做到同样效果就是一个非常消耗脑细胞的操作（我寻思加一个静态构造函数问题应该不大吧）。

**C#** 中的初始化和对象构造是一个并列的概念，总是在构造对象之后才进行初始化：

```c#
class MyClass {
 	public int x;
    public int y;
    private int z = 5;
    public MyClass(int z_) : z(z_) {}
}
class MyProgram {
 	static void Main() {
     	MyClass mc = new MyClass { x = 10, y = 20 };	// x = 10, y = 20, z = 5
        MyClass mc = new MyClass(5) { x = 10, y = 20 };	// x = 10, y = 20, z = 5
    }
}
```

这是一个比较方便的语法糖，可以通过初始化语法将指定的公有成员进行赋值。

至于 **C++** 的初始化？别问我 **C++** 的初始化，它简直像一坨屎。从功能上当然是不缺的，但是它就是一坨屎。**C++20** 开始支持的聚合初始化有类似于上文中初始化的格式，不过要求巨多（你可以基本认为只有写成 **C** 结构体那样的才能使用这样的初始化语法）。

### 其它特性

字段可以被声明为 `readonly`，这样在初始化之后就不能再进行修改，这一点和 **C++** 的 `const` 类似。

**C#** 中的 `this` 除了能表示当前实例的引用外，还能用于定义索引器。

```c#
class MyClass {
    private string str1;
    private string str2;
    private string str3;
    public string this [int idx] {	// 索引器使用 this 关键字以及 [] 符号，初看有些奇怪但是不无道理
        get {
            switch (idx) {
                case 0: return str1;
                case 1: return str2;
                case 2: return str3;
                default: throw new ArgumentOutOfRangeException("Index");
            }
        }
        set {
            switch (idx) {
                case 0: str1 = value;
                case 1: str2 = value;
                case 2: str3 = value;
                default: throw new ArgumentOutOfRangeException("Index");
            }
        }
    }
}
class MyProgram {
    static void Main() {
        MyClass mc = new MyClass();
        mc[0] = "abc";
        mc[1] = "def";
        mc[2] = "ghi";
        Console.WriteLine("mc.str1 = {mc[0]}, mc.str2 = {mc[1]}, mc.str = {mc[2]}");
    }
}
```

索引器本身也是一个方法（或者说是属性），因此可以被重载。这就给 **C#** 增加了不少表达力。**C++** 中会使用下标运算符重载来实现索引功能，从语言设计上噪声更小一点。

**C#** 中允许将类的定义分在不同文件中（称为分布类），这需要用到上下文关键字 `partial`。除此之外，也允许声明分布方法，即在分布类的一个部分中不给出定义，而在另一个部分中给出。分布方法不能再使用任何其它修饰符，因此分布方法一定是私有的。此外，返回类型只能是 `void`，参数中也不能包含 `out` 类型的参数。可以说是处处受限。

```c#
partial class MyClass {
    partial void Method(int x, int y);
    public void PrintSum(int x, imt y) {
     	method(x, y);   
    }
}
partial class MyClass {
 	void Method(int x, int y) {
        Console.WriteLine($"Sum is {x + y}");
    }
}
```

这个特性我感觉略有些意义不明，尤其是对于局部方法的限制过多。**C++** 中显然是不存在分布类的概念的，当然也不允许分布方法。如果考虑声明定义分离的形式，那可能比 **C#** 要灵活得多。

## 类和继承

**C#** 派生基类的方式和 **C++** 类似，即使用 `:` 而非引入新的关键字。所有类型都直接或间接继承自 `object` 类，这点和 **Java** 类似。如果要 **屏蔽（Mask）** 基类的成员，可以使用 `new` 关键字告知编译器。这个特性和 **C++** 的成员 **隐藏（Hiding）** 类似，不过 **C++** 没有提供 `new` 这样的编译器检查标记。

```c#
class BaseClass {
 	public int field1;
    static string field2;
    int field3;
    public void Method(int x) {
     	// 省略定义  
    }
}
class DerivedClass : BaseClass {
 	new public int field1;  		// 显式屏蔽
    new static string field2;		// 静态变量也可以被屏蔽
    int field3;						// 隐式屏蔽了基类的 field3，但是会产生编译器警告
    new public void Method(int x) {
        // 省略定义
    }
}
```

虽然屏蔽了基类的成员，我们依然可以通过 `base` 关键字来访问他们。

```c#
class BaseClass {
 	public string field = "String in the base class";   
}
class DerivedClass : BaseClass {
 	new public string field = "String in the derived class";
    public void PrintFields() {
     	Console.WriteLine(field);		// 输出派生类中的 field
        Console.WriteLine(base.field);	// 输出基类的 field
    }
}
```

派生类可以以基类类型进行引用；派生类也可以直接转换为基类类型。

```c#
class BaseClass {
    public void Print() {
        Console.WriteLine("Base class");
    }
}
class DerivedClass : BaseClass {
    new public void Print() {
        Console.WriteLine("Derived class");
    }
}
class MyProgram {
    static void Main() {
        DerivedClass dc = new DerivedClass();
        dc.Print();					// 输出 "Base class"
        BaseClass bc = (DerivedClass) dc;
        bc.Print();					// 输出 "Derived class"
    }
}
```

**C++** 中指定成员或成员函数访问更加的灵活：

```cpp
struct BaseClass {
    void print() {
        std::cout << "Base class";
    }
};
class DerivedClass : BaseClass {
    void print() {
        std::cout << "Derived class";
    }
};
int main() {
    DerivedClass* dc = new DerivedClass();
    dc->print();				// 输出 "Base class"
    dc->BaseClass::print();		// 输出 "Derived class"
    BaseClass* bc = dc;			// 从派生类指针到基类指针不需要显式转换
    bc->print();				// 输出 "Base class"
    static_cast<DerivedClass*>(bc)->print();		// 输出 "Derived class"
    return 0;
}
```

可以看到 **C++** 在这这方面的处理是非常自由的，可以说只要有继承关系，想用哪个类的成员或成员函数就能用哪个。

### 虚函数和重写

类与继承的一个核心内容就是虚函数与函数重写。**C#** 采用了和 **C++** 类似的关键字 `virtual` 和 `override`（**C++11**），不过在 **C#** 中 `override` 是必须的。

```c#
class BaseClass {
 	virtual public void Print() {
        Console.WriteLine("Base class");
    }   
}
class DerivedClass : BaseClass {
    override public void Prinmt() {			// 重写了基类的方法
        Console.WriteLine("Derived class");
    }
}
class MyProgram {
    static void Main() {
        BaseClass bc = new DerivedClass();
        bc.Print();				// 输出 "Derived class"，可以看到虽然声明为基类，却有派生类的行为。
    }
}
```

当存在一连串的重写时，会调用进行重写的最深的派生类（实例的类型可能更深，但它不一定进行了重写）：

```c#
class BaseClass {
    virtual public void Print() {
        Console.WriteLine("Base class");
    }
}
class FirstDervied : BaseClass {
    override public void Print() {
        Console.WriteLine("First derived class");
    }
}
class SecondDerived : FirstDerived {
    new public void Print() {			// 这里没有重写，而是屏蔽了基类的方法
        Console.WriteLine("Second derived class");
    }
}
class MyProgram {
    static void Main() {
        BaseClass bc = new SecondDerived();
        bc.Print();				// 输出 "First derived class"
    }
}
```

上面我们的举例都是对方法的重写，而同为函数成员的如属性、索引器也可以进行重写，其行为和我们已经看到的类似。

### 构造函数初始化

前面我们提到，**C#** 的构造函数初始化列表用法和 **C++** 相似。除此之外，也可以在初始化列表中使用构造函数或者基类的构造函数：

```c#
class BaseClass {
    readonly int cval;
 	BaseClass() : cval(10) {}
}
class DerivedClass : BaseClass {
    readonly string cstr;
    public string item;
    public int count;
    DerivedClass() : base() {		// 调用了（直接）基类的构造函数
        cstr = "abcdefg";
    }
    public DerivedClass(string item_, int count_) : this() {	// 调用了 DerivedClass() 构造器
        item = item_;
        count = count_;
    }
}
```

这点从功能上是和 **C++** 别无二致，但是后者需要显式写出类的名称，不是很方便但是可能更加清晰。此外，在进行虚继承的时候，也不得不使用显式给出类名的写法，因此 **C++** 的设计上没有什么问题。

### 程序集和访问修饰

和 **C++** 不同的是，**C#** 允许对类使用访问修饰符，包括：

- `public`：该类对任何程序集可视
- `internal`：该类只对同一个程序集可视

对成员的访问修饰符则分为下面几种：

- `public`：该成员对任何程序集的任何类都可视
- `protected`：该成员对任何程序集的继承于该类的类可视
- `internal`：该成员对同一程序集的任何类都可视
- `protected internal`：该成员对同一程序集的任何类以及不同程序集的继承于该类的类可视
- `private`：该成员对任何其它类都不可视

下面是一个展示不同程序集间类和方法调用的示例：

```c#
// Base.cs
using System;
namespace BaseClassNS {
	public class BaseClass {
        public void Print() {
            Console.WriteLine("Base class");
        }
    }   
}
```

```c#
// Main.cs
using System;
using BaseClassNS;				// 通过 using 其它程序集类所在的命名空间来导入
namespace MyNamespace {
    class DerivedClass : BaseClass {
        // 省略类定义
    }
    class MyProgram {
        static void Main() {
            DerivedClass dc = new DerivedClass();
            dc.Print();			// 输出 Base class
        }
    }
}
```

### 抽象类

**C#** 中当然也有抽象类的概念，使用 `abstract` 关键字，其中通常会有抽象成员（如方法、属性、索引器等）。抽象类不能被实例化。抽象方法则是没有提供实现的方法。抽象属性是使用默认 `get` 与 `set` 的属性（但实际上是没有实现）。一个继承于抽象类的非抽象类必须实现前者的所有抽象方法，这个实现被视作重写。

```c#
abstract class AbstractBase {
    abstract public void Print();
}
class DerivedClass : AbstractBase {
    override public void Print() {
        Console.WriteLine("Derived class");
    }
}
class MyProgram {
    static void Main() {
        AbstractBase ab = new DerivedClass();	// 可以声明为抽象类类型
        ab.Print();
    }
}
```

如果和 **C++** 对比，抽象方法等价于 **C#** 的纯虚函数，抽象类等价于包含纯虚函数的类型（因此不需要显式将类声明为抽象）。个人更喜欢 **C++** 的设计，更加简洁且清晰。

```cpp
struct AbstractBase {
    virtual void print() = 0;			// 纯虚函数表示为 virtual = 0 个人非常喜欢
};
struct DerivedClass : AbstractBase {
    void print() override {				// C++ 中的 override 仅提示编译器进行检查，并不是必须的
        Console.WriteLine("Derived class");
    }
};
int main() {
    AbstractBase* ab = new DerivedClass();
    ab->print();
    delete ab;
    return 0;
}
```

### 其它特性

密封类是使用关键字 `sealed` 关键字声明的类，防止该类被继承。这一点和 **C++** 的 `final` 标记（**C++11**）是一致的。

```c#
sealed class FinalClass {
    // 省略类定义
}
```

静态类是一系列静态成员的集合；这些成员可以像普通类里的静态成员一样被引用。

```c#
static class StaticClass {
    public const float PI = 3.14f;
    public static int add(int x, int y) {
        return x + y;
    }
}
```

静态类常用于扩展方法。扩展方法是指将静态类中的类当作其它类的成员一样调用：

```c#
class Point {
    public float x { get; set; }
    public float y { get; set; }
    public Point(float x_, float y_) : x(x_), y(y_) {}
}
static class PointExtension {
    public static float distance(this Point p) {	// 使用 this 关键字
        return Math.sqrt(p.x * p.x + p.y * p.y);
    }
}
class MyProgram {
    static void Main() {
        Point p = new Point(1.0f, 2.0f);
        Console.WriteLine("Distance from the origin: {p.distance()}");
    }
}
```

**C++** 并没有静态类的概念，但是常规类可以达到相同的效果；扩展方法这个特性则并不支持，但个人还挺喜欢这个语法糖的。

## 表达式和运算符

这个部分和 **C++** 基本上没有区别。这里就展开说明一下运算符重载。**C#** 的运算符重载不包括赋值运算符，且一定是静态方法：

```c#
class Pair {
    public float x { get; set; }
    public float y { get; set; }
    public Pair(float x_, float y_) : x(x_), y(y_) {}
    public static Pair operator -(Pair p) {
        return Pair(-p.x, -p.y);
    }
    public static Pair operator +(Pair p1, Pair p2) {
        return Pair(p1.x + p2.x, p1.y + p2.y);
    }
}
```

**C++** 中的 `friend` 运算符重载与这个的形式比较类似，不过其所在定义域比较模糊，不如 `static` 方法来的清楚。此外，**C++** 支持赋值运算符的重载并强制其定义为成员函数，这也是和类的“三大件”中拷贝赋值（即赋值运算符的重载）的一个拓展。不可否认的是 **C++** 有更为丰富的重载支持，尤其是对于函数调用符 `()` 的重载非常好用。

**C#** 中有一个 `typeof` 运算符可以得到一个 `System.Type` 的对象，这个类型提供了一些方法能够在运行时得到某个类型的信息。

```c#
using System.Reflection
class MyClass {
    public int field;
    public int Mehod(int x) {
        return x;
    }
}
class MyProgram {
    static void Main() {
        Type t = typeof(MyClass);
        FieldInfo[] fields = t.GetFields();		// 得到字段的信息数组
        MethodInfo[] methods = t.GetMethods();	// 得到方法的信息数组
        foreach (FieldInfo f in fields) {
            Console.WriteLine($"Field: {f.name}");
        }
        foreach (FieldInfo m in methods) {
            Console.WriteLine($"Method: {m.name}");
        }
    }
}
```

**C++** 不支持反射（手动微笑），从某种角度讲让 **C++** 支持反射那真的是一项恢弘的任务（标准委员会：已经新建文件夹了）。`typeid` 运算符倒是能够提供一点运行时的类型信息，但是功能非常有限。

## 语句

这个部分和 **C++** 也是大同小异，值得一提的是 `switch` 语句。在 **C# 7.0** 之后，可以利用 `switch` 语句匹配任意类型的对象（之前则和 **C++** 一致，只能匹配整型，即整数、布尔值、枚举等）。此外，所有的 `case` 语句必须 `break` 等跳转语句退出（即不允许 **穿过（Fallthrough）**），否则会报错。这一点有助于避免受到继承自 **C** 的穿过规则的荼毒，但是既然强制退出那为什么不直接默认不穿过呢？虽然不支持穿过，但是依然可以通过罗列 `case` 来让多种模式匹配同一段代码。

此外，也可以在 `switch` 语句中使用 `goto case Pattern` 的方式跳转到当前的 `switch` 语句中的某个标签处。

**C#** 也提供了用于捕捉异常的 `using` 语句：

```c#
using System;
using System.IO;
class MyProgram {
    static void Main() {
        using (TextWriter file = File.CreateText("data.txt")) {	// 文件操作可能会遇到错误
            file.WriteLine("Some text...");
        }
        using (TextReader file = File.OpenText("data.txt")) {
            string line;
            while ((line = file.ReadLine()) != null) {
                Console.WriteLine(line);
            }
        }
    }
}
```

`using` 语句本质是 `try` 语句的一个语法糖。在资源分配后，如果语句块内出现了任何异常，都会自动将资源释放。如果存在多个需要管理的同类型资源，可以在 `using` 语句中用逗号分隔多个变量的初始化语句，即 `ResrcType re1 = expr1, re2 = expr2`。

**C++** 没有这样的语法糖。

## 结构

**C#** 提供了类似于 **C** 语言中结构体的概念，但引入了访问修饰符、构造函数以及属性。比较特殊的是，结构不能定义无参数的构造函数以及析构函数，也不能初始化任何字段或属性。此外，结构一定是密封的，所以结构不能有继承关系（也因此，许多修饰符在结构中都没有意义，也不允许使用，比如 `protected`、`virtual` 等）。

好玩的是，所有结构都继承自一个类 `System.ValueType`，因此可以在结构中使用 `override` 关键字来重写这个类中的方法。

最后，虽然实际上是在栈上分配的，在初始化语句还是需要 `new` 运算符。

```c#
struct Point {
 	public int X;
    public int Y;
    public Point(int x, int y) {
        X = x;
        Y = y;
    }
}
class MyProgram {
    static void Main() {
        Point p = new Point(1, 2);
        Console.WriteLine($"p.x = {p.x}, p.y = {p.y}");
    }
}
```

**C++** 中因为类本身可以在栈上分配，因此没必要创建结构这样的概念。事实上 **C++** 中也有 `struct` 这个关键字，但是它和 `class` 唯一的区别就是前者的所有成员和成员函数默认为 `public`，而后者是 `private`。

## 枚举

**C#** 的枚举和 **C++** 基本一致，只是前者直接就有作用域，相当于 **C++** 的 `enum class`。此外可以使用 `using static` 将枚举中的元素引入到当前作用域。**C++** 直到 **C++23** 才引入 `using enum` 这个简直相当便利的语法。

```c#
enum Colors {
    Red, Blue, Green
}
using static Colors.Red;
class MyProgram {
    static void Main() {
        Console.WriteLine($"Red = {Red}");	// 输出 "Red = 0"
    }
}
```

## 数组

**C#** 的数组都是静态的，即其大小在创建时就已经确定。不过和 **C++** 不同，一维数组的类型是 `ElemType[]` 而非 **C++** 中的 `ElemType[N]`（个人感觉后者更为严谨）。至于多维数组，**C#** 的矩形数组和 **C++** 的（静态）多维数组一致，但是其类型是 `ElemType[,]`（二维数组）及 `ElemType[,,]`（三维数组）等，而非 `ElemType[M][N]` 及 `ElemTyp[L][M][N]` 等。

**C#** 中的一维数组和矩形数组在进行初始化时可以省略 `new` 的部分：

```csharp
class MyProgram {
    static void Main() {
        int[] arr = new int[3] { 1, 2, 3 };				// 一维数组，这里的 new int[3] 可以省略
        int[,] mat = new int[3,2] { {1, 2}, {3, 4}, {5, 6} };	// 二维矩形数组，同样可以省略 new int[3,2]
    }
}
```

交错数组则可以看作是指针的数组，指针指向的数组大小可以是任意的（只要它们的秩相同），但当然也可以指向特定的数组类型，比如 `new int[3][]` 就是存储 `int[]` 类型的交错数组（其长度为 3），`new int[][,]` 就是存储 `int[,]` 类型的交错数组：

```csharp
class MyProgram {
    static void Main() {
        int[][,] arr = new int[3][,];
        arr[0] = { {1, 2}, {3, 4} };			// arr[0] 是一个 new int[2,2]
        arr[1] = { {2, 3}, {4, 5}, {6, 7} };	// arr[1] 是一个 new int[3,2]
        arr[2] = { {8, 9} };					// arr[2] 是一个 new int[1,2]
    }
}
```

可以看到，交错数组并不要求数组中元素的完全一致（从 **C#** 角度看，它们确实是一个类型，只不过可以实例化为不同大小的数组）。在 **C++** 中，这实际上就是一个指针的数组，或多重指针：

```cpp
int main() {
    int arr1[3][] = { {1, 2}, {3, 4}, {5, 6} };	// 这依然是一个静态数组，左侧的 int[3][] 只是 int[3][2] 的简略写法
    int* arr2[3] = { new int[2] {1, 2}, new int[3] {3, 4, 5}, new int[1] {6} };
    	// 一个动态数组（指针）的静态数组。C++ 支持一句代码初始化这样的数组
    for (int i = 0; i < 3; ++i) {
        delete arr2[i];			// 不过并不支持一句代码释放资源
    }
    int** arr3 = new int*[2] { new int[3] {1, 2, 3}, new int[2] {4, 5} };
    	// 一个动态数组的动态数组。依然可以一句话初始化这样的数组。
    for (int i = 0; i < 2; ++i) {
        delete arr3[i];
    }
    delete arr3;
    return 0;
}
```

**C#** 提供了 `foreach` 语句专门用来遍历数组，它能够依次产生每个元素的引用。

```csharp
class MyCLass {
    public int Field = 0;
}
class MyProgram {
    MyClass[] mcArr = new MyClass[3];
    int[] iArr = new int[3];
    foreach (MyClass mc in mcArr) {		// mc 是一个迭代变量，代表每次遍历到的元素
        ++mc.Field;			// 可以通过遍历访问或修改每个元素的成员
    }
    foreach (int i in iArr) {
        ++i;				// 编译错误。这里其实我不是很理解为什么要强制成编译错误
    }
}
```

可以看到，值类型不能通过 `foreach` 语句修改，但是引用类型可以。在 **C++** 里面不太好找到等价的机制。`range for` 不能对内置数组使用，而是针对实现了 `begin()`、`end()` 以及 `operator ++()` 的类型使用。从某种角度讲，**C++** 的内置数组并不受待见，通常不被当作默认的线性存储结构（相比之下 `std::vector<>` 更通用），因此 **C++** 比起专门对数组设计一个 `foreach` 语法，它提供了更加通用抽象的，可以对任意可遍历结构进行遍历的 `range for`。而且，后者可以按照需求，将迭代变量声明为引用、常量或常引用。

```cpp
int main() {
    std::vector<int> v = { 1, 2 ,3 };
    for (int i : v) {			// 以值方式得到迭代变量，相当于每次拷贝一个元素
        std::cout << i << ' ';
    }
    for (int& i : v) {			// 以引用方式得到迭代变量，因此可以修改元素
        ++i;
    }
}
```

值得一提的是，**C#** 的数组类型继承自 `System.Array`，所以它本质上也是一个类。我们可以使用 `Clone()` 方法深拷贝一个数组。不过返回的类型是 `object[]`，必须要显式转换为需要的数组类型。

最后，数组中的任何一个元素都能通过引用方式从函数返回：

```csharp
class MyProgram {
    public static ref MaxRef(int[] nums) {
        int max_idx = 0;
        int max = nums[0];
        for (int i = 0; i < nums.Length; ++i) {
            if (nums[i] > max) {
                max_idx = i;
                max = nums[i];
            }
        }
        return ref nums[i];
    }
    static void Main() {
        int[] scores = { 30, 75, 24, 80, 40 };
        ref int max_score = ref MaxRef(scores);
        max_score += 10;
        Console.WriteLine($"Max is now: {scores[3]}");	// 输出 "Max is now: 90"
    }
}
```

回顾之前我们对 `ref` 和 **C++** 中指针的对应性，这样的特性是理所应当存在的。

## 委托

**C#** 的委托概念比较特殊，让我们先用一个例子展示：

```csharp
delegate void PrintDel(int val);
class MyProgram {
    void PrintTwice(int val) {
        Console.WriteLine("{0}, {0}", val);
    }
    void PrintThreeTimes(int val) {
        Console.WriteLine("{0}, {0}, {0}", val);
    }
    static void Main() {
        MyProgram program = new MyProgram();
        PrintDel print;
        int val = 10;
        print = new PrintDel(program.PrintTwice);	// print 委托赋值为 printTwice
        print(val);			// 输出 10, 10
        print = new PrintDel(program.PrintThreeTimes);
        print(val);			// 输出 10, 10, 10
    }
}
```

这个和 **C++** 的函数指针类型类似。

```cpp
using PrintType = void (*)(int);		// using 语句，右侧是一个函数指针的类型
typedef int (*AddType)(int a, int b);	// typedef 语句，形式上更加类似 C# 的委托

void print_int(int val) {
    std::cout << val;
}
int add_int(int a, int b) {
    return a + b;
}
int main() {
    PrintType print = &print_int;		// 可以省略这里的取地址运算符，因为 C++ 中函数会退化为函数指针
    print(10);				// 输出 10。C++ 中函数指针可以自动被解引用
    AddType add = &add_int;
    print(add(1, 2));		// 输出 3
    return 0;
}
```

可以看到，委托和函数指针基本上一致。不过上面举的例子都是普通的函数。如果是成员函数，指针的类型会变得异常复杂：

```cpp
struct MyClass {
    int my_method(int a) {
        return a;
    }
    MyClass* clone(MyClass* other) {
        return other;
    }
};
using MethodType = int (MyClass::*)(int);		// 需要显式给出成员函数所在类
typedef MyClass* (MyClass::* CloneType)(MyClass* other);

int main() {
    MyClass mc;
    MethodType method = &MyClass::my_method;
    std::cout << (mc.*method)(10);		// 需要额外注意的是，.* 运算符优先级要低于 operator ()，所以需要括号扩起来
    CloneType clone = &MyClass::clone;
    std::cout << (mc == (mc.*clone)(mc));
    return 0;
}
```

相比之下，**C#** 的委托用起来要更轻松一些（不用区分静态方法和普通方法），也没有引入什么奇怪的运算符。

**C#** 支持委托的组合，也即以一定顺序执行一系列相同类型的方法。这些方法的参数都是调用时提供的；但是对于 `ref` 类型参数，这个引用会一步步向下传递，因此可能每次调用的参数值（解引用）都是不一样的：

```csharp
delegate void PrintDel();
class MyClass {
    public void Print1() {
        Console.WriteLine("Print1");
    }
    public static void Print2() {
        Console.WriteLine("Print2");
    }
}
class MyProgram {
    static void Main() {
        MyClass mc = new MyClass();
        PrintDel print;
        print = mc.Print1;
        print += MyClass.Print2;
        print += mc.Print1;
        if (print != null) {
            print();		// 输出 Print1Print2Print1
        }
    }
}
```

委托组合并不在意其中函数的返回值；只有最后一个返回值会被采用并作为最后的返回值。这么来看，委托是一种相当松散的函数结构。

### 匿名函数

可以利用 `delegate` 关键字创造匿名函数：

```csharp
delegate int SomeDel(int x);
class MyProgram {
 	static void Main() {
     	SomeDel del1 = delegate (int x) { return x + 1; };
        SomeDel del2 = delegate { return 42; };		// 参数列表中没有 out 类型且没有使用任何参数时，可以省略参数列表
        int x = del1(1);		// x = 2
        int y = del2(1000);		// y = 42
    }
}
```

挺奇怪的是，参数数组在匿名函数中需要省略 `params` 关键字。这样的话怎么区分数组类型和参数数组？

此外，匿名函数可以自然地捕获外部的变量，并且延伸其生命周期（**C++** 馋哭了）。

```csharp
delegate int Del();
class MyProgram {
    static void Main() {
        Del del;
        {
            int x = 10;
            del = delegate { return x; };		// 捕获了局部变量 x
        }
        Console.WriteLine($"x = {del()}");		// 输出 10
    }
}
```

在 **C# 3.0** 之后，可以使用 `lambda` 表达式，这是比匿名函数更加简洁的形式：

```csharp
delegate int Del(int);
class MyProgram {
    static void Main() {
    	Del del1 = x => x + 1;	// 等价于 delegate (int x) { return x + 1; }
        Del del2 = x => 2 * x;	// 等价于 delegate (int x) { return 2 * x; }
    }
}
```

**C++11** 之后也引入了 `lambda` 表达式，但是不是很简洁：

```cpp
int main() {
    auto lambda1 = [] (auto x) { return x + 1; };
    auto lambda2 = [] (auto x) { return 2 * x; };
 	return 0;   
}
```

**C++** 中 `lambda` 最令人头疼的还是捕获局部变量造成的内存泄漏：

```cpp
auto return_lambda() {
 	int a = 0, b = 0;
    return [&a, &b] () { ++a; ++b; return a + b; };
}
int main() {
    std::cout << return_lambda();	// 当场内存泄露，这是因为 lambda 捕获了局部变量的引用，在函数返回后就失效了
    return 0;
}
```

在 **C#** 中就不会有这样的疑虑。

最后，所有委托类型都继承自 `System.Delegate`。

## 事件

事件机制是 **C#** 内置的一个特性，它能够实现 **发布者/订阅者模式（Publisher/Subscriber Pattern）**，这个模式用于让一些类在指定情形下通知其它类进行特定操作。发布者定义了一些 **事件（Event）**，然后订阅者向发布者提供一个 **回调方法（Callback Function）** 以在事件发生时得到通知。下面是对这种模式分项的详细介绍：

- 委托类型声明：我们需要为事件和事件处理方法定义一个公有的委托
- **事件处理方法（Event Handler）**声明：订阅者类在事件发生后会执行的方法声明，可以是一个匿名函数
- 事件声明：发布者类要声明一个订阅者类可以注册的事件成员。如果这个事件是 `public` 的，我们称发布了事件。
- 事件注册：订阅者必须要注册事件才能在事件被触发时得到通知。
- 触发事件：发布者类中触发事件，然后所有注册的订阅者调用对应的事件处理方法。

下面是一个例子：

```csharp
delegate void Handler();		// 委托类型声明
class Incrementer {				// 发布者
	public event Handler CountedADozen;	// 事件声明
    public void DoCount() {
        for (int i = 1; i < 100; ++i) {
            if (i % 12 == 0 && CountedADozen != null) {
                CountedADozen();	// 每 12 次计数就触发一次事件
            }
        }
    }
}
class Dozens {						// 订阅者
    public int DozensCount { get; private set; }
    public Dozens(Incrementer inc) {
        DozensCount = 0;
        inc.CountedADozen += IncrementDozensCount;	// 事件注册
    }
    void IncrementDozensCount() {	// 事件处理方法声明
        ++DozensCount;
    }
}
class MyProgram {
    static void Main() {
        Incrementer inc = new Incrementer();
        Dozens dozensCounter = new Dozens(inc);
        inc.DoCount();
        Console.WriteLine("Number of dozens = {0}", dozensCounter.DozenCount);
    }
}
```

**GUI** 编程中会大量频繁地使用事件。**.NET** 提供了一个标准事件模式，即使用 `EventHandler` 作为委托类型，其接口是 `public delegate void EventHandler(object sender, EventArgs e)`。其中 `EventArgs` 可以用来传递数据，不过需要声明一个派生自 `EventArgs` 的类型。我们下面将按照标准事件模式来重新实现之前的例子：

```csharp
public class IncrementerEventArgs : EventArgs {		// 自定义的事件参数类型
    public int IterationCount { get; set; }
}
class Incrementer {
    public event EventHandler<IncrementerEventArgs> CountedADozen;
    public void DoCount() {
        IncrementerEventArgs args = new IncrementerEventArgs();
        for (int i = 1; i < 100; ++i) {
            if (i % 12 == 0 && CountedADozen != null) {
                args.IterationCount = i;
                CountedADozen(this, args);
            }
        }
    }
}
class Dozens {
    public int DozensCount { get; private set; }
    public Dozens(Incrementer inc) {
        DozensCount = 0;
        inc.CountedADozen += IncrementDozensCount;
    }
    void IncrementDozensCount(object source, IncrementerEventArgs e) {
        Console.WriteLine($"Incremented at itertion: {e.IterationCount} in {source.ToString()}");
        ++DozensCount;
    }
}
class MyProgram {
    static void Main() {
        Incrementer inc = new Incrementer();
        Dozens dozensCounter = new Dozens(inc);
        inc.DoCount();
        Console.WriteLine($"Number of dozens = {dozensCounter.DozensCount}");
    }
}
```

**C++** 不存在原生的事件机制。

## 接口

接口是一个声明却不实现任何方法的一个结构，可以理解为一个仅由抽象方法（及抽象属性、事件、索引器）组成的抽象类。因此，所有继承接口的类型必须实现接口中所有的方法。接口的示例如下：

```csharp
// 接口的所有成员都是 public 的，且不能被任何修饰符修饰
interface IMyInterface {
 	int SomeMethod(int a);
    string OtherMethod();
}
class MyClass : IMyInterface {
    // 实现接口的方法不使用 override 关键字
    int SomeMethod(int a) {
        return a;
    }
    string OtherMethod() {
        return "MyClass";
    }
}
class MyProgram {
 	static void Main() {
     	MyClass mc = new MyClass();
        int a = mc.SomeMethod(10);
        IMyInterface imi = (IMyInterface) mc;	// 就像抽象类一样，可以从实现类转换为接口类型
        string str = imi.OtherMethod();			// 接口调用的方法会引导至具体类的实现。和抽象类一致
    }
}
```

一个类可以实现多个接口（但不能继承多个类）；当实现的多个接口有完全相同的方法时，可以只提供一个实现，也可以为不同接口提供不同的实现（称为显式接口成员实现）：

```csharp
interface IMyInterface1 {
    void Method();
}
interface IMyInterface2 {
    void Method();
}
class MyClass : IMyInteface1, IMyInterface2 {
 	void IMyInterface1.Method() {
     	// 为 IMyInterface1 实现 Method 方法   
    }
    void IMyInterface2.Method() {
     	// 为 IMyInterface2 实现 Method 方法   
    }
    void AnotherMethod() {
     	((IMyInterface1) this).Method();	// 此时必须转换为接口类型才能调用显式接口成员实现的方法   
    }
}
```

**C++** 并没有接口的概念，但是抽象类（含有纯虚函数的类）可以做到同样的效果（即强制子类实现纯虚函数）；同时，**C++** 原生支持多继承类，因此可以几乎覆盖接口的所有功能。不过，显式接口成员实现并没有 **C#** 这样方便的写法；有两种设计能够实现一样的效果：

```cpp
class Interface1 {
public:
  	virtual void method() = 0;  
};
class Interface2 {
public:
    virtual void method() = 0;
};
class Interface1Impl : public Interface1 {
public:
 	void method() override {
     	std::cout << "Impl 1";  
    }
};
class Interface2Impl : public Interface2 {
public:
 	void method() override {
     	std::cout << "Impl 2";
    }
};
class MyClass : public Interface1Impl, public Interface2Impl {
public:
 	void another_method() {
     	Interface2Impl::method();   // 不需要类型转换也可以调用
    }
};
int main() {
    MyClass mc;
    mc.Interface1Impl::method();	// 调用 Interface1Impl 的实现
    mc.Interface2Impl::method();	// 调用 Interface2Impl 的实现
    Interface1* i1 = new MyClass();
    i1->method();					// 调用 Interface1Impl 实现
    return 0;
}
```

或者使用零运行开销的 **CRTP**：

```cpp
class Interface1 {
public:
  	virtual void method() = 0;  
};
class Interface2 {
public:
    virtual void method() = 0;
};
template<class Derived>
class Interface1Impl : public A {
public:
    void method() override {
        static_cast<Derived*>(this)->method_A();	// 转换为子类只需要 static_cast
    }
};
template<class Derived>
class Interface2Impl : public B {
public:
    void method() override {
        static_cast<Derived*>(this)->method_B();
    }
};
class MyClass : Interface1Impl<MyClass>, Interface2Impl<MyClass> {
public:
    void method_A() {
     	std::cout << "Impl 1";   
    }
    void method_B() {
        std::cout << "Impl 2";
    }
};
```

**C++** 强大的构造能力不容小觑，但是 **C#** 的语法糖也着实方便。

## 转换

**C#** 使用 **C** 风格的强制类型转换风格，即 `(Type)` 作为类型转换运算符，可以将某个对象转换为另一个类型。对于一些类型间的转换，并不需要显式写出类型转换运算符，我们称这种类型转换为隐式类型转换。内置的数值类型间的转换可以参考下面的准则：

- 整数类型中，位数低的类型可以隐式转换为位数高的类型（比如 `char` 到 `long`），但有符号类型到无符号类型除外
- 整数类型可以隐式转换为浮点类型
- 浮点类型中 `float` 可以隐式转换为 `double`

引用类型之间，派生类转换到基类（或者实现的接口）是隐式转换；特别地，所有引用类型都可以隐式转换为 `object` 类型。值类型也可以隐式转换为 `object` 类型（或接口），不过其中要经历内存分配和复制，这个过程我们称为 **装箱（Boxing）**。

```csharp
class MyProgram {
    static void Main() {
    	int i = 10;
        object oi = i;		// oi 是一个 object 类型的变量，其指向堆上值为 10 的一段内存
    }
}
```

上面我们没有提到的转换，都可以认为是显式转换，其通常有下面的准则：

- 整数类型间，大整数类型转化为小整数类型时，高位会被截掉
- 浮点数转换为整数时，小数部分会被截掉
- `double` 转换为 `float` 时，会视精度约化为 `0` 或正负无穷大
- 一个引用类型只能成功转换为它的派生类（或基类，此时是隐式转换）
- 一个装箱后的类型可以进行 **拆箱（Unboxing）**，变为值类型。

可以自定义类型转换：

```csharp
class Person {
 	public string name { get; set; }
    public int age { get; set; }
    public Person(string name_, int age_) : name(name_), age(age_) {}
    // 将当前类型隐式转换为其它类型
    public static implicit operator string(Person p) {
        return p.name;
    }
    // 将当前类型显式转换为其它类型
    public static explicit operator int(Person p) {
        return p.age;
    }
    // 将其它类型隐式转换为当前类型
    public static implicit operator Person(string name) {
     	return new Person(name, 0);   
    }
}
class MyProgram {
	static void Main() {
     	Person p = new Person("Tom", 22);
        string name = p;
        int age = (int) p;
        Person newPerson = "Mary";
    }
}
```

需要注意的是，虽然可以自定义类型转换，能够隐式进行转换的最多只能三层，即标准转换、自定义转换、标准转换。超过三步的转换并不能自动进行。

如果需要判断一个类型是否能够转换为另一个类型，可以使用 `is` 运算符，但这个运算符只适用于引用类型间的转换以及装箱拆箱转换，值类型间的转换和用户自定义转换是不行的。

除了强制类型转换运算符外，也可以使用 `as` 运算符。不过转换失败时，`as` 会返回 `null`。

## 范型

**范型（Generic）** 是一种允许同一段代码适应多种类型的机制；范型的代码将类型参数化，在创建类实例时才会代入真实的类型。**C#** 中允许将类、结构、接口、委托和方法定义为范型。

```csharp
class MyClass<T1, T2> {		// 尖括号中罗列的就是范型参数，它可以根据实例生成不同的类定义
	public T1 field1;
    public T2 field2;
}
class MyProgram {
 	static void Main() {
        // 创造了 MyClass<int, string> 这个类实例，然后创造了实例对象
		MyClass<int, string> mc1 = new MyClass<int, string>();
        var mc2 = new MyClass<float, long>();
    }
}
```

不过，范型类的定义中，并不是所有行为都是被默许的，比如下面这个例子：

```csharp
class MyClass<T> {
 	static public bool LessThan(T t1, T t2) {
        return t1 < t2;			// 不能编译通过，因为不是所有类型都实现了小于运算符
    }
}
```

所以 **C#** 引入了 **约束（Constraint）** 的概念来规定范型参数需要满足的条件，这是通过 `where` 子句给出的：

```csharp
class MyClass<T> where T: IComparable {
    static public bool LessThan(T t1, T t2) {
        return t1 < t2;			// 现在就没有问题了
    }
}
```

`where` 子句后面可以接三种约束，它们的顺序也有一定的要求：

- 主约束：可以是类名（限定某个类型）、`class` （限制为引用类型） 或 `struct` （限制为值类型）。主约束必须出现在 `where` 子句最前面，且至多只能有一个
- 次约束：接口，可以有多个
- 构造函数约束：即 `new()`，要求带有无参 `public` 构造函数的类型

```csharp
class MyClass<T1, T2>
    where T1: class, new()	// 第一组约束，有两个约束
    where T2: IComparable {	// 第二组约束，只有一个约束
	// 省略实现        
}
```

范型方法和范型类语法相似，都是在名称后用尖括号给出范型参数，然后在大括号（方法体）前给出 `where` 子句。不过在调用范型方法时不一定需要给出范型实参，因为可以直接通过参数来推断出范型参数。

```csharp
class MyProgram {
 	void method<T1, T2>(T1 t1, T2 t2) where T1 : IEnumerable {
        // 省略实现
    }
    static void Main() {
     	int[] arr = { 1, 2, 3 };
        method(arr, 1.0f);			// 调用了 method<int[], float>
    }
}
```

最后让我们介绍 **C#** 中 **可变性（Variance）** 的概念，其有关于范型委托和范型接口中类型的匹配。

- **协变（Covariance）**：允许在期望传入派生类的模版参数处传入基类对象
- **逆变（Contravariance）**：允许在期望传入基类的模版参数处传入派生类对象
- **不变（Invariance）**：其余的情形都是不变

```csharp
class BaseClass { }
class DerivedClass : BaseClass { }
delegate T SimpleDel<T>();
delegate T CovarianceDel<out T>();
delegate T ContravarianceDel<in T>();
class MyClass {
    static BaseClass BaseMaker() {
        return new BaseClass();
    }
    static DerivedClass DerivedMaker() {
        return new DerivedClass();
    }
    static void Main() {
        SimpleDel<BaseClass> makeBase = baseMaker;
        SimpleDel<DerivedClass> makeDerived = derivedMaker;
        SimpleDel<BaseClass> makeBase_ = derivedMaker;	// 错误，不能讲派生类实例的范型委托转化为基类的范型委托
        SimpleDel<DerivedClass> makeDerived_ = baseMaker; // 错误，不能将基类实例的范型委托转化为派生类实例的范型委托
        
        CovarianceDel<DerivedClass> makeDerived2 = derivedMaker;
        CovarianceDel<BaseClass> makeBase2 = makeDerived2;
        
        ContravarianceDel<BaseClass> makeBase3 = derivedMaker;
        ContravarianceDel<DerivedClass> makeDerived3 = makeBased3;
    }
}
```

**C++** 中的模版和 **C#** 的范型原理类似，不过在实例化之前，编译器不会对模版代码进行任何检查；对实例化类型的约束在 **C++20** 才得到真正支持（此前只能使用 **SFINAE** 特性或者 `static_assert` 模拟约束，远不如 **C#** 的 `where` 子句方便）。但是 **C++** 模版的表现力非常强，模版参数不仅可以是类型，还可以是模版类型或非类型的常量；模版可以进行偏特化，也即部分实例化，因而针对不同实例类型实现不同行为；加上模版实例化是在编译器进行的，这能极度丰富 **C++** 在编译时的功能。

## 枚举器和迭代器

之前我们介绍了 `foreach` 语句，它能够针对数组类型进行遍历。事实上，所有实现了 `IEnumerable` 接口的类型（称为可枚举类）都能够通过 `foreach` 语句遍历。可枚举类可以通过 `GetEnumerator` 方法返回一个枚举类型（是一个实现了 `IEnumerator` 接口的类型），我们称为枚举器。枚举器有三个核心方法（属性）：

- `Current`：是一个只读属性，返回当前的位置，是一个 `object` 对象的引用
- `MoveNext`：让枚举器位置移动到下一项，并返回一个布尔值以表明下一项是否是有效位置
- `Reset`：将枚举器重置到原始状态，初始状态总是第一个元素之前的位置

下面我们通过数组的例子来展示枚举器的使用：

```csharp
class MyProgram {
 	static void Main() {
     	int[] arr = { 1, 2, 3 };
        IEnumerator ie = arr.GetEnumerator();
        while (ie.MoveNext()) {
         	int item = (int) ie.Current;	// 拆箱操作需要显式类型转换
            Console.WriteLine($"item = {item}");
        }
    }
}
```

如果需要自定义类型也能使用 `foreach` 语句，就要实现 `IEnumerable` 接口，通常也要同时实现一个枚举器：

```csharp
using System;
using System.Collections;

// 自定义的枚举器
class ColorEnumerator : IEnumerator {
    string[] colors;
    int position = -1;
    public ColorEnumerator(string[] colors_) {
     	colors = new string[colors_.Length];
        for (int i = 0; i < colors_.Length; ++i) {
            colors[i] = colors_[i];
        }
    }
 	public object Current {
        get {
            if (position == -1 || position >= colors.Length) {
             	throw new InvalidOperationException();   
            }
            return colors[position];
        }
    }
    public bool MoveNext() {
     	if (position < colors.Length - 1) {
         	++position;
            return true;
        }
        return false;
    }
    public void Reset() {
        position = -1;
    }
}
// 自定义的可枚举类
class Spectrum : IEnumerable {
 	string[] colors = { "red", "orange", "yellow", "green", "cyan", "blue", "violet" };
    public IEnumerator GetEnumerator() {
     	return new ColorEnumerator(colors);   
    }
}
class MyProgram {
 	static void Main() {
     	Spectrum spectrum = new Spectrum();
        foreach (string color in spectrum) {
         	Console.WriteLine(color);   
        }
    }
}
```

也可以使用范型接口 `IEnumerable<T>` 和 `IEnumerator<T>`，这样就不省略了（和 `object` 间的）类型转换了。

**C#** 提供了一个能够简单创建枚举器和枚举类型的方式，是一种被称为 **迭代器（Iterator）** 的结构，其是一个块结构，会作为方法、访问器或运算符的主体出现；其中的代码中可能出现两种特殊语句：`yield return` 语句指定了序列中要返回的下一项，而 `yield break` 语句指定了序列的结束。

```csharp
class MyClass {
	public IEnumerator<string> GetEnumerator() {
        return BlackAndWhite();
    }   
    public IEnumerator<string> BlackAndWhite() {	// 迭代器，指定了元素的返回顺序，返回值是一个枚举器
        yield return "black";
        yield return "gray";
        yield return "white";
    }
}
class MyProgram {
    static void Main() {
        MyClass mc = new Myclass();
        foreach (string shade in mc) {
            Console.WriteLine(shade);
        }
    }
}
```

也可以让迭代器返回一个可枚举类（比如上例就是 `IEnumerable<string>`）不过此例中没有必要。如果需要用多种方式遍历某个类型，可以这样做：

```csharp
using System;
using System.Collections.Generic;	// 迭代器在这里定义
class Spectrum {
    string[] colors = { "red", "orange", "yellow", "green", "cyan", "blue", "violet" };
    public IEnumerable<string> UVtoIR() {
        for (int i = colors.Length - 1; i >= 0; --i) {
            yield return colors[i];
        }
    }
    public IEnumerable<string> IRtoUV() {
        for (int i = 0; i < colors.Length; ++i) {
            yield return colors[i];
        }
    }
}
class Program {
    static void Main() {
        Spectrum spectrum = new Spectrum();
        foreach (string color in spectrum.UVtoIR()) {
            Console.WriteLine($"{color}");
        }
        foreach (string color in spectrum.IRtoUV()) {
            Console.WriteLine($"{color}");
        }
    }
}
```

上例中的方法可以改成属性，显得更为简洁一些。

最后需要注意的是，迭代器默认实现的枚举器并不包括 `Reset` 方法，如果调用会产生异常。

## LINQ

**LINQ（Language Integrated Query）** 是 **C#** 内置的一个查询语言子集，可以方便地使用类似数据库的语句。

```csharp
class MyProgram {
	static void Main() {
        int[] numbers = { 1, 3, 5, 7, 9 };
        IEnumerable<int> nums =
            from n in numbers
            where n < 10
            select n;
        foreach(int x in nums) {
            Console.WriteLine($"{x}");
        }
    }
}
```

