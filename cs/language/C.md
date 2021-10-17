# C 学习

[TOC]

## **C** 语言的历史和简介

**C** 语言是一个古老却依然耀眼的语言，直至今日依然在系统编程领域占据统治地位。

它诞生于 20 世纪 70 年代，早起的 **C** 为了方便编译器的实现引入了不少（或许）具有争议的特性，比如数组下标从 0 开始（最早可能追溯到 **BCPL**）、数组与指针的近乎等同、`register` 关键字等。其中也有一些影响到后来出现的语言，尤其是 **C++**。可以说，从一开始，**C** 语言就在避免一些复杂的语言特性，让其编译器便于编写的同时也让 **C** 非常贴近于汇编语言。

70 年代中期，**C** 语言经典作品 *The C Programming Language* 出版并广受欢迎。我们将这本书里描述的 **C** 语言标准以其两位作者的首字母命名，即 **K&R C**。其中包含了大部分我们熟悉的 **C** 语言特性，不过与后来的 **C** 语言标准仍有区别，我们后续会介绍。

80 年代，**C** 被广泛运用，但因为缺少标准，不同的编译器实现的特性都有所差异。为了确保 **C** 不变为一个松散的语族，**ANSI** 为 **C** 制定了一个标准，并在后来被 **ISO** 采纳。这就是我们熟知的 **ANSI C**。在这个标准中，引入了一系列有趣的术语，我们可能会在不少其它语言的标准中看到这样的描述：

- **不可移植的代码（Unportable Code）**：其中可能包括 **由实现定义的（Implementation-Defined）**，即编译器设计者所决定的，在不同编译器中的不同行为，但它们都是正确的行为；**未申明的（Unspecified）**，即标准中没有规定正确的行为，比如参数的求值顺序。
- **坏代码（Bad Code）**：即 **未定义的（Undefined）**，因为没有按照正确方式编写，程序可能采取任何可能的行动。通常，在标准中可能会给出一系列 **约束条件（Constraint）**，在没有遵守其中的要求时，程序的行为就是未定义的，比如 `%` 运算符需要操作数均为整型。在违反约束条件或语法规则时，编译器会产生警告信息；但是不受约束的未定义行为并不一定会受到检查。**C++** 中的 **未定义行为（Undefined Bahavior, UB）** 是同样的概念。
- **可移植的代码（Portable Code）**：一个 **严格遵循标准（Strictly-Conforming）** 的程序不依赖于任何未申明的或未定义的特性，它可以在任何平台的任何编译器中都会有相同的输出。相比之下，一个 **遵循标准（Conforming）** 的程序只忠于特定的编译器，因为它可能依赖于该编译器中的由实现定义的或未申明的行为。

后文中，我们主要会给出遵循标准的代码，否则会指出。

## **C** 程序的结构

一个 **C** 程序通常是由多个源文件分别经过 **编译（Compilation）**，分别形成 **编译单元（Compilation Unit）** 后，经汇编再通过链接器进行 **链接（Link）** 变为可执行文件，也即 **程序（Program）**。

### **C** 文件

一个 **C** 文件通常包括一系列 **预处理指令（Preprocessor Directive）**、**变量（Variable）** 及 **函数（Function）** 的定义及声明、**结构体（Structure）** 及 **枚举（Enumeration）** 的定义及声明，以及 **类型别名（Type Alias）** 的声明。接下来让我们快速地浏览这些概念，然后在后文中详细地介绍。

预处理指令是一些以 `#` 符号开头的标签，比如 `#define` 就是预处理定义指令，这里 `#` 和 `define` 之间可以有任意的空格。比较常用的预处理指令有下面这些：

```c
// define 指令所做的就是文本替换；
#define VARLIKE_MACRO 10				// 将后面所有 VARLIKE_MACRO 替换为 10
#define FUNCLIKE_MACRO(x) (x * x)		// 将后面所有 FUNCLIKE_MACRO(x) 替换为 (x * x)，这里 x 可以是任何形式的内容
int main() {
 	printf("%d", VARLIKE_MACRO);		// 输出 10
    printf("%d", FUNCLIKE_MACRO(10));	// 输出 100
}
// include 指令做的是文件内容拷贝
#include <string.h>						// 将 include 路径中的 string.h 经过预处理后全部复制到这里
#include "myheader.h"					// 将当前路径中的 myheader.h 经过预处理后全部复制到这里
```

变量是一个由下划线、字母和数字组成的标签（我们通常称为 **标识符（Identifier）**），编译器会根据其定义让它在运行时绑定一段内存空间；程序员使用变量时，就等同于在操作其绑定的内存。变量的定义由变量 **类型（Type）** 、标识符，以及一些限定符组成，举例如下：

```c
// 定义了 int 类型的变量 i，这里 i 就是我们说的标识符。最后需要加上一个分号表明语句的末尾
int i;
// 同时定义两个同类型（double）的变量，它们之间用逗号分隔
double _d123, _d124;
// * 是指针修饰符，这是一个指针类型
char* ptr;
// [] 是数组修饰符，这是一个数组类型
int arr[5];
// (*)() 是函数指针修饰符，这是一个函数指针类型
void (*fptr)(int i);
```

函数也是一个标识符，它和一个带有参数、返回值的子程序绑定，并可以被一些限定符修饰。

```c
// 定义了 int(int, double) 类型的函数 foo，其中 (int, double) 是其参数类型，int 是其返回类型
int foo(int a, double b) {		// 子程序（函数体）的部分通过大括号包围
	// 省略函数体
}
// 如果没有返回值，那么返回类型就是 void
void bar(char* s) {
 	// 省略函数体
}
// static 是函数修饰符，这是一个静态函数；没有参数的函数可以用 void 占位，或直接省略也可
static double _abc(void) {
 	// 省略函数体  
}
```

结构体是关联一个类型模版的标识符，可以用它构造一系列变量组成的结构。

```c
// 定义了 struct IntDouble 结构，其中包含两个“变量” i 和 d
struct IntDouble {
  	int i;
    double d;
};	// 需要注意结构体的结尾处需要添加分号
// 结构体定义语句处可以直接定义变量
struct TwoDoubles {
 	double d1, d2;
} td1, td2;
// 可以定义匿名结构体，然后利用前面所述的性质定义变量
struct {
 	int* ptr;   
} ptr_wrapper;
```

枚举也是一个标识符，它为一个可数的集合声明一个类型，其中给出了这个集合中的每个元素（作为标识符给出）：

```c
// 定义了 enum Weekday 枚举类型，其有效的值有 Monday 等（其等效整型值为 0, 1, ..., 6） 
enum Weekday { Monday, Tuesday, WednesDay, Thursday, Friday, Saturday, Sunday };
enum SomeNumbers {
  	Zero,		// 枚举的第一个值默认为 0
    One,		// 没有给指定值的默认为上一个值 +1
    Three = 3,	// 可以为枚举中的元素设指定值
    Four,
    NegOne = -1,	// 枚举中元素的大小并没有规定
    AnotherZero		// 枚举中元素的值可能会重复
};
// 匿名枚举类型是合法的，也可以直接在枚举定义语句处直接定义变量
enum {
 	SomeVal, OtherVal   
} e1, e2;
```

