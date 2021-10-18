---
title: LaTeX-学习笔记
tags:
  - notes
  - 学习笔记
  - LaTeX
categories: notes
date: 2021-10-12 08:34:46
---

### LaTeX 简介

> LaTeX, which is pronounced «Lah-tech» or «Lay-tech» (to rhyme with «blech» or «Bertolt Brecht»), is a document preparation system for high-quality typesetting. It is most often used for medium-to-large technical or scientific documents but it can be used for almost any form of publishing.

LaTeX 是一个文档准备系统 (Document Preparing System)，它非常适用于生成高印刷质量的科技类和数学类文档。它也能够生成所有其他种类的文档，小到简单的信件，大到完整的书籍。

上一篇文章{% post_link LaTeX环境搭建和配置教程 %}讲了 LaTeX 环境，本文是 LaTeX 详细用法，其内容主要归纳自《一份（不太）简短的 LaTeX 2ε 介绍》一书，文末参考连接中有该书的连接。

<!-- more -->

### LaTeX 基本概念

#### 命令

LATEX 中命令2以反斜线 \ 开头，为以下两种形式之一：
1. 反斜线和后面的一串字母，如 \LaTeX。它们以任意非字母符号（空格、数字、标点等）为界限。
2. 反斜线和后面的单个非字母符号，如 \$。


要注意 LATEX 命令是对大小写敏感的，字母形式的 LATEX 命令忽略其后的所有连续空格。如果要人为引入空格，需要在命令后面加一对花括号阻止其忽略空格。另外也可以在命令后面紧跟一个 \␣ 命令（反斜线加空格），代表插入一个间距。

```latex
\TeX ignore space.
\TeX{} stay space.
\TeX\ stay space.
```

一些 LATEX 命令可以接收一些参数，参数的内容会影响命令的效果。LATEX 的参数分为可选参数和必选参数。可选参数以方括号 [] 包裹；必选参数一般以花括号 {} 包裹。还有些命令可以带一个星号 *，可以把星号看作一种特殊的可选参数。

#### 环境

LATEX 中还包括环境，用以令一些效果在局部生效，或是生成特殊的文档元素。LATEX 环境的用法为一对命令 \begin 和 \end：

```latex
\begin{⟨environment name⟩}[⟨optional arguments⟩]{⟨mandatory arguments⟩}
% ...
\end{⟨environment name⟩}
```

其中 ⟨environment name⟩ 为环境名，\begin 和 \end 中填写的环境名应当一致。类似命令，{⟨mandatory arguments⟩} 和 [⟨optional arguments⟩] 为环境所需的必选和可选参数。LATEX 环境可能需要一个或多个必选/可选参数，也可能完全不需要参数。部分环境允许嵌套使用。

#### 文档类

LATEX 源代码以一个 \documentclass 命令作为开头，其可选参数指定了文档使用的文档类；而文档类规定了 LATEX 源代码所要生成的文档的性质——普通文章、书籍、演示文稿、个人简历等等。

```latex
\documentclass[⟨options⟩]{⟨class-name⟩}
% 指定纸张为 A4 大小，基本字号为 11pt，双面排版
\documentclass[11pt,twoside,a4paper]{article} 
```

其中 ⟨class-name⟩ 为文档类的名称，如 LATEX 提供的 article | book | report（称为标准文档类），在其基础上派生的一些文档类如支持中文排版的 ctexart | ctexbook | ctexrep，或者有其它功能的一些文档类，如 moderncv | beamer 等。

可选参数 ⟨options⟩ 为文档类指定选项，以全局地规定一些排版的参数，如字号、纸张大小、单双面等等；更多可选参数见附录。

#### 宏包

在 \documentclass 和 \begin{document} 之间的位置称为导言区。在导言区中一般会使用 \usepackage 调用宏包，以及会进行对文档的全局设置。

宏包就是一些增强或补充 LATEX 功能的扩展，比如排版复杂的表格、插入图片、增加颜色甚至超链接等等。调用宏包的方法非常类似调用文档类的方法：

```latex
\usepackage[⟨options⟩]{⟨package-name⟩}
% 一次性调用三个排版表格常用的宏包
\usepackage{tabularx, makecell, multirow}
```
\usepackage 可以一次性调用多个宏包，在 ⟨package-name⟩ 中用逗号隔开。这种用法一般不要指定选项；使用多个宏包时指定选项，相当于给每个宏包指定同样的选项。如果有某个宏包不能识别指定的选项，则会出错。

在命令行中可以通过`texdoc ⟨pkg-name⟩`命令打开宏包本地的英文文档。

#### 文件

当编写长篇文档时，可以将源文件分割成若干个文件，例如将每章内容单独写在一个文件中，会大大简化修改和校对的工作。LATEX 提供了命令 \include 用来在源代码里插入文件：

