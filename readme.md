# LaTeX 实验报告模板使用说明

本模板基于 `ctexart` 文档类，专为中文实验报告设计，集成了类 GitHub 风格的引用块、提示框、代码高亮以及规范的参考文献管理。以下将介绍模板的主要功能及使用方法。

## 1. 文件结构

- `template.tex` — 主文档，包含所有排版设置和内容示例。
- `template_reference.bib` — BibTeX 参考文献数据库，用于管理引用条目。

## 2. 修改报告基本信息

在 `template.tex` 中找到以下命令区域，按需修改：

```latex
\newlength{\namegap} % 定义一个namegap的变量用来调节name之间的宽度，可以自由地修改这里来保证美观
\setlength{\namegap}{4.4em} 
\newcommand{\name}{ 姓名1 \namegap 姓名2 \namegap ... }
\newlength{\numgap} % 定义一个numgap的变量用来调节学号之间的宽度
\setlength{\numgap}{0.9em} 
\newcommand{\studentNum}{ \scriptsize \textbf{学号1} \numgap \textbf{学号2} ... }
\newcommand{\expNum}{ICS-groupwork1}
\newcommand{\expName}{探究人工智能技术对网络空间安全的影响}
\newcommand{\finishdate}{2026年12月12日}
```
- `\name`：小组成员姓名，用 `\namegap` 控制间距 ，可以直接修改其中 `\setlength{\namegap}{4.4em}` 后面的数字来修改长度 。
- `\studentNum`：对应学号，用 `\numgap` 控制间距 ，可以直接修改其中 `\setlength{\numgap}{0.9em}` 后面的数字来修改长度。
- `\expNum`：实验序号或编号。
- `\expName`：实验专题名称。
- `\finishdate`：最后修改日期。
- 每个成员还可通过 `\footnotetext[编号]{...}` 补充个人贡献说明（模板中已给出示例）。

## 3.编译方法
由于模板使用 biber 处理参考文献，建议按以下步骤编译：

1. XeLaTeX 编译：xelatex template.tex

2. Biber 处理参考文献：biber template

3. XeLaTeX 再次编译（2 次）：xelatex template.tex

或使用支持 latexmk 的工具，设置引擎为 xelatex 并启用 biber。

⚠️ 注意：Biber 对含中文的文件名支持不佳，建议将主文件名命名为纯英文（如 homework1.tex），最终生成 PDF 后再手动重命名。

## 4.常用环境与命令
### 4.1 摘要与正文
```latex
\begin{abstract}
    这里是摘要内容。
\end{abstract}
```
正文按 `\section、\subsection、\subsubsection` 组织。

### 4.2 提示框
模板提供了三种可选择的提示框
```latex
\begin{note}
    普通提示信息。
\end{note}

\begin{warning}
    警告信息。
\end{warning}

\begin{tip}
    小技巧或建议。
\end{tip}
```

### 4.3 引用块
一种单独展示的引用块可以用来单独放置引文
```latex
\begin{ghquote}
    这是引用文本，适合放置引文或注释。
\end{ghquote}
```

### 4.4 代码块
使用 `codeblock` 环境，需在第二个大括号中指定语言（支持所有 `minted` 支持的语言，如 `bash、C++、Python` 等）：
```latex
\begin{codeblock}{bash}
git clone https://github.com/user/project
cd project
make
\end{codeblock}
```
行内代码可以使用 `\inlinecode` 命令：
```latex
使用 \inlinecode{git status} 查看状态。
```

### 4.5 表格
推荐使用 `booktabs` 宏包提供的三线表：
```latex
\begin{center}
    \begin{tabular}{lll}
        \toprule
        \tabularcenter{Name} & \tabularcenter{Language} & \tabularcenter{Stars} \\
        \midrule
        Project C & BrainFuck & 10000 \\
        Project A & C++ & 1000 \\
        \bottomrule
    \end{tabular}
\end{center}
```
其中 `\tabularcenter{内容}` 用于在列中强制居中（默认表格列对齐方式可通过参数调整）。

### 4.6 列表
推荐直接使用常用的itemize,enumerate

### 4.7 分栏
使用 `\twocolumn` 命令后，后续内容将按双栏排版。若需恢复单栏，可使用 `\onecolumn`（但通常用于文档末尾的参考文献之前）。

## 5.参考文献管理
### 5.1 添加引用条目
在 `template_reference.bib` 中按照 `BibTeX` 格式添加文献。模板已给出常用类型示例（期刊、会议、书籍、学位论文等），并包含一个示例条目 `icsbook`。

### 5.2 在正文中引用
使用 `\cite{引用标签}` 命令：
```latex
网络空间安全\cite{icsbook}
```

### 5.3 输出参考文献列表
在文档末尾（通常位于 `\twocolumn` 之后）使用：
```latex
\printbibliography
```
模板已默认设置参考文献样式为 `gb7714-2015`（符合中文国家标准），后端为 `biber`。

## 6.其他定制选项
- 页眉页脚：修改 `\fancyhead[L]、\fancyhead[R]、\fancyfoot[C]` 中的内容。

- 页面边距：调整 `\geometry{left=2cm,right=2cm,top=2cm,bottom=2.5cm}`。

- 标题样式：可通过 `\titleformat` 修改 `\section、\subsection` 的字体、大小等。

- 颜色主题：预定义的 GitHub 颜色变量（如 gh-link、gh-codebg）可在 `\definecolor` 处重新定义。

## 7.常见问题

|问题|解决方法|
|---|---|
|中文乱码或字体缺失|确保使用 `XeLaTeX` 编译，系统已安装中文字体（模板未显式指定中文字体，依赖 ctex 默认配置）。|
|`minted` 报错缺少 `Pygments`	|安装 `Python` 并执行 `pip install pygments`，编译时添加 `--shell-escape` 参数（如 `xelatex --shell-escape template.tex`）。|
|参考文献未显示或引用为 \[?\]|	检查编译步骤是否包含 `biber`，且 `.bib` 文件路径正确（模板中已 `\addbibresource{template_reference.bib}`）。|
|两栏模式下表格/图片宽度异常|	使用 `\begin{table*}` 或 `\begin{figure*}` 可跨栏排版，或使用 \onecolumn 临时恢复单栏。|

## 8. 最终输出
编译成功后生成 PDF 文件，建议使用脚本或手动重命名为 `姓名列表_第n次大作业_实验标题.pdf` 格式，方便提交。

本模板适用于中国科学院大学《网络空间安全导论》课程及相关实验报告，欢迎根据实际需求进行二次修改。