最后让我们来看类型别名。`typedef` 关键字可以将一个类型声明为一个标识符；之后所有该标识符出现的地方都会理解为该声明。

```c
// 将内置的 int 类型声明为 MyInt
typedef int MyInt;
// 将内置的 int[10] 数组类型声明为 MyArr
typedef int MyArr[10];
// 将函数指针类型 void (*)(int, double) 声明为 MyFunc
typedef void (*MyFunc)(int i, double d);
// 将 struct MyStruct 类型声明为 MyStruct
typedef struct MyStruct MyStruct;
```

### 定义和声明

首先我们需要达成共识的是，一个程序在编译时会从第一行扫描到最后一行，且仅扫描一遍。因此一个标识符被使用之前，必须经过定义或声明。**C** 中区分了定义和声明两个概念；前者提供了一个标识符的实际定义，而后者仅声明一个标识符的存在。在编译时，声明过的标识符均可以随意使用，但是如果链接时被使用的变量找不到定义，会产生链接错误。

在一些语境中，定义也是一种声明，因为通常定义包含了声明中需要包含的内容。但是本节中我们将声明和定义严格区分开来，**声明（Declaration）** 向程序中引入一个标识符，却不为其分配内存空间（不记录其信息）；而 **定义（Definition）** 则是在引入标识符的同时为其分配内存空间。

变量的定义除了必须给出类型和标识符外，也可以提供一个初始值。我们称其为变量的 **初始化（Initialization）**。一些类型的初始化比较复杂，我们会在后面再介绍。这里先介绍普通类型的初始化：

```c
// 定义了变量 i，并将其初始化为 10
int i = 10;
// 定义了变量 j，但是其值是未知的（字面意义上）
int j;
// 数组类型的初始化，数组中存储了三个元素
int arr[3] = { 1, 2, 3 };
```

变量的声明中使用 `extern` 关键字，并给出变量的类型和标签：

```c
// 声明了一个变量，这个变量可能定义在任何地方，甚至其它文件中
extern int i;
int main() {
    int j = i;		// 可以使用声明过的变量，但如果 i 没有被定义，会在链接时报错
    printf("%d". j);
    return 0;
}
int i = 10;			// i 在这里定义；定义出现在了使用之后，这没有任何问题，因为使用时它已经被 *声明* 过了
```

函数的声明也可以使用 `extern` 关键字，并不给出函数体，而用分号结束声明语句：

```c
// 声明了一个函数，这个函数可能定义在任何地方，甚至在其它文件中
extern void foo(int, char);		// 参数列表中的参数名称可以被省略，extern 关键字也可以被省略
int main() {
    foo(10, 'c');		// 可以调用声明过的函数，但如果 foo 没有被定义，会在链接时报错
 	return 0;   
}
// 真正定义了 foo 函数
void foo(int ct, char c) {
    while (ct--) {
        printf("%c", c);
    }
}
```

结构体的声明则只需要给出一个标签，但直到给出该类型的定义之前，这个类型都是 **不完整类型（Incomplete Type）**：

```c
// 声明了结构体 MyStruct，现在可以将其作为结构体的标识符使用了，但是目前它还是一个不完整类型
struct MyStruct;
// 声明一些帮助函数
void set_data(struct MySturct* ms, int i);
int get_data(struct MyStruct* ms);

int main() {
 	struct MyStruct ms;		// 错误！不能定义一个不完整类型的变量
    struct MyStruct* ms;	// 不完整类型的指针是一个完整类型
    set_data(ms, 10);		// 帮助函数被声明了，因此可以使用
    printf("%d", get_data(ms));
}
// 定义了结构体 MyStruct，其中有一个成员 data。之后就可以访问这个成员了
struct MyStruct {
    int data;
};
void set_data(struct MyStruct* ms, int i) {
 	ms->data = i;   	// 可以使用 -> 运算符来访问结构体指针变量的成员
}
int get_data(struct MyStruct* ms) {
    return ms->data;
}
```

枚举和结构体非常类似，也可以进行声明，但在定义前都是不完整类型。

```c
// 声明了枚举类型 MyEnum，但是它现在还不是一个完整类型
enum MyEnum;
int get_num(enum MyEnum* e);

enum MyEnum {
    One, Two, Three
};
int main() {
    enum MyEnum e = One;
    printf("%d", get_num(&e));
    return 0;
}
int get_num(enum MyEnum* e) {
    return *e;
}
```

### 变量修饰符

**C** 语言提供了一些变量修饰符来描述变量的行为特征。比如 `const` 修饰符可以让这个变量绑定的对象不能被修改：

```c
int main() {
    const int i = 10;		// i 是一个常变量，我们不能通过 i 来修改其绑定对象的值
    i = 20;					// 错误
    int const j = 20;		// const 修饰符可以放在 type 的左侧或右侧都可以
    int* const p = NULL;	// 这是一个 const 指针，也即该指针不能被修改
    p = NULL;				// 错误，即使是和初始化时相同的值也不能赋给常变量
    int const x = some_function();	// 错误，初始化的值不是一个常数
}
```

除此之外，`const` 还可以将一个对象“声明”为 `const` 的，即使它本来并不是。这个主要出现于指针类型：

```c
int main() {
 	int i = 10;
    const int* ptr = &i;	// ptr 被声明为指向一个 const int 类型的指针，此时 i 绑定的对象从 ptr 来看就是 const 的
    *ptr = 20;				// 错误
    int const* qtr = &i;	// const 可以放在 int 的后面，不过注意对比 int* const 类型，指针修饰符的顺序必须要确认
}
```

从这里来看 `const` 摆放的位置相当容易令人迷惑。我们会在后文详细介绍 **C** 语言变量声明的逻辑，不过这里可以给出一个基本的建议：`const` 总是能够修饰左侧（如果存在）的类型，因此如果需要表明某个类型（或指针）是 `const` 的，只需要在类型名称右侧（或指针修饰符右侧）加上 `const` 即可。比如 `int const` 是一个整数常变量；`int const*` 是一个指向整数常变量的指针；`int* const` 是一个指向整数的常指针；`int const* const` 是一个指向整数常变量的常指针。

**C** 从 **C++** 那里引入了 `const` 关键字，但是其强制程度要不如 **C++**，比起是类型的一部分，它更多地像一个普通的编译期检查标签。比如下面这段代码可以轻松地“破解” `const`：

```c
int main() {
 	int const i = 10;
    int* ptr = &i;			// 虽然右侧的类型是 int const*，但是却可以直接赋值给左侧。这会引起一个编译器警告，但不是错误
    *ptr = 42;				// 没有问题
}
```

