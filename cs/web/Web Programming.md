# Web Programming

**Web** 深似海；自从开始学 **HTML** + **CSS** + **JavaScript** 三件套，感觉 **C++** 都显得眉清目秀的（大概也是因为 **C++** 我只关注语言特性和标准库）。但即使它功能繁杂体系庞大，在 **Web** 应用空前繁荣的现在，如果对它一无所知未免有点落后于时代了。这篇是我学习 **Web** 开发相关知识的笔记，主要资料来源是 **Udemy** 的在线课程 *The Web Developer Bootcamp*，同时 **Mozilla** 提供的高质量文档也给我很多帮助。

**Web** 应用跑在浏览器上，因此一般在网页上直接编写，或在 **VS Code** 编写并使用插件浏览会非常方便。可惜 **Markdown** 并不支持这样高级的功能，所以本篇笔记中基本上只会给出代码，仅在少数情况下会给出网页的图片预览。

[TOC]



## **HTML** 入门

**超文本标记语言（HyperText Markup Language，HTML）** 是网页的实质，所有网页的内容，比如图片、视频、链接、文本等都是 “嵌在” **HTML** 中显示的。初学的时候，我们可以手写一个 **HTML**，就像写一个 **Markdown** 文件一样（只不过繁琐得多）。从这个角度看，它和我们熟悉的编程语言截然不同：它是一个标记语言，用于结构化网页内容，但不能进行通用的工作（比如编写算法等），可以类比 **Markdown** 或 **Tex**。

### **HTML** 元素

**HTML** 使用尖括号结构 `<>` 包围的名字来表示一个 **标签（Tag）**，用于提示浏览器和程序员其作用。标签可以分为 **开始标签（Opening Tag）** 和 **结束标签（Closing Tag）**，后者的标签名以斜杠 `/` 开始。比如 `<abc>` 是一个 `abc` （的开始）标签，而 `</abc>` 是 `abc` 的结束标签。这两种标签的结合形成了一个 **HTML** **元素（Element）**。比如 `<abc></abc>` 就是一个 `abc` 元素。少数 **HTML** 元素不需要结束标签，我们之后会遇到。

下面介绍一些比较常见的元素：

```html
<p>
    This is a paragraph. <!-- 这是一个段落元素 p，其表示文本的一个段落。开始和结束标签中间的内容就是段落内容。需要注意：一些空白字符（如换行
                              和制表符会被忽略，此时需要用其它方式输入，后面会介绍 -->
    We can <i>italicize</i> texts and <b>bold</b> texts easily.  <!-- i 和 b 元素分别用于斜体或粗体 -->
</p>
<h1>
    THIS IS THE TOP HEADING!!! <!-- 这是一个头元素 h1，其表示最高层级的一个标题。类似地，h2、h3、h4 等是按照顺序排列的更低层级的标题。所有头
                                    元素都默认为粗体，其字体大小随层级降低而变小 -->
</h1>
```

段落元素中的所有内容会作为一个组和其它元素（比如其它段落）分开；在网页中我们可以看到明显的分隔。头元素一共有六个，即 `h1` 至 `h6`。

[这里](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)是 **Mozilla** 指南列出的所有 **HTML** 支持的具有特殊含义的元素。

#### **HTML** 模板

有了关于 **HTML** 元素的基础知识，我们可以写出一个正式的 **HTML** 文件了。下面是一个示例：

```html
<!DOCTYPE html> <!-- 用于标识该文件为 HTML5 -->
<html> <!-- 所有 HTML 文件的根元素 -->
    <head> <!-- HTML 文件的配置信息，通常不包含网页显示的内容 -->
        <title>First webpage</title> <!-- 显示在浏览器标签页的文本，通常也是搜索引擎返回结果的标题 -->
    </head>
    <body> <!-- 网页内容的主体 -->
        <h1>
            Hello World!!
        </h1>
    </body>
</html>
```

