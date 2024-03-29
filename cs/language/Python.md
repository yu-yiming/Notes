# Python 学习

**Python** 是一门近些年非常热门的脚本语言，它以简洁易懂的语法和强大的社区支持受人喜爱。比起传统的编程语言，如 **C++**、**Java** 等，它在开发速度上有天然的优势。**Python** 的官方文档已经非常详实，包括一个新人友好的[教程](https://docs.python.org/3/tutorial/index.html)，不过我打算记录一些 **Python** 库的使用教程，本篇中会将它们混合在一起。

[TOC]



## 基础

**Python** 并不像 **C++**、**Java** 那样需要提前进行编译再执行，而是运行在 **解释器（Interpreter）** 上，每输入一段代码它就可以执行一段代码，这也是它开发快的原因之一（可以迅速看到代码的部分结果）。下面是一个 Hello World 程序：

```python
print("Hello, world!")
```

### 游标卡尺

**Python** 拥有和 **C** 风格类似的语法，比如 `if` 分支语句、`while` 循环语句、函数调用等，如果对 **C/C++**、**Java**、**C#** 等熟悉的朋友可以快速写出类似的 **Python** 代码。唯一需要注意的是，在 **Python** 中不再使用大括号来标明代码块（同时也不需要小括号来标明条件表达式），而是使用 **缩进（Indentation）**：

```python
x = 42                 # 定义变量
if x == 42:            # 用冒号来表示代码块开始
    print("Take this branch")     # if 代码块内部
    print("Inside the block")     # 依然在 if 代码块内部
print("Out of the if branch statement")   # 离开了 if 代码块
```

这一方面展现了 **Python** 代码的简洁，也意味着不能随意填入空格或对代码换行来整理代码格式，属于有利有弊。游标卡尺是对 **Python** 这种强制性缩进管理的一种揶揄。

### 数学计算

**Python** 解释器内置了强大的数学计算功能，我们可以进行任意精度及大小的整数运算以及任意大小的浮点数运算：

```python
x = 99999999
print(x ** 99)         # ** 表示乘方，结果可能会吓你一跳
```

在命令行中，我们可以直接进行计算（不需要绑定到变量上）：

```python
>>> 3 * 3
9
>>> _                  # 使用 _ 来得到上次直接进行数学计算得到的值
9
```

### 字符串

