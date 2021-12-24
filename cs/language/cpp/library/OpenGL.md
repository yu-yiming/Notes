# OpenGL

**OpenGL** 是 **C/C++** 的著名图形库。它建立于硬件和系统之上，提供了跨平台的软件接口（即 **API**），用以渲染 2D 和 3D 的矢量图。它可以和 **图形处理单元（Graphics Processing Unit, GPU）** 进行交互，并可以使用 **图形库着色器语言（Graphic Library Shader Language, GLSL）** 在 **GPU** 上进行编程。从 20 世纪 90 年代至今，**OpenGL** 已经广泛用于大量生产程序中，如 **计算机协助设计（Computer-Aided Design, CAD）**、**虚拟现实（Virtual Reality, VR）**、**数据可视化**、**游戏引擎** 等。一些更高层的 **GUI** 软件会将 **OpenGL** 包装起来以便于使用，比如 **Qt**。

[TOC]



## 基础部分

### 入门

**OpenGL** 涉及的领域是 **计算机图形（Computer Graphics）**。图形计算最显著的特点是 **管线（Pipeline）** 以及 **并行（Parallelism）** 机制。在随后我们会介绍的管线中存在多个 **阶段（Stage）**；程序可以同时进行不同对象的不同阶段，因为它们之间互不干扰，这也被称为并行。这非常类似于工厂中的流水线：不同岗位的工人可以同时进行工作，因为每一步加工都是独立的。有了这样的机制，才能有现代 **GPU** 高效的表现。

**OpenGL** 提供了底层图形子系统与用户的 **GUI** 程序间的抽象接口。我们不需要知道每个平台上的 **GPU** 或操作系统的优化瓶颈，而可以专注于程序逻辑。另一方面，**OpenGL** 也没有成为过高层的 **GUI** 接口，否则不利于用户针对底层进行优化；而且当底层硬件加入新特性后，高层 **GUI** 接口可能需要做出较大改变才能适应新特性。

现代的 **GPU** 拥有每秒万亿级浮点数计算能力、数十亿级比特的内存、读取速率在千亿级比特每秒，有能力在高刷新率下驱动多个数百万像素的显示屏。由于它极高的性能，许多和图形无关的事情 **GPU** 也可以胜任，如物理模拟、人工智能等~~（还有挖矿）~~。**GPU** 由许多小型可编程处理器组成，我们将这些小型结构称为 **着色器核（Shader Core）**。每个着色器核的效果不算高，只能在一至数个时钟周期执行着色器的一个指令，且没有像 **CPU** 那样高级的功能，如乱序执行、分支预测、超标量处理等。不过，由于 **GPU** 中存在数十到数千个这样的核，加起来的效果是非常可观的。

通常，我们将图形系统按照顺序排列为下面这些阶段。每个阶段或者通过着色器处理、或者通过固定函数处理。这里的固定函数可以通过 **着色器程序（Shader Program）** 进行调整，不过我们一般让 **GPU** 自行提供。

- **顶点拾取（Vertex Fetch）**：固定函数。
- **顶点着色器（Vertex Shader）**：可编程。
- **曲面细分控制着色器（Tessellation Control Shader）**：可编程。
- **曲面细分（Tessellation）**：固定函数。
- **曲面细分求值着色器（Tesellation Evaluation Shader）**：可编程。
- **几何着色器（Geometry Shader）**：可编程。
- **栅格化（Rasterization）**：固定函数。
- **片段着色器（Fragment Shader）**：可编程。
- **帧缓冲区操作（Framebuffer Operations）**：固定函数。



### 第一个 **OpenGL** 程序

**OpenGL** 程序通常需要一些起始代码，用以定义一个通用的应用类 `application`，这些代码对于现在来说没必要进行阐明。我们这里假设它们定义在 `app.h` 的 `app` 命名空间。在源文件中，我们需要包含这个头文件，并定义 `application` 的子类；此外，还应该使用 `DECLARE_MAIN` 宏来定义 `main` 函数（这个宏也是定义在 `app.h` 头文件里的）：

```cpp
#include "app.h"
class MyApplication : public app::application {
public:
    void render(double currentTime) override {
        static GLfloat const red[] = { 1.0f, 0.0f, 0.0f, 1.0f };
        glClearBufferfv(GL_COLOR, 0, red);
    }
};

DECLARE_MAIN(MyApplication);
```

接下来让我们好好解析这个程序。首先，`render` 是一系列应用类会默认调用的函数之一，其参数接收系统当前的时间。`main` 函数会调用应用对象的 `run` 方法，其中会调用 `startup` 方法，并在循环中不断调用 `render` 方法，在屏幕中渲染我们定义的图形。

这里有一个重要的函数，`glClearBufferfv`，它会将一个缓冲区用特定值覆盖。其签名如下：

```cpp
void glClearBufferfv(GLenum buffer, // 一个 OpenGL 缓冲区
                     GLint drawBuffer, // 描述缓冲区索引
                     GLfloat const* value) // 填充缓冲区的值
```

此处我们放入的参数如下：

- `GLCOLOR : GLenum`：表明我们指定的是颜色缓冲区。
- `0 : GLint`：表明这是第一个缓冲区。
- `red : GLfloat[]`：值为 `1.0f, 0.0f, 0.0f, 1.0f` 的数组，表示 **RGBA** 中的红色。

