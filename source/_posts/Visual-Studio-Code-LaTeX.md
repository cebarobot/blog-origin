---
title: "Visual Studio Code 折腾记：LaTeX 集成编辑环境"
date: 2018-11-2 18:00:00
categories: "开发者手册"
tags: ["Visual Studio Code", "LaTeX"]
thumbnail: https://i.loli.net/2018/11/03/5bdc7e775468e.png
---

Visual Studio Code 是微软推出的一款开源编辑器，一般简称为 VSCode。相较于收费的 Sublime Text、奇慢无比的 Github Atom，VSCode 在我看来是 Windows 平台上最合我胃口的编辑器了。虽然扩展插件没有那么丰富，对我来说也是完全够用了。试过 VSCode 之后，果断把主力编辑器换成了 VSCode。接下来大概会有一系列文章讲讲我折腾 VSCode 的事情。

LaTeX 是现在最流行的科技写作工具，是最棒的文档排版系统之一。LaTeX 有很多的 IDE，但主流的 TeXworks 太过于简单了、TeXStudio 在 Windows 下的显示有非常糟糕。把 VSCode 配置成 LaTeX 的 IDE 用起来就非常舒服了。

> 本文中使用的软件版本为：
> - TeX Live 2018-20180414
> - Visual Studio Code 1.28.2
> - LaTeX Workshop 5.13.0
>
> 主版本号不同的 LaTeX Workshop 的配置文件可能会有较大差异，早先（2017 年及以前）的配置文章很可能已经失效。


## 软件安装
* LaTeX 发行版：TeX Live、MikTeX 或者 CTeX
* Visual Studio Code
* LaTeX Workshop

### LaTeX 发行版的安装
LaTeX 国内用户主要使用的发行版基本上集中在 TeX Live 和 CTeX 上，任选一个安装即可。