**HTML5** 是 **HTML** 最新的标准，其规定了一系列 **HTML** 标签与属性的命名规则和语义，且仍在不断更新。可以在[这里](https://html.spec.whatwg.org/)找到其全文。目前所有主流浏览器都支持 **HTML5** 且默认将所有 **HTML** 文件以该标准进行理解。

#### **HTML** 列表

和 **Markdown** 中的列表（`1.` 或 `-` 等开头的段落）类似，我们可以用 `ul` 或 `ol` 元素表示无序和有序列表；列表中的每一项则用 `li` 元素表示：

```html
<ul> <!-- ul 元素表示无序列表，在网页中默认会以黑色实心小点表示 -->
    <li>Monday</li>
    <li>Wednesday</li>
    <li>Friday</li>
</ul>
<ol> <!-- ol 元素表示有序列表，在网页中默认会以数字显示 -->
    <li>Nested list:
        <ul>
            <li>first</li>
            <li>second</li>
        </ul>
    </li>
    <li>42 is the answer.</li>
</ol>
```

#### **HTML** 锚

**HTML** 中的链接通过 **锚（Anchor）** 元素 `a` 表示，其地址作为这个元素的一个 **属性（Attribute）** `href` 表示。示例如下：

```html
<a href="https://www.google.com">SOURCE OF MAGIC</a> <!-- a 表示锚，其文本会被默认渲染为蓝色带下划线 -->
```

这里 `href` 和标签名用空格分隔，后面接上 `=` 和字符串表示它的值。这里的等号两边不能出现空格，因为 **HTML** 元素的属性之间就是通过空格来区分的；后面我们会遇到使用更多 **HTML** 属性的情形。有关这里的链接，有一点需要阐明：url 中的 `https://` 在这种情况下是必须的，否则浏览器会从当前目录（可以是服务器或本地）出发寻找对应的文件。

#### **HTML** 图像

**HTML** 的图像元素 `img` 没有结束标签；我们可以使用 `src` 属性来给出图源，并用 `width` 等属性来规定图片尺寸：

```html
<img src="images/stuff.jpg" width="200px"> <!-- img 表示图像，如果找不到 src 指定的文件，则会在图像所在位置显示一个“破碎的图片”图像 -->
```

当图像不存在时，我们可以指定其显示特定的提示信息（而非令人沮丧的“破碎的图片”）：

```html
<img src="does-not-exist/stuff.jpg" alt="Image of some stuff.">
```

#### **HTML** 块与内联

**HTML** 中，元素之间的布局可以简单分为两个类型：**块（Block）** 或 **内联（Inline）**。前者描述了元素在竖直方向的分布，后者描述水平方向的分布。我们学到的段落元素就是一个块。我们有两个基本的元素，`div` 和 `span` 用于表示块和内联布局，其充当了容器的作用：

```html
<div> <!-- div 通常用于框出一系列段落和其它元素 -->
    This is <span style="color:red">red</span>. <!-- span 通常用于行内部分内容的 style 特化，比如标红，使用不同字体等 -->
</div>
```

#### 其它元素

- `br`：换行。不需要结束标签。
- `hr`：一行横线。不需要结束标签。
- `sup`：上标。
- `sub`：下标

### **HTML** 实体

**HTML** 将一些字符设置为保留字，比如 `<`。这很好理解，因为 **HTML** 中存在大量的尖括号。为了能够表示出这样的保留字，以及大量其它不能用键盘打出的字符，我们可以使用 **HTML** **实体码（Entity Code）**，它颇类似于转义字符。一个实体码由 `&` 起始，`;` 作为终止，中间可能是一个英文单词，或由 `#` 及 `#x` 开头的一串数字（即其 **UTF** 编码）。可以在[这里](https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references)找到所有有名字的 **HTML** 实体以及其对应的编码。

```html
<p>
    1 &lt; 2 <br> <!-- < 字符 -->
    ABC<sup>&reg;</sup> <!-- ® 字符 -->
</p>
```

### 语义标记

在许多网站上，用 F12 进入网页审查后，会发现其元素基本上都是 `div`，这相当无趣。自从 **HTML5** 开始，标准建议使用一些特殊的名字，即 **语义标记（Semantic Markup）** 来提示不同 `div` 或 `span` 的功能。下面是一些例子：

- `main`：表示网页中心部分内容的主体，在网页中应是唯一的。其不包括搜索栏 、导航栏、侧边栏、网站 Logo 等通常出现在网页边际的部分。
- `nav`：表示网页的导航栏，其中包含通往其它网址的链接。网页顶部的指引列表可以通过 `nav` 标记。
- `section`：表示一个独立的部分，其中可以包含几个头和段落。
- `article`：表示一个自足的整体，可以独立使用于其它上下文。比如网页中内嵌的天气预报。
- `aside`：表示和网页主体内容相关的信息，通常用于标记侧边栏或引用框。
- `header`：表示网页的头部，通常包含网站 Logo 和导航栏等，和 `main` 并列。也可以用于 `article` 当中。
- `footer`：表示网页的尾部，通常包含联系信息和版权信息等，和 `main` 并列。也可以用于 `article` 当中。
- `time`：表示一个特定的时间，用于提供文本的格式化时间表示。比如 `<time datetime="2022-02-22">Today</time>`。和前面的标记不同，这个标记是一个 `span`，
- `figure`：表示一个图像，其中可以包含 `figcaption` 标记图像的标题。

### **HTML** 表格

**表格（Table）** 可以用来表示任何网格状的结构。事实上在网页出现的早期，程序员经常依赖于表格结构来控制网页布局。

```html
<table>
    <thead> <!-- 表格的头部，可以类比 head 元素 -->
        <caption>This is a 2x3 matrix</caption> <!-- 表格的标题 -->
        <tr> <!-- 表格的一行 -->
            <th>Column #1</th> <!-- 表格的头 -->
            <th>Column #2</th>
            <th>Column #3</th>
        </tr>    
    </thead>
    <tbody> <!-- 表格的主体，可以类比 body 元素 -->
        <tr>
            <td>Row #1</td> <!-- 表格的项 -->
            <td>a_11</td>
            <td>a_12</td>
            <td>a_13</td>
        </tr>
        <tr>
            <td>Row #2</td>
            <td>a_21</td>
            <td>a_22</td>
            <td>a_23</td>
        </tr>    
    </tbody>
</table>
```

利用 `colspan` 和 `rowspan` 属性，我们可以创建布局更加复杂的表格：

```html
<table>
    <thead>
    	<tr>
            <th rowspan="4">Rows</th> <!-- 这个 row 会跨越四行 -->
            <th colspan="2">Columns</th> <!-- 这个 col 会跨越两列 -->
        </tr>
        <tr>
            <th>Column #1</th>
            <th>Column #2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>a_11</td>
            <td>a_12</td>
        </tr>
        <tr>
            <td>a_21</td>
            <td>a_22</td>
        </tr>
    </tbody>
</table>
```

### **HTML** 表单

**HTML** **表单（Form）** 是一个非常重要的元素，在其中可以放置文本、按钮等元素，然后将所有信息在特定时机提交。网页中的搜索栏、登录框等都利用了这个元素。

```html
<form action="https://somesite.com"> <!-- form 包含一系列 input 元素，在 submit 表单时会将数据发送到 action 属性指定的地址 -->
    <input id="some-text" type="text" placeholder="enter some text" name="t"> <!-- input 作为各种类型的输入，type 属性决定了它们的类型 -->
    <input id="some-secret" type="password" placeholder="enter some secrets" name="s"> <!-- placeholder 属性给出默认的值 -->
    <input id="some-color" type="color" name="c">
    <div>
        <label for="choice-1">I choose this.</label> <!-- label 元素的 for 属性会绑定在某个元素的 id 上，点击 label 会聚焦于该元素 -->
        <input type="checkbox" name="1" id="choice-1"> <!-- id 属性在同一个网页中对每个元素都是唯一的，我们总可以通过 id 锁定一个元素 -->   
    </div>
    <div>
        <label for="choice-2">I choose THIS.</label>
   	    <input type="checkbox" name="2" id="choice-2">
    </div>
    <button type="button"> <!-- button 表示按钮，使用 type 属性设置其类型 -->
        Don't submit yet!
    </button>
    <button type="submit"> <!-- 在表单中，submit 是 button 元素的默认类型。点击 submit 按钮后会将表单中所有数据以特定格式打包后发送 -->
        Submit everything!
    </button>
</form>
```

提交表单时，每个元素会以 `?name=value` 的形式依次附到 `action` 属性给出的网址后面，其中 `name` 和 `value` 都是 `input` 元素的属性（后者是其内容）。

```html
<form action="https://www.google.com/search">
    <input id="target" type="text" placeholder="search on google" name="q">
    <button>search</button>
</form>
```

