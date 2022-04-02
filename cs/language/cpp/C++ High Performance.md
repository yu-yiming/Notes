# C++ 高性能笔记

[TOC]

本篇是 *C++ High Performance, 2nd Edition (Bjorn Andrist, Viktor Sehr)*  的阅读笔记，按照原书章节划分。

## C++ 简介

### 为什么选择 **C++**

**C++** 是一门高度 **可移植的（Portable）** 的语言，提供 **零成本抽象（Zero-Cost Abstraction）**。此外，利用 **C++** 能够编写大型的、富有表达力且健壮的程序。

#### 零成本抽象

当代码量庞大时，我们需要一些语言特性来让编码变得简单。比如 **C** 语言引入的变量、函数等概念，以及 **C++** 引入的类、模版等特性，它们都将顺序执行的代码 （过程式的编程范式）包装、抽象，使其更易于编写。这些包装进行“解包装”时通常只需付出很小的运行时代价，因此我们称其为零成本抽象。

##### 机器码抽象

**C++** 在历史上深受 **C** 的影响，因此其和底层硬件交互的特性，如 **指针（Pointer）**。不过，如今的 **C++** 和 **C** 差别相当之大。让我们用一个统计特定词频的例子来展示：

```c
// C
#include <string.h>
typedef struct string_elem_t { 
    char const* str; string_elem_t* next; 
} string_elem_t;
int count_word(string_elem_t* article, char const* word) {
 	int ct = 0;
    string_elem_t* it;
    for (it = article; it != NULL; it = it->next) {
     	if (strcmp(it->str, word) == 0) {
         	++ct;   
        }
    }
    return ct;
}
```

```cpp
// C++
#include <algorithm>
#include <string>
int count_word(std::forward_list<std::string> const& article, std::string const& word) {
 	return std::count(article.cbegin(), article.cend(), word);   
}
```

可以看到 **C++** 的实现利用了标准库中的模版类，包装了指针（`std::forward_list` 和 `std::string`），并使用 `std::count` 函数代替循环。

##### 其它语言中的抽象