此外，虽然 `const` 被称为常变量，但是它实质上还是一个变量。它（在 **C99** 之前）不能用于表示数组大小的表达式中，同时也不能用于` switch` 语句的 `case` 标签。`const` 通常用于全局变量或函数参数等情景。

`volatile` 是一个和 `const` 地位和语法类似的修饰符，其描述一个变量可能会随时发生变化，因此不能让编译器对其进行优化。需要注意 `const volatile` 和 `volatile const` 也是合法的标识符，它们都声明一个变量不可通过赋值语句改变却可能随时发生变化。

最后是 `restrict` 修饰符（**C99**），它通常用于修饰函数参数列表中的多个指针类型参数（不包括函数指针），其暗示编译器这些参数在函数过程中不存在指向上的重叠，因此可以使用一些优化算法，下面是一个例子：

```c
void copy(int* restrict dest, int* restrict src, int ct) {
    while (ct--) {
        *dest++ = *src++;	// restrict 蕴含在整个函数过程中都不存在 dest == src 的情形，此时可以使用例如向量化的优化
    }
}
int main() {
    extern int arr[100];
    copy(arr + 50, arr, 50);
    copy(arr + 1, arr, 50);		// 未定义行为
}
```

也可以将 `restrict` 修饰符用于其它的任何声明语句中，其含义会有微妙的变化：

- 在作用域中，`restrict` 会对当前的作用域生效
- 在结构体声明中，`restrict` 暗示编译器在以该结构体声明的对象中的 `restrict` 成员互不相同

### 作用域和生命周期

**C** 的 **作用域（Scope）** 是一个划分 **C** 文件不同部分和层级的概念，通过一对大括号 `{}` 来表示。作用域允许相互嵌套。不存在嵌套关系的作用域之间是相互“隔离”的，其中任一个作用域中定义的所有标识符不能再另一个作用域中引用；存在嵌套关系的作用域中，只有外部作用域的标识符能够被内部引用。值得一提的是，函数体也是一个作用域；在文件中“直接暴露”的作用域只能是函数。换句话说，所有作用域都是函数或者嵌套在函数中的作用域（即 **全局作用域（Global Scope）**，或 **文件作用域（File Scope）**，我们马上就会提到）。

```c
// 在文件中直接定义的标识符被称为处于 全局作用域 中，所有其它作用域都嵌套在它当中
int i;
void func() {				// 函数这里开始了一个作用域
    i = 10;					// 将全局作用域中的 i 赋值为 10
    {						// 开始了新的一个作用域
        int i;				// 内部作用域中的标识符会 覆盖 外部作用域中同名标识符的定义
        i = 20;
        printf("%d", i);	// 输出 20
    }						// 作用域结束，其中定义的标识符都将失效
    printf("%d", i);		// 输出 10
    int i;					// 覆盖了全局作用域中的 i
    i = 15;
    printf("%d", i);		// 输出 15
}
int main() {
    func();					// 所有函数都在全局作用域中
    return 0;
}
```

变量的 **存储类型（Storage Class）** 也和作用域有关。我们用下面的表来进行说明：

| 关键字                | 存储类型       | 说明                                                         |
| --------------------- | -------------- | ------------------------------------------------------------ |
| `auto`（可以省略）    | 自动存储类型   | 该变量只能定义在非全局作用域，且会在其作用域开始时分配内存，结束时释放内存 |
| `extern` （可以省略） | 全局存储类型   | 该变量只能定义在全局作用域，其在程序中全程存在               |
| `static`              | 静态存储类型   | 该变量在程序中全程存在，但其标识符只能在所在文件及作用域中有效 |
| `register`            | 寄存器存储类型 | 该变量和自动存储类型的变量基本一致，只不过会直接使用寄存器而非内存 |

下面我们将用一些例子具体描述上面这四种存储类型的特征：

```c
// example.c
int global_var;				// 全局变量的定义
extern int external_var;	// 全局变量的声明。注意这不是一个定义！
static int static_var;		// 静态全局变量的定义

int main() {
    auto int auto_var;			// 自动变量，也称为局部变量，这里的 auto 可以被省略
    static int func_static_var;	// 函数中的静态变量，其也存在于整个程序运行过程中，但是只有所在函数能够引用这个标识符
    extern int another_var;		// 全局变量的声明；与定义不同，声明出现的地方并没有限制
    
    int* get_local();			// 声明了两个函数
    int* get_static();
    printf("%d", *get_local());	// 错误，因为 get_local 返回了一个自动变量的指针，自动变量在函数返回后就会被释放
    printf("%d", *get_static());// 没有问题
    
    register int register_var;	// 寄存器变量
    printf("%p", &register_var);// 错误，不能对寄存器变量取地址
 	return 0;   
}
int* get_local() {
    int temp;
    temp = 10;
    return &temp;			// 返回自动变量的地址是危险的！
}
int* get_static() {
    static int temp;
    temp = 10;
    return &temp;
}
```

```c
// external.c
int external_var;			// 定义了全局变量 external_var，这样 example.c 中的声明就有了保障
extern int static_var;		// 错误，其它文件中不存在全局变量 static_var 的定义（事实上在 example.c 中它定义为静态变量）
```

为了描述一个标识符（变量或函数）在其它作用域的可访问性，我们可以引入 **链接（Linkage）** 的概念，这其实和链接器寻找标识符定义相关：

- **无链接（No Linkage）**：只能在当前作用域中被引用，所有函数参数、自动变量和静态变量都在这个分类中
- **内部链接（Internal Linkage）**：只能在当前文件中被引用，所有全局变量、静态全局变量和静态函数都在这个分类中
- **外部链接（External Linkage）**：可以被程序的任何文件引用，所有非静态函数、全局变量都在这个分类中

如果一个标识符在一个编译单元中同时以内部链接及外部链接的形式出现，其行为是未定义的。

如果我们不局限于变量，而只探究变量所绑定对象的存储性质时，可以将它们分为下面几种 **存储周期（Storage Duration）**：

- 自动存储周期：自动变量、函数参数绑定的对象都有自动存储周期
- 静态存储周期：全局变量、静态变量、静态全局变量绑定的对象都有静态存储周期
- 动态存储周期：根据需要手动分配及回收的对象

上面提到的动态存储周期我们还没有遇到过，这里举例进行说明：

```c
#include <malloc.h>
int main() {
    int* iptr = malloc(sizeof(int));	// 在内存中动态分配一个 int 所需的空间，这个对象就是动态存储周期的
    *iptr = 42;							// 解引用后向内存赋值
 	return 0;   
}
```

### 函数

函数是一个子程序，事实上所谓的主程序就是前文中经常出现的 `main` 函数。程序开始运行时，`main` 函数会被调用，然后其中还可能调用其它的函数。

