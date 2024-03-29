# System Programming

本笔记是对应 *UIUC CS 241 System Programming* 的学习笔记，主要参考资料是这门课的 [Coursebook](https://cs241.cs.illinois.edu/coursebook/index.html)，其中包括了 **C** 语言、计算机系统、多线程编程、网络和计算机安全相关的知识。

[TOC]



## 基础知识

在正式开始系统编程相关内容前，让我们先回顾关于计算机架构的知识和编程工具的使用。

### 计算机架构

- **汇编语言（Assembly）**：它们是机器码的另一种形态。所有的 **C** 代码会首先转变成汇编语言，然后再一对一转化为机器码执行。
- **原子操作（Atomic Operation）**：如果某个指令是一个原子操作，任何其它的处理器就不会中断它。我们可以为汇编语句中加上原子操作的限制，比如 **x86** 中使用 `lock`，这样在一个处理器进行某个指令时，就不会因为其它处理器使其中断，再访问未经更新的数据导致错误。不过原子操作有一定的效率问题，所以其使用与否值得探究。
- **缓存（Caching）**：缓存是计算机科学中最有趣的问题之一。通常，当我们想要访问内存时，我们首先会访问缓存中对应的位置。如果数据存在，就只对缓存进行操作（因为它速度很快），否则就从内存中将一大块临近的数据复制到对应的缓存区域，此时此处原来的数据就被覆盖了。因此，计算机中会存在同一份数据的至少两份复制。缓存会让你的程序受到下面的影响（积极或消极）：
  - **竞争条件（Race Condition）**：如果某个数据同时在两个处理器的缓存，那么这个数据只能被一个处理器访问（否则无法同时进行更新）。
  - 速度：缓存会显著提升程序的速度，因此建议善用 **局部性（Locality）** 来利用缓存的高效性。
  - 副作用：每一次读写操作都会影响缓存的状态。
- **中断（Interrupt）**：中断在系统编程中无处不在。任何送往处理器的电子信号都被视为 **硬件中断（Hardware Interrupt）**。在那里，硬件会判断是否应该处理（比如一些老式的键盘和鼠标操作），或者交给操作系统处理。操作系统再判断是否应该由自己处理（比如给硬盘分页）或交由应用处理（比如 `SEGFAULT`）。后一种情况下，系统会向应用层层传播 **软件错误（Software Fault）**；软件判断这是一个错误（如 `SEGFAULT`）或与否（如 `SIGPIPE`），后者会被交给用户处理。此外，应用也可以向操作系统内核和硬件发送信号。中断在系统调用（一种特殊的 **C** 函数，我们会在后文中介绍）中有重要作用，当参数都被准备好时，会调用中断指令（**x86** 中是 `int 80`），系统会触发中断，然后让内核捕获并执行系统调用。因此，系统调用比常规的 **C** 库函数要更加低效。

### 调试与环境

- **Shell**：我们是如何运行程序的？**Shell** 就是调用我们应用程序的软件，它运行在 **终端（Terminal）** 上，后者则是一个用于输入指令的可视化窗口。除了 **POSIX** 中的 **sh**，我们也常常使用同样遵守 **POSIX** 标准的 **bash**，其它诸如 **zsh** 会有更强大的功能。
- **SSH（Secure Shell）** 是一个让你在远程机器上启动 **Shell** 的网络协议。在 **UIUC** 的一些 **CS** 课中，我们通过 **SSH** 来连结虚拟机。如果是 **Windows** 的用户且使用 **WSL（Windows Subsystem for Linux）** 的，也可以在 **VS Code** 等编辑器中启用 **SSH** （可以找到相关的插件）来启用 **WSL** 中的环境。
- **Git** 是一个分布式版本控制系统，它可以存储一个文件夹，称为 **仓库（Repository）** 的所有历史信息，包括每一次添加、删除或修改文件的时间、操作者和内容变化。我们也可以建立远程仓库（比如 **GitHub**），将代码存储在多个地方。我为 **Git** 的使用额外写了一篇笔记，可以参考其用法。
- 编辑器的选择有很多，比如 **Shell** 风格的 **Vim**。对于新手来说它可能有点过于令人迷惑，但是熟练之后其好用程度是惊人的。**Emacs** 则是以强大著称，其功能堪比一个操作系统。不过，一些更加轻量的编辑器，如 **Atom**、**Sublime**、**VS Code** 等可能更易于上手，界面也更加美观（或许有争议）。
- **Valgrind** 是一个提供调试和运行时问题检测（如内存泄露）的工具集。对于像 **C** 这样需要频繁手动管理资源的语言，调试时非常需要这样的工具。
- **TSAN** 是一个谷歌的工具，用于检测代码中存在的竞争条件。
- **GDB（GNU Debugger）** 是一个可交互的调试工具。我们可以在 **GDB** 中为程序设置断点、逐行执行代码、分析汇编代码、实时查看变量的值（且可以用其进行类似于 **C** 语法的运算）。我为 **GDB** 的使用额外写了一篇笔记，可以参考其用法。
- **strace** 和 **ltrace** 是用来跟踪程序每一次系统调用和库函数调用的工具。
- `printf` 总是 **C** 程序员最好的伙伴。在单线程程序中，我们总是可以通过 `printf` 判断程序中发生了什么错误。多线程中由于竞争条件，我们可能需要 **TSAN** 的帮助。

### **C** 语言回顾

**C** 语言在 1973 年于贝尔实验室诞生，其创造者是 *Dennis Ritchie* 与 *Ken Thompson*。它出现的动机是希望有一个能移除部分底层操作，能够以过程式编写代码，与此同时还能和系统交互的编程语言。最开始时，**C** 被用于 **UNIX** 操作系统，随后便如我们所见遍地开花。由于早期的 **C** 缺少官方标准，不同平台的 **C** 语言的特性集和库大不相同。后来，**ANSI** 和 **ISO** 都推出了 **C** 语言的标准（后者在此似乎更加权威），我们本篇中用的 **C** 库遵守的是 **ISO** 标准的一个扩展，称为 **POSIX（Portable Operating System Interface）** 。需要注意的是 **Linux** 并不是完全遵守 **POSIX** 的系统。此外，本篇中的 **C** 特性主要是 **ISO** 的 **C99** 标准，偶尔会用到 **C11** 和 **GNU C** 标准库中的特性。

**C** 语言的优势在于下面几点：

- 快。能和 **C** 程序运行速度比肩的语言屈指可数。
- 简单。**C** 的特性和标准库比较轻量。
- 手动内存管理。这有助于和系统进行更为直接的互动，但考虑到潜在的错误，这也可能是一个缺点。
- 易调用。**C** 借助 **对外函数接口（Foreign Function Interface）**，可以和许多其它语言相互调用库函数。

我额外写了一篇 **C** 语言的概览笔记 ，不过下面还是打算简单过一遍 **C** 语言的特性，毕竟它确实特性很少。首先是一个 hello world 程序：

```c
#include <stdio.h>
int main(void) {
    printf("Hello World\n");
    return 0;
}
```

让我们来分析一下这段代码：

- 第一行的 `#include` 语句是一个 **预处理指令（Preprocessor Directive）**。它将后面的文件 `stdio.h` 中所有内容复制到这一行。
- 第二行的 `int main(void)` 是一个函数声明，它声明了 **主函数（Main Function）** 的参数列表（空，即 `void`）和返回类型 `int`。这里需要注意，和 **C++** 等语言不同，如果函数声明为类如 `void foo()` 而不是 `void foo(void)`，它可以接收任意数量的参数（虽然无法使用）。主函数除了可以声明为 `int main(void)` 外，标准允许的还有 `int main()` 和 `int main(int argc, char *argv[])`。
- 第三行的 `printf("Hello World\n")` 进行了函数调用。`printf` 是一个声明于 `stdio.h` 的库函数，用于将信息输出到标准输出流 `stdout` 中。我们之后会经常使用它。注意 `printf` 是一个带缓冲区的输出函数，这意味着如果我们手动让它 `flush`（调用 `flush(stdout)`），它不会立即输出信息。不过，换行符 `\n` 和 `flush` 有等同的作用。
- 第四行的 `return 0` 是一个返回语句。注意到主函数的声明中，返回类型是 `int`。习惯中，返回 `0` 代表程序正常运行完毕。

为了编译并运行上面的代码，我们可以在命令行中输入：

```shell
gcc main.c -o main && ./main
```

这段指令也值得解释一下：

- `gcc` 是一个编译器集，可以编译 **C** 程序。`-o` 和后面紧跟的参数表示编译得到的可执行文件名称。
- `&&` 是一个运算符，当且仅当前面一个指令执行成功后才会执行后面的指令。
- `./main` 让 **Shell** 执行当前目录下名为 `main` 的程序。
