

# Haskell 学习

[TOC]

**Haskell** 是一个通用的静态类型函数式编程语言。除了函数式语言普遍拥有的惰性求值、纯函数、柯里化等特征外，它还以强大抽象的 **类族（Type Class）** 特性闻名，并因此有极强的抽象能力。**Haskell** 得名于著名的数学家 **Haskell Brooks Curry** 。第一个稳定可移植并广泛使用的 **Haskell** 版本是上世纪末的 **Haskell 98**，随后在 2009 年发布的 **Haskell 2010** 标准对语言进行了部分重要的更新。

**Haskell** 在程序风格上相当简约且富有表达力。它只定义了很少的关键字，并广泛运用各种运算符，且支持几乎任意的运算符重载（作为对比，**C++** 对运算符重载的限制极多，比如只允许特定运算符、继承其原来的结合性和优先级、只允许作为成员定义等）。

网上有一个相当不错的 [**Haskell** 教程](http://learnyouahaskell.com/chapters)，可以作为参考。

## 基础知识：基础中的基础

我们假定大家已经熟悉了 **程序（Program）**、**源文件（Source File）**、**标识符（Identifier）**、**关键字（Keyword）**、**运算符（Operator）**、**变量（Variable）**、**类型（Type）** 等概念。但是依然有必要为 **Haskell** 中的一些基本特性进行讲解，以减少后文介绍中的赘言。

**Haskell** 的源文件通常是以 `.hs` 或 `.lhs` 为后缀的文件（前者更为常见），通过 `ghc` 编译器转化为可执行文件，也可以使用解释器 `ghci` 对代码进行测试。后者提供了比较实用的一些工具，可以在命令行和程序交互并追加变量，比较适合用来演示程序效果。不过除非另有提及，本文中所有的例程都是在源文件中代码。

**Haskell** 拥有静态类型系统，因此所有变量的类型在编译时都能够知晓。我们可以直接通过 `= ` 定义一个变量：

```haskell
-- = 是定义符，而 :: 是类型声明符
x = 10 :: Int
```

这样就定义了一个整数，它的值是 `10`。事实上这里的类型 `Int` 可以被省略，但是此时 `x` 的类型会变得比较复杂，此处暂且不表。更流行的一种写法是分离变量的 **声明（Declaration）** 和定义，我们后文会主要采用这种形式：

```haskell
-- 和上面例子的效果是一样的
x :: Int
x = 10
```

注意与大多数非函数式语言不同，**Haskell** 中的变量一经定义不能修改，因此 `=` 并没有赋值运算符的含义。

一个 **Haskell** 程序是由一系列变量的定义（以及类型和类族的定义）组成的。每个变量的定义（等号右侧）都是一个表达式，这个表达式的值形成了该变量的定义。比如上面的例子中，等号右边的表达式求解所得的（一个值 `10`）就是变量 `x` 的定义。包括后文会提到的函数也是类似的情形。

最后，本文中可能会出现一些 **Haskell** 的原生对象或库函数的声明（或定义），但它们仅用于展示，并不是源文件中需要写出来的。

## 基础知识：类型和类族

**Haskell** 有一系列基础类型（或内置类型），如 `Int`（整数）、`Integer`（无边界整数）、`Float`（浮点数）、`Double`（双精度浮点数）、`Char`（字符）、 `Bool`（布尔）、`()`（单元），以及其它的一些基础类型。**Haskell** 中很多类型都是以上面这些类型为基础构建的。下面是一些典型的复合类型：

- 函数类型：通过 `->` 符号连接参数类型和返回值类型，比如 `Int -> Int -> Int` 是一个可以接收两个 `Int` 并返回一个 `Int` 的函数类型
- 列表类型：通过 `[]`符号框住一个类型，比如 `[Char]` 是一个字符列表类型。**Haskell**  的字符串类型 `String` 就是 `[Char]` 的别名
- 元组类型：通过 `()` 符号框住一系列逗号分隔的类型，比如 `(Int, Bool)` 是一个二元组类型，其中第一个元素是 `Int` 类型，而第二个元素是 `Bool` 类型。

在类型之上，**Haskell** 中还存在 **类族（Type Class）** 的概念。类族是一种类似于 **Java** 中的 **接口（Interface）**、**C++** 中的 **概念（Concept）** 的一种特性（虽然有一定差别）。类族规定了一个类型的行为，即需要为该类型定义一些函数。我们暂时不展开讲这个部分，但先罗列出一些常见的类族：

- 相等类族：`Eq`。该类型需要能够进行等号 `==` 以及不等号 `/=` 操作，即两个该类型的对象可以判断相等关系。
- 序类族：`Ord`。该类型需要能够进行比较运算 `<`、`<=`、`>`、`>=` 且属于相等类族。
- 显示类族：`Show`。该类型需要能够使用 `show` 函数转换为字符串。
- 读取类族：`Read`。需要能够使用 `read` 函数从字符串类型转化为此类型。这个函数比较特殊，我们之后还会提到。
- 数值类族：`Num`。需要该类型属于相等类族、序类族，且能进行加减乘等算术运算。

我们会在后文接触到类族的使用，到时候再逐步讲解它的意义和用法。

## 基础知识：列表

### 列表的结构与空列表

**Haskell** 支持一种可扩展的 **同质（Homogeneous）** 列表结构（可以在表头添加或删除元素的，存放同种类型对象的线性结构）。列表可以通过 `:` 运算符和空列表 `[]` 构建：

```haskell
-- 空列表的声明，它可以是任意的列表类型。这里的 a 是一个范型，即任意类型
[] :: [a]
-- 列表声明：
list :: [Int]
-- : 是一个右结合的运算符，左侧是一个元素，右侧是一个列表。元素类型需要符合列表中的元素类型。空列表 [] 可以是任何类型的
list = 1 : 2 : 3 : []
```

构建列表时，最右侧的 `[]` 千万不能忘记，不然会出现类型错误。

**Haskell** 提供了更易于理解的列表形式，即将元素排列于方括号中：

```haskell
list :: [Int]
-- 下面的定义等价于 list = 1:2:3:[]，或 list = 1:[2, 3] 等写法
list = [1, 2, 3]
```

需要注意的是，`[]` 并不等价于不存在，所以 `[[]]`、`[[], [[]]]`、`[]` 是完全不同的东西:

- `[[]]` 是一个列表的列表，其中 `[]` 是一个元素（空列表）。
- `[[], [[]]]` 是一个列表的列表的列表，其中有两个元素（空列表以及只包含一个空列表的列表）。这里前面的空列表对应的其实是列表的列表类型。
- `[]` 是一个列表，其中没有任何元素。

如果用 **Haskell** 语言来表示上面的几个对象的类型（注意这不是合法的 **Haskell** 代码）：

```haskell
[[]] :: [[a]]
[[], [[]]] :: [[a]]
[] :: [a]
```

### 列表操作

后面例子都将使用下面列出的列表：

```haskell
list :: [Int]
list = [1, 2, 3]
list_a :: [Int]
list_a = [1, 2]
list_b :: [Int]
list_b = [3]
list_result :: [Int]
x :: Int
```

可以通过 `:` 运算符生成一个在列表头添加了新的元素的列表：

```haskell
-- 运算符 : 的声明。同样，a 是一个范型，注意前后的 a 需要保持一致
(:) :: a -> [a] -> [a]
-- 举例
list_result = 0 : list_a         -- list_result = [0, 1, 2]
```

通过 `++` 运算符合并两个列表生成新的列表，第一个列表的表尾接第二个列表的表头：

```haskell
-- 运算符 ++ 的声明
(++) :: [a] -> [a] -> [a]
-- 举例
list_result = list_a ++ list_b    -- list_result = [1, 2 ,3]
```

利用 `length` 和 `null` 函数可以知道列表的元素数量信息：

```haskell
-- 下面两个函数的声明中，Foldable 是一个类族，也即对 a 类型的限制，这里要求 a 满足类族 Foldable
-- 我们先不深究这个类型的特征，只需要了解所有列表类型都满足这个要求即可
length :: Foldable a => a -> Int
null :: Foldable a => a -> Bool
-- 举例
l = length list         -- l = 3
empty = null list       -- empty = false
```

用 `!!` 运算符访问任意位置的元素：

```haskell
-- 运算符 !! 的声明
(!!) :: [a] -> Int -> a
-- 距离
a = list!!1             -- a = 2
```

此外，`head`、`tail`、`last`、`init` 等函数可以得到列表的特定元素或子列表：

```haskell
head :: [a] -> a
tail :: [a] -> [a]
last :: [a] -> a
init :: [a] -> [a]
-- 举例
x = head list               -- a = 1
list_result = tail list     -- list_result = [2, 3]
x = last list               -- a = 3
list_result = init list     -- list_result = [1, 2]
```

最后，`take` 和 `drop` 也是常用的提取子列表的函数：

```haskell
take :: Int -> [a] -> [a]
drop :: Int -> [a] -> [a]
-- 举例
front = take 2 list         -- front = [1, 2]
back = drop 2 list          -- back = [2, 3]
```

### 范围与无限列表

**Haskell** 有一些十分好用的构造列表的语法糖，其中之一是 **范围（Range）**。我们可以通过指定首个元素、（可选的）次个元素以及（可选的）最后一个元素（实际上不一定包含这个元素）来构建一个列表：

```haskell
-- [first..last] 将构建列表 [first, first+1, ..., last]
range1 :: [Int]
range1 = [1..5]            -- range1 = [1, 2, 3, 4, 5]
-- [first, second...last] 将构建列表 [first, first+step, first+2*step, ... first+k*step]
-- 其中 step = second-first，且 k 是最后一个使得 first+k*step <= last 成立的数
range2 :: [Int]
range2 = [1, 3..9]         -- range2 = [1, 3, 5, 7, 9]
-- step < 0 的情况依然适用，只不过 k 是最后一个使得 first+k*step >= last 成立的数
range3 :: [Int]
range3 = [9, 7..1]         -- range3 = [9, 7, 5, 3, 1]
```

不难发现，范围这种结构默认次个元素为首个元素后继（即加一），因此如果需要构建公差为 `-1` 的列表，我们依然需要显式给出第二个元素。除了整数之外，浮点数也支持范围。只不过局限于浮点数的精度，可能会出现存储的数据不精确，甚至存入多余数据的情况：

```haskell
rangef :: [Float]
rangef = [0.1, 0.3..1.0]    -- rangef = [0.1,0.3,0.5,0.7,0.8999999999999999,1.0999999999999999]
```

对此没有很好的预防手段，因此最好避免对浮点数列表使用范围。

前文我们提到了，范围的最后一个元素是可选的。当省略这个元素时，我们就构建了一个无限列表。这个列表可以通过首个元素和 `step` 构建后面所有的元素。

```haskell
natural :: [Int]
natural = [1..]             -- natural = [1, 2, 3, ...] 是无限长的列表，在解释器中会不断地打印出数来
odds :: [Int]
odds = [1, 3...]            -- odds = [1, 3, 5, 7, ...] 也是一个无限长的列表
range1 :: [Int]
range1 = take 3 natural     -- range1 = [1, 2, 3]，实际上也并不会进行求值，此处仅作为说明
range2 :: [Int]
range2 = take 3 odds        -- range2 = [1, 3, 5]
```

无限列表能够存在的原因是 **Haskell** 惰性求值的特点。所有范围在定义时都不会真正地分配内存，只有真正使用（比如 `print`）时才会根据定义进行操作。上面的例程中，实际上到最后都不会真正地进行赋值，只有最后调用输出函数时，注释中写出的值才会被计算出来（详细请见后文的 [惰性求值](##表达式求值)）。

与无限列表相关的有两个函数 `cycle` 和 `repeat`：

```haskell
-- 生成将一个列表中元素重复无数遍的列表
cycle :: [a] -> [a]
-- 生成将一个元素重复无数遍的列表
repeat :: a -> [a]
-- 举例
list_a :: [Int]
list_b :: [Int]
list_a = cycle [1, 2, 3]        -- list_a = [1, 2, 3, 1, 2, 3, ...]
list_b = repeat 42              -- list_b = [42, 42, 42, ...]
```

### 列表概括

熟悉 **Python** 的朋友可能听说过 **列表概括（List Comprehension）** 的概念，它通过一系列谓词和范围生成一个列表，从语法上来看非常简洁。**Haskell** 的列表概括有类似的作用，但是语法颇为不同。实际上它采用了类似于数学语言的格式：

```haskell
-- 列表概括：
-- [expr | arg1 <- range1, arg2 <- range2, ..., argk <- rangek, pred1, pred2, ..., predl]
-- 其中 pred 和 arg <- range 可以混合列出，但是需要注意先后顺序
list1 :: [Int]
list1 = [x*x | x <- [1..5]]        -- list1 = [1, 4, 9, 16, 25]
list2 :: [Int]
list2 = [x*y | x <- [1, 3..7], y <- [2, 4..8], x^2 < y^2]    -- list2 = [2, 4, 6, 8, 12, 18, 24, 30, 40, 56]
-- (x, y, z) 是一个三元组类型，即 (Int, Int, Int)
list3 :: [Int]
list3 = [(x, y, z) | x <- [1..20], y <- [x..20], z <- [y..20], x^2 + y^2 == z^2]
    -- list3 = [(3, 4, 5), (5, 12, 13), (6, 8, 10), (8, 15, 17), (9, 12, 15), (12, 16, 20)]
```

上面第三个是一个典型的求勾股三元组的程序，其含义简单易懂，表达力远远强于其它常见写法。这就是列表概括的威力。

### 字符串

在 **C/C++** 中，原生的字符串就是一个字符数组。**Haskell** 采用了类似的设计，字符串类型 `String` 本身等价于 `[Char]`。因此，经常可以把字符串当作字符列表来处理，非常方便。下面给出一些常见的字符串处理函数。

首先是进行字符串分割的 `words` 函数和拼接字符串的 `unwords` 函数：

```haskell
words :: String -> [String]
unwords :: [String] -> String

-- 距离
list :: [String]
list = words "This is a sentence"          -- list = ["This", "is", "a", "sentence"]
sentence :: String
sentence = unwords list                    -- sentence = "This is a sentence"
```

## 基础知识：元组

**元组（Tuple）** 是一种非同质的有限定长容器，我们可以在一个元组中存储任意类型的组合，且定义时可以直接知道元组中元素的个数（从类型上可以直接看出来）。我们常用元组表示一系列有特殊意义的、结构固定的数据，比如点、错误信息等。我们可以通过 `fst` 和 `snd` 函数来访问二元组中的元素。

```haskell
-- fst 与 snd 的声明
fst :: (a, b) -> a
snd :: (a, b) -> b
-- 举例
points :: [(Float, Float)]
points = [(1.0, 2.0), (-3.5, 0.5), (2.5, -1.0)]
pt :: (Float, Float)
pt = head points         -- pt = (1.0, 2.0)
pt_x :: Float
pt_x = fst pt            -- pt_x = 1.0
pt_y :: Float
pt_y = snd pt            -- pt_y = 2.0
```

有一个比较常用的列表函数 `zip` 可以生成一个元组的列表：

```haskell
-- 如同拉链一样将两个列表融合在一起，生成的列表长度将是两者较短的那个
zip :: [a] -> [b] -> [(a, b)]
-- 举例
colors :: [String]
list :: [(Int, String)]
colors = ["red", "yellow", "green", "blue"]
list = zip [1..] colors            -- list = [(1, "red"), (2, "yellow"), (3, "green"), (4, "blue")]
```

相比列表，元组的操作灵活性比较差（缺少通用的函数），其通常高度依赖于模式匹配。

## 基础知识：函数

### 函数的声明和定义

我们在此前见到了不少预定义的库函数，我们自己也可以定义函数。作为函数式编程语言，**Haskell** 将函数视为一等公民，可以像普通类型的变量一样声明和定义函数：

```haskell
-- 函数声明：
-- function_name :: ParaType1 -> ParaType2 -> ... -> RetType
add :: Int -> Int -> Int
-- 函数定义：
-- function_name arg1 arg2 ... argk = function_expr
add a b = a + b
```

调用函数时，和函数定义的左侧语法一致，即：

```haskell
-- 声明一个整数
result :: Int
-- 等号右侧：
-- function_name arg1 arg2 ... argk
result = add 1 2
```

当然也可以将函数作为函数参数或返回值：

```haskell
-- 在函数声明处，将函数的类型两侧加上括号防止歧义（返回一个函数时不需要括号）
transform :: [Int] -> (Int -> Int) -> [Int]
-- 函数定义的 (x:xs) 是一个 *模式匹配*。它将一个列表匹配为首个元素（x）以及余下的列表（xs）。注意这里无法匹配空列表
transform (x:xs) f = f x:transform xs
-- 匹配空列表的情形，也返回一个空列表
transform [] f = []
```

上面的声明中，`transform` 函数需要接收一个 `Int -> Int` 类型的参数，如果不加括号，在定义函数时就无法正确确认 `f` 的类型（可能是 `Int` 或 `Int -> Int`，实际上会默认为前者）。此外，注意到这个函数拥有“两个定义”，这是因为 **Haskell** 允许对参数进行模式匹配，而每次定义都可以对应一种模式。进行函数调用时，会从头到尾顺序尝试匹配，遇到第一个匹配的模式则进入。

如果使用“单一定义”的规则，可以使用 **护卫（Guard）** 的语言特性，或者利用 `case...of...` 表达式：

```haskell
-- 函数护卫：
-- function_name | boolean_expr1 = function_expr1
--                  | boolean_expr2 = function_expr2
--                 ...
--                  | otherwise = function_exprk
-- 第一个满足的 boolean_expr 后面的表达式会作为函数体
transform' l f
    | null l = []
    | otherwise = f (head l):transform' (tail l) f
-- case...of...表达式，本质也是模式匹配：
-- case expr of
--     pattern1 -> expr1
--        pattern2 -> expr2
--        ...
--        patternk -> exprk
-- 第一个匹配的 pattern 后面的表达式会作为函数体
transform'' l f = 
    case l of
        (x:xs) -> f x:transform'' xs f
        [] -> []
```

需要注意的是，无论是哪一种方案，都需要保证能够匹配给定类型所有可能的输入值。比如如果不处理空列表的情况，编译器会报错。

### 中缀表示

**Haskell** 中，所有的二元函数（拥有两个参数的函数）都可以通过中缀形式定义或使用，让我们以 `add` 为例：

```haskell
add :: Int -> Int -> Int
-- 这里就使用了函数的中缀形式，使用一对反引号，可读性更高。其等价于 add a b = a + b
a `add` b = a + b
```

因此，我们可以反推：**Haskell** 中的很多运算符都是二元函数且采用中缀形式，那么实际上它们也可以通过前缀形式调用：

```haskell
add :: Int -> Int -> Int
add a b = (+) a b
```

需要注意的是，运算符在使用前缀形式时，需要在两端加上括号，否则会出现语法解析错误。

### 柯里化与部分应用

让我们再次回到函数的类型声明。前面有提到，函数的返回类型不需要括号。然而这实际上会让函数的返回类型不明确。比如 `add :: Int -> Int -> Int` 究竟是接受两个 `Int` 类型，然后返回 `Int` 类型的函数，还是接受一个 `Int` 类型，然后返回 `Int -> Int` 类型的函数呢？

答案是都对。如果向上面的函数传入两个 `Int` 类型的数，那么就返回一个 `Int` 值；如果只传入一个 `Int` 类型的数，则会返回一个 `Int -> Int` 的函数，一些语言中也将这个特性称为 **部分应用（Partial Application）**，因为函数并没有被完整调用。下面给出一个简单的示例：

```haskell
-- add1 相当于 add 函数的第一个参数已经给定了，当向 add1 传入一个 Int 的时候，相当于 add 的第二个参数
add1 :: Int -> Int
add1 = add 1

two :: Int
two = add1 1
```

我们把将一个函数变为逐个参数连锁调用的函数的过程称为 **柯里化（Currying）**。**Haskell** 中的所有函数都是柯里化的，也即所有的 **Haskell** 函数都只传入一个参数，然后返回一个函数（或最后一个值）。这个特性可以极大地提高函数的灵活性，很多函数都可以通过其它函数的柯里化产生。

**Haskell** 还提供了一个 **运算符组（Operator Section）** 的语法，它能够对二元运算符产生类似部分应用的效果。但是仔细观察它和部分应用的区别：

```haskell
-- (operator n) m = m operator n
-- (n operator) m = n operator m
prefix :: String -> String -> String
-- 这里 ++ 是一个拼接运算符，可以拼接两个字符串
prefix fix = (fix ++)        -- prefix "abc" "def" = "abcdef"
suffix :: String -> String -> String
suffix fix = (++ fix)        -- suffix "abc" "def" = "defabc"
```

我们会发现，如果用部分应用的思想去看运算符组，`(operator n)` 应该是先调用了 `n`，然后等待第二个参数传入；然而事实是此处的 `n` 是第二个参数，等待的是第一个参数。反而 `(n operator)` 才等价于部分应用 `(operator) n`，这一点比较微妙，需要熟悉一下。

此外，还有一个容易混淆的点。二元函数进行部分调用时和其中缀形式在运算符组中的语法几乎一样，但是行为不同：

```haskell
mod :: Integral a => a -> a -> a
two_mod_n :: Integral a => a -> a
two_mod_n = (mod 2)        -- 相当于 (2 `mod`)
n_mod_two :: Integral a => a -> a
n_mod_two = (`mod` 2)
```

### 高阶函数

**高阶函数（Higher-Order Function）** 是指将函数作为参数或返回值的函数，我们在前面也已经见识过了，比如柯里化后的函数总是只接收一个参数而可能返回一个函数。将函数作为参数也是相当实用的，比如：

```haskell
twice :: (a -> a) -> a -> a
twice f = f . f
x :: Int
x = twice (* 2) 2               -- x = 8
str :: String
str = twice ("Yo " ++) "Yee"    -- str = "Yo Yo Yee"
```

**Haskell** 中有一些相当常用的库函数是高阶函数，我们在这里简单介绍一下：

```haskell
-- map 函数将一个转换函数作用于列表中所有元素上，并返回全部转换过后的列表
map :: (a -> b) -> [a] -> [b]
-- filter 函数将一个谓词（返回 Bool 的函数）作用于列表中所有元素上，返回通过谓词的元素组成的列表
filter :: (a -> Bool) -> [a] -> [a]
-- concat 函数将一个可折叠结构中的所有列表拼接到一起
concat :: Foldable t => t [a] -> [a]
-- foldl 将一个可折叠结构中的元素以某个二元函数和某个初值从左到右进行累计运算
-- 直观举例： foldl (+) 0 [1, 2, 3] = ((0 + 1) + 2) + 3 = 6
foldl :: Foldable t => (b -> a -> b) -> b -> t a -> b
-- foldr 将一个可折叠结构中的元素以某个二元函数和某个初始值从右到左进行累计运算
-- 直观举例： foldr (+) 0 [1, 2, 3] = 1 + ((2 + 3) + 0) = 6
foldr :: Foldable t => (a -> b -> b) -> b -> t a -> b
```

如果还记得之前介绍的列表概括，会发现上面的函数定义等价于：

```haskell
map f list = [f x | x <- list]
filter p list = [x | x <- list, p x]
```

这两种定义哪种更好见仁见智；后面我们会介绍列表概括的底层实现，其本身也是一个高阶函数，只是更加通用。

### lambda 表达式

**Lambda 表达式（Lambda Expression）** 是一种 **匿名函数（Anonymous Function）**，下文中可能会简称为 lambda。通常是因为我们“不屑于”为这个函数起一个名字，只是当场使用（通常是传入一个高阶函数）而构建一个 lambda。让我们用一个例子来熟悉它：

```haskell
-- \par1 par2 ... park -> function_expr
list :: [Int]
list = filter (\x -> x > 10) [1, 100, 2, 99, 3, 98]     -- list = [100, 99, 98]
```

不过敏感的同学可能会发现，上面的 lambda 其实可以换成 `(> 10)`。确实如此，很多简单的 lambda 可以简化为运算符组，或者其它函数的复合，此时要根据代码清晰度和简洁度进行权衡。

lambda 和普通函数的用法大同小异，比如它也支持模式匹配：

```haskell
squash :: [(Int, Int)] -> [Int]
squash = map (\(x, y) -> x + y)
```

它本身也是柯里化的一种体现：

```haskell
form3 :: Int -> Int -> Int -> (Int, Int, Int)
form3 = \x y z -> (x, y, z)
form3' :: Int -> Int -> Int -> (Int, Int, Int)
form3' = \x -> \y -> \z -> (x, y, z)
```

前后两种定义是等价的，而后面这种清晰地展现了柯里化函数的特征。

和传统函数比起来，lambda 很多时候更能体现其函数的本质，比如下面这个 `flip` 函数：

```haskell
flip :: (a -> b -> c) -> b -> a -> c
flip f = \x y -> f y x
flip' :: (a -> b -> c) -> b -> a -> c
flip' f x y = f y x
```

前后两种定义是等价的，但前面这种更能体现 `flip` 接受一个函数，并返回一个函数的特征（因为很可能我们会部分应用 `flip`，此时第一种定义更加“符合”这个函数的作用）。

### 函数的复合和应用

如果对数学中的 **函数复合（Function Composition）** 概念有了解，**Haskell** 中的函数复合基本上是一个东西。对于函数 $f : A \to B$  和 $g : B \to C$ ，则函数 $g \circ f : A \to C$ 称为这两个函数的复合。**Haskell** 中复合运算符 `.` 使用完全相同的形式。

```haskell
-- 运算符 . 的声明
(.) :: (b -> c) -> (a -> b) -> (a -> c)
-- (.) 可能的实现
f . g = \x -> f (g x)
-- 举例
to_string :: Int -> String          -- 定义略
first_character :: String -> Char   -- 定义略
first_digit :: Int -> Char
first_digit = first_character . to_string
```

需要注意的是，如果需要两个函数中任一个存在超过一个参数，那么它们很可能不适用于函数复合。请见下面的例子：

```haskell
add :: Int -> Int -> Int
add = (+)
magic :: (Int -> Int) -> Int    -- 这个声明是我们期望的效果，但实际上不能如愿
magic = negate . add        -- 错误！add 被看作 Int -> (Int -> Int) 类型的函数，但是 negate 并不能处理 Int -> Int
```

可以看到，函数类型在进行匹配的时候并不是贪婪的，因此遇到第一个符合的参数类型，就直接讲接下来的所有东西当作返回类型了。

最后让我们介绍 **函数应用（Function Application）** 的运算符。顾名思义，这个函数做的事情就是函数调用。但是明明函数调用可以通过函数名、空格、参数这样的形式做到，为什么还需要这样的概念呢？让我们来看下面的例子：

```haskell
-- 运算符 $ 接收一个函数并返回其自身
($) :: (a -> b) -> a -> b
-- ($) 可能的实现
f $ x = f x
-- 举例
operation_list :: [Int -> Int]
list :: [Int]
operation_list = [(+ 3), (* 2), (^ 3)]
-- 相当于让 operation_list 中每个函数都调用 2，没有 $ 运算符的话没法实行这样的操作
list = map ($ 2) operation_list     -- list = [5, 4, 6]
```

这里引入 `$` 就是非常必要的。本质上（柯里化后的）函数调用也可以看作一种二元运算，做这样的处理也更有助于归纳程序的行为并建立统一的抽象语法。顺便，回忆运算符组的定义，可以发现 `(op $)` 和 `op` 是等价的。

此外，函数应用运算符也可以用在一些想要避免括号的地方，这是因为 `f (g x)` 和 `f $ g x` 是等价的（其实也和 `(f . g) x` 在特定情况等价），而后者噪声更少（不是所有人都喜欢遍地的括号），因此相当常见。可以举一个例子：

```haskell
add :: Int -> Int -> Int
add = (+)
num = String
num = show $ add 1 2        -- num = "3" 这里和 num = show (add 1 2) 等价
```

而之所以 `f $ g x` 中的 `g x` 会优先被运算，是因为默认的函数调用（如 `f x`）比任何运算符（包括函数应用运算符 `$`）都有更高的优先级，且为左结合的运算。因此虽然做的是相同的事，后面那个却优先进行了，这也变相达到了括号的效果。不过，全然不用括号也是不明智的，我们应该根据实际情况取舍。

## 基础知识：运算与表达式

### 一些常见的运算符和表达式

在深入研究 **Haskell** 之前，我们还应该了解常见的一些表达式。

#### 算术运算

这里我们给出所有 **Haskell** 中算术运算符或相关函数的声明：

```haskell
(+) :: Num a => a -> a -> a
(-) :: Num a => a -> a -> a
(*) :: Num a => a -> a -> a
(/) :: Fractional a => a -> a -> a
mod :: Integral a => a -> a -> a                     -- 求余
(^) :: (Integral a, Num b) => a -> b -> a            -- 整数的幂
(^^) :: (Fractional a, Integral b) => a -> b -> a    -- 有理数的整数幂
(**) :: Floating a => a -> a -> a                    -- 浮点数的幂
negate :: Num a => a -> a                            -- 取负数
sqrt :: Floating a => a -> a                         -- 平方根
abs :: Num a => a -> a                               -- 绝对值
signum :: Num a => a -> a                            -- 正负性
```

#### 比较运算

这里我们给出 **Haskell** 中和比较相关函数的声明：

```haskell
(==) :: Eq a => a -> a -> Bool
(/=) :: Eq a => a -> a -> Bool
(<) :: Ord a => a -> a -> Bool
(<=) :: Ord a => a -> a -> Bool
(<) :: Ord a => a -> a -> Bool
(<=) :: Ord a => a -> a -> Bool
```

#### 逻辑与位运算

**Haskell** 支持基本的逻辑运算，但是位运算相关的函数/运算符定义在包 `Data.Bits` 中。

```haskell
(&&) :: Bool -> Bool -> Bool
(||) :: Bool -> Bool -> Bool
not :: Bool -> Bool
-- 下面的运算符 `.&.` 定义在 Data.Bits 中，需要用 import 语句
Data.Bits.(.&.) :: Bits => a -> a -> a        -- 按位与
Data.Bits.(.|.) :: Bits => a -> a -> a        -- 按位或
Data.Bits.xor :: Bits => a -> a -> a          -- 按位异或
Data.Bits.complement :: Bits => a -> a        -- 按位补
```

#### `if` 表达式

根据一个布尔值决定返回的值，注意给出的两个分支需要是同种类型的表达式：

```haskell
myfunc :: Int -> Int
-- if cond then expr1 else expr2
-- 如果将 if 表达式视作一个函数，其类型大致如下：
-- conditional :: Bool -> a -> a -> a
myfunc n = if n > 100 then n / 10 else n * 10 
```

#### `let` 表达式

建立临时的名称绑定（类似变量），并对表达式进行求值：

```haskell
mynumber :: Int
-- let var1 = val1; var2 = val2; ...; valk = valk in expr
mynumber = let x = 10; y = 20 in x*y - 100        -- mynumber = 100
```

`let` 表达式左侧可以使用模式匹配，这也相当常用：

```haskell
mylist :: [Int]
mylist = let (x:xs) = [1, 2, 3] in xs ++ [x] ++ xs    -- mylist = [2, 3, 1, 2, 3]
```

#### `where` 子句

通常出现在函数的“最外层”表达式，为这个表达式中未经声明的名称进行定义，形式上比 `let` 表达式更加整洁。

```haskell
mynumber :: Int
mynumber = x + y*z          -- mynumber = 14
    where x = y + z         -- 再次出现了未经定义的名称，where 子句会继续向下找
          y = z ^ 2
          z = 2
```

除此之外，也可以在 `where` 子句中定义函数（时刻记住 **Haskell** 中函数和其它的类型没有区别），甚至声明变量的类型：

```haskell
myadd :: Int -> Int -> Int
myadd x y = myadd' x y 0
    where myadd' x y 0 :: Int -> Int -> Int -> Int
          myadd' x _ 10 = x
          myadd' x y n = myadd' (x + y) y (n + 1)
```

### 表达式求值

**Haskell** 代码中充满了各式各样的表达式，而一个 **Haskell** 程序绝大部分工作都是在对表达式进行求值。因此，我们对它们的求值过程有一定兴趣。通常，有两种求值的策略。

现在比如有一个表达式 `f (x + y)`，不难发现有两种求值方式：第一种是计算好 `x + y` 的值，然后代入函数 `f` 中进行求解；另一种是将 `f` 的定义展开，然后对展开式进行求解。前面一种我们称为 **勤奋求值（Eager Evaluation）**，因为它在函数调用/变量赋值前会将所有参数求值完毕；后面一种我们称为 **惰性求值（Lazy Evaluation）**，因为它会一步步展开函数/变量定义，在合适时才进行求值。

很多常见的编程语言，如 **C/C++**、**Java**、**Python** 等都是采用的前面一种，实际上这也更符合我们的直觉；**Haskell** 采用的是后面一种。现在让我们来举例说明惰性求值的一些优势：

```haskell
list :: [Float]
list = map sqrt [1.0, 2.0, 3.0]        -- Haskell 中，我们是通过右边的表达式定义了变量，但是右侧表达式并未进行求值
first :: Float
first = head list             -- 这里实际上依然没有进行求值，但是先假设有一个让它进行求值的函数 eval
eval :: a -> a                -- 这个函数并不存在，我们仅作为演示
first_val :: Float
first_val = eval first
--1 eval first = eval (head list)
--2               = eval (head $ map sqrt [1.0, 2.0, 3.0])
--3            = eval (head (sqrt 1.0 : map sqrt [2.0, 3.0]))
--4               = eval (sqrt 1.0)
--5               = eval 1.0
--6               = 1.0
```

可以看到，**Haskell** 在展开定义的时候，每次都以从外到内的顺序进行。这里为了 `head` 定义能够展开，就先将 `list` 首个元素通过 `map` 定义（可以参考后文的[进阶部分](##进阶：库函数定义示例)）展开，然后就得到了 `sqrt 1.0`。这样就根本不需要对列表的每个元素都进行求值。这个例子或许并不够有说服力，那么来看下面的例子：

```haskell
multiple_of_7 :: [Int]
multiple_of_7 = [0, 7..]        -- 我们并不知道需要的数可能有多大，因此无限列表是最好的选择
list :: [Int]
list = take 10 $ filter (\x -> x `mod` 11 == 5) multiple_of_7
result_list :: [Int]
result_list = eval list         -- result_list = [49,126,203,280,357,434,511,588,665,742]
--1 eval result_list = eval (take 10 $ filter (\x -> x `mod` 11 == 5) multiple_of_7)
--2                     = eval (take 10 $ (49 : filter (\x -> x `mod` 11 == 5) [56, 63, ...]))
--3                     = eval (49 : take 9 $ filter (\x -> x `mod 11 == 5) [56, 63, ...])
-- ...                  ...
--k                     = [0, 7, 14, ..., 98]
```

此处的无限列表显然是没有办法完全求值的，因此惰性求值恰到好处地完成了任务。如果使用勤奋求值，就不得不为输入的列表预定一个上限，这样的实现显然不够优雅……

抛开优雅，我们也总能保证对惰性求值于一个表达式的求值能够使用不低于勤奋求值的步数得到结果。从上面的例子来看，有时惰性求值能够以相当高效的方式进行求值。不过它的缺点也是显然的：这样的定义展开对空间有一定的要求，且求值的顺序并不是很好理解；此外，还有一个涉及到“严格性”的缺点，我们马上就会提到。

### 严格性

**Haskell** 中有一个特殊的范型值，`undefined`，它是一个不能被求值的值，通常会被定义为：

```haskell
undefined :: a
undefined = error "Prelude.undefined"     -- 一旦试图对 undefined 进行求值会报错
```

这个值的类型在类型理论中有一个更广泛使用的名字，**底部（Bottom）**，记作 $\bot$，是所有其它类型的子类型并不包含任何值（作为对比，**顶部（Top）** 类型 `\top` 则是所有其它类型的祖先）。通常，我们将无限循环的定义的值视作底部类型。比如 `let loop = loop + 1 in loop` 中 `loop` 的值就是底部类型的。

一个函数 `f` 是 **严格的（Strict）**，如果满足 `f undefined = undefined`。这似乎是理所应当的，但实际上因为 **Haskell** 的惰性求值，在很多情况下这并不成立，让我们举一些例子：

```haskell
-- 无论传入什么变量，都返回单元类型
eat :: a -> ()
eat _ = ()
-- 无论传入什么数，都返回 1
one :: Int -> Int
one _ = 1
```

由此，**Haskell** 被称为 **非严格语言（Non-Strict Language）**，这会导致一些问题：对 `undefined` 处理不够敏感，很多应该及时报错的地方却一直传递下去甚至没有处理。**Haskell** 有一套机制让值必须是严格的（即不是 `undefined`），我们会在后面合适的时候进行介绍。

## 基础知识：模块

### 导入模块

**Haskell** 的 **模块（Module）** 就是一系列变量、类型、类族的定义的集合。而一个 **Haskell** 程序则是一到多个模块的组合，其中必定有一个 `Main` 模块作为程序核心，其中定义了程序入口函数 `main`。在代码中可以通过在所有声明与定义语句前用 `import` 语句加载外部模块，这个语句有不少附加选项：

```haskell
-- 加载了 Data.List 这个模块，这样这个模块中所有名字都在本模块中可视
import Data.List
-- 加载了 Data.List 这个模块中的 find 和 sort 函数，它们将在本模块中可视
import Data.List (find, sort)
-- 加载了 Data.List 这个模块中除了 find 和 sort 之外的所有函数，它们将在本模块中可视
import Data.List hiding (find, sort)
-- 加载了 Data.List 这个模块，但需要用例如 Data.List.find 调用其中的函数
import qualified Data.List
-- 加载了 Data.List 这个模块，但需要用例如 L.find 调用其中的函数
import qualified Data.List as List
-- 超级杂交
import qualified Data.List as List hiding (find, sort)
```

注意上面的代码仅作参考并不能通过编译，因为一个模块只能被加载一次。通常为了防止命名冲突且保证简洁性，使用 `qualified` 以及 `as` 是比较推荐的做法，此外 `hiding` 在规避特定命名冲突时也比较好用。

### 创建模块

**Haskell** 用非常通俗的方式创建模组：

```haskell
-- module 声明必须是程序的第一句话
module MyModule where
-- 诸多声明与定义
```

或者可以指定哪些内容对外界可视：

```haskell
module MyModule
( function_1
, function_2
, -- 更多的函数名
) where
-- 诸多声明与定义
```

可以构建模块的层级结构，这点和 **Java** 类似，是通过文件夹的层级结构实现的。我们可以在项目根目录的某个文件夹，如 `Core`，中建立子模组：

```haskell
-- ${项目路径}/Core/Submodule
module Core.Submodule where
-- 诸多声明与定义
```

同一个项目下，使用自己的模组只需要和导入标准库中的模组一样，使用 `import` 语句即可。

### 标准库模块：`Prelude`

请参考 **Haskell** 标准库的 [网站](https://hackage.haskell.org/package/base-4.15.0.0/docs/Prelude.html)。这个模块会自动加载到所有 **Haskell** 中的模块中。

### 标准库模块：`Data.List`

请参考 **Haskell** 标准库的 [网站](https://hackage.haskell.org/package/base-4.15.0.0/docs/Data-List.html)。这里面包含了对列表相关的函数。

### 标准库模块：`Data.Char`

请参考 **Haskell** 标准库的 [网站](https://hackage.haskell.org/package/base-4.15.0.0/docs/Data-Char.html)。

### 标准库模块：`Data.Map`

请参考 **Haskell** 标准库的 [网站](https://hackage.haskell.org/package/containers-0.4.0.0/docs/Data-Map.html)。

### 标准库模块：`Data.Set`

请参考 **Haskell** 标准库的 [网站](https://hackage.haskell.org/package/containers-0.6.5.1/docs/Data-Set.html)。

## 进阶：类型与类族

### 抽象数据类型

正如多数高级语言所具备的，**Haskell** 支持自定义类型。我们首先介绍 **抽象数据类型（Abstract Data Type）**，下文简称为 **ADT**：

```haskell
-- 定义抽象数据类型：
-- data DataType = Ctor1 Type11 ... Type1a | Ctor2 Type21 ... Type2b | ... | Ctork Typek1 ... Typekn
-- Bool 类型可能的定义 —— 类似通过枚举定义一个类型。
-- 需要注意，True 和 False 本身没有含义，它们本质是在定义类型 Bool 时定义的构造器
data Bool = True | False
data Shape = Circle Float | Rectangle Float Float

shape1 :: Shape
shape1 = Circle 1.0
```

仔细观察可以发现定义类型时，等号左侧出现的是类型名称，与 `Int`、`[Char]` 等类似可以作为变量的类型；而等号右侧利用符号 `|` 分隔的每一个都是该类型实际的值的结构，我们称它们为 **构造器（Constructor）**。就好像列表类型除了里面的元素，还需要 `[]`包围起来、元组还需要 `()` 包围，ADT 的值必须包含构造器和（零到多个）值。一个 ADT 可能有多种构造器，但是一个 ADT 的值只能通过一种构造器构成。

如果去探究构造器的本质，它其实依然是一个函数。以上面的 `Shape` 为例：

```haskell
-- 下面仅作为类型演示，并不是合法的 Haskell 代码
Circle :: Float -> Shape
Rectangle :: Float -> Float -> Shape
```

这样的类型其实也在意料之中，因为 `Shape` 类型的值就是通过类似函数调用得到的。不过和函数不同的是，构造器不允许部分应用。

模式匹配支持对 ADT 的匹配，通过制定构造器，我们可以对同种 ADT 进行不同的处理，依然以上面的 `Shape` 为例：

```haskell
area :: Shape -> Float
area (Cicle r) = pi * r * r
area (Rectangle w h) = pi * w * h
-- 举例
a1 :: Float
a1 = area (Circle 1.0)         -- a1 = 3.141592653589793
```

### 记录

之前介绍的 ADT 能够满足很多需求，但是因为它的数据项没有名称，在数目较多的时候容易造成困惑且不容易按项访问。此时我们可以使用 **Haskell** 的记录结构，它相当于在 ADT 上增加了每一个数据项的名称：

```haskell
-- 定义记录类型：
-- data DataType = Ctor1 { field1 :: Type1, ..., fieldk :: Typek }
--                  | ...
--                  | Ctorn { field1' :: Type1', ..., fieldk' :: Typek }
-- 这里类型名称和构造器同名，并没有问题，因为使用时不存在歧义
data Person = Person { firstName :: String
                     , lastName :: String
                     , age :: Int
                     , height :: Float
                     , contact :: String
                     }
```

记录类型中所有项的名字都类似面向对象中类的 `getter`，可以用这些“函数”得到相应的值：

```haskell
-- 下面仅作为类型演示，并不是合法的 Haskell 代码
firstName :: Person -> String
lastName :: Person -> String
-- 以此类推
```

### 新类型

关键字 `newtype` 用于定义只有一个构造器且其为单参数的类型，其余和普通的 ADT 一致：

```haskell
newtype SuperList a = SuperList { toList :: [a] }
```

大家在这里可能会感到疑惑，既然和 `data` 声明的类型一样，且功能还更少（不能拥有多个构造器，唯一的构造器仅能单参数），为什么还需要 `newtype` 呢？这也和 **Haskell** 的惰性求值有关。考虑下面两个看起来等同的定义：

```haskell
data Eager = Eager { toBool :: Bool }
newtype Lazy = Lazy { toBool :: Bool }
```

定义两个功能对等的函数：

```haskell
doEager :: Eager -> String
doEager (Eager _) = "eager"
doLazy :: Lazy -> String
doLazy (Lazy _) = "lazy"
```

现在让我们将 `undefined` 作为参数调用这两个函数：

```haskell
ghci> doEager undefined
*** Exception: Prelude.Undefined
ghci> doLazy undefined
"lazy"
```

出现这种区别的原因是，通常来讲 ADT 类型再进行模式匹配时，需要对输入的值进行求值以判断其符合哪一个模式，因此 `undefined` 被求值，出现错误。而 `newtype` 定义的类型只有一种构造器和一个值，因此对于 `Lazy _` 这样的模式，根本不需要对输入进行求值，其一定是匹配的。这样的处理方式实际上更加符合 **Haskell** 惰性的特征。

除了这一点之外，`newtype` 常用于对同种类型定义同类族不同的实现。后文中我们会提及。

### 同义类型

有时，我们根本不需要额外定义新的结构，只需提升一下类型的可读性。这个时候只需要用 `type` 关键字定义 **类型同义词（Type Synonym）**，即别名即可。比如 `String` 类型就是 `[Char]` 的别名。除此之外，`type` 还可以接纳类型构造器的“部分应用”，不过要注意此时这个类型依然是“不完整”的类型（即类型构造器），因此不能直接用它定义变量。

```haskell
-- String 可能的定义
type String = [Char]
-- 需要 import Data.Map
data Map k v
type IntMap = Data.Map Int
```

### 类型参数

我们可以在 **Haskell** 中定义带有类型参数（范型）的类型，熟悉 **C++** 的朋友会发现这和模版有一定相似之处：

```haskell
-- Maybe 类型可能的定义
-- 从字面就能理解它的构造器，要么什么都不是（Nothing），要么就是一个对象（Just a）。这里 a 是一个范型，即可以是任何类型
data Maybe a = Nothing | Just a
-- Either 类型可能的定义
-- 它要么是一个（错误的）a 类型的对象，或一个（正确的）b 类型的对象
data Either a b = Left a | Right b

maybe_int :: Maybe Int
maybe_int = Just 10
shouldbe_int :: Either String Int
shouldbe_int = Right 10
```

这里我们将 `Maybe` 称为一个 **类型构造器（Type Constructor）**。和右侧的构造器类似，它后面可能会跟着零到多个类型，比如 `Maybe Int`、`Maybe String` 等都是通过 `Maybe` 构造出的类型。而 `Just "abc"` 是 `Maybe [Char]` 类型的一个值。`Nothing` 的值则显得多态（范型），因为它可以是任意的 `Maybe a` 类型。类型构造器的概念实际上也包含了前面提到的没有类型参数的自定义类型，这一点和构造器的定义是类似的。

和普通的抽象数据结构一样，带有类型参数的 ADT 构造器本身也有类似函数的类型：

```haskell
-- 下面仅作为类型演示，并不是合法的 Haskell 代码
Just :: a -> Maybe a
```

类型参数在记录结构中也可以出现，其形式和 ADT 中的基本一致。

### IO 和 `do` 语句

迄今我们还没有介绍过输入和输出语句，这不太应该。趁刚刚见识了类型构造器，让我们把“亏欠”已久的你好世界用 **Haskell** 实现出来：

```haskell
-- putStrLn 将一个字符串打印在命令行并换行，然后返回一个 IO () 类型的值，这里 IO 是一个类型构造器
-- 类型 IO a 中，a 是 IO 相关函数的返回类型，这里是 () 说明 printStrLn 不返回有用的数据类型
putStrLn :: String -> IO ()
-- main “函数”实质上是一个 IO () 类型的变量
main :: IO ()
main = putStrLn "Hello, world!"        -- 在命令行上输出 "Hello, world!" 并换行
```

我们也有 `getLine` 函数来从命令行得到一行字符串：

```haskell
-- 可以看到这里 getLine 返回了 IO String 类型的值，其中包含了 String 类型的数据
getLine :: IO String
main :: IO ()
main = do             -- do 语句中包含一系列语句/表达式，通过缩进或大括号来控制；多用来处理 IO 等特殊类型（后续会提到）
    str <- getLine    -- <- 语句将右侧的 IO a “转换为” a 类型并绑定于左侧的“变量”
    putStrLn "Message put: " ++ str        -- do 的最后一个表达式的值会作为 do 的返回值，这里即 IO () 类型
```

如果需要将一个类型 `a` 包装成 `IO a` 类型，可以使用 `return` 函数：

```haskell
-- return 函数声明。这里 Monad 是单子类族，我们会在后面介绍
return :: Monad m => a -> m a
getLineDoubled :: IO String
getLineDoubled = do
    line <- getLine
    return line ++ line
```

`do` 语句不仅对于 `IO a` 类型，而是对于所有 `Monad` 类族都有类似于上面的用法，我们很快就会详细介绍。从形式上来看，非常相似于过程式语言。

### 在 `ghci` 中探究类型和类型构造器

**Haskell** 的解释器 `ghci` 中，有两个命令可以用来得到变量的类型或类型构造器的类型，即 `:t(ype)` 和 `:k(ind)`。前者能够得到变量的类型。

```haskell
-- Haskell 中的整数字面量是一个范型，它可能是任意的数值类型（如 Int、Float 等）
ghci> :t 10
10 :: Num p => p 
-- 类似地，小数字面量也是一个范型，它可以是任意的分数类型（如 Float、Double 等）
ghci> :t 1.0
1.0 :: Fractional p => p
-- 为整数字面量规定类型后，就能够得到确定的类型了
ghci> :t 10 :: Int
10 :: Int :: Int
-- 之前就提到过，空列表是范型
ghci> :t []
[] :: [a]
-- 同样可以为空列表指定类型
ghci> :t [] :: [Int]
[] :: [Int] :: [Int]
-- lambda 表达式必然是带有范型的函数，但范型可能带有类族的限制，比如这里的 (*) 蕴涵了这是一个 Num 类族的范型
ghci> \x -> x * x
\x -> x * x :: Num a => a -> a
-- 这里根据 (++) 的定义确定了 l 的类型：一个范型列表。
ghci> \l -> l ++ l
\l -> l ++ l :: [a] -> [a]
-- map 的类型
ghci> :t map
map :: (a -> b) -> [a] -> [b]
-- map 经过部分应用后的类型，这里根据运算符组 (+ 1) 推断出列表中是一个 Num 类族的范型
ghci> :t map (+ 1)
map (+ 1) :: Num b => [b] -> [b]
-- 构造器 Just 的类型，和函数类似
ghci> :t Just
Just :: a -> Maybe a
-- 一个完整的 Maybe [Char] 类型
ghci> :t Just "abc"
Just "abc" = Maybe [Char]
-- 和空列表类似，Nothing 是一个范型
ghci> :t Nothing
Nothing :: Maybe a
```

另一方面，`:k` 则用于得到类型的特征：

```haskell
-- * 表示一个类型，即 Int 是一个类型
ghci> :k Int
Int :: *
-- 任何函数依然是一个类型
ghci> :k Int -> Int
Int -> Int :: *
-- 然而，类型构造器并不是一个“完整的”类型
ghci> :k Maybe
Maybe :: * -> *
-- 像函数一样接收一系列类型后，类型构造器才能成为一个完整类型
ghci> :k Maybe Int
Maybe :: *
-- 类似地，需要更多类型参数的类型构造器就好像多元函数一样
ghci> :k Either
Either :: * -> * -> *
```

在集中复习了类型和类型构造器的特征后，我们可以做一个总结。**Haskell** 中只有被 `:k` 认证为 `*` 的对象才是完整类型，而所有变量的类型一定是完整类型。一个变量的类型可能是范型，会随着上下文（函数参数类型）有不同的表现。类型构造器在需要至少一个参数时，是不完整类型。

### 类族派生

终于来到了 **Haskell** 中极重要的概念，类族的主场。类族是类似 **Java** 中接口的概念，它可以在变量、类型定义中对范型进行约束，只有符合条件的类型或值才能进行类型实例化，或函数调用。比如，规定了 `Eq` 的范型必须定义了 `==` 和 `!=` 函数；规定了 `Show` 的范型必须定义了 `show` 函数等。

不过，对于相等的判断以及输出类型似乎有默认的行为（比如逐项比较，以及逐项输出），不需要对每个自定义类型都定义 `==` 等函数。此时可以使用 `deriving` 关键字让自定义类型拥有这些类族的默认行为：

```haskell
-- 在最后一个构造器后用 deriving (TypeClass1, TypeClass2, ..., TypeClassk) 的形式派生
data Person = Person { firstName :: String
                     , lastName :: String
                     , age :: Int
                     , height :: Float
                     } deriving (Eq, Show)    -- 现在 Person 满足了 Eq 和 Show 两种类族
tom :: Person
tom = Person "Tom" "Joestar" 18 188.6
mary :: Person
mary = Person "Mary" "Elizabeth" 18 166.8
samePerson :: Bool
samePerson = tom == mary         -- samePerson = False
tom_info :: String
tom_info = show tom    
    -- tom_info = "Person {firstName = \"Tom\", lastName = \"Joestar\", age = 18, height = 188.6}"
```

当一个类型满足了类族的要求时，就可以在以该类族为范型约束的函数中调用了，比如 `elem` ：

```haskell
-- elem 的声明，可以看到要求 a 要满足 Eq 类族
elem :: (Foldable t, Eq a) => a -> t a -> Bool
-- 举例：
teen = [Person]
teen = [tom, mary]
hasTom :: Bool
hasTom = tom `elem` teen        -- hasTom = True
```

除了 `Eq` 和 `Show` 以外，例如 `Ord` 和 `Read` 也支持默认的派生。`Ord` 将首先根据构造器的先后顺序判断（在较前定义的更小），然后再对构造器后的各项进行字典排序。比如对于类型 `Bool` 和 `Maybe`：

```haskell
-- Bool 类型可能的定义
Bool = False | True deriving (Ord)
-- Maby 类型可能的定义
Maybe a = Nothing | Just a deriving (Ord)
-- 举例：
b1 :: Bool
b1 = False < True          -- b1 = True
b2 :: Bool
b2 = Nothing < Just 10     -- b2 = True
b3 :: Bool
b3 = Just 10 > Just 20     -- b3 = False
```

`Read` 类族则和 `Show` 行为正好相反，它让 `read` 函数对某类型进行逐项读取，然后返回一个范型。注意需要在这里将类型声明出来，否则它只是一个范型：

```haskell
data Person = Person { firstName :: String
                     , lastName :: String
                     , age :: Int
                     , height :: Float
                     } deriving (Eq, Show, Read)
tom :: Person
tom = read "Person {firstName = \"Tom\", lastName = \"Joestar\", age = 18, height = 188.6}" :: Person
    -- tom = Person "Tom" "Joestar" 18 188.6
```

### 定义类族

现在让我们来学习类族的定义。类族中其实只有一些函数的声明（可能也包含定义）：

```haskell
-- Eq 类族可能的定义
-- a 是一个符合 Eq 类族的范型
class Eq a where
    (==) :: a -> a -> Bool        -- (==) 定义为 Eq 类族的一个必需函数
    (/=) :: a -> a -> Bool        -- (/=) 定义为 Eq 类族的一个必须函数
    x == y = not (x /= y)
    x /= y = not (x == y)
```

现在定义一个新的类型，`Color`：

```haskell
data Color = Red | Green | Blue
```

之前我们学习了利用 `deriving` 关键字让类型实现默认的类族行为，这次让我们来自己手动写一个对类族的实现，这需要用到关键字 `instance`：

```haskell
-- Color 成为 Eq 的一个实现类
-- 这里只定义了 (==)，则缺省的 (/=) 会根据类族中的定义为准
instance Eq Color where
    Red == Red = True
    Green == Green = True
    Blue == Blue = True
    _ == _ = False
```

类族之间可以存在继承关系，即子类族继承所有父类族要求的函数：

```haskell
-- Ord 类族可能的定义。可以看到这里用到了类型限制的语法，通过这个就能实现类族继承的特性
class Eq a => Ord a where
    x <= y :: a -> a -> Bool
```

需要再次注意，类型构造器可能不是完整类型，而此处实现类族的是一个完整类型。因此我们需要利用范型将类型补完：

```haskell
-- Maybe m 成为 Eq 的一个实现类
-- 注意这里的类型限制是必须的，因为下文中用到了 m 类型的两个数 x 和 y 的对比
instance (Eq m) => Eq (Maybe m) where
    Just x == Just y = x == y
    Nothing == Nothing = True
    _ == _ = False
```

### 类族举例：幺半群

除了非常常用的 `Eq`、`Ord`、`Maybe` 以外，还有许多常用的类族。这里简单介绍一下 **幺半群（Monoid）**。幺半群要求一个类型上定义一个可结合的二元运算符 `@`（这里仅举例为 `@`），且存在单位元 `e` 使得对该类型的任意元素 `x` 都有 `e @ x = x @ a = x`。

```haskell
-- Monoid 可能的定义
class Monoid m where
    mempty :: m                     -- 单位元
    mappend :: m -> m -> m          -- 幺半群上的二元运算
    mconcat :: [m] -> m             -- 将列表中的元素通过二元运算折叠为一个值
    mconcat = foldr mappend mempty  -- 默认的拼接定义
-- 列表类型 [] 实现了 Monoid
instance Monoid [a] where
    mempty = []
    mappend = (++)
```

我们可以通过一些例子来了解幺半群的操作：

```haskell
ghci> mempty `mappend` "abc"
"abc"
ghci> "abc" `mappend` "def"
"abcdef"
ghci> mconcat ["abc", "def", "ghi"]
"abcdefghi"
```

显然，普通的数类型也满足 `Monoid` 的定义，但是加法和乘法都能满足（单位元分别是 `0` 和 `1`），而实现类族只能有一种定义，这个时候应该怎么办呢？回忆我们之前介绍的 `newtype`，它在这里可以大展身手：

```haskell
newtype Product a = Product { getProduct :: a }
    deriving (Eq, Ord, Read, Show, Bounded)
newtype Sum a = Sum { getSum :: a }
    deriving (Eq, Ord, Read, Show, Bounded)
instance Num a => Monoid (Product a) where
    mempty = Product 1
    Product x `mappend` Product y = Product (x * y)
instance Num a => Monoid (Sum a) where
    mempty = Sum 1
    Sum x `mappend` Sum y = Sum (x + y)
```

下面是实用的例子：

```haskell
ghci> getProduct $ Product 10 `mappend` Product 5
50
ghci> getProduct . mconcat . map Product $ [1, 2, 3]
6
ghci> getSum $ Sum 10 `mappend` Sum 42
52
ghci> getSum . mconcat . map Sum $ [2, 4, 6]
12
```

除了数类型外，`Bool` 类型也可以满足 `Monoid` 类族：

```haskell
newtype Any = Any { getAny :: Bool }
    deriving (Eq, Ord, Read, Show, Bounded)
newtype All = All { getAll :: Bool }
    deriving (Eq, Ord, Read, Show, Bounded)
instance Monoid Any where
    mempty = Any True
    Any x `mappend` Any y = Any (x || y)
instance Monoid All where
    mempty = All False
    All x `mappend` All y = All (x && y)
```

`Maybe` 也可以理解为 `Monoid`：

```haskell
-- 仅作为说明，实际上 Maybe 并没有直接实现 Monoid
instance Monoid m => Monoid (Maybe m) where
    mempty = Nothing
    Nothing `mappend` m = m
    m `mappend` Nothing = m
    Just x `mappend` Just y = Just (x `mappend` y)
```

这其实相当于将一个 `Monoid` 包装在 `Maybe` 当中。

最后，幺半群上有一些定律前文中已经提到，这里让我们用 **Haskell** 代码的形式列出：

- ```mempty `mappend` x = x```：左单位元。
- ```x `mappend` mempty = x```：右单位元。
- ```(x `mappend` y) `mappend` z = x `mappend (y `mappend` z)```：满足结合率。

## 进阶：函子、应用函子和单子

这个部分是 **Haskell** 抽象性开始显著提升的知识点，它们在 **Haskell** 编程中是四处可见的概念，并作为类族被诸多类型所实现。

### 函子

**函子（Functor）** 是一种可以通过类似 `map` 的函数来遍历的类型；事实上我们熟悉的列表就实现了函子类族。如果一个类型构造器 `f` 实现了函子类族，我们也称它为 `f` 函子。比如 `[]` 函子。

```haskell
-- Functor 可能的定义
-- fmap 接收一个一元函数和一个函子构造的类型，生成一个函子构造的新类型
class Functor f where
    fmap :: (a -> b) -> f a -> f b  -- 从这里 f a 是完整类型可以推测 f 是一个类型构造器
-- 列表类型 [] 实现了 Functor
instance Functor [] where           -- 现在能够发现，实际上 [a] 是 [] a 的语法糖
    fmap = map
-- Maybe 实现了 Functor
instance Functor Maybe where
    fmap f (Just x) = Just $ f x
    fmap _ Nothing = Nothing
-- Either a 实现了 Functor
instance Functor (Either a) where
    fmap f (Right x) = Right $ f x
    fmap _ (Left x) = Left x
```

通过 `fmap` 可以将普通的函数作用于类型构造器构造的类型上：

```haskell
list :: [Int]
list = fmap (+ 10) = [1, 2, 3]        -- list = [11, 12, 13]
maybe_str :: Maybe String
maybe_str = fmap (++ "world") (Just "Hello, ")    -- maybe_str = "Hello, world"
shouldbe_int :: Either String Int
shouldbe_int = fmap (+ 10) (Left 42)     -- shouldbe_int = Left 42
```

上面这些类型构造器是比较显然的函子，下面让我们来介绍一些更加抽象或意料之外的函子：

```haskell
-- IO 实现了 Functor。
instance Functor IO where
    fmap f action = do
        result <- action
        return $ f result

-- (->) 实现了 Functor。这里的 (->) 是函数类型中用到的符号。
-- 因此没错，(->) 是一个 * -> * -> * 类型的类型构造器，只是一般以中缀形式写出。可以理解为 data (->) a b = a -> b
-- 可以横向对比二元运算符的部分调用， ((+) m) n 等价于 m + n，因此 ((->) r) a 等价于 r -> a
instance Functor ((->) r) where
    fmap f g = \x -> f $ g x            -- fmap :: (a -> b) -> (r -> a) -> (r -> b)
    
-- 举例
putStrReverse :: IO ()
putStrReverse = do
    str <- fmap reverse getLine         -- fmap reverse getLine :: IO String
    putStr "The string is reversed:" ++ str
seqOp :: Int -> Int
seqOp = fmap (+ 2) (* 3)        -- 等价于 (+ 2) . (* 3)，先进行 *3 再进行 +2
```

是不是觉得 `(->)`  中 `fmap` 的类型非常眼熟？没错，`(->)` 函子的 `fmap` 函数和函数的复合运算是一模一样的，上面的定义可以直接写成 `fmap = (.)`。

现在，让我们再回顾一下 `fmap` 函数的类型： ` (a -> b) -> f a -> f b`。如果我们进行部分调用，比如调用`fmap (+ 1)`，会发现此时返回了 `(Functor f, Num a) => f a -> f a` 类型的函数，此处 `f` 的类型待定。所以也可以将 `fmap` 看作将一个函数 **提升（lift）** 的操作。这和开头的描述有微妙的异同，可以尽情理解。

最后，函子满足一些定律。

- `fmap id = id`：作用在 `id` 上的 `fmap` 等价于 `id`。
- `fmap (f . g) = fmap f . fmap g`：作用于函数复合的 `fmap` 等价于 `fmap` 的函数复合。

### 应用函子

对应用函子的需求源于函子只能将一元函数作用在函子类型上，但是对于二元函数，比如 `(*)`，如果进行 `fmap (*)` 得到的是什么呢？我们可以进行简单的推演：`(*)` 的类型是 `a -> a -> a`，也即 `a -> (a -> a)`，因此 `fmap` 这里的类型是 `(a -> a -> a) -> f a -> f (a -> a)`，我们也因此得到了 `f (a -> a)` 类型的值。

那么，现在我们有一个 `f (a -> a)` 的包装过的函数，以及一个包装过的值 `f a`（按照上面的例子，比如是 `Just 10`），我们应该怎样通过再传入一个包装过的值 `f a` 来得到一个结果呢？这就是应用函子所需要定义的运算符 `<*>`：

```haskell
-- Applicative 可能的定义
-- 应用函子首先是一个函子，所以需要定义 fmap :: (a -> b) -> f a -> f b
class Functor f => Applicative f where
    pure :: a -> f a
    (<*>) :: f (a -> b) -> f a -> f b
-- [] 实现了 Applicative
instance Applicative [] where
    pure x = [x]                   -- pure :: a -> [a]
    ff <*> xx = [f x | f <- ff, x <- xx]
-- Maybe 实现了 Applicative
instance Applicative Maybe where
    pure = Just                    -- pure :: a -> Maybe a
    Nothing <*> _ = Nothing        -- 当函数不是有效值时，直接返回 Nothing
    Just f <*> x = fmap f x
-- IO 实现了 Applicative
instance Applicative IO where
    pure = return                  -- pure :: a -> IO a
    mf <*> mx = do
        f <- mf
        x <- mx
        return $ m x
-- (->) 实现了 Applicative
instance Applicative (->) where
    pure x = \_ -> x               -- pure :: a -> (r -> a)
    f <*> g = \x -> f x (g x)      -- (<*>) :: (r -> a -> b) -> (r -> a) -> (r -> b)
```

下面让我们来逐个看看他们的用法。首先是 `Maybe`：

```haskell
ghci> pure (* 2) <*> Just 2
Just 4
ghci> pure (* 2) <*> Nothing
Nothing
ghci> Nothing <*> Just 2
Nothing
-- 从头开始计算被应用函子提升的二元函数
ghci> pure (*) <*> Just 2 <*> Just 2
Just 4
ghci> pure (*) <*> Nothing <*> Just 2
Nothing
ghci> Nothing <*> Just 2 <*> Just 2
Nothing
-- 甚至任意多元的函数
ghci> add3 a b c = a + b + c
ghci> fadd3 = pure add3            -- 现在得到了提升后的 add3
ghci> fadd3 <*> Just 3 <*> Just 5 <*> Just 7
ghci> Just 105
```

可以看到，应用函子可以让任一个函数简单地提升到这个这个应用函子类型，非常实用。不过，一开始的函数和后面的参数之间都使用 `<*>` 运算符显得还是不够“规范”，因此让我们再定义一个运算符 `<$>` 用来模仿函数应用运算符 `$`：

```haskell
-- (<$>) 实际上就是 fmap 的中缀版本
(<$>) :: Functor f => (a -> b) -> f a -> f b
(<$>) = `fmap`
```

现在再让我们看看 `Maybe` 应用函子的运用：

```haskell
ghci> (++) <$> Just "Hello, " <*> Just "world!"     -- 对比 (++) "Hello, " "world!"
Just "Hello, world!"
ghci> fmap <$> Just (+ 3) <*> Just [0, 1, 2]        -- 对比 fmap (+ 3) [0, 1, 2]
Just [3, 4, 5]
```

这样的写法非常形象地展现了运算的特点，也反映出 **Haskell** 强大的抽象能力。

下一个是 `[]` 应用函子的示例：

```haskell
ghci> (++) <$> ["ah", "eh", "oh"] <*> ["!", "?", "."]
["ah!", "ah?", "ah.", "eh!", "eh?", "eh.", "oh!", "oh?" ,"oh."]    -- [] 函子的行为是组合调用函数
ghci> (+) <$> [1, 2] <*> [3, 4]
[4, 5, 5, 6]
ghci> filter (> 10) $ (*) <$> [2, 3, 5] <*> [2, 3, 4]    -- 先进行了应用函子运算，然后再进行 filter
[12, 15, 20]
```

`IO` 应用函子：

```haskell
getTwoLines :: IO String
getTwoLines = (++) <$> getLine <*> getLine
main :: IO ()
main = do
    twoLines <- getTwoLines
    putStrLn "Here are the two lines read: " twoLines
```

`(->)` 应用函子：

```haskell
ghci> pure 3 "mothing"
3
ghci> (+) <$> (+ 1) <*> (* 2) $ 3    -- 计算 (3 * 2) + (3 + 1)
10
ghci> (\x y z -> (x, y, z)) <$> (+ 1) <*> (* 2) <*> (+ 3) $ 2    -- 计算 (2 + 1, 2 * 2, 2 + 3)
(3, 4, 5)
```

最后让我们介绍 `Control.Applicative` 中的函数 `liftA` 系列：

```haskell
liftA :: Applicative f => (a -> b) -> f a -> f b
liftA f a = f <$> a
liftA2 :: Applicative f => (a -> b -> c) -> f a -> f b -> f c
liftA2 f a b = f <$> a <*> b
liftA3 :: Applicative f => (a -> b -> c -> d) -> f a -> f b -> f c -> f d
liftA3 f a b c = f <$> a <*> b <*> c
-- 举例
justPlus :: Maybe a -> Maybe a -> Maybe a
justPlus = liftA2 (+)
justList :: Maybe a -> Maybe a -> Maybe a -> [Maybe a]
justList = liftA3 (\x y z -> [x, y, z])
```

这是对应用函子使用的一种简化，可以将其等效看作是对函数的提升（和前文的 `fmap` 有相同效果，实际上 `liftA = fmap`）。利用 `liftA2` 可以尝试实现一个将形如 `[Maybe a]` 的类型转换为形如 `Maybe [a]` 的类型的函数 `sequenceA`：

```haskell
sequenceA :: Applicative f => [f a] -> f [a]
sequenceA = foldr (liftA2 (:)) (pure [])    -- 回忆折叠函数 foldr 将列表从右到左进行累积计算

list1 :: Maybe [Int]
list1 = sequenceA [Just 10, Just 42]        -- list1 = Just [10, 42]
list2 :: Maybe [Int]
list2 = sequenceA [Just 10, Nothing]        -- list2 = Nothing
list3 :: [Int]
list3 = sequenceA [(+ 1), (* 2)] 3          -- list3 = [4, 6]
```

最后，让我们给出应用函子定律：

- `pure f <*> x = fmap f x`：应用函子也是函子。
- `pure id <*> v = v`：等价于第一个函子定律。
- `pure (.) <*> u <*> v <*> w = u <*> (v <*> w)`：即 `(.) <$> u <*> v <*> w = u <*> (v <*> w)`
- `pure f <*> pure x = pure (f x)`：即 `f <$> pure x = pure $ f x`，比较好理解。
- `u <*> pure x = pure ($ x) <*> u`：

### 单子

让我们首先回忆一下函子和应用函子的核心功能。

- 函子的 `fmap` 函数接收一个从普通值到普通值的函数和一个包装过的值，然后返回一个包装过的值：`(a -> b) -> f a -> f b`。
- 应用函子的 `<*>` 运算符接收一个被包装过的从普通值到普通值的函数以及一个包装过的值，然后返回一个包装过的值： `f (a -> b) -> f a -> f b`。

现在有一个新的场景：我们有一个包装过的值，以及一个从普通值到包装过的值的函数，我们该如何得到一个包装过的值呢，也即：`m a -> (a -> m b) -> m b`。这就是 `Monad` 类族的功能：

```haskell
-- Monad 可能的定义
class Monad m where
    return :: a -> m a                 -- 和 Applicative 的 pure 一致
    (>>=) :: m a -> (a -> m b) -> m b  -- bind 函数
    (>>) :: m a -> m b -> m b
    x >> y = x >>= \_ -> y
    fail :: String -> m a
    fail msg = error msg
-- Maybe 是一个单子
instance Monad Maybe where
    return x = Just x
    Nothing >>= f = Nothing
    Just x >>= f = f x
    fail _ = Nothing
-- [] 是一个单子
instance Monad [] where
    return x = Just [x]
    xx >>= f = concat (map f xx)        -- 对列表中每个元素上作用 f
    fail _ = []
```

嗯…… 看起来并不是很直观。让我们用一个例子展示：

```haskell
ghci> return "abc" :: Maybe String
Just "abc"
ghci> Just 42 >>= \x -> return $ x * 10
Just 420
ghci> Nothing >>= \x -> return $ x * 10
Nothing
ghci> k  
Just 42
```

可以看到，`Monad` 的计算过程是将初始值用 `>>=` 通过一个个一元函数后得到结果。其中每个函数都会通过 `return` 函数得到包装过的结果。`return` 的形式颇有过程式编程的风格，我们很快会看到更多。

如果我们不需要前序操作的值，那么可以使用 `>>` 运算符：

```haskell
ghci> Just 10 >> Just 42 >>= (\x -> return $ x * 10)
Just 420
ghci> Nothing >> Just 42 >>= (\x -> return $ x * 10)
Nothing
```

`>>=` 的引入是非常方便的。我们可以对比一下使用或不使用 `bind` 函数的代码风格差异：

```haskell
work :: Maybe String
work x = case x of
    Nothing -> Nothing
    Just x' -> case return $ f x of
        Nothing -> Nothing
        Just x'' -> case return $ g x of
            Nothing -> Nothing
            Just x''' -> return x'''
workMonad :: Maybe String
workMonad = x >>= f >>= g        -- 难以置信的简洁
```

`Monad` 常用于 `lambda` 嵌套模拟的“变量绑定”，可以看看下面的例子：

```haskell
ghci> Just 3 >>= (\x -> Just "abc" >>= (\y -> return $ show x ++ y))
Just "3abc"
ghci> let x = 3; y = "abc" in show x ++ y
"3abc"
```

可以看到，第一个式子和第二个的作用类似，只是使用了单子的上下文。我们可以调整格式让它变得更加直观：

```haskell
monad :: Maybe String
monad = Just 3 >>= (\x ->
        Just "abc" >>= (\y ->
        return $ show x ++ y))
```

经过这样排列，是否有些眼熟呢？没错，这就是 `do` 语句的来源。上面的式子等价于下面的 `do` 语句：

```haskell
monad :: Maybe String
monad = do
    x <- Just 3              -- <- 符号将一个单子类型 M a 中的 a 类型值绑定于左侧的变量上
    y <- Just "abc"
    return show x ++ y
```

和原始格式有所不同的是，`do` 语句中 `<-` 左侧支持模式匹配，在匹配失败时，会使用 `fail` 函数的结果：

```haskell
firstChar :: String -> Maybe Char
firstChar str = do
    (x:xs) <- Just str
    return x
char1 :: Maybe Char
char1 = firstChar "abc"     -- char1 = Just 'a'
char2 :: Maybe Char
char2 = firstChar ""        -- char2 = Nothing
```

看了这么多的 `Maybe` 单子示例，让我们再看看别的单子。首先是 `[]` 单子：

```haskell
tupleList :: [(Int, Char)]
tupleList = do
    n <- [1, 2, 3]    -- 这里并不是将 [1, 2, 3] 赋给了 n，而是根据 (>>=) 的意义，让 n 遍历了 [1, 2, 3] 的元素
    s <- "abc"
    return (n, s)
-- tupleList = [(1,'a'),(1,'b'),(1,'c'),(2,'a'),(2,'b'),(2,'c'),(3,'a'),(3,'b'),(3,'c')]
```

这个结果或许有些出人意料，但如果将它的原始语法写出来，会更加清晰：

```haskell
tupleList :: [(Int, Char)]
-- 这里 >>= 让后面的函数遍历了给出的列表，所以实际上 n 和 s 是遍历列表时经历的列表元素，而 return (n, s) 是其组合的列表
tupleList = [1, 2, 3] >>= (\n -> "abc" >>= (\s -> return (n, s))
```

如果还记得列表概括的用法，下面的例子于之前的代码是等价的：

```haskell
tupleList :: [(Int, Char)]
tupleList = [(n, s) | n <- [1, 2, 3], s <- "abc"]
```

可以看到语法上甚至都有类似之处。

最后，让我们了解一下单子定律。

- `return x >>= f = f x`：左单位元定律
- `m >>= return = m`：右单位元定律
- `(m >>= f) >>= g = m >>= (\x -> f x >>= g)`：结合性

如果额外定义运算符 `(<=<)` 为：

```haskell
(<=<) :: Monad m => (b -> m c) -> (a -> m b) -> (a -> m c)
f <=< g = (\x -> g x >>= f)
```

可以将上面的定律重写为：

- `f <=< return = f`：左单位元定律（虽然看起来反过来了）
- `return <=< f = f`：右单位元定律
- `f <=< (g <=< h) = (f <=< g) <=< h`：结合性

观察可以发现，`(<=<)` 运算符与函数复合运算符 `(.)` 的功能类似。事实上，上面的三个定律中，将 `<=<` 替换为 `.`，将 `return` 替换为 `id` 后依然成立。我们可以从这样的类比性质中看到 `Monad` 本质的存粹。

## 进阶：数

现在让我们详细探索 **Haskell** 中的数。**Haskell** 主要包含下面这些数值类型：

- `Int`、`Integer`：有限精度整数和任意精度整数
- `Float`、`Double`：单精度浮点数和双精度浮点数
- `Rational`：任意精度有理数
- `Data.Complex`：复数

### 和数相关的类族

所有数都是 `Num` 类族的类型，这个类族的定义如下：

```haskell
-- Num 可能的定义
class (Eq a, Show a) => Num a where     -- 继承了 Eq 和 Show 类族，也即需要实现相等比较和 show 函数
    (+), (-), (*) :: a -> a -> a        -- 需要实现三个算术运算
    negate :: a -> a                    -- 取负运算
    abs, signum :: a -> a               -- 绝对值和符号数
    fromInteger :: Integer -> a         -- 从整数转换到改类型
    -- 默认的 abs、signum 定义
    abs x = if x < 0 then -x else x
    signum x | x < 0 = -1
             | x > 0 = 1
             | otherwise = 0
```

从 `Num` 出发，还有两个子类族 `Real` 和 `Fractional`：

```haskell
-- Real 可能的定义
class (Num a, Ord a) => Real a where
    toRational :: a -> Rational
-- Fractional 可能的定义
class (Num a) => Fractional a where
    (/) :: a -> a -> a                    -- 除法运算
    fromRational :: Rational -> a
```

`Fractional` 类型包括了所有定义了除法的数，因此复数也符合 `Fractional`。所有的整数字面量都视为 `Num` 范型；所有的小数字面量都视为 `Fractional` 范型：

```haskell
ghci> :t 1
1 :: Num p => p
ghci> :t 1.0
1.0 :: Fractional p => p
```

类族 `Integral` 则是 `Real` 和 `Enum` 的子类族，后者是其元素可以枚举的类型。

```haskell
-- Integral 可能的定义
class (Real a, Enum a) => Integral a where
    divMod :: a -> a -> (a, b)        -- 余数运算
    toInteger :: a -> Integer
```

这里 `divMod` 会返回两个值，第一个是整数除法的商，第二个是余数。

最后一个类族是 `Floating`，它包含了更多的数学函数，比如 `sqrt`、`log`、`cos` 等，这里就不尽列了。

### 取底函数

现在尝试对一个小数向下取整。一个思路是利用一个辅助函数 `until`：

```haskell
-- until 的声明和可能的定义
until :: (a -> Bool) -> (a -> a) -> a -> a
until p f x = if p x then x else until p f (f x)        -- 直到 x 符合要求 p，不断进行 f x
-- 示例：
x :: Int
x = until (> 10) (* 2) 1        -- x = 16
```

这样就可以定义 `floor` 函数了：

```haskell
floor :: Float -> Integer
floor x = if x < 0 
            then until ((<= x) . fromInteger) (subtract 1) (-1)        -- 这里为了类型一致需要用 fromInteger
            else until ((x <) . fromInteger) (+ 1) 1 - 1
```

可以看到上面我们使用了 `subtract` 而非 `-`，这是因为在 **Haskell** 中为了支持 `- n` 表示负 `n` 这样的前缀形式，`-` 不存在运算符组的语法，所以 `(- 1)` 会被理解为 `-1` 而非 `\x -> x - 1`。

此外，也可以通过二分查找逼近的方式计算取底函数：

```haskell
type Interval = (Integer, Integer)
floor :: Float -> Integer
floor x = fst $ until unit (shrink x) (lower x, upper x)
    where unit (m, n) = m + 1 == n
          shrink x (m, n) = if p `leq` x then (p, n) else (m, p)
          p = mid (m, n)
          mid (m, n) = (m + n) `div` 2
          lower x = until (`leq` x) (* 2) (-1)        -- 左边界是 -2^m
          upper x = until (x `lt`) (* 2) 1            -- 右边界是 2^n
          leq = (<= x) . fromInteger                  -- leq :: Float -> Integer -> Bool
          lt = (< x) . fromInteger                    -- lt :: Float -> Integer -> Bool
```

二分查找的复杂度相比此前的线性查找要快上许多。

### 自然数

**Haskell** 中没有自然数类型，但是我们可以通过 ADT 轻松地定义：

```haskell
-- Natural 类型的定义
data Natural = Zero | Succ Nat        -- 可以看到这是一个递归的定义
-- Natural 是一个 Eq
instance Eq Natural where
    Zero == Zero = True
    Zero == Succ _ = False
    Succ _ == Zero = False
    Succ m == Succ n = m == n
-- Natural 是一个 Show
instance Show Natural where
    show Zero = "Zero"
    show (Succ Zero) = "Succ Zero"
    show (Succ n) = "Succ (" ++ show n ++ ")"
instance Ord Natural where
    Zero < Zero = False
    Zero < Succ n = True
    Succ m < Zero = False
    Succ m < Succ n = m < n
-- Natural 是一个 Num
instance Num Natural where
    m + Zero = m
    m + Succ n = Succ (m + n)
    m - Zero = m
    Zero - Succ n = Zero
    Succ m - Succ n = m - n
    m * Zero = Zero
    m * Succ n = m * n + m
    abs m = m
    signum Zero = Zero
    signum (Succ n) = Succ Zero
    fromInteger x
        | x <= 0 = Zero
        | otherwise = Succ $ fromInteger (x - 1)
-- 为 Natural 类型设计一个底部类型的数
infinity :: Natural
infinity = Succ infinity
```

实际上，上面的 `Eq` 和 `Show` 都和默认的实现一致，所以我们可以直接从这两个类族派生，即使用 `deriving (Eq, Show)` 即可。

## 进阶：列表

### 通过 ADT 理解列表

列表本质是通过 `(:)` 运算符构建的同质结构。实际上，应该将这个运算符视作一个构造器，因为并没有约化这个表达式的规则。之前我们在学习函子时，已经把 `[]` 看作了一种类型构造器。事实上它的定义可能与此等效：

```haskell
-- [] 类型可能的定义。当然，这个仅作为类型演示，并不是合法的 Haskell 代码
data [] a = [] | (:) a ([] a)
```

`(:)` 是一个右结合的非严格函数（构造器），我们会发现除了利用 `[]` 和 `(:)` 构造的有穷列表外，还有仅用 `(:)` 构造的无限列表，如 `[1..]`，以及未知中止性的不完整列表，如 `filter (< 4) [1..]` 可以记作 `1:2:3:undefined`。

**Haskell** 中很多非严格函数都来自于列表，比如 `take`、`head` 等函数对于上述的无限列表或不完整列表都能“网开一面”，这实际上也是 **Haskell** 惰性的体现。

### 列表概括

前面我们见识到过列表概括高度简洁清晰的语法，我们这里再次用实例展示一次。下面的 `triads` 生成一个范围内的勾股数三元组：

```haskell
triads :: Int -> [Int]
triads n = [(x, y, z) | x <- [1..n], y <- [x+1..n], z <- [y+1..n], x*x+y*y == z*z]
```

让我们来尝试优化这个算法。首先，我们应该让 `x, y` 互素，否则会生成冗余的三元组。为了判断两个数是否互素，可以使用辅助函数 `divisors` 来得到一个数的因数数组，然后再用辅助的 `disjoint` 方法判断是否有重复。

```haskell
divisors :: Int -> [Int]
divisors x = [d | d <- [2..x-1], x `mod` d == 0]
disjoint :: [Int] -> [Int] -> Bool
disjoint [] _ = True
disjoint _ [] = True
disjoint xx@(x:xs) yy@(y:ys)        -- 这里的 @ 是一个语法糖，表明 xx "as" (x:xs)，后文中的 xx 就可以代表 (x:xs)
    | x == y = False
    | otherwise = disjoint xx ys && disjoint xs yy    -- 如果 xx 和 yy 有序，这里是可以简化的
coprime :: Int -> Int -> Bool
coprime x y = disjoint (divisors x) (divisors y)
```

现在可以这样定义 `triads`：

```haskell
triads :: Int -> [Int]
triads n = [(x, y, z) | x <- [1..n], y <- [x+1..n], coprime x y, z <- [y+1..n], x*x+y*y == z*z]
```

我们可以通过观察发现，列表概括的作用和一些高阶函数是等同的：

```haskell
map f xx = [f x | x <- xx]
filter p xx = [x | x <- xx, p x]
concat xxx = [x | xx <- xxx, x <- xx]
```

实际上，列表概括正是通过这些高阶函数定义的：

```haskell
[|] :: [a]
[e | True] = [e]            -- 右侧没有限制则直接输出内容
[e | q] = [e | q, True]     -- 如果只有一个判断条件，直接归纳到后一种情况
[e | b, Q] = if b then [e | Q] else []      -- 如果有一个判断条件 b 和后续序列 Q ，则根据 b 决定是否进行后续序列
[e | x <- xx, Q] = let ok x = [e | Q]       -- 对每一个元素判断是否满足
                       ok _ = []
                   in concat (map ok xx)    -- 将所有满足的列表拼接到一起
```

### `[]` 函子定律

之前我们接触过两个函子定律。这里我们深入 `[]` 函子，来探索更多的定律：

- `f . head = head . map f`：这一点显而易见
- `map f . tail = tail . map f`：这一点也显而易见
- `map f . concat = concat . map (map f)` ：拼接后的 `map` 等价于 `map` 后的拼接（`concat` 定律）

上面三个等式有相似之处，那就是经历一个操作后再被一个函数作用等价于被这个函数作用后再经历一个操作。我们可以通过更多例子来加深认识：

- `map f . reverse = reverse . map f`：这一点显而易见
- `concat . map concat = concat . concat`：这个一两句解释不清楚……
- `filter p . map f = map f . filter (p . f)` `map` 后的 `filter` 等价于 `filter` 作用后的再 `map`

我们可以尝试证明上面的第三个等式（注意，证明部分并不是合法的 **Haskell** 语句）：

```haskell
filter p . map f
=> -- filter 的另一种构造方式
concat . map (\x -> if p x then [x] else []) . map f
=> -- 函子定律 2
concat . map ((\x -> if p x then [x] else []) . f)
=> -- 等价变换
concat . map (map f . (\x -> if (p . f) x then [x] else []))
=> -- 函子定律
concat . map (map f) . map (\x -> if (p . f) x then [x] else [])
=> -- concat 定律
map f . concat . map (\x -> if (p . f) x then [x] else [])
=> -- filter 的另一种构造方式
map f . filter (p . f)
```

**Haskell** 中的证明是一个非常有意思的操作，可以通过证明恒等式来找到更有效率的函数定义方法。我们会在后文中介绍并证明更多的等式。

### 列表排序

首先，让我们尝试判断一个列表是否是升序（或降序）的。这里使用了高阶函数 `zipWith`：

```haskell
-- and 函数。当且仅当所有元素皆为 True 时得到 True
and :: [Bool] -> Bool
-- zipWith 函数，通过一个二元运算将两个列表操作为新的列表（类似于 map 扩张到二元函数）
zipWith :: (a -> b -> c) -> [a] -> [b] -> [c]
-- 检查列表是否是非递减的
nondec :: (Ord a) => [a] -> Bool
nondec xx = and $ zipWith (<=) xx (tail xx)
```

下面是一个归并排序的实现示例：

```haskell
-- mergesort 函数。对一个列表进行归并排序
mergesort :: Ord a => [a] -> [a]
mergesort [] = []
mergesort [x] = [x]
mergesort xx = merge (mergesort left) (mergesort right)
    where (left, right) = split xx
          split xx = let n = length xx `div` 2 in (take n xx, drop n xx)
          -- 合并两个已经排序的列表
          merge :: Ord a => [a] -> [a] -> [a]
          merge [] yy = yy
          merge xx [] = xx
          merge xx@(x:xs) yy@(y:ys)
              | x <= y = x:merge xs yy
              | otherwise = y:merge xx ys
```

## 进阶：**Haskell** 证明

之前我们通过高阶函数的定义和替换来证明恒等式的成立。本节将专注于利用数学归纳法证明更多的等式。

### 数学归纳法

如果需要证明某个命题 $P(n)$ 对于任意自然数 $n$ 成立，实际上只需要证明：

- $P(0)$ 成立
- 对于任意自然数 `n`，假定 `P(k)` 对任意 $k \le n$ 成立的情况下都成立，那么 $P(n + 1)$ 也成立。

下面让我们以 `exp` 函数为例，证明 `exp x (m+n) = exp x m * exp x n`：

```haskell
exp :: Num a => a -> Int -> a
exp x 0 = 1
exp x n = x * exp x (n-1)
-- 当 m = 0 时
-- 等式左侧
    exp x (0 + n)
    => -- 基本算术
    exp x n
-- 等式右侧
    exp x 0 * exp x n
    => -- exp 定义
    1 * exp x n
    => -- 基本算术
    exp x n
-- 考虑 m + 1 的情况
-- 等式左侧
    exp x (m + 1 + n)
    => -- 基本算术
    exp x (m + n + 1)
    => -- exp 定义
    x * exp x (m + n)
    => -- 归纳
    x * exp x m * exp x n
-- 等式右侧
    exp x (m + 1) * exp x n
    => -- exp 定义
    x * exp x m * exp x n
```

这样我们就证明了这个前面的等式。

### 列表证明：有穷列表

列表也可以使用数学归纳法，但是形式不太一样。我们首先考虑有穷列表。为了证明  $P(xx)$ 成立对于任意有穷列表  `xx@(x:xs)`，我们需要证明：

- $P($`[]`$)$ 成立。
- 对于任意列表 `xs`，假设 $P($`xs'`$)$ 对于任意的 `xs` 后缀 `xs'` 成立，则对于任意 `x` 都有 $P($`x:xs`$)$  成立。

我们可以将 `(++)` 作为例子，来证明 `(xx ++ yy) ++ zz = xx + (yy ++ zz)`：

```haskell
(++) :: [a] -> [a] -> [a]
[] ++ yy = yy
(x:xs) ++ yy = x : (xs ++ yy)
-- 当 xx = [] 时
-- 等式左侧
    ([] ++ yy) ++ zz
    => -- (++) 定义
    yy ++ zz
-- 等式右侧
    [] ++ (yy ++ zz)
    => -- (++) 定义
    yy ++ zz
-- 当 xx = x:xs 时
-- 等式左侧
    ((x:xs) ++ yy) ++ zz
    => -- (++) 定义
    (x:(xs ++ yy)) ++ zz
    => -- (++) 定义
    x:((xs ++ yy) ++ zz)
-- 等式右侧
    (x:xs) ++ (yy ++ zz)
    => -- (++) 定义
    x:(xs ++ (yy ++ zz))
    => -- 归纳
    x:((xs ++ y) ++ zz)
```

我们接下来尝试证明 `reverse . reverse = id`，也即 `reverse $ reverse xx = xx`：

```haskell
reverse :: [a] -> [a]
reverse [] = []
reverse (x:xs) = reverse xs ++ [x]
-- 当 xx = [] 时
-- 等式左侧：
    reverse $ reverse []
    => -- reverse 定义
    reverse []
    => -- reverse 定义
    []
-- 等式右侧
    []
-- 当 xx = x:xs 时
-- 等式左侧：
    reverse $ reverse (x:xs)
    => -- reverse 定义
    reverse (reverse xs ++ [x])
    => -- 引理
    x : reverse $ reverse xs
    => x:xs
-- 等式右侧：
    x:xs
```

上面有一个引理需要证明，即 `reverse (xs ++ [x]) = x : reverse xs` （这实际上有时候作为字符串的逆的定义）。这个也可以通过数学归纳法证明，根据 `xs` 的情况进行归纳：

```haskell
-- 当 xs = [] 时
-- 等式左侧：
    reverse ([] ++ [x])
    => -- (++) 定义
    reverse [x]
    => -- reverse 定义
    reverse [] ++ [x]
    => -- reverse 定义
    [] ++ [x]
    => -- (++) 定义
    x : []
-- 等式右侧：
    x : reverse  []
    => -- reverse 定义
    x : []
-- 当 xs = x':xs'
-- 等式左侧：
    reverse ((x':xs') ++ [x])
    => -- (++) 定义
    reverse (x':(xs' ++ [x]))
    => -- reverse 定义
    reverse (xs' ++ [x]) ++ [x']
    => -- 归纳
    (x : reverse xs') ++ [x']
    => -- (++) 定义
    x : (reverse xs' ++ [x'])
-- 等式右侧：
    x : reverse (x':xs')
    => -- reverse 定义
    x : (reverse xs' ++ [x'])
```

这样我们就完整证明了最初的等式，即 `reverse . reverse = id`。

### 列表证明：非完整和无限列表

对于非完整列表 `xs`，为证明 $P($`xs`$)$ 对于所有非完整列表成立，则需要证明：

- $P($`undefined`$)$ 成立
- 对任意非完整列表 `xs`，假定 $P($`xs`$)$ 成立，则对于任意 `x` 都有 $P($`x:xs`$)$ 成立。

下面让我们尝试证明，如果 `xx` 是一个非完整列表，则对于任意列表 `yy` 都有 `xx ++ yy = xx`。

```haskell
-- 当 xx = undefined
-- 等式左侧：
    undefined ++ yy
    => -- (++) 匹配失败，因此得到 undefined
    undefined
-- 等式右侧
    undefined
-- 当 xx = x':xs'
-- 等式左侧：
    (x':xs') ++ yy
    => -- (++) 定义
    x' : (xs' ++ yy)
    => -- 归纳
    x':xs'
-- 等式右侧：
    x':xs'
```

无穷列表可以理解成一个非完整列表序列的极限，比如 `[1..]` 是 `undefined, 1:undefined, 1:2:undefined, ...` 的极限。如果某个性质 $P$ 对任意以 `xs` 为极限的序列 `xs_1`、`xs_2`、…… 都成立时，对 `xs` 也成立，我们称 $P$ 是 **链完全（Chain Complete）** 的性质。全称量词约束的恒等关系是链完全的，还有链完全性质的合取（“且”关系）也是链完全的。与之对比，存在量词约束的性质和不等式并不是链完全的。

让我们尝试证明对于所有有穷列表 `xx` 和 *任意* 列表 `yy` 及 `zz` 都有 `(xx ++ yy) ++ zz = xx ++ (yy ++ zz)` 成立。首先这个性质是链完全的。因此我们只需要证明平凡情形，即 `xx = undefined`：

```haskell
-- xx = undefined
-- 等式左侧：
    (undefined ++ yy) ++ zz
    => -- (++) 匹配失败，得到 undefined
    undefined ++ zz
    => -- (++) 匹配失败，得到 undefined
    undefined
-- 等式右侧：
    undefined ++ (yy ++ zz)
    => -- (++) 匹配失败，得到 undefined
    undefined
```

### 折叠函数

我们之前有提到过折叠函数 `foldl` 和 `foldr`，它们用来进行带着一个结果从左向右或从右向左进行计算的操作。一些函数（甚至高阶函数）都可以通过折叠函数定义：

```haskell
sum :: (Foldable t, Num a) => t a -> a
sum = foldr (+) 0
concat :: Foldable t => t [a] -> [a]
concat = foldr (++) []
filter :: (a -> Bool) -> [a] -> [a]
filter p = foldr (\x xs -> if p x then x:xs else xs) []
map :: (a -> b) -> [a] -> [b]
map f = foldr ((:) . f) []
```

现在让我们探索等式 `foldr f e (xx ++ yy) = foldr f e xx (@) foldr f e yy` 成立的条件，这里 `(@)` 是某个合适的运算符。作为举例有：

- `sum (xx ++ yy) = sum xx + sum yy`：拼接的和等于部分和的和
- `concat (xx ++ yy) = concat xx ++ concat yy`：拼接的拼接等于部分拼接的拼接

对照之前用折叠函数的定义可以发现其完全符合上面的等式。

```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
foldr f e [] = e
foldr f e (x:xs) = f x (foldr f e xs)
-- 当 xx = []
-- 等式左侧：
    foldr f e ([] ++ yy)
    => -- (++) 定义
    foldr f e yy
-- 等式右侧：
    foldr f e [] @ foldr f e yy
    => -- foldr 定义
    e @ foldr f e yy
-- 当 xx = x:xs
-- 等式左侧：
    foldr f e ((x:xs) ++ yy)
    => -- (++) 定义
    foldr f e (x:(xs ++ yy))
    => -- foldr 定义
    f x (foldr f e (xs ++ yy))
    => -- 归纳
    f x (foldr f e xs @ foldr f e yy)
-- 等式右侧：
    f x (foldr f e xx) @ foldr f e yy
```

可见，为了让上面的两个等式成立，需要让 `f`、`e` 和  `@` 满足这样的性质（下面的 `x`、`y`、`z` 是任取的）：

- `e @ x = x`：即 `e` 是一个单位元
- `f x (y @ z) = f x y @ z`：一个特殊情况即 `f = (@)`，此时只要 `(@)` 是可结合的就满足条件了

显然我们之前举的两个例子 （`sum` 和 `concat`）都是满足这里的要求的，因此可以得证。

此外，`foldr` 有一个重要的 **融合定律（Fusion Law）**： `f . foldr g a = foldr h b`。举例比如：

- `double . sum = foldr ((+) . double) 0`：这里 `double x = x + x`。
- `length . concat = foldr ((+) . length) 0`

这里依然手动进行归纳来看 `f`、`g`、`h` 之间有什么关系：

```haskell
-- 当 xx = undefined
-- 等式左侧：
    f . (foldr g a undefined)
    => -- foldr 匹配失败
    f undefined
-- 等式右侧：
    foldr h b undefined
    => -- foldr 匹配失败
    undefined
-- 当 xx = []
-- 等式左侧：
    f $ foldr g a []
    => -- foldr 定义
    f a
-- 等式右侧：
    foldr h b []
    => -- foldr 定义
    b
-- 当 xx = x:xs
-- 等式左侧：
    f $ foldr g a (x:xs)
    => -- foldr 定义
    f $ g x (foldr g a xs)
-- 等式右侧：
    foldr h b (x:xs)
    => -- foldr 定义
    h x (foldr h b xs)
    => -- 归纳
    h x (f $ foldr g a xs)
```

于是我们得到三个需要满足的等式，其中 `x` 是任取的：

- `f undefined = undefined`：即 `f` 是一个严格函数
- `f a = b`：即对初始条件有要求
- `f $ g x = h x $ f`：这个式子有点抽象，需要用例子来理解

对于之前的两个例子，前两个条件都不难验证，这里我们稍微推导一下第三个条件：

- `f = double`、`g = (+)`、`h = (+) . double`

  ```haskell
  -- 等式左侧：
      double $ (+) x y
      => -- (+) 定义
      double (x + y)
      => -- double 定义
      x + y + x + y
      => -- 基本算术
      x + x + y + y
  -- 等式右侧：
      ((+) $ double x) $ double y
      => -- double 定义
      (+) (x + x) (y + y)
      => -- (+) 定义
      x + x + y + y
  ```

- `f = length`、`g = (++)`、`h = (+) . length`

  ```haskell
  -- 等式左侧：
      length $ (++) xx yy
      => -- 语法调整
      length (xx ++ yy)
  -- 等式右侧：
      ((+) $ length xx) $ length yy
      => -- 语法调整
      length xx + length yy
  ```

  这里需要证明引理 `length (xx ++ yy) = length xx + length yy`。

  ```haskell
  length :: [a] -> Int
  length [] = 0
  length (x:xs) = 1 + length xs
  -- xx = undefined
  -- 等式左侧：
      length (undefined ++ yy)
      => -- 匹配失败
      length undefined
      => -- 匹配失败
      undefined
  -- 等式右侧：
      length undefined + length yy
      => -- 匹配失败
      undefined + length yy
      => -- 匹配失败
      undefined
  -- xx = []
  -- 等式左侧：
      length ([] ++ yy) 
      => -- (++) 定义
      length yy
  -- 等式右侧：
      length [] + length yy
      => -- length 定义
      0 + length yy
      => -- 基本算术
      length yy
  -- xx = x:xs
  -- 等式左侧：
      length ((x:xs) ++ yy)
      => -- (++) 定义
      length (x:(xs ++ yy))
      => -- length 定义
      1 + length (xs ++ yy)
      => -- 归纳
      1 + length xs + length yy
  -- 等式右侧：
      length (x:xs) + length yy
      => -- length 定义
      1 + length xs + length yy
  ```

  这样我们就证明了原来那个结论。

实际上，如果我们定义 `h = f . g`，则等式 `foldr f a . map g = foldr h a` 总成立。左式中的 `map` 此前我们使用 `map f = foldr ((:) . f)` 的方式定义了。这样就可以尝试通过融合定律证明 `foldr f a . foldr ((:) . g) = foldr (f . g) a`。前两个条件依然明显满足，因此让我们验证 `f $ g x = h x $ f`，也即 `foldr f a $ g (x:xs) = (f . g) x $ foldr f a xs` ：

```haskell
foldr f a $ g (x:xs)
=> -- foldr 定义
f (g x) (foldr f a xs)
=> -- ($) 定义
f (g x) $ foldr f a xs
=> -- (.) 定义
(f . g) x $ foldr f a xs
```

这样就证明完毕。从这个结论我们其实可以轻松地得到：

- `double . sum = sum . map double = foldr ((+) . double) 0`
- `length . concat = sum . map length = foldr ((+) . length) 0`

上面我们一直使用 `foldr` 而非 `foldl`，这其实是因为前者比后者更加通用：对于无穷列表，`foldl` 并不能作用于无穷列表。考虑下面的例子：

- `foldr (++) [] [[i] | i <- [1..]]`
- `foldl (++) [] [[i] | i <- [1..]]`

前者的展开逻辑是（这里假设存在使其展开的驱使，比如使用函数 `putStrLn . show . (take 3)`）：

```haskell
foldr (++) [] [[i] | i <- [1..]]
=> -- 列表概括的定义
foldr (++) [] ([1] : [[i] | i <- [2..]])
=> -- foldr 定义
[1] ++ foldr (++) [] [[i] | i <- [2..]]
=> -- (++) 定义
1 : ([] ++ foldr (++) [] [[i] | i <- [2..]])
=> -- (++) 定义
1 : foldr (++) [] [[i] | i <- [2..]]
=> -- 列表概括的定义
1 : foldr (++) [] ([2] : [[i] | i <- [3..]])
=> -- foldr 定义
1 : ([2] ++ foldr (++) [] [[i] | i <- [3..]])
=> -- (++) 定义
1 : (2 : ([] ++ foldr (++) [] [[i] | i <- [3..]]))
=> -- (++) 定义
1 : (2 : foldr (++) [] [[i] | i <- [3..]])
=> -- 趋向于无穷列表 [1..]
1 : 2 : 3 : ..
```

可以看到非常巧妙地得到了一个无穷列表。然而对于 `foldl`，展开过程存在困难：

```haskell
foldl (++) [] [[i] | i <- [1..]]
=> -- 列表概括的定义
foldl (++) [] ([1] : [[i] | i <- [2..]])
=> -- foldl 定义
foldl (++) ([] ++ [1]) [[i] | i <- [2..]]
=> -- 列表概括的定义
foldl (++) ([] ++ [1]) ([2] : [[i] | i <- [3..]])
=> -- foldl 定义
foldl (++) (([] ++ [1]) ++ [2]) [[i] | i <- [3..]]
=> -- 这个尾递归永远不会结束，因此永远无法得到作为参数的无限列表
foldl (++) (..((([] ++ [1]) ++ [2]) ++ [3]) ++ ..) [[i] | i <- [..]]
```

由于 `foldl` 是一个尾递归函数，对于无穷列表其永远无法得到结果，所以其不适用。这也是为什么 **Haskell** 中默认使用 `foldr` 进行折叠操作。不仅如此， `foldr` 有时效率更高。观察下面的例子：

- `foldr (++) [] [xx, yy, zz] = xx ++ (yy ++ (zz ++ (tt ++ [])))`：需要 `O(x + y + z + t)` 时间，其中 `x`、`y`、`z` 、`t`是 `xx`、`yy`、`zz` 、`tt` 的长度。
- `foldl (++) [] [xx, yy, zz] = ((([] ++ xx) ++ yy) ++ zz) ++ tt`：需要 `O(3x + 2y + z)` 时间。

在上面的例子中如果 `x = y = z = t`，那么显然 `foldr` 的效率更高。这样的区别是源于 `(++)` 的定义：`(++)` 的时间复杂度仅取决于左侧列表的长度。

最后，虽然我们一直没有提到，但是涉及一些不满足结合律的运算时，`foldl` 和 `foldr` 的结果可能大相径庭。

### 扫描函数

扫描函数 `scanl` 和 `scanr` 分别将 `foldl` 作用于列表的每个前缀和每个后缀，我们可以通过两个例子说明：

- `scanl (+) 0 [1, 2, 3] = [0, 0+1, (0+1)+2, ((0+1)+2)+3] = [0, 1, 3, 6]`
- `scanr (+) 0 [1, 2, 3] = [1+(2+(3+0), 2+(3+0), 3+0, 0] = [6, 5, 3, 0]`

现在让我们尝试得到 `scanl` 的定义。一个比较显然的思路是：

```haskell
scanl :: (b -> a -> b) -> b -> [a] -> [b]
scanl f e = map (foldl f e) . inits
    where inits :: [a] -> [[a]]        -- inits 实际上是 Data.List 中的函数
          inits [] = [[]]
          inits (x:xs) = [] : map (x :) (inits xs)
```

这种方式通过构造所有列表前缀组成的列表，再对每个前缀 `map` 上 `foldl`，就能够得到结果。不过这样计算的复杂度略高，为 $O(n^2)$。如果能够活用已经计算的内容就好了。让我们尝试对 `scanl` 的定义进行变形：

```haskell
-- xx = []
    scanl f e []
    => -- scanl 定义
    map (foldl f e) $ inits []
    => -- inits 定义
    map (foldl f e) [[]]
    => -- map 定义
    [foldl f e []]
    => -- foldl 定义
    [e]
-- xx = x:xs
    scanl f e (x:xs)
    => -- scanl 定义
    map (foldl f e) $ inits (x:xs)
    => -- inits 定义
    map (foldl f e) ([] : map (x :) (inits xs))
    => -- map 定义
    foldl f e [] : map (foldl f e) $ map (x :) (inits xs)
    => -- 函子定律
    foldl f e [] : map (foldl f e . (x :)) (inits xs)
    => -- foldl 定义
    e : map (foldl f e . (x :)) (inits xs)
    => -- 引理： foldl f e . (x :) = foldl f (f e x)
    e : map (foldl f (f e x)) (inits xs)
    => -- (.) 定义
    e : map (foldl f (f e x)) . inits $ xs
    => -- scanl 定义
    e : scanl f (f e x) xs
```

上面的引理证明如下：

```haskell
foldl f e $ (x :) xx
=> -- (:) 定义
foldl f e (x:xx)
=> -- foldl 定义
foldl f (f e x) xx
```

这样我们就成功得到了 `scanl` 线性时间的定义：

```haskell
scanl :: (b -> a -> b) -> b -> [a] -> [b]
scanl f e [] = [e]
scanl f e (x:xs) = e : scanl f (f e x) xs
```

看起来有些梦幻，但是我们确实将开头的 $O(n^2)$ 算法通过恒等式变换转为 $O(n)$  的复杂度。这是在 **Haskell** 中常见的优化行为，且也能体现它的神奇之处。

## 进阶：**Haskell** 效率

本节我们将专注于 **Haskell** 中表达式的求值逻辑和效率。我们知道，**Haskell** 采用惰性求值，在需要求值时会先展开函数定义或 ADT 定义再进行运算（从外到内）。不过需要注意的是，一些情况下并不是真的在所有地方都展开定义，而是采用了展开定义混合类似 `let` 表达式中的方式进行表达式求值。考虑下面的例子：

```haskell
sqr :: Int -> Int
sqr x = x * x
x :: Int
x = sqr $ sqr (1 + 2)
-- sqr $ sqr (1 + 2)
-- = sqr $ sqr (let sum = 1 + 2 in sum)
-- = sqr (let sum = 1 + 2 in let res1 = sum*sum in res1)
-- = let sum = 1 + 2 in let res1 = sum*sum in let res2 = res1*res1
```

可以看到，上面注释中演示的展开步骤中，每一个值都只需要被求值一次。如果单纯地只是展开定义，`1+2` 以及 `sqr (1+2)` 这两个算式就要重复进行，这是非常低效的。这种避免重复进行同种展开的机制称为 **共享（Sharing）**。一个值被展开到一定程度后，之后再读取该值就会从之前展开的程度开始求值。

此外，我们之前应该也提到过，在展开定义时，并不是展开定义的全部，而是展开（函数定义或构造定义）直到能够进行下一次约化为止。比如列表 `xx` 通常会根据需要一步步进行展开为 `x:xs`、`x:x':xs'` 等。我们称一个完全化简的表达式为 **范式（Normal Form）**，而被函数或构造器（它们本质是类似的）应用的表达式为 **首范式（Head Normal Form）**。我们可以将 **惰性求值（Lazy Evaluation）** 正式定义为：

> 所有表达式只有在需要时才进行一次求值；此次求值仅计算到首范式

共享可以加速程序速度，但是因为这个值被存储了下来，因此会占用存储。在有大量子问题的递归函数中，可能会造成内存不足：

```haskell
-- 取子序列的函数，这里重复使用了 subseqs xs 会影响时间效率
subseqs :: String -> [String]
subseqs (x:xs) = subseqs xs ++ map (x :) (subseqs xs)
-- 利用 where 共享了 subseq' xs 的值（和 let 表达式等价），不过因此会占用内存
subseqs' :: String -> [String]
subseqs' (x:xs) = xxx ++ map (x :) xxx
    where xxx = subseqs' xs
```

不过，共享的结果对需要多次调用的低效函数有显著的效率提升：

```haskell
-- 最基本的 where 语句定义帮助函数
foo :: Int -> Int
foo n = sum (take n primes)
    where primes = [x | x <- [2..], divisors x == [x]]
          divisors x = [d | d <- [2..x], x `mod` d == 0]
-- 使用显示定义的函数
foo' :: Int -> Int
foo' n = sum (take n primes)
primes :: [Int]
primes = [x | x <- [2..], divisors x == [x]]
divisors :: Int -> [Int]
divisors x = [d | d <- [2..x], x `mod` d == 0]
-- 使用 lambda 表达式
foo'' :: Int -> Int
foo'' = \n -> sum (take n primes)
    where primes = [x | x <- [2..], divisors x == [x]]
          divisors x = [d | d <- [2..x], x `mod` d == 0]
```

上面的 `foo'` 在多次调用后可能会比 `foo` 有显著的效率提升，这是因为 `foo` 在每次调用中都要重复对 `primes` 进行求值，而 `foo'` 会重复使用 `primes` 在前序调用中已经得到的值。`foo''` 看起来和 `foo` 一模一样，但实际上它的效率和 `foo'` 相当。这是因为它没有任何参数，因此 `primes` 是直接绑定于这个函数上的，在多次调用中都可以使用已经计算展开得到的值；而 `foo` 拥有一个参数 `n`，这样对于任何不同的 `n`，都对应着不同的 `primes`，所以不能利用共享。

### 空间效率

大家应该对折叠函数 `foldl` 有印象。它对列表的操作可以定义为：

```haskell
foldl :: (b -> a -> b) -> b -> [a] -> b
foldl op result [] = result
foldl op initial (x:xs) = foldl op (op initial x) xs
```

之前我们提到，很多函数可以通过折叠函数定义，比如 `sum = foldl (+) 0`。不过如果这样定义，其展开过程就是：

```haskell
sum [1..100]
=> -- sum 定义
foldl (+) 0 [1..100]
=> -- foldl 定义
foldl (+) (0+1) [2..100]
=> -- foldl 定义
foldl (+) ((0+1)+2) [3..100]
=> -- 多次展开后
foldl (+) (..((0+1)+2)+3..) []
```

对于需要展开这个和的情形，我们需要 100 份空间来存储中间参数的一百次连加的参数，这无疑是低效的。如果有一种对 `foldl` 的定义能够强制每次加法都强制执行就能解决这个问题。**Haskell** 提供了这样的勤奋机制：`seq` 函数：

```haskell
-- seq 函数。它是被 Haskell 原生支持的，并不能定义出来
-- seq x y = y，但这里会强制要求 x 被首先求值出来，才会返回 y 求值的结果
seq :: a -> b -> b

-- foldl' 定义在 Data.List 中
foldl' :: (b -> a -> b) -> b -> [a] -> b
foldl' op result [] = result
-- 首先强制求出 next = op initial x 的值，然后再去对 foldl' op next xs 进行求值
foldl' op initial (x:xs) = next `seq` foldl' op next xs
    where next = op initial x
```

这样就可以将 `sum` 定义为 `foldl' (+) 0`。`foldl` 和 `foldl'` 的功能大体相同，不过如果 `op` 不是严格函数，可能会产生不同的结果；此外的情形，可以用后者代替前者。

## 拓展：库函数定义示例

在【基础知识】中，我们见到了不少库函数或者运算符的声明。实际上，它们基本都能通过模式匹配、lambda 表达式、列表构造等定义出来，下面我们将进行一些展示：

1. 列表相关函数：

   ```haskell
   -- 列表拼接
   (++) :: [a] -> [a] -> [a]
   [] ++ yy = yy        -- 空列表情形直接返回另一个列表
   (x:xs) ++ yy = x : (xs ++ yy)   
   -- 列表随机访问
   (!!) :: [a] -> Int -> a
   [] !! 0 = error "index too large"
   (x:xs) !! 0 = x
   (x:xs) !! n = xs !! (n - 1)
   -- 列表首个元素
   head :: [a] -> a
   head [] = error "empty list"
   head (x:_) = x
   -- 列表首个元素外的列表
   tail :: [a] -> [a] 
   tail [] = error "empty list"
   tail (_:xs) = xs
   -- 列表最后的元素
   last :: [a] -> a
   last [] = error "empty list"
   last [x] = x
   last (_:xs) = tail xs
   -- 列表最后的元素外的列表
   init :: [a] -> [a]
   init [] = error "empty list"
   init [x] = []
   init (x:xs) = x : init xs
   -- 列表长度
   length :: [a] -> Int
   length [] = 0
   length (_:xs) = 1 + length xs
   -- 列表是否为空
   null :: [a] -> Bool
   null [] = True
   null _ = False
   -- 取列表前缀
   take :: Int -> [a] -> [a]
   take _ [] = []
   take 0 xx = []
   take n (x:xs) = x : take (n - 1) xs
   -- 取列表后缀
   drop :: Int -> [a] -> [a]
   drop _ [] = []
   drop 0 xx = xx
   drop n (x:xs) = drop (n - 1) xs
   -- 逆置列表
   reverse :: [a] -> [a]
   reverse list = reverse' list []
       where reverse' [] result = result
             reverse' (x:xs) result = reverse' xs (x:result)
   -- 通过列表构建循环列表
   cycle :: [a] -> [a]
   cycle list = list ++ cycle list
   -- 通过元素构建循环列表
   repeat :: a -> [a]
   repeat x = x : repeat x
   ```

2. 字符串相关函数：

   ```haskell
   -- 根据空格字符分割字符串
   words :: String -> [String]
   words str = reverse $ map reverse (words' str [] [])    -- 利用两个辅助列表存储缓冲中的字符串和已经读取的字符串
       where words' (c:cs) buf result | isSpace c = words' cs [] (buf:result)
                                      | otherwise = words' cs (c:buf) result
             words' [] buf result | null buf = []
                                  | otherwise = buf:result
   -- 拼接字符串数组
   unwords :: [String] -> String
   unwords [] = []
   unwords (x:xs) = x ++ " " ++ unwords xs
   ```

3. 元组相关函数：

   ```haskell
   -- fst 函数
   fst :: (a, b) -> a
   fst (x, y) = x
   -- snd 函数
   snd :: (a, b) -> b
   snd (x, y) = y
   -- zip 函数
   zip :: [a] -> [b] -> [(a, b)]
   zip (x:xs) [] = []
   zip [] (y:ys) = []
   zip (x:xs) (y:ys) = (x, y) : zip xs ys
   ```

4. 高阶函数：

   ```haskell
   -- map 函数
   map :: (a -> b) -> [a] -> [b]
   map _ [] = []
   map f (x : xs) = f x : map f xs
   -- filter 函数
   filter :: (a -> Bool) -> [a] -> [a]
   filter _ [] = []
   filter p (x:xs)
       | p x         = x : filter p xs        -- 如果谓词判断为真，将 x 放入返回的列表中
       | otherwise = filter p xs            -- 否则跳过当前元素
   -- concat 函数
   concat :: Foldable t => t [a] -> [a]
   concat xx = foldr (++) [] xx
   -- foldl 函数
   foldl :: Foldable t => (b -> a -> b) -> b -> t a -> b
   foldl op result [] = result
   foldl op initial (x:xs) = foldl op (op initial x) xs
   -- foldr 函数
   foldr :: Foldable t => (a -> b -> b) -> b -> t a -> b
   foldr op result [] = result
   foldr op final (x:xs) = op x (foldr op final xs)
   ```

## 拓展：`Boolean` 类族定义示例

我们定义一个类族并为一些常见类型实现这个类族。`Boolean` 指代了能够被转化为 `Bool` 的特征。

```haskell
-- Boolean 的定义
class Boolean a where
    true :: a -> Bool
    false :: a -> Bool
    true x = not $ false x
    false x = not $ true x
-- 为 Int 实现 Boolean
instance Boolean Int where
    true 0 = False
    true _ = True
-- 为 Bool 实现 Boolean
instance Boolean Bool where
    true = id
-- 为列表类型实现 Boolean
instance Boolean [a] where
    true [] = False
    true _ = True
-- 为 Maybe 类型实现 Boolean
instance Maybe m where
    true Nothing = False
    true (Just _) = True
-- 为 Either 类型实现 Boolean
instance Either a b where
    true (Left _) = False
    true (Right _) = True
```

## 拓展：数独求解器

数独是一个 9x9 的棋盘上填入 1-9 使得每行每列和每个 3x3 的方格中都包含 1-9 所有数字的游戏（棋盘上已经填入了一些数字）。现在让我们写一个函数 `solve` 来对一个数独进行求解，以得到所有满足规则的解（通常是唯一解）。

首先是基本类型和基本函数：

```haskell
-- 基本的行类型和矩阵类型
type Row a = [a]
type Matrix a = [Row a]
-- 棋子和棋盘类型
type Digit = Char
type Grid = Matrix Digit
-- 棋子的枚举
digit :: [Digit]
digits = ['1'..'9']
blank :: Digit -> Bool
blank = (== '0')
```

### 一个简单的解

我们可以假设所给的谜题（`Grid` 类型）是一个合法的棋盘，即每一行每一列以及每一个方格都不存在重复数字。随后让我们考虑如何求解。函数式编程通常可以从一个直观却低效的算法出发，通过之前在列表章节见到的等式转换变为高效的算法。此处首先用最暴力的方式：在给定的棋盘上每个空白都填上所有可能的数字，最后再筛选出满足规则的可能性：

```haskell
-- 填充所有空白
complete :: Grid -> [Grid]
-- 判断是否满足规则
valid :: Grid -> Bool
-- solve 可能的定义
solve :: Grid -> [Grid]
solve = filter valid . complete
```

而为了产生所有可能性，可以通过生成一个 `[Digit]` 的矩阵，每个格子是一个可能值的数组，然后再将这个矩阵展开为棋盘的数组：

```haskell
-- 通过枚举生成所有可能性
-- 将例如 [[1, 2], [1, 2]] 展开为 [[1, 1], [1, 2], [2, 1], [2, 2]]
expand :: Matrix [Digit] -> [Grid]
-- 生成可能值列表的矩阵
choices :: Grid -> Matrix [Digit]
complete = expand . choices
choices = map (map choice)
    where choice d = if blank d then digits else [d]
```

让我们考虑 `expand`  的实现方式。它实际上做的是，将列表中第一个列表的每一个元素都拼接到后续列表通过 `expand` 的所有结果列表上。因此我们可以这样定义：

```haskell
expand = cp . map cp
    where cp :: [[a]] -> [[a]]    -- cp 是一个类似于笛卡尔积的函数
          cp [] = [[]]    -- 对于空列表，展开为空列表
          cp (xx:xxs) = [x:xs' | x <- xx, xs' <- xxs']
              where xxs' = cp xxs
```

最后，让我们定义之前的 `valid` 函数，来判断产生的棋盘是否满足规则：

```haskell
-- 判断一个列表是否有重复元素
nodup :: (Eq a) => [a] -> Bool
nodup [] = True
nodup (x:xs) = all (/= x) xs && nodup xs
-- 提取一个矩阵中的所有行
rows :: Matrix a -> Matrix a
rows = id
-- 提取一个矩阵中的所有列，实际上就是求矩阵的转置
cols :: Matrix a -> Matrix a
cols [xx] = [[x] | x <- xx]
cols (xx:xxs) = zipWith (:) xx (cols xxs)
-- 提取一个矩阵中的所有方格
-- 这个实现非常巧妙，等价于将矩阵以 3x3 为单位组合后进行转置，然后再解组合
boxes :: Matrix a -> Matrix a
boxes = map ungroup . ungroup . map cols . group . map group
    where group :: [a] -> [[a]]
          group [] = []
          group xx = take 3 xx : group $ drop 3 xx
          ungroup :: [[a]] -> [a]
          ungroup = concat
-- valid 定义。确保所有行、列、方格都没有重复数字
valid g = all nodup (rows g) && all nodup (cols g) && all nodup (boxes g)
```

### 一些等式

关于取矩阵的行、列和方格操作有下列等式成立：

```haskell
rows . rows = id
cols . cols = id
boxes . boxes = id
```

第一条是显然的。第二条的证明比较复杂，这里暂不进行（但是道理是非常清晰的，即转置的转置等于原矩阵）。让我们在这里给出第三条的证明：

```haskell
boxes . boxes
=> -- boxes 定义
map ungroup . ungroup . map cols . group . map group . map ungroup . ungroup . map cols . group . map group
=> -- 函子定律 2
map ungroup . ungroup . map cols . group . map (group . ungroup) . ungroup . map cols . group . map group
=> -- group . ungroup = id
map ungroup . ungroup . map cols . group . map id . ungroup . map cols . group . map group
=> -- 函子定律 1
map ungroup . ungroup . map cols . group . ungroup . map cols . group . map group
=> -- group . ungroup = id
map ungroup . ungroup . map cols . map cols . group . map group
=> -- 后续省略多步，和前面逻辑类似
map (ungroup . group)
=> -- group . ungroup = id
id
```

上面的证明过程和祖玛有点类似（就是左右一碰变成 `id`）。下面我们继续给出一些等式：

```haskell
map rows . expand = expand . rows
map cols . expand = expand . cols
map boxes . expand = expand . boxes
```

这类等式是我们应该已经比较熟悉的了。以及还有一些和 `cp` 相关的等式：

```haskell
map (map f) . cp = cp . map (map f)
filter (all p) . cp = cp . map (filter p)
```