```c
int process(int i) {
 	return i * 2 + 1;   
}
int main() {
    printf("%d", process(10));	// 通过括号调用函数，输出 21
    return 0;
}
```

函数的参数会在传递时进行拷贝，因此传入的参数是不能通过函数进行修改的：

```c
void inc(int i) {	// i 在复制之后才传入函数
 	++i;   
}
int main() {
    int n;
    n = 0;
    inc(i);			// 这个函数不能改变任何内容
    return 0;
}
```

但是，指针类型存储的是地址，因此在复制过后，其内容（地址）不受影响，可以通过这个方式修改函数外面的值：

```c
void inc(int* i) {
    ++*i;				// *i 访问了 i 存储的地址处的内存，我们称为解引用；随后将这个对象自增 1
}
int main() {
    int n;
    n = 0;
    int(&n);			// 将地址传入
    printf("%d", n);	// 输出 1
    return 0;
}
```

**C** 语言中经常利用这个性质来将函数参数作为额外的返回值。函数的返回值也是通过拷贝传到调用处，因此返回一个自动变量并不会造成内存错误，但是返回一个自动变量的指针则是非常危险的。

函数的调用会包含内存中 **栈（Stack）** 的行为。每次调用函数时，会在栈上分配函数中所需要的内存，然后在函数返回时将这些内存释放。表面上是这些工作，但实际上底层是将寄存器中的值压入栈中，然后根据函数中所需要的内存移动栈指针；在函数返回时将栈指针移动回来并将之前压入栈中的值还到寄存器中。这个过程是有一定性能损耗的，尤其是在需要大量函数调用（递归调用）的场景中，我们可能希望将函数定义“嵌入”调用处，而这就引出了内联函数的概念：

函数和变量类似，可以被声明为静态的。因为函数一定定义在全局作用域，因此静态函数的意义就是其标识符只能在当前文件可见的函数。除此之外，还可以将函数声明为 **内联的（Inline）**。内联函数会建议编译器在函数调用处将其定义展开，从而避免了函数调用的开销（**警告**：从现在开始的知识点可能有些枯燥，如果没有兴趣对内联进一步了解可以跳到下一节）。

```c
inline int add(int a, int b) {
    return a + b;
}
static int static_var;
inline void wrong() {
 	static int x;				// 错误，内联函数中不能定义静态变量
    printf("%d", static_var);	// 错误，内联函数中不能使用静态全局变量（很好理解，因为无法推测静态全局变量会展开为什么）
}
```

从上面缺失的高亮也可以看出，`inline` 并不是 **C** 早期的关键字；事实上它在 **C99** 才正式纳入标准。同时标准并不要求编译器忠实执行内联操作（也即我们只能 *建议* 编译器进行内联）。从某种角度讲 `inline` 和 `static` 有同样的效果，因为它们都只能在当前文件中可视。需要注意 `inline` 并非和 `static` 、`extern` 并列（它并不是存储类型修饰符），因此可能会出现 `static inline` 或 `extern inline` 这样的用法，而它们的区别相当微妙：

- `inline`：如果声明为内联函数，则必须在当前文件中给出定义；内联函数的定义只在当前文件中可视；不能定义静态变量或引用静态全局变量
- `static inline`：和内联函数一致，但是其中可以定义静态变量或引用静态全局变量
- `extern inline`：对外部可视的内联函数

```c
// example.c
static int static_var = 20;
// 声明了函数，其定义不一定在当前文件，且在定义处可能会被 inline 修饰
extern int foo();
// 声明了静态内联函数，声明为 inline 的函数必须在同文件中给出定义
static inline int bar();
int main() {
    printf("%d", foo());
    printf("%d", bar());
 	return 0;   
}
// 静态内联函数中允许定义静态变量或使用静态全局变量
static inline int bar() {
 	return static_var; 
}
```

```c
// external.c
int i = 10;
// 内联函数只可以使用自动变量及全局变量；只有声明为 extern 时才能被外部引用
extern inline int foo() {
    return i;
}
```

可以说是一团乱麻；因为 `inline` 并不要求在声明处和定义处同时出现，有一种会产生歧义的情形：

```c
// example.c
inline void foo() {
 	puts("Internal version");   
}
int main() {
	foo();
}
extern void foo();			// 这个声明会链接 external.c 中的 foo 定义
```

```c
// external.c
extern inline void foo() {
    puts("External version");
}
```

这是一个未申明的行为。在 `gcc` 中会产生重复符号的错误，也即同一个函数 `foo` 被定义了两次，尽管它们使用了不同的声明（一个是 `inline` 仅在当前文件可视而另一个是 `extern inline`，在其它文件中也可视）。相比之下，下面的代码就是遵循标准的：

```c
// example.c
inline void foo() {
    puts("Internal version");
}
int main() {
    foo();						// 输出 "Internal version"
}
extern void foo();
```

```c
// external.c
inline void foo() {				// 不再使用 extern 修饰，因此这个定义只对当前文件可视
    puts("External version?");
}
```

下面的代码也是遵循标准的：

```c
// example.c
inline void foo() {
    puts("Internal version");
}
int main() {
    foo();						// 输出 "External version"
}
```

```c
// external.c
inline void foo() {
    puts("External version");
}
extern inline void foo();		// 这个声明会找 foo 的定义并将其变为对外部可视
```

可以看到，只是一个 `inline` 关键字的引入，就带来了这么多莫名其妙的不直观的性质。个人的建议是不要使用 `inline` 关键字。现代的编译器已经足够智能，能够自动判断函数是否进行内联。也就是说，即使不被声明为内联的函数，也可能为提升性能进行内联化。如果一定要使用，则务必声明为静态内联函数，以防止产生未申明的行为。通常，在头文件中定义的函数会被声明为 `static inline` 的。

### 结构体与联合体

我们在之前已经看到，结构体是一种将零或多个变量统一进行定义的模版，利用 `struct` 关键字及一个标识符可以定义一个结构体，随后可以再使用它们声明变量：

```c
// 定义了一个结构体
struct MyStruct {
	int i;			// 第一个成员 i
    double d;		// 第二个成员 d
};
int main() {
	struct MyStruct ms;		// struct MyStruct 是一个自定义的类型
    printf("%zu", sizeof(struct MyStruct));		// 输出 12，一个结构体的大小是其所有成员大小之和
    ms.i = 10;				// 使用成员访问符 . 来访问一个结构体对象的成员。这里 ms.i 的行为和一个变量是一样的
    return 0;
}
```

利用结构体可以匿名的性质，我们可以在结构体中包含一个匿名结构体类型的变量，甚至这个变量：

