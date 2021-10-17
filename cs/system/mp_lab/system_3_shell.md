# MP Shell

本次 MP 的目标是实现一个微型的 Shell 程序。

## 学习目标

- 了解 Shell 工作原理
- `fork`、`exec`、`wait` 三件套的使用
- 信号处理
- 进程管理
- 僵尸进程

## 背景故事

呃，长话短说——你被巨硬公司炒了。你老板带给你了一个 code review 并对你失望至极。显然他们更需要一个 **C++** 风格的向量，我们没能提前打听到这个消息。你决定为 *插入热科技公司* 工作，且如今你已经被录取了！不过，所有 *插入热科技公司* 的新员工都得完成一个入职测试才能保证自己的饭碗，没错，就是写一个 Shell。因此你得写一个相当 :fire: 的 Shell 让你老板不仅热情地接纳你进入新公司，还会立马为你涨薪。

Shell 基本的功能就是接受命令行的输入，并执行命令。 `vector`、`sstring` 和 `format.h` 库你可以尽情使用。希望这些能帮你稳住自己在 *插入热科技公司* 的饭碗。也不要忘了时刻都可以参考 **Unix** 的 Shell。

## 重要事项

### Fork 炸弹

为了防止你炸掉自己的虚拟机，劝你利用 [`ulimit`](https://linux.die.net/man/3/ulimit) 指令，它可以帮你限制一共能够 `fork` 的次数。不过这个指令仅限于当前的终端，因此每次打开新的终端都要输入这个指令。或者，把这个指令写到 `~/.bashrc` 中，就可以在每次启动虚拟机时都自动执行了。需要注意至少给它 100 到 200 个左右的上限，如果给的太少可能会在当前终端中没法启动任何东西（它们都需要 `fork`），此时只能新开一个终端。

### 做好计划

从这个 MP 开始，你基本上不会有什么初始代码了，你需要开动自己设计能力，为程序的功能预留足够的修改余地。我们推荐首先把整个文档读完（包括第二部分），并浏览每个头文件，在清楚 MP 的脉络之后，再开始写码。下面是一些琐碎的建议：

- 写下你想要实现的功能，最好列一个 to-do list 防止你漏掉什么。
- 为整个 MP 做出计划，先写下代码的框架，这样就不会因为添加新功能而重构代码。
- 确保自己清楚地知道调用的系统调用和库函数的功能，包括每个参数、返回值、错误的意义。
- 将代码分为不同模块（函数），毕竟 debug 一个 1500 行的 `while` 循环大概是人生至暗时刻。
- 一点点增加功能。实现一个功能后进行测试，然后 debug 完毕后再添加下一个功能。
- 好好给变量和函数起名字，合理利用空格，不要写天书。
- 在没有完成的代码处标上 `TODO`，在一些编辑器（或安装插件后）这个关键字会被高亮。

### 不许用 `system`

别以为你想到了一个捷径。这个 MP 禁止使用 `system` 函数。

### 输入格式

不用担心输入指令中有不寻常的空格使用，比如开头或结尾多余的空格、两个单词间多于一个的空格等。

### 输出格式

我们已经在 `format.h` 中提供了用于格式化输出的函数，你不应该随意地直接输出到 `stdout` 或 `stderr` 中。如果在 debug 时输出了一些信息，确保使用 `DEBUG` 宏的条件编译。

## 待实现功能列表

这个 MP 分为两个部分，分别要求在一周内完成：

### 第一部分

- 启动 Shell。
- 启动 Shell 时使用可选的参数。
- Shell 交互。
- 执行内置指令。
- 前台执行外部指令。
- 使用逻辑运算符。
- 处理 `SIGINT` 信号。
- 退出 Shell。

### 第二部分

第一部分中的所有内容，以及：

- 后台执行外部指令。
- `ps` 指令。
- 重定向指令。
- 信号指令。

## 详细功能

### 启动 Shell

Shell 程序应该是一个无限循环，其中不断执行：

- 打印命令行提示符。
- 从输入读取指令。
- 打印执行该指令的进程 ID（除了内置指令外），并执行指令。

#### 历史记录

Shell 程序需要存储指令的历史记录。通过下面的指令从一个文件中读取指令的历史记录：

```bash
./shell -h <filename>
```

这个 Shell 在启动后，应将 `<filename>` 中的指令作为历史记录。在 Shell 退出时，应该讲所有历史记录加在原来的历史记录之后。举个例子，假设我们的历史记录存在 `history.txt` 文件中：

```
cd cs241
Hm
```

我们在如下执行 Shell：

```bash
./shell -h history.txt
(pid=1234)/home/user/cs241$ echo Hey!
Command executed by pid=1235
Hey!
(pid=1234)/home/user/cs241$ exit
```

现在我们的 `history.txt` 文件中应该有下面的内容：

```
cd cs241
Hm
echo Hey!
```

#### 脚本

Shell 程序允许从一个脚本文件中读取一系列指令：

```bash
./shell -f <filename>
```

这个 Shell 在启动后应将 `<filename>` 中所有指令顺序执行。举例说明，如果我们将一系列命令写在 `command.txt` 文件中：

```
cs cs241
echo Hey!
```

然后我们执行 Shell，就会将该文件中的指令顺序执行一遍：

```bash
./shell -f command.txt
(pid=1234)/home/user$ cd cs241
(pid=1234)/home/user/cs241$ echo Hey!
Command executed by pid=1235
Hey!
```

脚本和前面所述的历史记录应该使用同样的格式，这样我们可以同时指定一个文件为历史记录及脚本。

提示：[`getopt`](https://linux.die.net/man/3/getopt)函数或许很有帮助。:smile:

### Shell 交互

#### 提示符

提示输入指令时， Shell 应该使用下面的格式（在 `format.h` 中有相应的函数）：

```bash
(pid=<pid>)<path>$
```

其中 `<pid>` 是 Shell 的进程 ID，`<path>` 则是当前所在路径。需要注意提示符后面并没有换行符。

#### 读取命令

Shell 通常从 `stdin` 读取指令，或者在初始运行时根据 `-f` 选项从文件读取指令。

#### 指令类型和格式

Shell 支持两种指令类型：内置指令或外部指令。内置指令是写在 Shell 代码中的，它们不需要新建进程就能执行，而外部指令 *必须* 在新进程中执行，这是通过在 Shell 中 `fork` 得到的。指令的参数之间应该通过空格键相隔，首尾不应该出现空格。你的这个 Shell 不需要支持引号，比如 `echo "hello world"`。

#### 运行指令

如果一个指令被新的进程运行，应该输出下面这样的语句（在 `format.h` 中有相应的函数）：

```bash
Command executed by pid=<pid>
```

这应该在即将运行该指令的进程中打印出来，并且确保在该指令输出任何内容之前打印出来。

#### 记录历史

Shell 需要记录下来用户输入的每一个指令，用于之后再次调用。可以利用向量来存储。

#### 退出 Shell

Shell 应该在接收到 `exit` 指令或指令开头是 `EOF` 时退出程序。`EOF` 可以通过 `Ctrl-D` 来从终端输入，同时所有文件结尾都会默认发送一个 `EOF`。此时你的 Shell 应该以状态 `0` 退出。如果 Shell 后台正在跑进程时收到了 `exit` 或 `EOF`，你应该杀掉所有子进程后再退出 Shell。不用担心 `SIGTERM`。

:warning:如果你不处理这两种情况，很多 testcase 都跑不过！

:warning:**不要** 把 `exit` 存到历史记录中！

#### 处理 `Ctrl-C`

通常在使用 `Ctrl-C` 时，当前运行的程序都会退出，不过我们希望 Shell 能够忽略 `Ctrl-C` 放出的 `SIGINT` 信号，而仅仅将当前在 Shell 中运行的前台程序利用 `SIGINT` 终止。一个做法是在 `SIGINT` 被捕获时对前台程序调用 `kill` 函数，*不过* 因为当信号被发向一个进程时，会自动发向其所在进程组的所有进程，而前台程序显然是 `fork` 自 Shell 程序的进程，同处于一个进程组。因此实际上我们在补货 `SIGINT` 后什么都不需要做即可。*不过* 在后台程序存在时，要确保只终止前台程序的进程，因此你应该使用 [`setpgid`](https://linux.die.net/man/3/setpgid) 将后台程序分配到不同的进程组。

### 内部指令

#### `cd <path>`

将当前 Shell 的工作目录变为路径 `<path>`。如果路径不是 `/` 开始的，就应该是一个相对路径。如果路径不存在，应该打印出相应的错误。注意 `<path>` 是必须的参数，这和其它的 Shell 不太一样。

#### `!history`

将所有历史记录中的指令按照顺序打印出来。但注意这个指令不应该存储在历史记录中：

```bash
(pid=1234)/home/user$ !history
0    ls -l
1    pwd
2    ps
```

#### `#<n>`

打印并执行第 `n` 个历史记录中的指令，其序号应该和历史记录中的一致，然后将执行的指令记录进历史中。如果 `n` 是一个无效的序号，应该打印出相应的错误，并不将该指令记录进历史。

```bash
(pid=1234)/home/user$ echo Echo This!
Command executed by pid=1235
Echo This!
(pid=1234)/home/user$ echo Another echo
Command executed by pid=1236
Another echo
(pid=1234)/home/user$ !history
0    echo Echo This!
1    echo Another echo
(pid=1234)/home/user$ #1
echo Another echo
Command executed by pid=1237
Another echo
(pid=1234)/home/user$ #9001
Invalid Index
(pid=1234)/home/user$ !history
0    echo Echo This!
1    echo Another echo
2    echo Another echo
(pid=1234)/home/user$
```

:warning: 在确定序号是否有效前就应该打印出运行命令的提示（“Command executed by...”）。

:warning:`#<n>` 本身并不会存进目录，*实际上执行* 的指令才会。

### `!<prefix>`

打印并执行最后一个以 `<prefix>` 为前缀的指令。如果找不到这样的指令，打印出相应的错误并不将该指令记录进历史。

```bash
(pid=1234)/home/user$ echo Echo This!
Command executed by pid=1235
Echo This!
(pid=1234)/home/user$ echo Another echo
Command executed by pid=1236
Another echo
(pid=1234)/home/user$ !e
echo Another echo
Command executed by pid=1237
Another echo
(pid=1234)/home/user$ !echo E
echo Echo This!
Command executed by pid=1238
Echo This!
(pid=1234)/home/user$ !d
No Match
(pid=1234)/home/user$ !
echo Echo This!
Command executed by pid=1239
Echo This!
(pid=1234)/home/user$ !history
0       echo Echo This!
1       echo Another echo
2       echo Another echo
3       echo Echo This!
4       echo Echo This!
(pid=1234)/home/user$
```

:warning: 在确定序号是否有效前就应该打印出运行命令的提示（“Command executed by...”）。

:warning:`!<prefix>` 本身并不会存进目录，*实际上执行* 的指令才会。

### 外部指令