#### TeX Live 的安装
Tex Live 可以从它的官网上下载：[Tex Live - Tex Users Group](http://tug.org/texlive/)（这官网真的简陋）。

由于国内的网络环境，网络安装会非常缓慢，建议选择 `other methods` 中的 `Downloading one huge ISO file`。之后可以手动从景象列表里面选择一个比较近的景象下载即可。其中 `texlive.iso` 是最新版本。

这里给出清华开源镜像站的地址：[texlive.iso](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/texlive.iso)

Windows 10 下可以双击直接挂载 ISO 镜像，之后运行其中的 `install-tl-windows.bat` 就可以启动安装程序，TeXWorks 可以不用安装。

之后需要将 `C:\texlive\2018\bin` 添加到 PATH 变量中。

#### CTeX 的安装
CTeX 同样从它的官网上下载：[Welcome to Chinese TeX:CTEX](http://www.ctex.org/HomePage)。

这里给出清华开源镜像站的地址：[CTeX_2.9.2.164_Full.exe](https://mirrors.tuna.tsinghua.edu.cn/ctex/legacy/2.9/CTeX_2.9.2.164_Full.exe)

直接双击运行安装就好，可以不需要安装 LaTeX 的 IDE，注意选择要将 CTeX 添加到 PATH 变量。

### Visual Studio Code 与 LaTeX Workshop 的安装
Visual Studio Code 的官网是：[Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)。

安装 Visual Studio Code 时根据需要选择一些选项：
* 将 Code 添加到右键菜单
* 将 Code 注册为可用编辑器
* 将 Code 添加到 PATH

安装完成后，点击左侧扩展按钮（KEY: `Ctrl+Shift+X`），搜索 `LaTeX Workshop`，选择安装、重新加载即可。

## 编译模式配置

### 使用可变 TeX 引擎 / TeX 魔术命令
编写 LaTeX 文件时有两个魔术命令（Magic comments）：`% !TEX root` 和 `% !TEX program`，前者指定 LaTeX 根文件，后者制定编译方式。新版本的 LaTeX Workshop，默认支持魔术命令，无需配置。高度建议使用魔术命令来制定编译方式。

```LaTeX
% !TEX program = xelatex
% !TEX root = relative/or/absolute/path/to/root/file.tex
```

### 添加编译工具
LaTeX Workshop 默认包含了几种编译工具，对中文用户来说一般需要把 XeLaTeX 添加进去。

点击左下角设置图标，选择设置，打开`settings.json`，在右侧添加下面的代码：
```Json
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
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
                "%DOC%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
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
```

### 添加编译方案
相应的，我们还需要添加一些编译方案。

点击左下角设置图标，选择设置，打开`settings.json`，在右侧添加下面的代码：
```Json
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
            "name": "latexmk",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "BibTeX",
            "tools": [
                "bibtex"
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
        {
            "name": "xelatex -> bibtex -> xelatex*2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        }
    ],
```

### 使用项目任务文件（tasks.json）
这个我暂时没有尝试，请参考：[Visual Studio Code 搭建 LaTeX 编写环境 | Ethlisan](http://ddswhu.com/visual-studio-code-latex/)

## 其他的一些配置
其他有一些细小的配置需要调整：
```Json
"latex-workshop.view.pdf.hand": true, // 预览 PDF 文件时默认使用手型工具
"latex-workshop.latex.autoBuild.onSave.enabled": false, // 关闭保存时自动编译
"latex-workshop.synctex.afterBuild.enabled": true, // 启用反向搜索 （在 PDF 预览器中按下 Ctrl + ←，同时鼠标点击要反向搜索的位置）
```

## 愉快地使用
配置（调教）Visual Studio Code 和 LaTeX Workshop 基本告一段落，现在就让我们愉快地玩耍吧！

### 侧边栏
LaTeX Workshop 提供了侧边栏组件。打开 LaTeX 文件后，可以点击左边的 TeX 按钮，就可以打开侧边栏。

侧边栏提供了 LaTeX Workshop 提供的大部分命令，包括所有的编译方案等等。几乎所有的 IDE 功能都集中在了这里。

侧边栏同时也提供了 LaTeX 文件的结构树，这一下基本上和一般的 LaTeX IDE 无差了。

### 命令面板与快捷键
Visual Studio Code 提供的命令面板可通过在任意地方单机右键、选择命令面板打开（KEY: `Ctrl + Shift + P`），在命令面板中也可使用 LaTeX Workshop 提供的大部分命令。在命令面板中输入 `LaTeX Workshop` 就可以看到所有的命令，在命令旁边就可以看到快捷键。

LaTeX Workshop 的常用快捷键：
* `Ctrl + Alt + B`：编译
* `Ctrl + Alt + C`：清除 auxiliary 文件
* `Ctrl + Alt + J`：定位跳转到光标所在位置对应的 PDF 文件位置
* `Ctrl + Alt + V`：打开 PDF 预览

有的人可能觉得快捷键不好用，可以自己调整。

### 正向和反向定位跳转
LaTeX Workshop 提供了正向和反向定位跳转功能：
* 在 LaTeX 文件中，按 `Ctrl + Alt + J` 跳转到对应的 PDF 文件位置。
* 在 PDF 文件中，按下 `Ctrl + ←` 同时鼠标单机，跳转到对应的 LaTeX 文件位置。

反向跳转比较奇葩，经常跳转到 LaTeX 文件后会会激活 `Ctrl + ←` 的另一项功能——跳转到上一单词……

## 参考资料
- James-Yu/LaTeX-Workshop: README [https://github.com/James-Yu/LaTeX-Workshop](https://github.com/James-Yu/LaTeX-Workshop)
- Visual Studio Code 搭建 LaTeX 编写环境 | Ethlisan [http://ddswhu.com/visual-studio-code-latex/](http://ddswhu.com/visual-studio-code-latex/)
- 如何配置 Visual Studio Code 作为 LaTeX 编辑器 [http://www.latexstudio.net/archives/12260.html](http://www.latexstudio.net/archives/12260.html)
- Visual Studio Code 搭建 LaTeX 编写环境 [http://ddswhu.com/visual-studio-code-latex/](http://ddswhu.com/visual-studio-code-latex/)
- 使用VSCode编写LaTeX - 知乎 [https://zhuanlan.zhihu.com/p/38178015](https://zhuanlan.zhihu.com/p/38178015)