# 字体目录

模板优先使用官方 Word 模板要求的字体，并提供跨平台回退：

- 正文：Times New Roman、宋体；回退到 Noto Serif SC、思源宋体等。
- 标题：Arial、黑体；回退到 Noto Sans SC、思源黑体、微软雅黑等。
- 图表题注：Times New Roman、国标楷体或楷体。

仓库附带的 `gbkai-Regular.ttf` 来自本仓库的 LaTeX 参考模板。Windows 自带的宋体、黑体、楷体、仿宋和 Times New Roman 未复制进仓库，避免重复分发系统专有字体。

编译时使用：

```powershell
typst compile --font-path assets/font main.typ
```

如需完全可复现的跨平台输出，可在确认字体授权后，将所需字体文件放入本目录，并在 `template/common.typ` 的字体回退列表中加入其字体族名称。

