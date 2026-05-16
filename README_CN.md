# IEEE LaTeX Template

<p align="center">
  <a href="./README.md">English</a> |
  <a href="./README_CN.md">中文</a>
</p>

一个面向中文科研写作者的 IEEE LaTeX 模板，支持 VSCode、TeX Live、XeLaTeX 中文编译与参考文献工作流。

本项目适用于希望使用 **VSCode + TeX Live + LaTeX Workshop** 编写 IEEE 风格论文的用户，尤其适合需要在 IEEE 模板中编译中文内容、中文注释或中英文混排内容的场景。

---

## 目录

- [环境安装](#环境安装)
  - [Windows](#windows)
  - [Linux](#linux)
- [验证 TeX Live 是否安装成功](#验证-tex-live-是否安装成功)
- [安装 VSCode 插件](#安装-vscode-插件)
- [配置 LaTeX Workshop](#配置-latex-workshop)
- [中文编译说明](#中文编译说明)

---

## 环境安装

本项目依赖 TeX Live。TeX Live 是一个跨平台的 TeX/LaTeX 发行版，支持 Windows、Linux 和 macOS。

---

### Windows

1. 进入 [TeX Live 官网](https://www.tug.org/texlive/) 下载安装程序。
2. 安装方式选择 `Easy install`。
3. 下载完成后，运行安装程序。
4. 根据需要修改安装路径。
5. 点击安装并等待完成。

TeX Live 安装过程通常需要较长时间，可能需要 1 至 2 小时。安装完成后，点击“完成”即可。

---

### Linux

在 Ubuntu 或 Debian 系统中，可以直接安装完整版本：

```bash
sudo apt update
sudo apt install texlive-full
```

`texlive-full` 体积较大，但可以避免后续因缺少宏包导致的编译错误。

---

## 验证 TeX Live 是否安装成功

安装完成后，在终端中输入：

```bash
xelatex -v
```

如果终端输出 XeLaTeX 的版本信息，说明 TeX Live 已经安装成功。

---

## 安装 VSCode 插件

打开 VSCode，进入扩展商店，搜索并安装：

```text
LaTeX Workshop
```

该插件用于在 VSCode 中编译、预览和管理 LaTeX 项目。

---

## 配置 LaTeX Workshop

打开 VSCode 的全局配置文件 `settings.json`：

1. 按快捷键：

```text
Ctrl + Shift + P
```

2. 输入：

```text
Open User Settings (JSON)
```

3. 选择该选项，打开 VSCode 的全局配置文件 `settings.json`。

4. 将下面的配置复制到 `settings.json` 中。

注意：如果你的 `settings.json` 中已经有其他配置，只需要把下面这些字段合并进去，不要重复写最外层的大括号 `{}`。

```jsonc
// ==================== LaTeX Workshop 核心配置 ====================

// 1. 定义编译工具
"latex-workshop.latex.tools": [
    {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "-xelatex",
            "-outdir=%OUTDIR%",
            "%DOC%"
        ]
    },
    {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-shell-escape",
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

// 2. 定义编译链 recipes
"latex-workshop.latex.recipes": [
    {
        "name": "latexmk (xelatex)",
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
        "name": "xelatex",
        "tools": [
            "xelatex"
        ]
    }
],

// 3. 设置默认编译链
"latex-workshop.latex.recipe.default": "latexmk (xelatex)",

// ==================== 编译与清理设置 ====================

// 保存文件时自动编译
"latex-workshop.latex.autoBuild.run": "onSave",

// 编译成功后自动清理临时文件
"latex-workshop.latex.autoClean.run": "onBuilt",

// 清理临时文件的类型
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
    "*.fdb_latexmk",
    "*.bcf",
    "*.run.xml"
],

// ==================== PDF 预览设置 ====================

// 在 VSCode 新标签页中打开 PDF
"latex-workshop.view.pdf.viewer": "tab",

// 启用正向和反向搜索
"latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",

// ==================== 辅助功能 ====================

// 在资源管理器侧边栏显示 LaTeX 结构
"latex-workshop.showContextMenu": true,

// 引用补全设置
"latex-workshop.intellisense.citation.backend": "biblatex",
"latex-workshop.intellisense.citation.format": "latex",

// 简化错误和警告弹窗
"latex-workshop.message.error.show": false,
"latex-workshop.message.warning.show": false,

// LaTeX 文件自动换行
"[latex]": {
    "editor.wordWrap": "on",
    "editor.wrappingIndent": "same"
}
```

---

## 中文编译说明

本项目推荐使用 XeLaTeX 编译中文内容。

如果你的论文中包含中文，建议在 `.tex` 文件中使用（默认打开）：

```latex
\usepackage[UTF8, scheme=plain, fontset=fandol]{ctex}
```


注意：不要同时使用 `CJKutf8` 和 `ctex`，否则容易造成中文编译错误。

不推荐在 XeLaTeX 中继续使用：

```latex
\usepackage{CJKutf8}
\begin{CJK*}{UTF8}{gbsn}
...
\end{CJK*}
```

---

## License

This project is released under the MIT License.