```c
struct MyStruct {
	struct { int i; int j; };
    struct { double d; } s;
}
int main() {
    struct MyStruct ms;
    ms.i = 10;			// 结构体中定义的匿名结构体中的匿名成员会视作外部的结构体的成员
    ms.s.d = 1.0;		// 如果结构体的成员不是匿名的，即使其类型是一个匿名的结构体也不能直接调用其成员，这里 ms.d 就不行/
 	return 0;   
}
```

联合体则是一种让零或多个变量占用相同一段空间的模版，利用 `union` 关键字及一个标识符可以定义一个联合体，随后可以再使用它们声明变量：

```c
// 声明了一个结构体
union MyUnion {
  	int i;
    double d;
};
int main() {
    union MyUnion mu;
    printf("%zu", sizeof(union MyUnion));		// 输出 8，一个联合的大小是其所有成员大小中最大的那个
    mu.i = 10;				// 现在联合体变量 mu 中 i 是有效的成员
    printf("%ld", mu.d);	// 未定义行为。
	return 0;
}
```

## **C** 语言的类型

类型是 **C** 语言从 **BCPL** 的一大进步。从几乎毫无类型到划分出整数、字符、浮点数、数组、指针等类型，这是一个惊人的进步。不过毫无疑问在类型相关的很多方面（比如隐式转换，还有变量的声明语法），**C** 语言设计者有着比较独特的想法。让我们按照不同类型来一个个分析 **C** 语言在类型设计上的思考。

### 整型

**C** 语言中的 **整型（Integral Type）** 指所有等价于整数类型的类型，包括：

- 有符号整数类型：`signed char`、`short`、`int`、`long`、`long long`
- 无符号整数类型：`_Bool`、`unsigned char`、`unsigned short`、`unsigned`、`unsigned long`、`unsigned long long`
- 枚举类型：`enum <EnumName>`，也即所有枚举类型都是整型

有符号整型是采用补码的二进制序列。在补码中，其所有位对二的幂进行加权（最高位采用负数权重）后就得到了其代表的值。作为举例，一个 4-bit 补码 `1011` 的值是 $1 \times 2^0 + 1 \times 2^1 - 1 \times 2^3 = -5$。无符号整型同样也是对二的幂进行加权，比如一个 4-bit 无符号整数 `1011` 的值是 $1 \times 2^0 + 1 \times 2^1 + 1 \times 2^3 = 11$。可以看到同样的二进制序列，有符号和无符号整型的值并不一定是一致的。

在程序中可能还会看到上面没有列出的整型，比如 `char` 根据实现可能等同于 `signed char` 或 `unsigned char`；`signed int` 和 `int`、`unsigned int` 和 `unsigned` 完全等价（等价的类型我们会使用最简短的记法）；`__int128` 是由实现定义的，等等。这么纷繁复杂的分类，它们的区别在于什么呢？多数的区别在于其占用内存空间的大小。

我们可以通过 `sizeof` 运算符来得到某个类型的大小：

```c
int main() {
 	printf("%zu", sizeof(int));		// 可能输出 4  
    printf("%zu", sizeof(_Bool));	// 可能输出 1
    return 0;
}
```

注释中我们用了“可能”这个词，并不是因为我们过于谨慎，而是在标准中，只要求了这些类型 *最小* 的占用内存大小。这是有历史原因的。远古的处理器是 16-bit ，甚至是 8-bit 的，当时的 `int`（举例）只有 2 个字节大小（因为没有条件提供更大的大小），但后来在 32-bit 及 64-bit 的编译器中，`int` 多被定义为 4 字节大小的整型。标准为了包容这个差异，将 `int` 定义为大小至少为 16-bit  的整型。下面这张表给出了常见的整型在标准中规定的大小：

| 类型          | 大小（字节数） | 类型                 | 大小（字节数） |
| ------------- | -------------- | -------------------- | -------------- |
| `char`        | 1              | `_Bool`              | >= 1           |
| `signed char` | 1              | `unsigned char`      | 1              |
| `short`       | 2              | `unsigned short`     | 2              |
| `int`         | >= 2           | `unsigned`           | >= 2           |
| `long`        | >= 4           | `unsigned long`      | >= 4           |
| `long long`   | 8              | `unsigned long long` | 8              |

如果想要指定某种大小的整型，可以使用固定宽度的整型，它们是定义在 `<stdint.h>` 的类型别名，并保证其大小是可移植的。比如 `int32_t` 是大小为 32-bit 的有符号整数，而 `uint8_t` 是大小为 8-bit 的无符号整数。后文中，我们不太会使用这些固定宽度的类型，还是倾向于使用 `int` 这样的内置类型；当不进行额外说明时，让我们约定 `int` 的大小为 `4` 而 `long` 的大小为 `8`。

最后让我们来看看整型相关的字面量。所有的整型字面量，在能够用 `int` 容纳的时候，都是 `int`，否则是 `unsigned int`，若还是容不下则是 `long`，再容不下则是 `unsigned long`，以此类推（直到 `unsigned __int128`）。

```c
int main() {
    int i = 123;		// 这里的 123 是一个 int 类型的对象
    char c = 'a';		// 这里的 'a' 也是一个 int 类型的对象，它等同于 97
    long l = 123123123;	// 这里的 123123123 是一个 long 类型的对象
    unsigned u = 0xDeadBeaf;	// 这里的 0xDeadBeaf 是一个十六进制 unsigned int 类型的对象
    unsigned long ul = 01112223334445556667777;	// 这里是一个八进制的 unsigned long 类型的对象
    __int128_t i128 = 31415926535897932384626;	// 这里是一个 __int128_t 类型的对象
 	return 0;   
}
```

上面涉及了几种特殊的字面量，由单引号包括的单个字符是一个字符字面量，但它本身是一个整数（遵循 **ASCII** 编码）；由 `0x` 开头的是十六进制的整型字面量，其中可以包含 `a-f`、`A-F` 字符来表示 10 到 15；由 `0` 开头的是八进制的整型字面量；由 `0b` 开头的是二进制的整型字面量。

### 浮点型

浮点数是用来表示小数的类型，在 **C** 语言中提供了三种大小和精度不同的浮点类型：

- `float` 是一个 4 字节的浮点类型
- `double` 是一个 8 字节的浮点类型
- `long double` 是一个 10 字节的浮点类型

### 指针

**C** 引入了 **指针（Pointer）** 的概念，它存储的是一个对象的地址，其类型也由这个对象的类型决定。比如一个指向 `int` 对象的指针，它的类型是 `int*`，这里的 `*` 是指针修饰符。

```c
int dumb() {
    return 42;
}
int main() {
    int* iptr;
    printf("%d", sizeof(int*));	// 输出 8。指针类型的大小和处理器位数是一致的，它们都表示了内存地址的容纳范围
    int i = 10;					// 定义一个 int 变量
    iptr = &i;					// & 是取地址运算符，它能够取得一个对象的地址
    *iptr = 20;					// * 是解引用运算符，它能够访问一个地址中的内容
    printf("%d", i);			// 输出 20
    iptr = &20;					// 错误，字面量不能取地址
    iptr = &dumb();				// 错误，函数返回值不能取地址
    iptr = &&i;					// 错误，（解引用以外的）运算结果不能取地址
 	return 0;   
}
```

