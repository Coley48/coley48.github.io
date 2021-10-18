---
title: LaTeX环境搭建和配置教程
tags: [LaTeX, 文字排版]
date: 2021-10-10 08:34:46
categories: blog
---

### 关于 LaTeX

> LaTeX is not a stand-alone typesetting program in itself, but document preparation software that runs on top of Donald E. Knuth's TeX typesetting system. 

LaTeX 不是一个独立运行的排版程序，LaTeX 运行在高德纳 (Donald E. Knuth) 开发的、以排版文字和数学公式为目的的排版软件上。LaTeX 使用 TEX 作为它的排版引擎，可以粗略的把 LaTeX 理解为对 TeX 的一层封装。

本文主要记录了 LaTeX 环境搭建以及配置的过程。

<!-- more -->

### 环境搭建

LaTeX 还有很多编辑器，如 TeXworks、TexStudio、WinEdt、MiKtex、protex等等。因为 TeX Live 同 VS Code 一样是开源免费的，且跨平台的工具；且维护更新都比较快；因此我选用 TeX Live 和 VS Code 搭建 LaTeX 的编译环境。

#### 安装 tex Live

这里只记录一下大致的安装步骤，详细带图的安装步骤可以参考[最新TeXLive 环境的安装与配置](https://zhuanlan.zhihu.com/p/41855480)；

1. 从[TeX Live 官网](https://www.tug.org/texlive/)或者[清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)下载 TeX Lsive 文件；
2. 打开下载好的 iso 文件，找到文件夹下的`install-tl-windows.bat`，右键以管理员身份运行，就会看到安装的界面；
3. 在安装界面中，修改一下安装的目录，去掉安装texwork前端界面选项，然后在高级选项中只保留中文，之后点击安装；
4. 安装的时间稍微有点长，最后看到欢迎字样时，表示安装完成，可以直接关闭；
5. 在命令行中输入`xelatex -v`命令测试，会有很多版本信息出现，表示安装成功；

{% asset_img TeX-Live安装完成.png %}

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

{% asset_img hello-latex.png %}

在 VS Code 中可以使用快捷键：
`Ctrl`+`Alt`+`b`：编译输出当前编辑的 tex 文件；
`Ctrl`+`Alt`+`v`：预览当前编辑的 tex 文件输出的 pdf 文件；

主要的编译器：

- latex 虽然名为 latex 命令，底层调用的引擎其实是 pdfTEX。该命令生成 dvi（Device Indexpendent）格式的文档，用 dvipdfmx 命令可以将其转为 pdf。
- pdflatex 底层调用的引擎也是 pdfTEX，可以直接生成 pdf 格式的文档。
- xelatex 底层调用的引擎是 XƎTEX，支持 UTF-8 编码和 TrueType / OpenType 字体。当前较为方便的中文排版解决方案基于 xelatex。
- lualatex 底层调用的引擎是 LuaTEX，这个引擎在 pdfTEX 引擎基础上发展而来，除了支持 UTF-8 编码和 TrueType / OpenType 字体外，还支持通过 Lua 语言扩展 TEX 的功能。lualatex 编译命令下的中文排版支持需要借助 luatex-ja 宏包。

### 参考文献

- [LaTeX 官网](https://www.latex-project.org/)
- [TeX Live 官网](https://www.tug.org/texlive/)
- [TeX Live 清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)
- [最新TeXLive 环境的安装与配置](https://zhuanlan.zhihu.com/p/41855480)
- [Visual Studio Code 配置LaTeX](https://zhuanlan.zhihu.com/p/166523064)
- [overleaf 在线编辑工具](https://www.overleaf.com/)
- [ShareLaTeX 在线编辑工具](https://www.sharelatex.com/)
- [LaTeX 表格在线生成工具](https://www.tablesgenerator.com/)