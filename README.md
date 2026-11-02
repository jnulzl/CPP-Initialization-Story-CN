# C++ 初始化全解（中文翻译版 · LaTeX 工程）

把 Bartłomiej Filipek 的 *C++ Initialization Story*（2022, Leanpub）全书译为中文，
并使用 Manning 风格 LaTeX 模板排版。本目录即编译工程根目录；原书英文 PDF 在上一层目录。

- 原书：*C++ Initialization Story — A Guide Through All Initialization Options and Related C++ Areas*
- 作者：Bartłomiej Filipek（cppstories.com）
- 中文名：**C++ 初始化全解**
- 成品：`latex_source/main.pdf`

---

## 目录结构

```
latex_source/                  # 本目录即工程根
├── main.tex                   # 编译入口：\input{book/ccs.tex} + \subfile{book/index.tex}
├── build.bat                  # ⚠️ 模板遗留的另一本书脚本，本项目【不用它】编译
├── fonts/                     # 字体文件（JetBrains Mono 等）
├── images/                    # 69 张插图 + icon_info.png / icon_warning.png（提示框图标）
├── main.pdf                   # 编译成品（约 210 页）
└── book/
    ├── ccs.tex                # 导言区：自定义命令/环境、minted v3 缓存配置
    ├── index.tex              # 章节装配（手写静态文件，增删章节改这里）
    ├── manning-pygments-style.tex  # 代码高亮配色
    └── content/
        ├── front/             # 00-title(封面) / 00-about / 01-author / 02-ack / 03-rev
        ├── main/              # 第 1–14 章（main/07 是第 1–6 章小测验）
        ├── back/              # 00-quiz(第 14 章「最终测验与练习」) / 02-references
        └── appendix/          # 01(特殊成员函数生成规则) / 02(测验与练习答案)
```

> 上一层目录另有原书英文 PDF（`*.pdf`）与 `work/` 工作区（提取脚本、章节 txt、日志等，非编译所需）。

---

## 环境依赖

| 依赖 | 版本 / 说明 |
|---|---|
| TeX Live | 2026（含 `xelatex`、minted **v3** / `latexminted`、`pygmentize`） |
| Python | 3.x（提供 `pygmentize`，且 `latexminted` 脚本需要可用的 `python.exe`） |
| 中文字体 | **Source Han Serif SC（思源宋体）**、**Microsoft YaHei** |
| 拉丁字体 | Cambria、Franklin Gothic Medium、Consolas |
| 等宽字体 | JetBrains Mono（字体文件已放 `fonts/`，也可系统自带 Consolas） |

> 字体缺失会导致编译报 "font not found"。思源宋体与微软雅黑需本机已安装。

---

## 编译

本项目**不**使用 `build.bat`（那是模板自带的另一本书的脚本）。在本目录（`latex_source/`）下用命令行编译：

```bash
# 1) 设置 PATH：Python 根目录必须在 TeX bin 之前
export PATH="/d/ProgramData/texlive/2026/bin/windows:$PATH"
# 2) 让 minted v3 把缓存写到绝对路径（见「已知坑」第 2 条），必须指向本工程绝对根
export TEXMF_OUTPUT_DIRECTORY="D:/Ego/trans/C++ Initialization Story/latex_source"
export TEXMFOUTPUT="$TEXMF_OUTPUT_DIRECTORY"

# 3) 跑两遍（封面用 remember picture，需两遍才定位）
xelatex -shell-escape -interaction=nonstopmode -file-line-error main.tex
xelatex -shell-escape -interaction=nonstopmode -file-line-error main.tex
```

- `-shell-escape` 是必须的（minted 调外部高亮器）。
- 首次或 `book/content/**` 有变动后，minted 会重高亮全部代码清单（约数分钟）；
  缓存命中时一遍约 1 分钟。
- 用 `-file-line-error` 可快速定位报错所在的文件与行号。
- 编译产物：`main.pdf`（约 210 页）、`main.aux`、`main.toc`、`main.log` 等。

---

> 如果环境配置遇到问题或者编译失败可以直接到[releases](https://github.com/jnulzl/CPP-Initialization-Story-CN/releases)下载最新的pdf文件
> 

## 官方代码

[cppinitbook_public](https://github.com/fenbf/cppinitbook_public) —— 书中示例代码。