上面我们列出的不能取地址的对象被称为 **右值（Right Value）**，得名于它们通常出现在赋值运算的右侧；与之相对应的，其余的对象都称为 **左值（Left Value）**，包括且不限于变量、函数等，我们能够对它们取值并得到其地址。

指针本身也是一个类型，所以指针的指针也是合法的类型。不仅是二重指针，三重、四重乃至任意重指针都是合法的，尽管它们并不合理。

```c
int main() {
    int******************** nptr;	// 一个 20 重指针；当然，它的意义就像 20 阶导数一样难以理解
    nptr = NULL;					// 将 nptr 置为空指针（一个无效值），毕竟真的找不到其它合适的值了
    printf("%p", nptr);				// 输出 0x0，这是一个地址
    printf("%p", *nptr);			// 错误，尝试解引用一个空指针
}
```

上面出现的空指针 `NULL` 是一个宏，其通常定义为 `(void*) 0`。这个定义中包含了好几个目前看来有些陌生的东西：

- `0` 代表了地址 `0x0`。在内存中这是约定俗成的不可读写的地址，一般都作为无效的指针值
- `void*` 是无类型指针类型。它是最“纯粹”的指针类型，因为其指向的可能是任何类型
- `(void*)` 是一个显式类型转换，将整数 `0` 转换为无类型指针。在 **C** 中任意两种指针间的显式类型转换都是合法的

至于为什么 `NULL` 可以直接赋值给 `nptr`，是因为无类型指针类型可以隐式类型转换为任何其它的指针类型。

有了指针类型，我们就能够利用函数修改调用处的值：

```c
void modify(int* iptr) {
    *iptr = 42;
}
int main() {
    int i = 10;
    modify(&i);			// 将 i 的地址传入函数
    printf("%d", i);	// 输出 42
}
```

如果需要利用函数修改一个指针的值（也即修改其指向的地址），那就需要传入指针的指针：

```c
void redirect(int** ipp, int* new_addr) {
    *ipp = new_addr;
}
int main() {
 	int i = 10;
    int j = 42;
    int* ptr = &i;
    redirect(&ptr, &j);		// 将 ptr 的地址传入函数
    printf("%d", *ptr);		// 输出 42
}
```

返回一个指针当然也是允许的，但是回忆我们之前讨论的作用域和对象生命周期，返回一个自动变量的指针是非常危险的，因为尝试访问该指针指向的内存可能回产生致命的错误。因此，只有静态生命周期或动态生命周期的对象的地址才能被安全地返回：

```c
typedef enum ObjectType { AUTO, STATIC, DYNAMIC } ObjectType;
int* getptr(ObjectType ot) {
    int auto_var = 10;				// auto_var 绑定的对象有自动生命周期
    static int static_var = 42;		// static_var 绑定的对象有静态生命周期，这个对象是唯一的，只会初始化一次
    int* ptr = malloc(sizeof(int));	// ptr 指向的对象有动态生命周期
 	switch(ot) {
        case AUTO:		return &auto_var;		// 危险！这几乎总是错误的行为
        case STATIC:	return &static_var;		// 没有问题，静态生命周期的对象在程序运行期间不会被释放
        case DYNAMIC:	return ptr;				// 多数情况下没有问题，动态生命周期的对象只要不被显式释放就没有问题
    }
}
int main() {
    int* ptr_auto = getptr(AUTO);
    int* ptr_static = getptr(STATIC);
    int* ptr_dynamic = getptr(DYNAMIC);
    
    printf("%d", *ptr_auto);		// 段错误
    ++*ptr_static;					// 将 getptr 中的 static_var 自增 1
    printf("%d", *ptr_dynamic);		// 输出 0。动态生命周期的对象会被自动初始化为 0
    free(ptr_dynamic);				// 不要忘记释放内存空间
 	return 0;   
}
```

最后，为了给接下来的数组做铺垫，让我们介绍一下指针的算术运算。指针的本质是内存地址，那么为一个内存地址加上或减去某个数的意义是什么呢？

```c
int main() {
    int* ptr = malloc(sizeof(int) * 3);		// 提前分配三个整数的内存空间
    printf("%p", ptr);			// 输出 0x12341234（举例）
    printf("%p", ptr + 1);		// 输出 0x1234123c
    int* p1 = ptr;
    int* p2 = ptr + 2;
    print("%ld", p2 - p1);		// 输出 2。这也符合我们的理解，也即 a + b - b = a
    return 0;
}
```

可以看到，为某个指针类型 `type*` 的变量加上某个数 `n` 得到的是这个内存地址的整数加上 `n * sizeof(type)` 所在的地址，减去则同理。两个 `type*` 类型指针的差则返回其中能够容纳的 `type` 类型的最多个数。至于为什么我们这里要强调“最多个数”，考虑下面这种情况：

```c
int main() {
    int* ptr = malloc(sizeof(int) * 3);		// 提前分配三个整数的内存空间6
    int* top = ptr + 2;				// 与 ptr 有 8 个字节的weiyi
    printf("%ld", &arr[2] - ptr);	// 输出 2
    void* addr = (void*) ptr;
    ptr = addr + 1;					// void* 类型的算术运算和整数无异
    printf("%ld", &arr[2] - ptr);	// 输出 1
    return 0;
}
```

### 数组

数组类型是一类非常重要的类型，它是一系列同种类型对象的 **聚合（Aggregation）**，其大小通常需要在编译时就已经确定。因为它的所有元素都相邻，因此能够吃到缓存的红利，让程序对同个数组的重复调用变得异常高效。

```c
int main() {
    int arr[3] = { 1, 2, 3 };
    printf("%d", arr[0]);		// 输出 1，通过下标运算符访问数组中的元素；数组从 0 开始计数
    printf("%d", arr[2]);		// 数组的最后一位下标是它的大小减 1
    printf("%zu", sizeof(arr));	// 输出 12（假设 sizeof(int) 是 4），正是数组中所有元素的大小之和
    arr = { 2, 3, 4 };			// 错误，数组不能被赋值
    arr[1] = 0;					// 通过下标运算符修改数组中的元素
    
    int vec_1[] = { 1, 2, 1 };	// 可以省略数组声明中的大小，让编译器从右侧初始化列表中判断数组大小
    int vec_2[3] = { 1 };		// 可以省略数组后部的元素，缺失的元素都会被赋值为 0
 	return 0;   
}
```

数组还有一种非常奇异的初始化方式，可以熟悉一下：

```c
int main() {
 	int arr[3] = { [0] = 3, [2] = 1 };   // 等同于int arr[3] = { 3, 0, 1 }，没有指出的项会初始化为 0
}
```

