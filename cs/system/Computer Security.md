# Computer Security

[TOC]

## 软件安全

### 基础知识

#### 内存布局

本章我们将主要介绍 **控制流劫持（Control Overflow Hijacking）** ，它是让程序的控制流发生改变并执行攻击者的指令。在开始介绍前，我们有必要讲述一下 **C** 程序的内存布局（从低地址到高地址顺序）：

- **文本段（Text Segment）**：存储程序的可执行代码，通常是只读的。
- **数据段（Data Segment）**：存储初始化过的静态或全局变量。
- **BSS 段（BSS Segment）**：存储没有初始化过的静态或全局变量。这段内容会被全置为 0。
- **堆（Heap）**：存储动态内存。这部分内存被程序员手动管理。
- **栈（Stack）**：存储函数中的局部变量（自动变量），以及所有和函数相关的临时对象，比如函数参数、返回地址等。

下面用一段程序来说明：

```c
int x;									// BSS 段
static double d = 1.0;					// 数据段
int main() {
 	int a = 10;							// 栈
    static float f = 10;				// 数据段
    int *ptr = malloc(2 * sizeof(int));	// 栈
    ptr[0] = 42;						// 堆
}
```

#### 栈



### 缓冲区溢出

**缓冲区溢出（Buffer Overflow）** 是当我们对超出数组大小的部分进行读写时出现的错误。可以通过下面的例子进行理解：

```c
void foo() {
 	char buf[16];
    gets(buf);			// gets 是一个危险的函数，它不对缓冲区边界进行任何检查，输入多少就向缓冲区写入多少
}
```

`buf` 在 `foo` 调用是在栈上得到了 16 个字节的空间，然而输入，比如 `"good morning you are hacked"` 的长度超过了这段空间。多出的字符会覆盖栈上存储的参数，甚至函数的返回地址。这就可能导致函数返回到任意指定的地址处，执行攻击者提前放置的恶意代码。这个恶意代码通常就是输入的字符串的某个位置。

我们可以直接尝试调用 shell 来侵入系统。利用恶意函数打开 shell 之后，就可以方便地直接使用指令了：

```assembly
# 一个 shellcode 程序
push	$0xb			# execve 的地址
pop		%eax			# EAX = 0xb
xor		%ecx, %ecx		# ECX = 0x0
xor		%edx, %edx		# EDX = 0x0
push	%edx			# 0
push	$0x68732f2f		# "//sh"，注意 Intel 是小端（Little Endian）的，因此字符串是从低字节读向高字节
push	$0x6e69622f		# "/bin"
mov		%esp, %ebx		# EBX = "/bin//sh"
int		$0x80			# 进行系统调用
```

可以发现中间有一些别扭的操作，比如对 EAX 赋值时首先 `push` 然后再 `pop`；对 ECX、EDX 赋 0 时使用 `xor` 等，这是因为为了利用 `gets` 等函数和伪装成字符串的恶意代码，需要注意所有 `'\0'` 会终止字符串，以及所有 `'\n'` 会终止 `gets`，所有 `' '` 会终止 `scanf`，我们需要避免这一点。

那么如何保证我们一定能够覆盖函数的返回地址，并且准确找到恶意代码的地址呢？通常的方案是：

- **nop sled**：在 shellcode 开始之前放置很多个 `nop` 指令，这样即使跳转地址不是真正的 shellcode，也会在执行一定的 `nop`（空指令）后开始执行 shellcode
- 在 shellcode 之后放置一系列对缓冲区地址的猜测（这也是基于 shellcode 中调用 `execve` 后的代码不会执行），这样有更高概率越过缓冲区长度并覆盖返回地址

### 栈金丝雀

**栈金丝雀（Stack Canary）** 是一个能够检验返回地址被覆盖的机制。它将某一个特殊值（即 **金丝雀（Canary）**）存入返回地址之前的栈上的某个地址，并在执行 `ret` 时对其进行检查。如果金丝雀被修改了，那么就认为返回地址被覆盖了。下面是一段演示代码：

```assembly
foo:
	push	%ebp
	mov		%esp, %ebp
	push	CANARY
	sub		$16, %esp
	# 省略定义部分
	call	gets
	mov		-4(%ebp), %eax
	cmp		CANARY, %eax
	jne		stack_chk_fail		# 这是一个特殊的地址
	leave
	ret
```