```latex
\include{⟨filename⟩}
\include{chapters/a.tex} % 相对路径
\include{/home/Bob/file.tex} % Linux/macOS 绝对路径
\include{D:/file.tex} % Windows 绝对路径，用正斜线
```

⟨filename⟩ 为文件名，如果和要编译的主文件不在一个目录中，则要加上相对或绝对路径；⟨filename⟩ 可以不带扩展名，此时默认扩展名为 .tex。\include 在读入 ⟨filename⟩ 之前会另起一页，因此可以使用 \input 命令，它纯粹是把文件里的内容插入：

```latex
% 插入内容，不换页
\input{⟨filename⟩}
% 限制引用的文件
\includeonly{⟨filename1⟩,⟨filename2⟩,…}
```

另外 LATEX 提供了一个 \includeonly 命令来组织文件，用于导言区，指定只载入某些文件：导言区使用了 \includeonly 后，正文中不在其列表范围的 \include 命令不会起效。

> 一个实用的工具宏包 syntonly。加载这个宏包后，在导言区使用 \syntaxonly 命令，可令 LATEX 编译后不生成 DVI 或者 PDF 文档，只排查错误，编译速度会快不少：

```latex
\usepackage{syntonly}
\syntaxonly
```

#### 概念

- **引擎**：全称为排版引擎，是编译源代码并生成文档的程序，如 pdfTEX、XƎTEX 等。有时也称为编译器。
- **格式**：是定义了一组命令的代码集。LATEX 就是最广泛应用的一个格式，高德纳本人还编写了一个简单的 plain TEX 格式，没有定义诸如 \documentclass 和 \section 等等命令。
- **编译命令**：是实际调用的、结合了引擎和格式的命令。如 xelatex 命令是结合 XƎTEX 引擎和 LATEX 格式的一个编译命令。

### LaTeX 排版文字

#### 编码

Unicode 是一个多国字符的集合，覆盖了几乎全球范围内的语言文字。UTF-8 是 Unicode 的一套编码方案，一个字符可以由一个到四个字节编码，其中单字节字符兼容 ASCII 编码。

latex 命令及 pdflatex 命令下可以使用 inputenc 宏包支持 UTF-8 编码：

```latex
\usepackage[utf8]{inputenc}
```

xelatex 和 lualatex 命令原生支持 UTF-8 编码，而且也不适用 inputenc 宏包。将 .tex 源代码保存为 UTF-8 编码，并借助 fontspec 宏包调用适当的字体，就可以在源代码中输入任意语言的文字。

#### 中文排版

ctex 宏包和文档类是对 CJK 和 xeCJK 等宏包的进一步封装。ctex 文档类包括 ctexart / ctexrep / ctexbook，是对 LATEX 的三个标准文档类的封装，对 LATEX 的排版样式做了许多调整，以切合中文排版风格。

```latex
\documentclass{ctexart}
\begin{document}
中文\LaTeX{}排版。
\end{document}
```

注：建议在使用 ctex 宏包和文档类时总是将源代码保存为 UTF-8 编码，并用 xelatex 命令编译。

#### 字符

##### 空格

LATEX 源代码中，空格键和 Tab 键输入的空白字符视为“空格”。连续的若干个空白字符视为一个空格。一行开头的空格忽略不计。

行末的回车视为一个空格；但连续两个回车，也就是空行，会将文字分段。多个空行被视为一个空行。也可以在行末使用 \par 命令分段。

```latex
An empty line starts a new paragraph.\par 123456
% A \verb|\par| command also starts a new line.
```

##### 特殊字符

以下字符在 LATEX 里有特殊用途，如 % 表示注释，$、^、_ 等用于排版数学公式，& 用于排版表格，等等。直接输入这些字符得不到对应的符号，还往往会出错：

```txt
# $ & { } _ ^ ~ \ %
```

若要使用特殊字符需要在前面添加反斜杠 \，进行转义；其中 \^ 和 \~ 两个命令是需要带参数的，如果不加一对花括号（空参数），就将后面的字符作为参数，形成重音效果；而 \\ 被直接定义成了手动换行的命令，输入反斜杠就只好用 \textbackslash。

```latex
\# \$ \% \& \{ \} \_
\^{} \~{} \textbackslash
```

##### 注释

LATEX 用 % 字符作为注释。在这个字符之后直到行末，所有的字符都被忽略，行末的回车也不引入空格。

##### 连字

西文排版中经常会出现连字（Ligatures），常见的有 ff / fi / fl / ffi / ffl。

```latex
It's difficult to find \ldots .\\
It's dif{}f{}icult to f{}ind \ldots .
```

##### 引号

LATEX 的单引号 ‘ ’ 用 ` 和 ' 输入；双引号 “ ” 用 `` 和 '' 输入，虽然 " 也能表示双引号，但没有合适的单个符号用来表示前双引号，所以不常用在正文。

```latex
``Please press the `x' key.''
```

