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

> LaTeX is not a stand-alone typesetting program in itself, but document preparation software that runs on top of Donald E. Knuth's TeX typesetting system. 

LaTeX 是一个文档准备系统 (Document Preparing System)，它非常适用于生成高印刷质量的科技类和数学类文档。它也能够生成所有其他种类的文档，小到简单的信件，大到完整的书籍。LaTeX 使用 TEX 作为它的排版引擎。TeX 是高德纳 (Donald E. Knuth) 开发的、以排版文字和数学公式为目的的软件。

<!-- more -->

### 环境搭建

LaTeX 还有很多编辑器，如 TeXworks、TexStudio、WinEdt、MiKtex、protex等等。因为 TeX Live 同 VS Code 一样是开源免费的，且跨平台的工具；且维护更新都比较快；因此我选用 TeX Live 和 VS Code 搭建 LaTeX 的编译环境。

#### 安装 tex Live

这里只记录一下大致的安装步骤，详细带图的安装步骤可以参考[最新TeXLive 环境的安装与配置](https://zhuanlan.zhihu.com/p/41855480)；

1. 从[TeX Live 官网](https://www.tug.org/texlive/)或者[清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)下载 TeX Lsive 文件；
2. 打开下载好的 iso 文件，找到文件夹下的`install-tl-windows.bat`，右键以管理员身份运行，就会看到安装的界面；
3. 在安装界面中，修改一下安装的目录，去掉安装texwork前端界面选项，然后在高级选项中只保留中文，之后点击安装；
4. 安装的时间稍微有点长，大约需要一个半小时，最后看到欢迎字样时，表示安装完成，可以直接关闭；
5. 在命令行中输入`xelatex -v`命令测试，会有很多版本信息出现，表示安装成功；

#### 配置 VS Code

这里省略 VS Code 安装步骤，详细安装配置以及配置项说明可以参考[Visual Studio Code 配置LaTeX](https://zhuanlan.zhihu.com/p/166523064)；

1. 安装插件 LaTeX Workshop；
2. 在`setting.json`设置中添加一下配置；

```json
{
    "latex-workshop.latex.autoBuild.run": "never",
    "latex-workshop.showContextMenu": true,
    "latex-workshop.intellisense.package.enabled": true,
    "latex-workshop.message.error.show": false,
    "latex-workshop.message.warning.show": false,
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "XeLaTeX",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "PDFLaTeX",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "BibTeX",
            "tools": [
                "bibtex"
            ]
        },
        {
            "name": "LaTeXmk",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "xelatex -> bibtex -> xelatex*2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdflatex -> bibtex -> pdflatex*2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        },
    ],
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk"
    ],
    "latex-workshop.latex.autoClean.run": "onFailed",
    "latex-workshop.latex.recipe.default": "lastUsed",
    "latex-workshop.view.pdf.internal.synctex.keybinding": "double-click"
}
```

#### 使用方式

1. 新建 tex 文件；
2. 切换到右侧的 tex 栏；
3. 选择构建方式`xelatex -> bibtex -> xelatex*2`；
4. 编译后文件夹下会生成 pdf 文件和很多其他文件；

```latex
\documentclass{article}

\begin{document}
Hello, \LaTeX!
\end{document}
```

在 VS Code 中可以使用快捷键：
`Ctrl`+`Alt`+`b`：编译输出当前编辑的 tex 文件；
`Ctrl`+`Alt`+`v`：预览当前编辑的 tex 文件输出的 pdf 文件；

主要的编译器：

- latex 虽然名为 latex 命令，底层调用的引擎其实是 pdfTEX。该命令生成 dvi（Device Independent）格式的文档，用 dvipdfmx 命令可以将其转为 pdf。
- pdflatex 底层调用的引擎也是 pdfTEX，可以直接生成 pdf 格式的文档。
- xelatex 底层调用的引擎是 XƎTEX，支持 UTF-8 编码和 TrueType / OpenType 字体。当前较为方便的中文排版解决方案基于 xelatex。
- lualatex 底层调用的引擎是 LuaTEX，这个引擎在 pdfTEX 引擎基础上发展而来，除了支持 UTF-8 编码和 TrueType / OpenType 字体外，还支持通过 Lua 语言扩展 TEX 的功能。lualatex 编译命令下的中文排版支持需要借助 luatex-ja 宏包。

### LaTeX 教程

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

### 附录

- LATEX 的三个标准文档类可指定的选项包括：

|参数|说明|
|:-----|:-----|
| 10pt, 11pt, 12pt | 指定文档的基本字号。默认为 10pt。|
| a5paper, b5paper, executivepaper, legalpaper | 指定纸张大小，默认为美式信纸 letterpaper （8.5 × 11 英寸）。|
| twoside, oneside | 指定单面/双面排版。双面排版时，奇偶页的页眉页脚、页边距不同。article 和 report 默认为 oneside，book 默认为 twoside。 |
| onecolumn, twocolumn | 指定单栏/双栏排版。默认为 onecolumn。 |
| openright, openany | 指定新的一章 \chapter 是在奇数页（右侧）开始，还是直接紧跟着上一页开始。report 默认为 openany，book 默认为 openright。对 article 无效。 |
| landscape | 指定横向排版。默认为纵向。 |
| titlepage, notitlepage | 指定标题命令 \maketitle 是否生成单独的标题页。article 默认为 notitlepage，report 和 book 默认为 titlepage。 |
| fleqn | 令行间公式左对齐。默认为居中对齐。 |
| leqno | 将公式编号放在左边。默认为右边。 |
| draft, final | 指定草稿/终稿模式。草稿模式下，断行不良的地方会在行尾添加一个黑色方块。默认为 final。 |

### 参考文献

- [LaTeX 官网](https://www.latex-project.org/)
- [LaTeX 英文文档](https://www.overleaf.com/learn)
- [TeX Live 官网](https://www.tug.org/texlive/)
- [TeX Live 清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)
- [TeX 进阶 CTAN](https://ctan.org/)
- [最新TeXLive 环境的安装与配置](https://zhuanlan.zhihu.com/p/41855480)
- [Visual Studio Code 配置LaTeX](https://zhuanlan.zhihu.com/p/166523064)
- [LaTeX零基础入门教程](https://www.jianshu.com/p/3e842d67ada2)
- [overleaf 在线编辑工具](https://www.overleaf.com/)
- [ShareLaTeX 在线编辑工具](https://www.sharelatex.com/)
- [LaTeX 表格在线生成工具](https://www.tablesgenerator.com/)