为了破解栈金丝雀，我们需要保证 shellcode 中包含金丝雀。不过由于前面提到的标准库函数有不同的终止符，当金丝雀的值是 0 时，我们就不能通过 `strcpy` 来执行恶意代码；当金丝雀的值是 `'\n'` 时，就不能通过 `gets` 来执行恶意代码；当金丝雀的值是根据一些条件随机生成的，就不能通过 `write` 来执行恶意代码。

总结来讲，栈金丝雀是一个非常轻量却有效的阻止缓存区溢出导致返回地址被覆盖的方法。在 **GCC** 和 **Clang** 中这是默认实施的机制，可以通过 `-fno-stack-protector` 来禁止这个机制。

### 安全函数

比较好的根除缓冲区溢出的方式是干脆不使用那些有问题的函数，如 `gets`、`strcpy` 等，而是使用指定了读写字节数的 `fgets`、`strncpy` 等。通过指定输入的最大字节数，可以阻止 shellcode 任意注入代码。

### 变种：函数指针

我们可以通过函数指针来进行控制流劫持：

```c
struct msg_t {
  	char text[128];
    void (*foo)(char*);
};
```

此时可以通过让 `text` 溢出来让 `foo` 指向攻击者给定的恶意函数。**C++** 也有类似的用法，其通过虚函数做到：

```cpp
class Shape {
public:
  	virtual float area();  
};
class Circle : public Shape {
public:
   	Circle(float r_) : r(r_) {}
    float area() const override { return PI * r * r; }
private:
    float r;
};
class Square : public Shape {
public:
    Square(float a_) : a(a_) {}
    float area() const override { return r * r; }
private:
    float a;
};
```

虚函数本质的实现方式是在对象的开头存储一个虚指针，指向其实例类型需要调用的成员函数表。我们可以在这样的虚对象前设立一个数组，然后通过缓冲区溢出来覆盖虚指针，让通过虚指针调用的函数为已经设置好的恶意代码（如 shellcode）。

### 变种：释放后使用

```c
struct msg_t {
  	void (*foo)(char*);
    char text[128];
};
struct student_t {
  	int uid;
    char name[128];
};

void hack() {
 	student_t* s = malloc(sizeof(student_t));
    free(s);
    msg_t* m = malloc(sizeof(msg_t));	// 这里分配的地址和 s 可能是一样的
    s->uid = update_uid();				// 这里会将 foo 覆盖，因为 uid 和 foo 占同样的内存空间
}
```

### 总结：缓冲区溢出

缓冲区溢出曾经是非常常见的攻击手段，但是随着栈金丝雀和更安全的函数的普及，它在内存安全中的威胁度下降了很多。不过它的变种，比如释放后使用，依然是主要的内存安全问题的诱因之一。

缓冲区溢出及其变种的本质实际上在于将数据和代码进行了混用。不难发觉数据的特点应该是可修改但不可执行，而代码的特点是可执行却不可修改。如果能够利用这个性质禁止执行数据的执行和可执行文件的修改，就能够杜绝该问题。不过即使如此，在已存在的代码中，我们依然能够找到漏洞。

### 返回到标准库攻击

## 恶意程序

在我们成功攻入一个电脑，无论是通过缓冲区溢出、USB 自动运行、特权提升还是其它任何手段，通常随后我们都会试图运行 **恶意程序（Malware）**，其指代任何会破坏计算机、服务器抑或是网络的软件，包括了木马、病毒、蠕虫等分类。它们造成的伤害包括但不限于：

- 破坏硬件
- 消除数据
- 加密数据
- 泄露数据
- 骚扰用户
- 启动其它活动（比如垃圾邮件）
- 对用户进行录屏、录像、记录键盘交互信息等 

我们接下来会一一介绍并分析这些恶意程序的类型。对它们进行分类的依据来源于它们的 **传播行为（Propagation Behavior）** 或 **负载行为（Payload Behavior）**，也即它们如何传播以及如何造成破坏。

### 木马

**木马（Trojan）** 得名于特洛伊木马，它伪装成一个无害的程序，在用户下载之后，其中的负载将对用户进行利用。安卓平台上由于 **应用重打包（App Repackaging）** （下载应用包（.apk），解压缩后放入有害负载，再重新压缩并发布）的流行，木马的鉴别相当困难。这也是为什么即使不从第三方应用市场下载应用包，从 Google Play 依然能下载到重打包的应用。

### 后门

**后门（Backdoor）** 顾名思义，是软件中的特殊“通道”，让特定的用户能够拥有对数据更高的管理权限。当正常使用软件时，并不会出现任何问题。但当特殊功能启动时，应用可能会做出其权限以外的事，比如权限提升。

### 计算机病毒