##### 连字号与破折号

LATEX 中有三种长度的“横线”可用：连字号（hyphen）、短破折号（en-dash）和长破折号（em-dash）。它们分别有不同的用途：连字号 - 用来组成复合词；短破折号 – 用来连接数字表示范围；长破折号 — 用来连接单词，与中文语境中的破折号用法类似。

```latex
daughter-in-law, X-rated\\
pages 13--67\\
yes---or no?
```

##### 省略号

LATEX 提供了命令 \ldots 来生成省略号，相对于直接输入三个点的方式更为合理。\ldots和 \dots 是两个等效的命令。

```latex
one, two, three, \ldots one hundred.\\
A, B, C, \dots Z.
```

##### 波浪号

\~ 命令，它可以用来输入波浪号，但位置靠顶端（\~ 命令主要用作重音，参考下一小节）。西文中较少将波浪号作为标点符号使用，通常使用中文的全角波浪号（～）。

```latex
H\^otel, na\"\i ve, \'el\`eve,\\
sm\o rrebr\o d, !`Se\ norita!,\\
Sch\"onbrunner Schlo\ss{}
Stra\ss e
```

{% asset_img LaTeX文本中的重音和特殊字符.png %}

#### 强调

LATEX 定义了 \underline 命令用来为文字添加下划线；但 \underline 命令生成下划线的样式比较机械，不同的单词可能生成高低各异的下划线，并且无法换行。

可以使用 ulem 宏包解决了这一问题，它提供的 \uline 命令能够轻松生成自动换行的下划线：

```latex
An \underline{underlined} text.
% 需要 \usepackage{ulem}
An example of \uline{some long and underlined words.}

Some \emph{emphasized words, including \emph{double-emphasized} words}, are shown here.
```
\emph 命令用来将文字变为斜体以示强调。如果在本身已经用 \emph 命令强调的文字内部嵌套使用 \emph 命令，内部则使用直立体文字：

#### 断行和断页

LATEX 将文字段落在合适的位置进行断行，尽可能做到每行的宽度一致，并且单词间距匀称。文字段落和公式、图表等内容从上到下顺序排布，并在合适的位置断页，分割成匀称的页面。在绝大多数时候，我们无需自己操心断行和断页。但偶尔会遇到需要手工调整的地方。

##### 手动断行

使用以下两个命令实现断行，它们有两点区别：一是 \\ 可以带可选参数 ⟨length⟩，用于在换行处向下增加垂直间距，而 \newline 不带可选参数；二是 \\ 也在表格、公式等地方用于分行，而 \newline 只用于文本段落中。

```latex
\\[⟨length⟩]
\\[20pt] % 增加 20pt 单位的垂直间距
\newline
```

##### 手动断页

断页的命令也有两个，通常情况下两个命令都能起到另起一页的作用，但有一些区别：一是在双栏排版中 \newpage 只起到另起一栏的作用；二是涉及到浮动体的排版上行为不同。

```latex
\newpage 
\clearpage
```

##### 自定义断行断页

可以通过以下命令告诉 LATEX 哪些地方适合断页，哪些地方不适合：

```latex
\linebreak[⟨n⟩]
\nolinebreak[⟨n⟩]

