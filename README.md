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

## 封面与提示框

- **封面**（`book/content/front/00-title.tex`）：用 TikZ `remember picture, overlay`
  满版重绘原书封面版式（蓝条 / 暗带代码纹理 / 亮蓝标题带）。
  **必须跑满两遍**，否则 `current page` 定位失败，封面错位、页数也会异常。
- **提示框**：原书**没有**任何文字标签（NOTE/TIP/WARNING 都不存在），所谓"标签"
  就是页边图标。本项目沿用原书两个图标：
  - `images/icon_info.png`（ℹ 信息框）
  - `images/icon_warning.png`（⚠ 警告框）
  导言区提供 `myInfoBox` / `myWarnBox` 两个环境（左侧页边图标 + 正文缩进），无参数。

---

## 已知坑与约定

1. **封面必须两遍**：只跑一遍 `xelatex` 会因 `remember picture` 未定位而封面错位，
   且页数会骤降（曾出现 138 页假象，实为内容被吞）。两遍后恢复正常（210 页）。
2. **minted v3 缓存必须绝对路径**：`latexminted` 拒绝相对路径写入，默认
   `_minted` 缓存会全部失败。已在 `book/ccs.tex` 把 `\minted@cachedir` 设成
   `TEXMF_OUTPUT_DIRECTORY` / `TEXMFOUTPUT` 给出的工程绝对根。
3. **代码清单行号污染（已修）**：原书代码清单带行号栏，提取器把行号当成独立代码行
   混入两处——清单 4.4（`main/04`，行号 1–13）与清单 8.4（`main/08`，行号 1–20）。
   已在译稿中按"从 1 起、无间断的连续整数序列"判据整行删除；程序输出中的数字
   （如未初始化垃圾值、内存地址）与编译器诊断的 `N |` 四行结构均**保留**（后者是原书
   忠实还原，非 bug）。
4. **按行号指代的措辞**：全书采用无行号设计，但个别正文有"考虑第 3 行："这类按行号
   指代的措辞，去行号后略显突兀，属遗留措辞，暂未统一改写。
5. **LaTeX 转义铁律**：`\texttt{}` 不改变字符类别，`_ # % &` 仍需转义；
   `\url{}` 中 `_` 直接写、`# %` 必须转义；代码体 / `minted` 内部无需转义。
6. **章节装配**：增删/调序章节只改 `book/index.tex` 里的 `\myChapter{}` / `\myAppendix{}`
   / `\myFront{}` 行即可，无需动其它文件。

---

## 字体映射

| 用途 | 字体 |
|---|---|
| 中文正文（衬线） | Source Han Serif SC |
| 中文标题（无衬线） | Microsoft YaHei |
| 拉丁正文 | Cambria |
| 拉丁标题 | Franklin Gothic Medium |
| 代码 / 等宽 | Consolas / JetBrains Mono |

---

## 工作流（供复核）

1. 从原书 PDF 用 `../work/extract.py` 提取章节文本（`../work/chapters/*.txt`）。
2. 对照 `../work/TRANSLATION_GUIDE.md` 翻译，产出 `book/content/**/*.tex`。
3. 按上节命令编译，检查 `main.log` 无报错、目视核对封面与提示框。

> 原书索引未纳入本翻译工程（全书采用无行号 + 静态章节装配，未做页级索引）。