**计算机病毒（Computer Virus）** 会通过修改其它文件的内容来不断复制自己。也可以通过用户行为，如点击邮件附件或加载 USB 驱动来传播。其得名于现实世界的病毒，因为它的模式确实和其非常相似：

- 潜伏期：此时计算机病毒存在，但是并不行动以防被发现。
- 传播期：通过感染其它文件来复制自己。
- 触发期：在某个逻辑条件成立的情况下，计算机病毒会从潜伏期或传播期进入最后阶段。
- 行动期：计算机病毒执行恶意负载。

有下面几种形式感染一个文件：

- 覆盖原有的信息。
- 前缀在文件信息之前。
- 感染库文件，此时每次使用这个库文件都会感染新的文件。此时计算机病毒可能会长期存储在内存中。
- 存储在 MS Office 文档中，也即宏病毒。

### 网络蠕虫

**网络蠕虫（Internet Worm）** 和计算机病毒类似，都有自我复制的能力，但不同的是它通过修改正在运行的代码来立即执行它自己，使得其具有非常强大的传播能力。相比之下计算机病毒修改的是存储的代码，且通常需要用户的行为才能传播。 两者的分别也没有那么明显，有些恶意程序会结合两者的特点设计。 

### 利用包

**利用包（Exploit Kit）** 包含了多种利用手段，并根据攻击对象的弱点实行不同的利用方式。

### 勒索软件

**勒索软件（Ransomware）** 会将用户数据加密、混淆，甚至销毁，并对用户进行勒索。

### 远程访问木马

**远程访问木马（Remote Access Trojan）** 能够在远程访问用户的桌面，安装并执行其它的恶意程序，包括密码偷窃程序等。

### 逻辑炸弹

**逻辑炸弹（Logica Bomb）** 在特定时机满足时进行恶意行动。

### 释放器

**释放器（Dropper）** 用于下载并执行其它的恶意程序，通常以木马形式传播。

### 僵尸网络

**僵尸网络（Botnet）** 通过多种传播方式，将被感染的对象（bot）组建成可以被攻击者（botmaster）控制的网络。它们之间通过 **指令与控制（Command and Control, C&C）** 进行交流，其包括：

- 激活信息，当对象被感染时会发送给攻击者。
- 垃圾邮件指令
- DDos 指令
- HTTP 代理指令

### 其它的恶意程序

- **可能不需要的程序（Potentially Unwanted Program, PUP)**：一些程序可能提供用户未经同意的其它功能，包括额外下载其它应用、广告、应用功能欺骗、卸载困难等。
- 网络劫持 PUP/木马：一些网页可能在后台运行 Javascript 脚本占用计算机资源。

### 总结

虽然恶意程序各式各样，但是它们依然类似于普通软件，只能执行它们被系统允许的事情。在用户空间中，恶意程序的权限和启动它的用户权限一致；核空间中，恶意权限的权限则相当于超级用户。



## 网络安全

### 网络基本知识

网络中最典型的模型就是 **客户端-服务器模型（Client-Server Model）**。客户端向服务器发出 **请求（Request）**，然后服务器根据请求向客户端发送 **响应（Response）**，这是通过 **超文本传输协议（Hypertext Transfer Protocol, HTTP）** 完成的。因此我们也可以构想出网络安全威胁的三种类型：

- 恶意客户端：目的是入侵并控制服务器；从服务器中窃取数据或攻击其它客户端。
- 恶意服务器：向客户端安装恶意程序，侵犯用户隐私，钓鱼网站，传播虚假信息等。
- 传播路径攻击者：在客户端和服务器交换信息途中进行监听或篡改数据。

网络的资源通常通过 **统一资源定位器（Uniform Resource Locator, URL）** 来识别，其组成部分按顺序如下：

- 协议：比如 **HTTP** 、**HTTPS** 等。
- 主机：也即是我们平时熟知的网址，比如 `baidu.com`。
- 端口：由于网络连接中首先会进行 **TCP/IP** 连接，此时需要指定端口。当省略这一项时，会使用默认端口 80（**HTTP**）或 443 （**HTTPS**）等。
- 路径：服务器中的路径。
- 查询：特定的参数，如 `?user=admin`。
- 片段：在加载好资源后，引导到网页的某个位置。

下面这个例子拥有以上提到的所有部分：

```
http://courses.engr.illinois.edu:80/cs461/sp2018?user=admin#grading
```

利用 **HTTP** 向服务器发送请求时，有下面几个 **请求方法（Request Method）**：

- `GET`：从某个资源中请求数据。
- `POST`：上传数据。
- `DELETE`：删除数据。