\pagebreak[⟨n⟩]
\nopagebreak[⟨n⟩]
```

以上命令都带一个可选参数，用数字 ⟨n⟩ 代表适合/不适合的程度，取值范围为 0-4，不带可选参数时，缺省为 4。比如 \linebreak[3] 意味着此处在断行时优先考虑；\nopagebreak 或\nopagebreak[4] 意味着禁止在此处断页。

以上命令适合给出优先考虑断行断页/禁止断行断页的位置，但不适合直接拿来断行或断页，使用 \newline 或 \newpage 等是更好的选择。因为 \newline 和 \newpage 会在断行/断页位置正确添加空白，但 \linebreak 和 \pagebreak 不能，它们的强行断行/断页会导致 LATEX 报 Underfull \hbox 等警告。

##### 断词

如果 LATEX 遇到了很长的英文单词，仅在单词之间的位置断行无法生成宽度匀称的行时，就要考虑从单词中间断开。对于绝大部分单词，LATEX 能够找到合适的断词位置，在断开的行尾加上连字符 -。如果一些单词没能自动断词，我们可以在单词内手动使用 \- 命令指定断词的位置。

```latex
a very very very long word abcdefghijklmnop\-qrstuvwxyz.
```

### LaTeX 文档元素

本章了解一个结构化的文档所依赖的各种元素——章节、目录、列表、图表、交叉引用、脚注等等。

#### 章节

一篇结构化的、条理清晰文档一定是层次分明的，通过不同的命令分割为章、节、小节。三个标准文档类 article、report 和 book1提供了划分章节的命令：

```latex
\chapter{⟨title⟩}
\section{⟨title⟩}
\subsection{⟨title⟩}
\subsubsection{⟨title⟩}
\paragraph{⟨title⟩}
\subparagraph{⟨title⟩}
```

其中 \chapter 只在 book 和 report 文档类有定义。这些命令生成章节标题，并能够自动编号。除此之外 LATEX 还提供了 \part 命令，用来将整个文档分割为大的分块，但不影响 \chapter 或 \section 等的编号。

上述命令除了生成带编号的标题之外，还向目录中添加条目，并影响页眉页脚的内容。每个命令有两种变体：

- 带可选参数的变体：\section[⟨short title⟩]{⟨title⟩} 标题使用 ⟨title⟩ 参数，在目录和页眉页脚中使用 ⟨short title⟩ 参数；
- 带星号的变体：\section*{⟨title⟩} 标题不带编号，也不生成目录项和页眉页脚。不影响较低层级的 \paragraph 和 \subparagraph 命令。

带编号层级：
- article 文档类带编号的层级为 \section / \subsection / \subsubsection 三级；
- report/book 文档类带编号的层级为 \chapter / \section / \subsection 三级。

####  目录

在 LATEX 中生成目录非常容易，只需在合适的地方使用命令，而正确生成目录项，一般需要编译两次源代码。

```latex
\tableofcontents
```

这个命令会生成单独的一章（book / report）或一节（article），标题默认为 “Contents”，可
通过后续给出的方法定制标题。\tableofcontents 生成的章节默认不写入目录（\section*
或 \chapter*），可使用 tocbibind 等宏包修改设置。

有时我们使用了 \chapter* 或 \section* 这样不生成目录项的章节标题命令，而又想手动生成该章节的目录项，可以在标题命令后面使用：

```latex
\addcontentsline{toc}{⟨level⟩}{⟨title⟩}
\addcontentsline{toc}{section}{manual add title}
```

其中 ⟨level⟩ 为章节层次 chapter 或 section 等，⟨title⟩ 为出现于目录项的章节标题。titletoc、tocloft 等宏包提供了具体定制目录项格式的功能，详情请参考宏包的帮助文档。

#### 文档结构

所有标准文档类都提供了一个 \appendix 命令将正文和附录分开2，使用 \appendix 后，最高一级章节改为使用拉丁字母编号，从 A 开始。

book 文档类还提供了前言、正文、后记结构的划分命令：

- \frontmatter 前言部分，页码为小写罗马字母格式；其后的 \chapter 不编号。
- \mainmatter 正文部分，页码为阿拉伯数字格式，从 1 开始计数；其后的章节编号正常。
- \backmatter 后记部分，页码格式不变，继续正常计数；其后的 \chapter 不编号。

以上三个命令还可和 \appendix 命令结合，生成有前言、正文、附录、后记四部分的文档。章节和目录部分再结合之前的 \include 命令和其它一些命令示意了一份完整的文档结构。

```latex
\documentclass[...]{book}
% 导言区，加载宏包和各项设置，包括参考文献、索引等
\usepackage{...}
\usepackage{makeidx}
\makeindex
\bibliographystyle{...}
\begin{document}
\frontmatter
\maketitle % 标题页
\include{preface} % 前言章节 preface.tex
\tableofcontents
\mainmatter
\include{chapter1} % 第一章 chapter1.tex
\include{chapter2} % 第二章 chapter2.tex
...
\appendix
\include{appendixA} % 附录 A appendixA.tex
...
\backmatter
\include{prologue} % 后记 prologue.tex
\bibliography{...} % 利用 BibTeX 工具生成参考文献
\printindex % 利用 makeindex 工具生成索引
\end{document}
```

#### 标题页

LATEX 支持生成简单的标题页。首先需要给定标题和作者等信息：

```latex
\title{⟨title⟩}
\author{⟨author⟩}
\date{⟨date⟩}
```

其中前两个命令是必须的（不用 \title 会报错；不用 \author 会警告），\date 命令可选。LATEX 还提供了一个 \today 命令自动生成当前日期，\date 默认使用 \today。在 \title、\author 等命令内可以使用 \thanks 命令生成标题页的脚注，用 \and 隔开多个人名。

在信息给定后，就可以使用 \maketitle 命令生成一个简单的标题页了。下面的代码给出了一个标题页的示例。article 文档类的标题默认不单独成页，而 report 和 book 默认单独成页。可在 \documentclass 命令调用文档类时指定 titlepage / notitlepage 选项以修改默认的行为。

```latex
\title{Test title}
\author{ Mary\thanks{E-mail:*****@***.com}
\and Ted\thanks{Corresponding author}
\and Louis}
\date{\today}