数组的大小通常是一个编译期可以确定的（也就是所谓 **常量（Constant）**）大于 `0 ` 的整数，下面的代码在 **C99** 以前不能编译成功：

```c
int main() {
 	int size;
 	scanf("%d", &size);
    int arr[size];				// 这个 size 的大小在运行时才能知道
    return 0;
}
```

常量是一个比较狭隘的概念，通常字面量（比如 `123`、`'a'`）或其构成的表达式都能被视作常量。我们也可以通过宏来为它们绑定一个预处理期的标识符；比较意外的是，`const` 修饰符修饰的变量并不是常量。不过这些在 **C99** 之后都显得没有那么重要，因为 **C** 语言引入了 **变量长度数组（Variable-Length Arrays）** 的概念。它允许声明“变量”长度的数组，当然这个值也应该大于 `0`。

```c
// 函数参数列表中，可以按照顺序声明一个变量长度数组，虽然这实际上没有意义（我们马上就会认识到）
void print_arr(int n, int arr[n]) {	
    for (int i = 0; i < n; ++i) {
     	printf("%d", arr[i]);   
    }
}

int main() {
 	int size;
    scanf("%d", &size);
    int arr[size];				// 会在运行时在栈上分配 size 个 int 大小的空间
    print_arr(size, arr);
    return 0;
}
```

对比数组和指针的算术运算，我们会发现一个“惊人”的事实：

```c
int main() {
 	int arr[3] = { 1, 2, 3 };
    int* ptr = &arr[0];			// 指向数组首个元素的指针
    printf("%d", *(ptr + 0));	// 输出 1
    printf("%d", *(ptr + 1));	// 输出 2
    printf("%d", *(ptr + 2));	// 输出 3
    return 0;
}
```

嗯…… 如果把上面的 `*(ptr + k)` 换成 `arr[k]`，似乎是同样的结果。没错，下标运算符本质就是指针的算术运算和解引用运算的语法糖，因此对于指针，我们也可以使用下标运算符：

```c
int main() {
 	int arr[3] = { 1, 2, 3 };
    int* ptr = &arr[2];
    printf("%d", ptr[-1]);		// 负数下标当然也是合法的，它等同于 *(ptr - 1)，输出 2
    printf("%d", ptr[-2]);		// 输出 1
    return 0;
}
```

至此我们可以认识到，下标反映的是偏移量，这也就是为什么数组的下标从 0 开始，因为数组的首位的地址偏移量是 0。实际上，**C** 中引入了 **动态数组（Dynamic Array）** 的概念，我们可以在 **堆（Heap）** 上分配连续的一段内存，然后将其当作数组使用：

```c
int main() {
    int* ptr = malloc(10 * sizeof(int));	// 分配了 10 个 sizeof(int) 大小的内存
    for (int i = 0; i < 10; ++i) {
     	ptr[i] = 10;			// 将这 10 块内存赋值为 10   
    }
    printf("%d", ptr[3]);		// 输出 10，像数组一样操作
    return 0;
}
```

动态数组的好处在于，不需要为变量声明确切的数组大小，只需要按照需求用 `malloc` 函数为其分配足够大小的内存即可。

有了对下标运算符和指针这样的认识，也就不难理解下面的用法了：

```c
int main() {
 	int* arr = malloc(3 * sizeof(int));
    arr[0] = 1;
    1[arr] = 2;					// 等同于 arr[1] = 2
    0[arr + 2] = 3;				// 等同于 arr[2] = 3
    return 0;
}
```

我们已经证实指针可以像数组一样使用，那么数组是否也可以像指针一样使用呢？答案是肯定的，数组在绝大多数表达式（除了比如取地址、`sizeof` 表达式等）中都会被视作指向其首个元素的指针，这被称为数组的 **退化（Decay）**（这个名词实际上常用于 **C++**，这里是我根据个人喜好的沿用）。所以实际上我们前面见到的 `arr[k]` 的求解过程都是：

1. 将 `arr` 退化为其首个元素的指针，比如可能是 `int*` 类型的指针 `ptr`
2. 将语法糖展开，即将 `ptr[k]` 解释为 `*(ptr + k)`
3. 对指针 `ptr` 进行算术运算，随后进行解引用运算

再进行延伸，形如 `arr + 1` 的表达式都是合法的，其等价于 `&arr[0] + 1`。实际上，我们甚至可以将数组初始化的语句稍作修改，让它直接退化成一个指针：

```c
int main() {
    int* ptr = (int[]) { 1, 2, 3 };	// 没有问题，将右侧的数组字面量退化为指针
 	return 0;   
}
```

数组类型的退化也常见于函数调用。作为参数传入或作为返回值返回时，数组也会退化为指针。这实际上带来了一个问题：不可能直接将一个数组作为函数参数或返回值，它的大小信息会在这一过程中丢失。下面是一些解决方案：

```c
int sum1(int arr[5]);		// 会产生一个警告，因为这个声明等同于 int sum1(int* arr)
int sum1(int* arr, unsigned long size);	// 可以传入一个数组首个元素的指针，以及数组的长度
typedef struct array_t {
    int* arr;
    unsigned long size;
} array_t;
int sum2(array_t arr);		// 也可以定义一个结构体，其中包含了数组首个元素的指针，以及数组的长度
int generate_array1()[5];	// 错误，数组不能作为函数返回类型
void generate_array2(int** arrptr, unsigned long* size);	// 可以将返回值作为参数，传入它们的地址
array_t generate_array3();	// 也可以返回一个结构体变量
```

**C** 语言将字符串定义为一个以 `'\0' `结尾（这是为了能知道一个字符串的长度）的字符数组，这是非常有洞见的设计，它用了极小的代价设计了一个字符串模型。

```c
int main() {
 	char arr[] = "Hello, world!";	// 右侧等价于 { 'H', 'e', 'l', 'l', 'o', ..., 'd', '!', '\0' }
    char* cstr = "Immutable array";	// 可以直接将字符串赋给一个字符指针，但以这种方式初始化的字符串是不可变的
    arr[0] = 'B';			// 可以修改字符数组的内容
    printf("%s", arr);		// 输出 "Bello, world!"
    cstr[0] = 'M';			// 错误，尝试修改一个只读的内存块
}
```

使用数组类型字符串和指针类型字符串的微妙差异可能会令人有些不适，同样采取这种设计的 **C++** 则强制后一种字符串声明中加上 `const` 修饰符。**C** 引入这个关键字也是后来的事情，为了兼容之前的代码只能让普通的 `char*` 也能引用一段不可变的字符串。

可以定义多维数组，其定义方式和特征可以从语法上推断出来：

```c
int main() {
    int mat1[3][2] = { {1, 2}, {3, 4}, {5, 6} };
    int mat2[2][2] = { 1, 2, 3, 4 };
    printf("%d", mat1[0][1]);	// 输出 2
    printf("%d", mat2[1][1]);	// 输出 4
    return 0;
}
```