服务器收到请求后的响应有下面几种，可以通过其 **响应状态码（Response Status Code）** 的首位来判断：

- 1xx：信息性的。
- 2xx：成功连接，如 200 OK。
- 3xx：重定向，如 301 页面永久移除；304 没有修改。
- 4xx：客户端错误，如 403 禁止访问；404 找不到页面；405 方法不允许。
- 5xx：服务器错误，如 500 网络服务器错误。

下面我们用详细例子来演示 **HTTP** 连接的过程：

客户端向服务器发送了请求：

```
GET / HTTP/1.1
Host: www.example.com
Accept: text/html
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows 98)
```

第一行是请求方法，后面的部分是 **HTTP 头部（HTTP Header）**，其中包括了一系列的键值对。下面则是服务器的响应：

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Expires: Tue, 22 Sep 2020 14:33:51 GMT
...
<!doctype html>
<html>
<head>
  <title>Example Domain</title>
  ...
```

其中传回的网页内容是通过 **超文本标记语言（Hypertext Markup Language, HTML）** 表示的。**HTML** 可以为网页的布局进行一定的格式划分和依赖关系说明。下面的 **HTML** 就实现了一个非常简单的网页：

```html
<!DOCTYPE html>
<html>
    <head>
        <title>My First Webpage</title>
    </head>
    <body>
        <p>Hello, World!</p>
        <a href="/webpage2.html">Next Page</a>
        <br>
        <img src="http://wiki.com/Earth_globe.png">
    </body>
</html>
```

**HTML** 标签中，非常重要的一个是 `<form>`，它可以将一些信息打包成请求发送给网站的某个目标。示例如下：

```html
<!DOCTYPE html>
<html>
    <head>
        <title>My Second Webpage</title>
    </head>
    <body>
        <form action="/login" method="POST">
            Username:
            <input type="text" name="username">
            <br>
            Password:
            <input type="password" name="password">
            <input type="submit" name="submit">
        </form>
    </body>
</html>
```

**HTML** 能显示的内容都是静态的，如果需要动态的内容，我们就需要使用 **Javascript**，它能够：

- 改变网页内容。
- 响应事件，比如鼠标点击、移动等。
- 访问硬件，比如摄像头。
- 设置或读取 cookies。
- 发送网络请求。

我们在 **HTML** 中可以使用 `<script>` 标签来内嵌 **Javascript** 语句。一些 **Javascript** 语句也可以出现在其它标签中，比如：

- 事件响应，比如下面例子中 `<img>` 标签的 `onMouseOver` 属性：

  ```html
  <img src="picture.gif" onMouseOver="alert('Leave the picture alone!')">
  ```

服务器接收 **HTTP** 请求后，会返回 **HTML**、**Javascript**、**CSS** 等文件，其中 **HTML** 中的内容可以被动态解释，比如通过 **超文本处理器（Hypertext Processor, PHP）**：

```html
<!DOCTYPE html>
<html>
    <body>
        Welcome! Your IP address is:
        <?php echo $_SERVER['REMOTE_ADDR'] ?>
    </body>
</html>
```

上面的 **HTML** 经过 **PHP** 处理后会变为：

```html
<!DOCTYPE html>
<html>
    <body>
        Welcome! Your IP address is:
        130.126.255.93
    </body>
</html>
```

除了客户端和服务器之外，通常还有一个 **数据库（Database）** 用于存储服务器的数据。客户端向服务器请求数据时，服务器可能还需要从数据库中通过 **结构化查询语言（Structured Query Language, SQL）** 来得到数据。

#### 一些网络安全政策

同一个浏览器中，不同网站之间的数据是不互通的，这是遵循 **同源政策（Same-Origin Policy）**的结果。通常一个 **网页源（Web Origin）** 是指协议、主机以及端口的三元组。只有拥有相同网页源的网页才能共享数据（注意协议和主机是大小写不敏感的）。

**HTTP** 的响应会在 **HTTP** 头中说明其是否能被另一个网页源读取。默认情况下不允许 **跨源请求分享（Cross-Origin Request Sharing, CORS）**。如果某物被同源政策拦截，浏览器会报告这违反了 **CORS** 。

浏览器中通常存储许多敏感信息，但却不得不运行来自各个网站上的 **Javascript** 程序。比较常见的解决方案是 **浏览器隔离（Browser Isolation）**。它将

#### Cookies

浏览器为每个网页源提供一系列键值对存储每次请求的内容，我们将每个键值对称为 **cookie**。
