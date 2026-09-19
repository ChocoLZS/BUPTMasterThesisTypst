> **说明：** 本模板不代表最终或最准确的排版标准。仓库将在作者撰写毕业论文期间持续更新，也欢迎通过 Issue、Pull Request 或 Fork 参与改进。

# BUPTMasterThesisTypst

北京邮电大学研究生学位论文 Typst 模板。模板以学校发布的《北京邮电大学研究生学位论文模板—理工科（2025 年 1 月 15 日）》为主要依据，并参考仓库中的 LaTeX 模板实现。

> [!IMPORTANT]
> 本项目是非官方模板。学院、研究生院或当年通知如有更新，应以最新要求为准；提交前请人工核对封面信息、声明、页码、目录、图表、参考文献和打印装订要求。

## 模板特点

- 使用一个 `main.typ` 集中填写论文信息和控制章节顺序。
- 正文按章节拆分，摘要、符号说明和后置部分分别存放，便于长期维护。
- 自动生成中英文封面、答辩委员会名单、原创性声明、授权说明、中英文摘要、目录、符号说明、参考文献、附录、致谢、成果目录和学习经历。
- 自动处理章节标题、页眉、页码、目录、首行缩进和中英文混排字体。
- 图、表、公式按章编号；提供手动分页、双语图题、双语表题、三线表和题注说明组件。
- 支持电子版连续排版和双面打印奇数页起排。
- 示例内容全部为占位内容，不包含真实姓名、学号、导师或研究数据。

## 环境要求

