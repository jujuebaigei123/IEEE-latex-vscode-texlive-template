# IEEE LaTeX Template

<p align="center">
  <a href="./README.md">English</a> |
  <a href="./README_CN.md">中文</a>
</p>

An IEEE LaTeX template for Chinese academic writers, supporting VSCode, TeX Live, XeLaTeX Chinese compilation, and bibliography workflows.

This project is designed for users who want to write IEEE-style papers with **VSCode + TeX Live + LaTeX Workshop**. It is especially useful for scenarios where Chinese text, Chinese comments, or Chinese-English mixed content need to be compiled within an IEEE LaTeX template.

---

## Table of Contents

- [Environment Setup](#environment-setup)
  - [Windows](#windows)
  - [Linux](#linux)
- [Verify TeX Live Installation](#verify-tex-live-installation)
- [Install VSCode Extension](#install-vscode-extension)
- [Configure LaTeX Workshop](#configure-latex-workshop)
- [Chinese Compilation Notes](#chinese-compilation-notes)
- [License](#license)

---

## Environment Setup

This project depends on TeX Live. TeX Live is a cross-platform TeX/LaTeX distribution that supports Windows, Linux, and macOS.

---

### Windows

1. Go to the [TeX Live official website](https://www.tug.org/texlive/) and download the installer.
2. Select `Easy install` during installation.
3. Run the installer after the download is complete.
4. Modify the installation path if needed.
5. Click install and wait for the installation to complete.

The TeX Live installation may take a long time, usually around 1 to 2 hours. After the installation is finished, click “Finish”.

---

### Linux

On Ubuntu or Debian-based systems, you can install the full TeX Live distribution directly:

```bash
sudo apt update
sudo apt install texlive-full
```

`texlive-full` requires a large amount of disk space, but it helps avoid compilation errors caused by missing LaTeX packages.

---

## Verify TeX Live Installation

After installation, enter the following command in the terminal:

```bash
xelatex -v
```

If the terminal outputs the XeLaTeX version information, TeX Live has been installed successfully.

---

## Install VSCode Extension

Open VSCode, go to the Extensions marketplace, search for and install:

```text
LaTeX Workshop
```

This extension is used to compile, preview, and manage LaTeX projects in VSCode.

---

## Configure LaTeX Workshop

Open the global `settings.json` file in VSCode:

1. Press the shortcut:

```text
Ctrl + Shift + P
```

2. Search for:

```text
Open User Settings (JSON)
```

3. Select it to open the global VSCode configuration file `settings.json`.

4. Copy the following configuration into `settings.json`.

Note: If your `settings.json` already contains other settings, only merge the following fields into it. Do not duplicate the outermost braces `{}`.

```jsonc
// ==================== LaTeX Workshop Core Configuration ====================

// 1. Define LaTeX build tools
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

// 2. Define build recipes
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

// 3. Set the default build recipe
"latex-workshop.latex.recipe.default": "latexmk (xelatex)",

// ==================== Build and Clean Settings ====================

// Automatically build the project when saving files
"latex-workshop.latex.autoBuild.run": "onSave",

// Automatically clean auxiliary files after successful compilation
"latex-workshop.latex.autoClean.run": "onBuilt",

// Auxiliary file types to clean
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

// ==================== PDF Preview Settings ====================

// Open the generated PDF in a VSCode tab
"latex-workshop.view.pdf.viewer": "tab",

// Enable forward and inverse search
"latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",

// ==================== Auxiliary Features ====================

// Show LaTeX structure in the VSCode sidebar
"latex-workshop.showContextMenu": true,

// Citation completion settings
"latex-workshop.intellisense.citation.backend": "biblatex",
"latex-workshop.intellisense.citation.format": "latex",

// Hide error and warning pop-up messages
"latex-workshop.message.error.show": false,
"latex-workshop.message.warning.show": false,

// Enable word wrap for LaTeX files
"[latex]": {
    "editor.wordWrap": "on",
    "editor.wrappingIndent": "same"
}
```

---

## Chinese Compilation Notes

This project recommends using XeLaTeX to compile Chinese content.

If your paper contains Chinese text, it is recommended to use the following package in your `.tex` file. This option is enabled by default in this template:

```latex
\usepackage[UTF8, scheme=plain, fontset=fandol]{ctex}
```

Do not use `CJKutf8` and `ctex` at the same time, as this may cause Chinese compilation errors.

When using XeLaTeX, the following legacy CJK configuration is not recommended:

```latex
\usepackage{CJKutf8}
\begin{CJK*}{UTF8}{gbsn}
...
\end{CJK*}
```

---

## License

This project is released under the MIT License.