% 在文档中
\maketitle
```

LATEX 标准类还提供了一个简单的 titlepage 环境，生成不带页眉页脚的一页。用户可以在这个环境中使用各种排版元素自由发挥，生成自定义的标题页以替代 \maketitle 命令。甚至可以利用 titlepage 环境重新定义 \maketitle：

```latex
\renewcommand{\maketitle}{\begin{titlepage}
... % 用户自定义命令
\end{titlepage}}
```

事实上，为标准文档类指定了 titlepage 选项以后，使用 \maketitle 命令生成的标题页就是一个 titlepage 环境。

以上是 LATEX 标准文档类的标题页相关命令用法。在各种文档模板中经常有自定义的标题页，有可能需要除了 \title 和 \author 以外的命令给定信息，用法也可能与标准文档类的不一致（甚至有些模板可能没有定义 titlepage 等环境）。使用文档模板前一定要仔细阅读文档模板的帮助文档。

#### 交叉引用

交叉引用是 LATEX 强大的自动排版功能的体现之一。在能够被交叉引用的地方，如章节、公式、图表、定理等位置使用 \label 命令：为了生成正确的交叉引用，一般也需要多次编译源代码。

```latex
\label{⟨label-name⟩}
```

之后可以在别处使用 \ref 或 \pageref 命令，分别生成交叉引用的编号和页码：

```latex
\ref{⟨label-name⟩}
\pageref{⟨label-name⟩}
```

\label 命令可用于记录各种类型的交叉引用，使用位置分别为：

- 章节标题 在章节标题命令 \section 等之后紧接着使用。
- 行间公式 单行公式在公式内任意位置使用；多行公式在每一行公式的任意位置使用。
- 有序列表 在 enumerate 环境的每个 \item 命令之后、下一个 \item 命令之前任意位置使用。
- 图表标题 在图表标题命令 \caption 之后紧接着使用。
- 定理环境 在定理环境内部任意位置使用。

在使用不记编号的命令形式（\section*、\caption*、带可选参数的 \item 命令等）时不要使用 \label 命令，否则生成的引用编号不正确。

#### 脚注边注

使用 \footnote 命令可以在页面底部生成一个脚注：

```latex
\footnote{⟨footnote⟩}
```

有些情况下（比如在表格环境、各种盒子内）使用 \footnote 并不能正确生成脚注。我们可以分两步进行，先使用 \footnotemark 为脚注计数，再在合适的位置用 \footnotetext 生成脚注。

```latex
\begin{tabular}{l}
\hline
“天地玄黄，宇宙洪荒。日月盈昃，辰宿列张。”\footnotemark \\
\hline
\end{tabular}
\footnotetext{表格里的名句出自《千字文》。}
```

使用 \marginpar 命令可在边栏位置生成边注：

```latex
\marginpar[⟨left-margin⟩]{⟨right-margin⟩}
\marginpar{\footnotesize 边注较窄，不要写过多文字，最好设置较小的字号。}
```

如果只给定了 ⟨right-margin⟩，那么边注在奇偶数页文字相同；如果同时给定了 ⟨left-margin⟩，则偶数页使用 ⟨left-margin⟩ 的文字。

#### 特殊环境

##### 列表

LATEX 提供了基本的有序和无序列表环境 enumerate 和 itemize，两者的用法很类似，都用 \item 标明每个列表项。enumerate 环境会自动对列表项编号。

```latex
% 有序列表
\begin{enumerate}
\item An item.
\begin{enumerate}
\item A nested item.\label{itref}
\item[*] A starred item.
\end{enumerate}
\item Reference(\ref{itref}).
\end{enumerate}

% 无序列表
\begin{itemize}
\item An item.
\begin{itemize}
\item A nested item.
\item[+] A `plus' item.
\item Another item.
\end{itemize}
\item Go back to upper level.
\end{itemize}
```

其中 \item 可带一个可选参数，将有序列表的计数或者无序列表的符号替换成自定义的符号。列表可以嵌套使用，最多嵌套四层。

关键字环境 description 的用法与以上两者类似，不同的是 \item 后的可选参数用来写关键字，以粗体显示，一般是必填的：

```latex
\begin{description}
\item[Enumerate] Numbered list.
\item[Itemize] Non-numbered list.
\end{description}
```

各级无序列表的符号由命令 \labelitemi 到 \labelitemiv 定义，可以简单地重新定义它们；而有序列表的符号由命令 \labelenumi 到 \labelenumiv 定义，重新定义这些命令还需要用到计数器相关命令：

```latex
% 无序列表
\renewcommand{\labelitemi}{\ddag}
\renewcommand{\labelitemii}{\dag}
% 有序列表
\renewcommand{\labelenumi}{\Alph{enumi}>}
```

##### 对齐环境