- [Typst](https://typst.app/) 0.15 或更新版本；本模板使用 Typst 0.15.1 验证。
- 推荐使用 VS Code 和 Tinymist 扩展进行预览、跳转和自动补全。
- Windows 上安装宋体、黑体、楷体和 Times New Roman 时，模板会优先使用这些字体；其他平台会自动尝试 Noto/思源字体。

## 快速开始

1. 在 [`main.typ`](main.typ) 的 `info` 中填写论文元数据。
2. 分别编辑 `frontmatter/`、`chapters/`、`backmatter/` 和 `bibliography/references.bib`。
3. 编译 PDF。

PowerShell：

```powershell
New-Item -ItemType Directory -Force build | Out-Null
typst compile --font-path assets/font main.typ build/main.pdf
```

需要双面打印版时，无需修改 `main.typ`，可临时开启奇数页起排：

```powershell
typst compile --font-path assets/font --input print-mode=true main.typ build/main-print.pdf
```

持续预览：

```powershell
typst watch --font-path assets/font main.typ build/main.pdf
```

Linux/macOS：

```bash
mkdir -p build
typst compile --font-path assets/font main.typ build/main.pdf
```

`--font-path assets/font` 用于加载仓库附带的国标楷体。即使省略该参数，模板也会尝试使用系统字体，但不同机器上的换行和分页可能略有差异。

## 目录结构

```text
.
├── main.typ                       # 论文入口、元数据和整体结构
├── template/
│   ├── bupt-thesis.typ            # 主模板与页面编排
│   ├── common.typ                 # 字体、编号和公共工具
│   ├── cover.typ                  # 中英文封面、答辩委员会页
│   ├── statement.typ              # 原创性与授权声明
│   ├── frontmatter.typ            # 摘要、目录、符号说明
│   ├── backmatter.typ             # 后置章节和学习经历
│   └── components.typ             # 分页、图、表、三线表和题注组件
├── frontmatter/
│   ├── abstract-zh.typ            # 中文摘要正文
│   ├── abstract-en.typ            # 英文摘要正文
│   └── nomenclature.typ           # 符号说明数据
├── chapters/
│   ├── chapter1.typ               # 示例章，可复制后改名
│   └── chapter2.typ
├── backmatter/
│   ├── appendix.typ
│   ├── acknowledgement.typ
│   ├── achievements.typ
│   └── experience.typ
├── bibliography/references.bib    # BibLaTeX/BibTeX 文献库
└── assets/
    ├── font/                       # 可再分发字体及字体说明
    └── img/                        # 校名、校徽等模板图片
```

`references/` 用于存放官方 Word 和 LaTeX 参考材料，已被 `.gitignore` 忽略，避免把大文件或可能含个人信息的参考稿提交到仓库。

## 填写论文信息

论文元数据集中在 `main.typ` 的 `info` 字典中：

| 字段 | 含义 | 示例或可选值 |
| --- | --- | --- |
| `title_zh` / `title_en` | 中英文题目 | 单行内容或最多两行数组 |
| `student_id` | 学号 | 字符串，避免丢失前导零 |
| `author_zh` / `author_en` | 中英文姓名 | 任意内容 |
| `major_zh` / `major_en` | 学科专业 | 任意内容 |
| `supervisor_zh` / `supervisor_en` | 导师姓名和职称 | 任意内容 |
| `institute_zh` / `institute_en` | 学院名称 | 任意内容 |
| `degree` | 学位层次 | `"master"` 或 `"doctor"` |
| `degree_type` | 学位类别 | `"academic"` 或 `"professional"` |
| `study_mode` | 学习方式 | `"full-time"` 或 `"part-time"` |
| `confidentiality` / `confidentiality_en` | 密级 | 通常为“公开”/`"Public"` |
| `date` | 论文日期 | `(year: 2027, month: 6, day: 1)` |
| `defense_date` | 答辩日期 | 同上 |

答辩委员会在 `committee` 数组中逐行填写。没有内容的单元格可以保留为空：

按照官方 Word 模板，封面题目固定预留两条等宽下划线，“题目：”固定在左侧。标题内容默认在每条下划线内居中；建议把题目明确分成两行：

```typst
title_zh: (
  [较长论文题目的第一行],
  [论文题目的第二行],
),

// 只有一行时，也可以直接填写字符串；第二条下划线会保留为空
title_zh: "单行论文题目",
```

两条下划线使用官方封面的固定宽度，不会随标题长短缩放。建议主动分行，避免依赖自动换行导致封面结果随字体变化。若希望标题从下划线左端开始，可在 `settings` 中设置 `cover_title_alignment: left`。

```typst
#let committee = (
  (role: [主席], name: [姓名], title: [职称], affiliation: [工作单位]),
  (role: [委员], name: [], title: [], affiliation: []),
  (role: [秘书], name: [], title: [], affiliation: []),
)
```

## 撰写正文

每一章放在独立文件中，文件只写正文，不需要重复设置页边距、字体、页眉或页码：

```typst
= 绪论

这里是正文。正文会自动使用小四号字、20 pt 固定行距、两端对齐和首行缩进。

== 研究背景

二级及以下标题会自动编号。

=== 三级标题
==== 四级标题
===== 五级标题
```

在 `main.typ` 末尾按顺序包含章节：

```typst
#include "chapters/chapter1.typ"
#include "chapters/chapter2.typ"
#include "chapters/chapter3.typ"
```

标题样式由模板根据级别自动计算：一级到三级使用官方模板参数，四级及更深层级统一采用 12 pt 宋体、上方 5 pt、下方 5 pt 的兜底样式。编号会随层级自动扩展，不需要为五级、六级标题新增排版规则。标题自身会自动取消正文首行缩进；标题结束后，后续正文仍恢复两字符首行缩进。

目录默认收录到三级标题。可通过 `settings.toc_depth` 修改深度，例如设为 `5` 时会收录到五级；目录层级每深入一级自动增加 21 pt 缩进。

### 手动分页

需要在章节内部强制换页时，导入并调用 `thesis-pagebreak`：

```typst
#import "../template/bupt-thesis.typ": thesis-pagebreak

上一页的内容。

#thesis-pagebreak()

从新一页开始的内容。
```

双面打印时还可以指定下一部分从奇数页或偶数页开始：

```typst
#thesis-pagebreak(to: "odd")
#thesis-pagebreak(to: "even")
```

如果当前位置本来就在新页页首，可传入 `weak: true`，避免再额外生成空白页。

正文默认采用 12 pt 字号、20 pt 基线间距。模板同时把段间距设为行间空白值，因此普通段落之间不会额外空出一行。所有正文段落（包括标题、公式、图表之后的第一段）都会缩进 `2em`，即两个全角字符。摘要正文同样采用 20 pt 基线间距。

表格、图题、表题、题注、目录和签名栏会在各自的局部作用域中取消首行缩进；组件结束后 Typst 会自动恢复正文的缩进和行距设置，不会污染后续段落。

## 图、表、公式与交叉引用

章节中先导入组件：

```typst
#import "../template/bupt-thesis.typ": thesis-pagebreak, thesis-figure, thesis-table, academic-table, caption-note
```

插入带中英文图题的图片：

```typst
#thesis-figure(
  image("../assets/img/example.png", width: 70%),
  [系统结构示意图],
  caption_en: [System architecture],
  label: <fig:architecture>,
)

如图 @fig:architecture 所示。
```

插入三线表：

```typst
#thesis-table(
  academic-table(
    (1fr, 1fr, 1fr),
    ([项目], [单位], [数值]),
    (
      ([样例 A], [ms], [12.3]),
      ([样例 B], [ms], [15.8]),
    ),
  ),
  [实验结果],
  caption_en: [Experimental results],
  label: <tab:results>,
)
```

表格支持行、列合并。在 `rows` 中直接使用 Typst 原生的 `table.cell`，`rowspan` 表示跨行数，`colspan` 表示跨列数：

```typst
#thesis-table(
  academic-table(
    (1fr, 1fr, 1fr),
    ([项目], [属性], [数值]),
    (
      (table.cell(rowspan: 2)[正文], [字号], [12 pt]),
      ([固定行距], [20 pt]),
      (table.cell(colspan: 2)[页边距（左右）], [31.7 mm]),
    ),
  ),
  [包含合并单元格的示例表],
  label: <tab:merged-cells>,
)
```

被 `rowspan` 占用的位置不再重复填写单元格。上例第二个数据行只有两个单元格，因为第一列由上一行的“正文”跨行占用。

插入公式并引用：

```typst
$ y = sum_(i=1)^n x_i / n $ <eq:mean>

由公式 @eq:mean 可得……
```

图、表和公式分别独立计数，显示为 `1-1`、`1-1` 和 `(1-1)`。进入新的有编号章节时计数自动归零。

`caption-note[...]` 可用于图表下方的数据来源或注释，字号和缩进由模板统一设置。

## 参考文献

将文献写入 `bibliography/references.bib`，正文使用 `@引用键` 引用。例如：

```bibtex
@standard{gbt7714,
  title = {信息与文献 参考文献著录规则},
  organization = {全国信息与文献标准化技术委员会},
  year = {2015},
}
```

```typst
参考文献著录可遵循国家标准 @gbt7714。
```

模板使用 Typst 内置的 `gb-7714-2015-numeric` 样式生成参考文献列表。若学院要求其他著录格式，可在 `main.typ` 的 `bibliography(...)` 中更换 `style`。

## 页面和输出设置

`main.typ` 的 `settings` 提供常用开关：

| 参数 | 默认值 | 说明 |
| --- | --- | --- |
| `print_mode` | `false` | 为 `true` 时，主要部分从奇数页开始，适合双面打印；命令行可用 `--input print-mode=true` 临时覆盖 |
| `show_english_cover` | `true` | 是否生成英文封面 |
| `show_committee` | `true` | 是否生成答辩委员会页 |
| `show_statement` | `true` | 是否生成声明与授权页 |
| `show_nomenclature` | `true` | 是否生成符号说明 |
| `toc_depth` | `3` | 目录标题深度 |
| `cover_title_alignment` | `center` | 中英文封面标题在固定下划线内的对齐方式，可设为 `left`、`center` 或 `right` |
| `page_margin` | 官方 Word 页边距 | 可传入自定义页边距字典 |
| `body_size` | `12pt` | 正文字号 |
| `body_line_spacing` | `20pt` | 正文固定基线间距 |
| `abstract_size` | `14pt` | 中英文摘要正文字号 |
| `abstract_line_spacing` | `20pt` | 中英文摘要固定基线间距 |
| `fonts` | 模板字体回退表 | 自定义字体族 |

例如：

```typst
#let settings = (
  print_mode: true,
  toc_depth: 3,
  page_margin: (top: 25.4mm, bottom: 25.4mm, left: 31.7mm, right: 31.7mm),
  body_size: 12pt,
  body_line_spacing: 20pt,
  abstract_size: 14pt,
  abstract_line_spacing: 20pt,
)
```

模板默认按官方 Word 文件采用 A4 纸、上下 25.4 mm、左右 31.7 mm 的正文页边距。除非学院另有通知，不建议只为减少页数而修改这些参数。

## 字体

字体回退列表定义在 `template/common.typ`：

- 正文：Times New Roman、宋体，并回退到 Noto Serif SC、思源宋体等。
- 标题：Arial、黑体，并回退到 Noto Sans SC、思源黑体、微软雅黑等。
- 图表题注：Times New Roman、国标楷体/楷体，并回退到仿宋。
- 英文：Times New Roman，并回退到 Libertinus Serif。

仓库只附带原 LaTeX 参考模板中已有的 `gbkai-Regular.ttf`。Windows 系统专有字体没有复制进仓库；这样可以避免重复分发受许可约束的字体。若需要跨平台完全一致的分页，请在确认字体授权后把所需字体放入 `assets/font/`，并在 `template/common.typ` 中把实际字体族名加入回退列表。

可用以下命令检查 Typst 识别到的字体：

```powershell
typst fonts --font-path assets/font
```

## 页面顺序

默认输出顺序如下：

1. 中文封面
2. 英文封面
3. 答辩委员会名单
4. 原创性声明和论文使用授权说明
5. 中文摘要
6. 英文摘要
7. 目录
8. 符号说明（有数据且已启用时）
9. 正文章节
10. 参考文献
11. 附录
12. 致谢
13. 攻读学位期间取得的创新成果目录
14. 作者学习经历

前置部分使用大写罗马数字页码，正文及后置部分使用阿拉伯数字页码。页眉在奇数页显示当前一级标题，在偶数页显示学校和学位论文名称。

## 提交前检查

- 删除所有“请填写”、示例标题、空白委员会成员和示例文献。
- 检查学位层次、学位类别、学习方式、密级和日期。
- 更新目录并检查长标题是否换行合理。
- 检查每个图表都有编号、题名、正文引用和必要的数据来源。
- 检查公式、参考文献、附录和成果目录的交叉引用。
- 根据电子提交或双面装订要求设置 `print_mode`。
- 在最终提交机器上重新编译，确认没有缺失字体警告。
- 将 PDF 与学院、研究生院发布的最新版模板逐页核对。

## 常见问题

### 编译时提示缺少字体

先运行 `typst fonts --font-path assets/font` 检查字体族名。可安装宋体/黑体/Times New Roman，或安装 Noto/思源字体；也可以修改 `template/common.typ` 中的回退列表。

### 图片路径找不到

章节文件中的相对路径以章节文件所在目录为起点，例如 `chapters/chapter1.typ` 引用图片时通常写 `../assets/img/example.png`。模板内部以 `/assets/...` 引用根目录资源。

### 双面打印出现空白页

这是 `print_mode: true` 的预期行为，用于让主要部分从奇数页开始。仅提交电子版时可设为 `false`。

### 修改内容后目录页码没有变化

Typst 会自动多轮排版并更新页码。若编辑器预览没有刷新，重新运行 `typst compile` 或重启 `typst watch`。

## 许可

模板源代码采用 [MIT License](LICENSE)。校名、校徽、官方参考文档和字体仍受其各自权利人及许可条款约束；使用者应确保自己的使用场景符合学校规定和相关授权。
