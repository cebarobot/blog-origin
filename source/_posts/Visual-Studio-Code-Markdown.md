---
title: "Visual Studio Code 折腾记：Markdown 集成编辑环境"
date: 2020-01-21 12:19:53
categories: "开发者手册"
tags: ["Visual Studio Code", "Markdown"]
thumbnail: https://i.loli.net/2020/01/21/vyfNtw7LkCMub9Y.png
# deletelink: https://sm.ms/delete/fGDyzhkxXM5YqWZ1galeLiIBmS
---

嘛……之前已经写过一篇用 Visual Studio Code 编辑 LaTeX 的文章，这回来把拖了很久的编辑 Markdown 的文章补一补。

Markdown 是个好东西，作为一种轻量级的标记语言，很适合用来写一些简单的文章，并输出成 HTML 或者 PDF 格式。在不少情况下，我们不需要 LaTeX 这样重量级的排版工具，用 Markdown 就能完成很漂亮的文章。

## Markdown 语法
我觉着 Markdown 语法没啥写的必要……随便搜一下都能搜到很多的介绍，这里随便列几个写的不错的：
* [Markdown 教程-  菜鸟教程](https://www.runoob.com/markdown/md-tutorial.html)
* [Markdown 语法说明](http://shouce.jb51.net/markdown/)

<!--，但这里还是简单说一下吧……Markdown 的语法多种多样，也不是所有的编辑器都支持这么多的语法，这里选择的只是一部分我通常使用的 Markdown 语法。

> 
### 标题
用对应数量的 `#` 表示一至六级标题。
```Markdown
# 这是一级标题
## 这是二级标题
### 这是三级标题
#### 这是四级标题
##### 这是五级标题
###### 这是六级标题
```
> 一般很少使用五级和六级标题。

### 段落
在一行文本的后面添加 2 个空格来换行，在一段文本的后面添加 1 个空行来分段。
```Markdown
这是第一段的第一行  
这是第一段的第二行

这是第二段
```

在新的一个段落，用 3 个以上的减号 `-` 添加一条分隔线。
```Markdown
分隔线前

---
分隔线后
```

### 强调
在文字两端添加 1~3 个 `*`，分别表示斜体、粗体、斜粗体三种强调。在文字两端添加 2 个 `~`，表示添加删除线。
```Markdown
*斜体*
**粗体**
***斜粗体***
~~删除线~~
```

> 由于字体的限制，斜粗体很少使用。

### 列表
在文字前添加 `*`、`-` 或 `+` 和一个空格来生成无序列表。在文字前添加数字、`.` 和一个空格来生成有序列表。可以通过 4 个空格缩进来形成嵌套。

```Markdown
* 列表第 1 项
* 列表第 2 项
* 列表第 3 项

1. 列表第 1 项
2. 列表第 2 项
8. 列表前的数字编号并不重要
    * 嵌套列表
    * 嵌套列表
3. 列表第 4 项
```

### -->

## 软件安装
* [Visual Studio Code](https://code.visualstudio.com/)
* [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)

VS Code 的安装没啥需要说的，Markdown All in One 这个插件在应用商店中可以直接安装。和这个插件的名字一样，这个插件几乎把编写 Markdown 需要的所用功能都囊括进去了。

## 一些玩耍技巧
### 用 VS Code 编辑
嘛……Markdown All in One 这个插件提供了不少快捷键和代码补全的功能，请仔细阅读 README。

这个插件提供了对数学公式的支持，但需要注意，使用行内公式时，`$` 与公式内容间不能有空格。

编辑器的右上角有预览按钮，点开之后可以实时显示渲染结果。

### 生成 PDF 文件
以纸质印刷品为目标的文档，最后都得转化为 PDF 格式。从 Markdown 文件生成 PDF 文件一般都要先生成 HTML 文件，再通过浏览器打印成 PDF 文件。下面是具体步骤

1. 打开命令面板（`Ctrl` + `Shift` + `P` 或 `查看/命令面板`），输入 `HTML` 找到 `Markdown All in One: Print current document to HTML` 一项，回车执行。
2. 在 Markdown 文件所在文件夹下会生成对应的 HTML 文件，用 Chrome 浏览器打开它，打开打印窗口（`Ctrl` + `P`），选择目标打印机为 `另存为 PDF`，点击保存。

一般来说，插件给出的 CSS 样式已经足够好了，直接另存为 PDF 就可以。当然，你也可以根据自己的需要调整 CSS 样式。

似乎 Chrome 在导出 PDF 的功能上比 Firefox 要好用一点，唯一不方便的是页脚不能自定义。

如果需要在文件的某个位置分页，可以在 Markdown 文件的对应位置添加下面的 HTML 代码：
```HTML
<div style="page-break-after: always;"></div>
```

其他的玩耍技巧还请大家边玩耍边摸索。（总算水了一篇博客，逃）