center、flushleft 和 flushright 环境分别用于生成居中、左对齐和右对齐的文本环境。

```latex
\begin{center} … \end{center}
\begin{flushleft} … \end{flushleft}
\begin{flushright} … \end{flushright}
```

除此之外，还可以用以下命令直接改变文字的对齐方式：\centering \raggedright \raggedleft；

```latex
\centering
Centered text paragraph.
\raggedright
Left-aligned text paragraph.
\raggedleft
Right-aligned text paragraph.
```

三个命令和对应的环境经常被误用，有直接用所谓 \flushleft 命令或者 raggedright 环境的，都是不甚严格的用法（即使它们可能有效）。有一点可以将两者区分开来：center 等环境会在上下文产生一个额外间距，而 \centering 等命令不产生，只是改变对齐方式。

##### 引用环境

LATEX 提供了两种引用的环境：quote 用于引用较短的文字，首行不缩进；quotation 用于引用若干段文字，首行缩进。引用环境较一般文字有额外的左右缩进。verse 用于排版诗歌，与 quotation 恰好相反，verse 是首行悬挂缩进的。

```latex
% quote
Francis Bacon says:
\begin{quote}
Knowledge is power.
\end{quote}

% quotation
《木兰诗》：
\begin{quotation}
万里赴戎机，关山度若飞。
朔气传金柝，寒光照铁衣。
将军百战死，壮士十年归。
归来见天子，天子坐明堂。
策勋十二转，赏赐百千强。……
\end{quotation}

% verse
Rabindranath Tagore's short poem:
\begin{verse}
Beauty is truth's smile
when she beholds her own face in
a perfect mirror.
\end{verse}
```

##### 摘要环境

摘要环境 abstract 默认只在标准文档类中的 article 和 report 文档类可用，一般用于紧跟\maketitle 命令之后介绍文档的摘要。如果文档类指定了 titlepage 选项，则单独成页；反之，单栏排版时相当于一个居中的小标题加一个 quotation 环境，双栏排版时相当于 \section* 定义的一节。

##### 代码环境

有时我们需要将一段代码原样转义输出，这就要用到代码环境 verbatim，它以等宽字体排版代码，回车和空格也分别起到换行和空位的作用；带星号的版本更进一步将空格显示成 ␣。

```latex
\begin{verbatim}
#include <iostream>
int main()
{
std::cout << "Hello, world!"
<< std::endl;
return 0;
}
\end{verbatim}
```

要排版简短的代码或关键字，可使用 \verb 命令：⟨delim⟩ 标明代码的分界位置，前后必须一致，除字母、空格或星号外，可任意选择使得不与代码本身冲突，习惯上使用 | 符号。同 verbatim 环境，\verb 后也可以带一个星号，以显示空格。\verb 命令对符号的处理比较复杂，一般不能用在其它命令的参数里，否则多半会出错。

```latex
\verb⟨delim⟩⟨code⟩⟨delim⟩
\verb|\LaTeX|
\verb+(a || b)+ \verb*+(a || b)+
```

verbatim 宏包优化了 verbatim 环境的内部命令，并提供了 \verbatiminput 命令用来直接读入文件生成代码环境。fancyvrb 宏包提供了可定制格式的 Verbatim 环境；listings 宏包更进一步，可生成关键字高亮的代码环境，支持各种程序设计语言的语法和关键字。详情请参考各自的帮助文档。

#### 表格

排版表格最基本的 tabular 环境用法为：

```latex
\begin{tabular}{⟨column-spec⟩}
⟨item1⟩ & ⟨item2⟩ & … \\
\hline
⟨item1⟩ & ⟨item2⟩ & … \\
\end{tabular}
```

直接使用 tabular 环境的话，会和周围的文字混排。tabular 环境可带一个可选参数控制垂直对齐（默认是垂直居中）；但是通常情况下，tabular 环境一般会放置在 table 浮动体环境中，并用 \caption 命令加标题。

##### 列格式

tabular 环境使用 ⟨column-spec⟩ 参数指定表格的列数以及每列的格式。且表格中每行的单元格数目不能多于列格式里 l/c/r/p 的总数（可以少于这个总数），否则出错。

| 列格式      | 说明                                 |
| :---------- | :----------------------------------- |
| l/c/r       | 单元格内容左对齐/居中/右对齐，不折行 |
| p{⟨width⟩}  | 单元格宽度固定为 ⟨width⟩，可自动折行 |
| \|          | 绘制竖线                             |
| @{⟨string⟩} | 自定义内容 ⟨string⟩                  |

```latex
\begin{tabular}{|l|c|r|}
    \hline
    left & center & right \\
    \hline
\end{tabular}
```

@ 格式可在单元格前后插入任意的文本，但同时它也消除了单元格前后额外添加的间距。@格式可以适当使用以充当“竖线”。特别地，@{} 可直接用来消除单元格前后的间距：