多维数组其实是一个语法糖，其本质也是一维数组。在进行下标运算时，其依然遵循着数组退化的机制，比如 `mat1[0][1]` 等价于：

```c
// mat1 的类型是 int[3][2]，它会自动退化为 int (*)[2] 类型，也即指向 int[2] 的指针
// 因而 mat1[0] => *(mat1 + 0) 的类型就是一个数组
// mat1[0] 会自动退化为 int* 类型，也即指向 int 的指针
// 因而 mat[0][1] => *(*(mat1 + 0) + 1) 的类型就是一个整数
```

### 函数指针

**C** 语言中的函数并不能赋值给一个变量，但是可以求一个函数的地址并复制给一个函数指针变量：

```c
void foo();
int add(int, int);

int main() {
    void (*foo_ptr)() = &foo;		// result_t (*)(arg1_t, ..., argk_t) 是函数指针的类型
    int (*add_ptr)(int, int) = &add;
    (*foo_ptr)();					// 将函数指针解引用之后调用，相当于调用 foo()
    (*add_ptr)(1, 2);				// 相当于调用 add(1, 2)
 	return 0;
}
```

函数指针有一个奇特的性质，它可以自动进行解引用。所以上面的例子中，可以直接使用 `foo_ptr()` 和 `add_ptr(1, 2)`，这是一个非常方便的语法糖。同时，函数在表达式中会自动退化为函数指针，因此在定义函数指针时，右侧不需要使用取地址运算符。从这个角度看，除了声明处，函数指针和函数的行为是一致的，但反而能够随时被同样签名的函数赋值，非常灵活。

```c
void hello() {
    puts("Hello");
}
void nihao() {
    puts("Ni hao");
}
void bonjour() {
    puts("Bonjour");
}
void greetings(void (*grtn)()) {
 	grtn();   
}
int main() {
	greetings(hello);
    greetings(nihao);
    greetings(bonjour);
    return 0;
}
```

### 结构体类型

结构体类型并非单纯是其定义处给定的标识符，而是同时包含 `struct` 关键字和标识符的一个标签。通常的做法是利用类型别名来简化这个标签：

```c
typedef struct MyStruct {
    int i;
    int j;    
} MyStruct;        
```

结构体变量的初始化方式非常类似于数组的初始化，使用大括号以及一系列按照顺序排列的值：

```c
typedef struct MyStruct {
    int i;
    char* str;
} MyStruct;

int main() {
    MyStruct ms = { 10, "abc" };    
	return 0;
}            
```

也可以使用一种比较灵活的初始化方式：

```c
typedef struct MyStruct {
    int i;
    char* str;
} MyStruct;

int main() {
    MyStruct ms = { .str = "abc", .i = 10 };	// 可以以任何顺序初始化各个成员，可以省略部分成员
    return 0;    
}                    
```

对比数组的特殊初始化方式，可以猜测 **C** 语言对聚合类型的特殊初始化语法规律，即可以在大括号中使用省略了当前变量的下标运算及成员访问。

### 变量的声明及别名

现在我们已经见识了 **C** 语言中每一个类型分类的声明语法（这里的声明包括了变量定义），其中有些初见时相当令人迷惑。比如数组类型 `type arr[K]` 中数组的元素类型在标识符的前面，而数组的元素个数则在标识符的后面给出；函数指针类型，如 `void* (*func_ptr)(int)` 更是修饰关系复杂。或许比较典型的（比如上面的这两个例子）并不会造成阅读困难，但当类型极为复杂时，比如 `void (*)(unsigned) (*fptr)(int (*vec)[3])` 究竟是什么类型就不好推断了。遇到一个声明时，究竟应该如何阅读它的类型，这在 **C** 语言中也是一门必修课。

这门课的核心思想就是：以阅读表达式的方式去理解变量声明，将变量视为一个未知类型，然后逐步进行运算直到编程基本的类型定义 `type var` 为止，然后再逆向推出原来的类型。下面我们给出几个例子：

```c
int i;		// int 类型，这一开始就是最简状态
int *p;		// *i 是对 i 进行解引用，它是 int 类型，因此 i 是一个 int 的指针类型
int arr[3];	// arr[3] 是对 arr 用下标运算符，它是 int 类型，因此 arr 是一个 int 的数组类型，大小为 3
			// arr 这里其实也可以是一个指针，但这里按照标准就是声明一个数组类型
void (*fptr)(int);	// *fptr 是对 fptr 进行解引用，然后 (*fptr)(int) 是让 *fptr 对 (int) 进行调用，其结果是一个 void 
					// 类型，因此 fptr 是一个指向 void (int) 的函数指针
int (*(*idputptr)(int (*)(char*)))(char*);
    // *idputptr 是对 idputptr 进行解引用，然后 (*idputptr)(int (*)(char*)) 让 *idputptr 对 (int (*)(char*)) 进行
    // 调用，然后对结果解引用再对 (char*) 进行调用得到 int
    // 所以…… 这是一个指向接收一个“指向接收 char* 并返回 int 的函数指针”并返回同种类型的函数指针
```

可以在同一行中声明多个变量，但是要保证最外层的类型是一致的。比如整数、整数指针、整数数组以及返回类型为整数的函数指针都可以在同一行中声明：

```c
int MyInt, *MyPtr, MyArr[3], (*MyFptr)(void);	// 声明了四种不同类型的变量，它们的“最外层”类型都是 int
```

因此虽然我们会说指针的类型是 `type*`，数组的类型是 `type[K]`，但他们在声明处表现得相当令人感觉微妙，就仿佛它们需要经过运算后是 `type` 类型一样。这种设计方式实际上是为了编译器的方便，让变量声明和变量的运算拥有一致的运算符优先级，因此牺牲了可读性。我们可以利用别名声明语句来简化一些复杂的类型。

声明别名的方式和声明变量的语法是一致的，只不过这里变量就是别名的名称，然后需要用到 `typedef` 关键字：

```c
typedef int MyInt;				// 声明了等价于 int 的类型 MyInt
typedef int *MyPtr;				// 声明了等价于 int* 的类型 MyPtr
typedef int MyArr[3];			// 声明了等价于 int[3] 的类型 MyArr
typedef void (*MyFptr)(int);	// 声明了等价于 void (*)(int) 的类型 MyFptr
```

为什么说它和声明变量是一致的，观察同时声明多个别名的语法：

```c
typedef int MyInt1, MyInt2;		// 相当于同时将 MyInt1 和 MyInt2 都声明为 int 的别名
typedef int *MyPtr, MyArr[3];	// 相当于将 MyPtr 声明为 int* 的别名，将 MyArr 声明为 int[3] 的别名
```

可以发现 `typedef` 和 `extern`、`static` 这样的关键字使用完全相同的语法。