注意到这个函数的命名，除了表示其作用的 `ClearBuffer` 以外，有一个 `gl` 前缀和一个 `fv` 后缀。这个前缀 *所有* **OpenGL** 函数都会加上；而 `fv` 表明这个函数会接受一个浮点数（`f`）的数组（`v`），正好对应最后一个参数。

上面的程序未免过于简单。让我们接下来尝试探索标准的 **OpenGL** 程序需要关心的主要步骤。首先是顶点着色器：

```glsl
#version 450 core
void main(void) {
    gl_Position = vec4(0.0, 0.0, 0.5, 1.0);
}
```

之后是片段着色器：

```glsl
#version 450 core
out vec4 color;
void main(void) {
    color = vec4(0.0, 0.8, 1.0, 1.0);
}
```

为了将着色器程序和主程序链接，我们需要编写一些 **C++** 代码，举例如下：

```cpp
[[nodiscard]]
GLuint compile_shaders() {
    static GLchar const* vertex_shader_source[] = {
        "#version 450 core\n"
        "void main(void) {\n"
        "    gl_Position = vec4(0.0, 0.0, 0.5, 1.0);\n"
        "}\n"
    };
    static GLchar const* fragment_shader_source[] = {
        "#version 450 core\n"
        "out vec4 color;\n"
        "void main(void) {\n"
        "    color = vec4(0.0, 0.8, 1.0, 1.0);\n"
        "}\n"
    };
    
    // 创建并编译顶点着色器
    GLuint vertex_shader = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vertex_shader, 1, vertex_shader_source, nullptr);
    glCompileShader(vertex_shader);
    
    // 创建并编译片段着色器
    GLuint fragment_shader = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fragment_shader, 1, fragment_shader_source, nullptr);
    glCompileShader(fragment_shader);
    
    // 创建程序，和着色器链接在一起
    GLuint program = glCreateProgram();
    glAttachShader(program, vertex_shader);
    glAttachShader(program, fragment_shader);
    glLinkProgram(program);
    
    // 释放着色器资源
    glDeleteShader(vertex_shader);
    glDeleteShader(fragment_shader);
    
    return program;
}
```

上面的程序中用到了下面几个重要的库函数：

- `glCreateShader`：用于创建空的着色器对象。
- `glShaderSource`：使着色器对象得到着色器代码的复制。
- `glCompileShader`：编译着色器对象中存储的着色器代码为二进制代码，并存储在着色器对象中。
- `glCreateProgram`：创建程序对象。
- `glAttachShader`：在程序对象上添加着色器对象。
- `glLinkProgram`：将程序对象上所有的着色器对象和程序对象链接在一起。此时程序对象中会得到着色器对象的二进制代码。之后就可以在 **GPU** 上运行了。
- `glDeleteShader`：释放着色器对象。

令人惊奇的是，我们直接用字符串字面量来表示一段着色器代码，这看起来有点粗暴；但这是最方便的做法。

上面这个函数 `compile_shaders` 只需在程序开始时调用一次，准备好所有着色器程序即可。因此我们将其使用于 `startup` 成员函数中。

最后，为了能在屏幕上画出图形，我们需要创建一个 **顶点数组对象（Vertex Array Object, VAO）**，其用于为顶点着色器提供输入。即使不需要输入的情况（比如我们这个例子），也要创建 **VAO**。可以通过 `glCreateVertexArrays` 函数创建 **VAO**：

```cpp
void glCreateVertexArrays(GLsizei n,
                          GLuint* arrays);
```

然后用 `glBindVertexArray` 来将这个 **VAO** 和当前上下文绑定：

```cpp
void glBindVertexArray(GLuint array);
```

（题外话，**OpenGL** 中经常可以看到这样的构建模式：即先通过某种 `Create` 函数构造一个对象，然后再通过某种 `Bind` 函数将其引入当前上下文）

同时，对应的还有

下面是主程序类的定义：

```cpp
class MyApplication : public app::application {
public:
    void startup() override {
        rendering_program = compile_shaders();
        glCreateVertexArrays(1, &vertex_array_object);
        glBindVertexArray(vertex_array_object);
    }
    
    void shutdown() override {
        glDeleteVertexArrays(1, &vertex_array_object);
        glDeleteProgram(rendering_program);
        glDeleteVertexArrays(1, &vertex_array_object);
    }
    
    void render(double currentTime) {
        GLfloat const color[] = {
            static_cast<float>(sin(currentTime)) * 0.5f + 0.5f,
            static_cast<float>(cos(currentTime)) * 0.5f + 0.5f,
            0.0f, 1.0f
        };
        glClearBufferfv(GL_COLOR, 0, color);
        glUseProgram(rendering_program);			// 使用我们的程序
        glDrawArrays(GL_POINTS, 0, 1);				// 画一个点
    }
    
private:
    GLuint rendering_program;
    Gluint vertex_array_object;
}
```

其中新出现的函数 `glDrawArrays` 将顶点送往 **OpenGL** 管线，其签名如下：

```cpp
void glDrawArrays(GLenum mode, // 渲染的模式
                  GLint first,
                  GLsizei count); // 渲染顶点个数
```

每一个顶点都会被送入顶点着色器程序中，



