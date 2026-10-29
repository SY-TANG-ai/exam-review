# exam-review

**生成结构化考试复习资料 + 转换为可打印的 LaTeX PDF。**

一个双组件工具包，帮助学生把课件和作业变成条理清晰的复习材料，并可选择输出为紧凑或宽松的 Cheat Sheet PDF 用于打印。

---

## 组件

| 组件 | 功能 | 适用人群 |
|------|------|---------|
| [`skill/`](./skill/) | AI 助手技能——读取课件 PDF 和作业文件，生成中英夹杂的逐课讲解 + 全英文 Cheat Sheet | AI 助手用户 |
| [`md2tex/`](./md2tex/) | Python CLI 工具——将 Markdown Cheat Sheet 转为单个自包含的 `.tex` 文件，在 Overleaf（XeLaTeX）编译为紧凑 PDF | 任何有 Markdown Cheat Sheet 的人 |

两者可以**配合使用**：skill 产出 Cheat Sheet，md2tex 渲染成可打印格式。但每个组件也可独立使用。

---

## 快速开始

### 1. 生成复习资料（需 AI 助手）

直接告诉你的 AI 助手：

> *"下载并安装这个 skill：https://github.com/SY-TANG-ai/exam-review"*

然后说：

> *"帮我复习这门课，这是课件地址和作业地址{课件地址}{作业文件地址}"*
如果使用WorkBuddy，也可以将课件和作业文件上传到IMA知识库或是WorkBuddy资料库，配合使用。

Skill 产出：逐课讲解 → `Cheat Sheet.md`

### 2. 转换为 PDF（不需要 AI 助手）

```bash
python md2tex/md2tex.py "Cheat Sheet.md" -o "cheatsheet.tex"
```

将 `cheatsheet.tex` 上传到 [Overleaf](https://www.overleaf.com)，编译器选 **XeLaTeX**，直接编译。

> 不用 AI 助手？`md2tex` 可以处理任何 Markdown 文件——自己写一份 Cheat Sheet 就行。

---

## 项目结构

```
exam-review/
├── README.md                 ← 英文版
├── README.zh.md              ← 本文件（中文版）
├── LICENSE                   ← MIT
├── .gitignore
├── skill/                    ← AI 助手 skill
│   ├── SKILL.md              ← 主 skill 文件（AI 执行指令）
│   ├── README.md             ← Skill 安装与使用指南
│   └── design-notes.md       ← 设计决策说明
├── md2tex/                   ← 转换工具
│   ├── md2tex.py             ← CLI 脚本（Python 3）
│   ├── README.md             ← 工具用法、规格与已知限制
│   └── examples/
│       ├── sample.md         ← 示例 Cheat Sheet（Markdown）
│       └── sample.tex        ← 生成的输出（XeLaTeX）
```

---

## MD 转 PDF：输出规格(紧凑)

| 参数 | 值 |
|------|-----|
| 纸张 | A4 横向，四边各 3mm 边距 |
| 字体 | Verdana（无衬线），**全文统一 5pt** |
| 分栏 | 默认 7 栏（通过 `--columns` 可调）|
| 行距 | 与字号相同（5pt）|
| 配色 | `##` 中蓝 / `###` 暗红 / `####` 深橙 / **粗体** 深绿 / `` `代码` `` 暗紫 |
| 表格（≤3 列）| 固定比例 `p{...}` 列宽，自动换行 |
| 表格（≥4 列）| 在 `\end{multicols}` 后用 `table*` 并排 |
| 公式 | `$$...$$` → `\[...\]` 直接放在正文栏内 |
| 中文支持 | 通过 `ctex` 宏包 |

完整说明见 [`md2tex/README.md`](./md2tex/)。

---

## 使用示例

```bash
# 基本转换
python md2tex/md2tex.py my_cheatsheet.md -o my_cheatsheet.tex

# 自定义栏数
python md2tex/md2tex.py my_cheatsheet.md -o out.tex --columns 5

# 试用附带的示例
python md2tex/md2tex.py md2tex/examples/sample.md -o sample.tex
```

---

## 许可证

[MIT](./LICENSE)