```latex
\begin{tabular}{@{} r@{:}lr @{}}
\hline
1 & 1 & one \\
11 & 3 & eleven \\
\hline
\end{tabular}
```

另外 LATEX 还提供了简便的将格式参数重复的写法 *{⟨n⟩{⟨column-spec⟩}，比如以下两种写法是等效的：

```latex
\begin{tabular}{|c|c|c|c|c|p{4em}|p{4em}|}
\begin{tabular}{|*{5}{c|}*{2}{p{4em}|}}
```

有时需要为整列修饰格式，比如整列改变为粗体，如果每个单元格都加上 \bfseries 命令会比较麻烦。array 宏包提供了辅助格式 > 和 <，用于给列格式前后加上修饰命令：

```latex
% \usepackage{array}
\begin{tabular}{>{\itshape}r<{*}l}
\hline
italic & normal \\
column & column \\
\hline
\end{tabular}
```

array 宏包还提供了类似 p 格式的 m 格式和 b 格式，三者分别在垂直方向上靠顶端对齐、居中以及底端对齐。

```latex
\newcommand\txt
{a b c d e f g h i}
\begin{tabular}{cp{2em}m{2em}b{2em}}
\hline
pos & \txt & \txt & \txt \\
\hline
\end{tabular}
```

##### 列宽

LATEX 本身提供了 tabular* 环境用来排版定宽表格，但是不太方便使用，比如要用到 @ 格式插入额外命令，令单元格之间的间距为 \fill，但即使这样仍然有瑕疵：

```latex
\begin{tabular*}{14em}%
{@{\extracolsep{\fill}}|c|c|c|c|}
\hline
A & B & C & D \\ \hline
a & b & c & d \\ \hline
\end{tabular*}
```

tabularx 宏包为我们提供了方便的解决方案。它引入了一个 X 列格式，类似 p 列格式，不过会根据表格宽度自动计算列宽，多个 X 列格式平均分配列宽。X 列格式也可以用 array 里的辅助格式修饰对齐方式：

```latex
\begin{tabularx}{14em}%
{|*{4}{>{\centering\arraybackslash}X|}}
\hline
A & B & C & D \\ \hline
a & b & c & d \\ \hline
\end{tabularx}
```

##### 横线

通过 \hline 命令绘制表格线，而 \cline{⟨i⟩-⟨j⟩} 命令用来绘制跨越部分单元格的横线：

```latex
\begin{tabular}{|c|c|c|}
\hline
1 & 2 & 3 \\ \cline{1-2}
4 & 5 & 6 \\ \cline{3-3}
7 & 8 & 9 \\ \hline
\end{tabular}
```
在科技论文排版中广泛应用的表格形式是三线表，形式干净简明。三线表由 booktabs 宏包支持，它提供了 \toprule、\midrule 和 \bottomrule 命令用以排版三线表的三条线，以及和 \cline 对应的 \cmidrule。除此之外，最好不要用其它横线以及竖线：

```latex
\begin{tabular}{cccc}
\toprule
& \multicolumn{3}{c}{Numbers} \\
\cmidrule{2-4}
& 1 & 2 & 3 \\
\midrule
Alphabet & A & B & C \\
Roman & I & II& III \\
\bottomrule
\end{tabular}
```

##### 合并单元格

LATEX 通过 \multicolumn 合并横向单元格；

```latex
\multicolumn{⟨n⟩}{⟨column-spec⟩}{⟨item⟩}
```

其中 ⟨n⟩ 为要合并的列数，⟨column-spec⟩ 为合并单元格后的列格式，只允许出现一个 l/c/r 或 p 格式。如果合并前的单元格前后带表格线 |，合并后的列格式也要带 | 以使得表格的竖线一致。

```latex
\begin{tabular}{|c|c|c|}
\hline
1 & 2 & Center \\ \hline
\multicolumn{2}{|c|}{3} &
\multicolumn{1}{r|}{Right} \\ \hline
4 & \multicolumn{2}{c|}{C} \\ \hline
\end{tabular}
```

此外，形如 \multicolumn{1}{⟨column-spec⟩}{⟨item⟩} 的命令可以用来修改某一个单元格的列格式；

纵向合并单元格需要用到 multirow 宏包提供的 \multirow 命令：

```latex
\multirow{⟨n⟩}{⟨width⟩}{⟨item⟩}
```

⟨width⟩ 为合并后单元格的宽度，可以填 * 以使用自然宽度。

```latex
% \usepackage{multirow}

\begin{tabular}{ccc}
\hline
\multirow{2}{*}{Item} &
\multicolumn{2}{c}{Value} \\
\cline{2-3}
& First & Second \\ \hline
A & 1 & 2 \\ \hline
\end{tabular}
```

##### 嵌套表格

相对于合并单元格，拆分单元格对于 LATEX 来说并非易事。在单元格中嵌套一个小表格可以起到“拆分单元格”的效果。在以下的例子中，注意要用 \multicolumn 命令配合 @{} 格式把单元格的额外边距去掉，使得嵌套的表格线能和外层的表格线正确相连；

如果不需要为“拆分的单元格”画线，并且只在垂直方向“拆分”的话，makecell 宏包提供的 \makecell 命令是一个简单的解决方案：

```latex
\begin{tabular}{|c|c|}
\hline
a & \makecell{d1 \\ d2} \\
\hline
b & c \\
\hline
\end{tabular}
```

##### 控制行距

LATEX 生成的表格看起来通常比较紧凑。修改参数 \arraystretch 可以得到行距更加宽松的表格，但后续的行距都会修改；

```latex
\renewcommand\arraystretch{1.8}
\begin{tabular}{|c|}
\hline
Really loose \\ \hline
tabular rows.\\ \hline
\end{tabular}
```

另一种增加间距的办法是给换行命令 \\ 添加可选参数，在这一行下面加额外的间距，适合用于在行间不加横线的表格；

#### 图片

LATEX 本身不支持插图功能，需要由 graphicx 宏包辅助支持，然后通过 \includegraphics 命令加载图片：

```latex
\includegraphics[⟨options⟩]{⟨filename⟩}
```

其中 ⟨filename⟩ 为图片文件名，与使用 \include 命令类似，文件名有时需要使用相对路径或绝对路径。图片文件的扩展名可写可不写。

另外 graphicx 宏包还提供了 \graphicspath 命令，用于声明一个或多个图片文件存放的目录，使用这些目录里的图片时可不用写路径：

```latex
% 假设主要的图片放在 figures 子目录下，标志放在 logo 子目录下
\graphicspath{{figures/}{logo/}}
```

\includegraphics 命令的可选参数 ⟨options⟩ 支持 ⟨key⟩=⟨value⟩ 形式赋值，常用的参数如下：

| 参数            | 含义                              |
| :-------------- | :-------------------------------- |
| width=⟨width⟩   | 将图片缩放到宽度为 ⟨width⟩        |
| height=⟨height⟩ | 将图片缩放到高度为 ⟨height⟩       |
| scale=⟨scale⟩   | 将图片相对于原尺寸缩放 ⟨scale⟩ 倍 |
| angle=⟨angle⟩   | 令图片逆时针旋转 ⟨angle⟩ 度       |



### 附录

- LATEX 的三个标准文档类可指定的选项包括：

| 参数                                         | 说明                                                                                                                                    |
| :------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------- |
| 10pt, 11pt, 12pt                             | 指定文档的基本字号。默认为 10pt。                                                                                                       |
| a5paper, b5paper, executivepaper, legalpaper | 指定纸张大小，默认为美式信纸 letterpaper （8.5 × 11 英寸）。                                                                            |
| twoside, oneside                             | 指定单面/双面排版。双面排版时，奇偶页的页眉页脚、页边距不同。article 和 report 默认为 oneside，book 默认为 twoside。                    |
| onecolumn, twocolumn                         | 指定单栏/双栏排版。默认为 onecolumn。                                                                                                   |
| openright, openany                           | 指定新的一章 \chapter 是在奇数页（右侧）开始，还是直接紧跟着上一页开始。report 默认为 openany，book 默认为 openright。对 article 无效。 |
| landscape                                    | 指定横向排版。默认为纵向。                                                                                                              |
| titlepage, notitlepage                       | 指定标题命令 \maketitle 是否生成单独的标题页。article 默认为 notitlepage，report 和 book 默认为 titlepage。                             |
| fleqn                                        | 令行间公式左对齐。默认为居中对齐。                                                                                                      |
| leqno                                        | 将公式编号放在左边。默认为右边。                                                                                                        |
| draft, final                                 | 指定草稿/终稿模式。草稿模式下，断行不良的地方会在行尾添加一个黑色方块。默认为 final。                                                   |

### 参考文献

- [LaTeX 官网](https://www.latex-project.org/)
- [LaTeX 英文文档](https://www.overleaf.com/learn)
- [TeX 进阶 CTAN](https://ctan.org/)
- [LaTeX零基础入门教程](ht;tps://www.jianshu.com/p/3e842d67ada2)
- [一份（不太）简短的 LaTeX 2ε 介绍.pdf](http://www.080910t.com/wp-content/uploads/2019/05/LATEX-%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B.pdf)
- [overleaf 在线编辑工具](https://www.overleaf.com/)
- [ShareLaTeX 在线编辑工具](https://www.sharelatex.com/)
- [LaTeX 表格在线生成工具](https://www.tablesgenerator.com/)