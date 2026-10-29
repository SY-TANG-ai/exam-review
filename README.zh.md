# exam-review

**生成结构化考试复习资料 + 转换为可打印的 LaTeX PDF。**

一个 AI 助手技能 + 一个 Markdown → LaTeX 转换器。把课件和作业变成可直接用于考试的复习材料：逐课讲解、全英文 Cheat Sheet，以及可选的打印版 PDF。

---

## 组件

| 组件 | 功能 |
|------|------|
| [`skill/`](./skill/) | AI 助手技能——读取课件 PDF 和作业文件，生成逐课讲解 + 全英文 Cheat Sheet |
| [`md2tex/`](./md2tex/) | Python CLI——将 Markdown 转为单个自包含的 `.tex`，在 Overleaf（XeLaTeX）编译。三种版式：阅读版、紧凑版、宽松版 |

两者可以**配合使用**——skill 产出 Cheat Sheet，md2tex 打印它。每个组件也可独立使用。

---

## 快速开始

### 1. 安装技能

直接告诉你的 AI 助手：

> *"下载并安装这个 skill：https://github.com/SY-TANG-ai/exam-review"*

多数助手能从 GitHub URL 自动安装。若不支持，克隆本仓库并把 `skill/` 目录放进你的助手 skills 文件夹。

验证：对助手说 *"帮我复习"*——技能应开始询问你的课件文件。

### 2. 触发方式

以下任一句均可：

- *"帮我复习这门课"* / *"Help me review this course"*
- *"生成考试复习资料"* / *"Generate exam review materials"*
- *"整理一下这些课件"* / *"Organize these lecture slides"*
- *"期末考试"* / *"Final exam prep"*
- *"期中复习"* / *"Midterm review"*

### 3. 你会得到什么

| 阶段 | 产出 |
|------|------|
| 逐课讲解 | 每章一篇叙述式讲解——默认中文叙述 + 英文术语，也可要求全英文。面向初学者，**完整覆盖**材料中的每个概念、公式、定义、条件与特例 |
| `Cheat Sheet.md` | 全英文参考。不含例子、不含废话——只有知识点、定义、公式、条件 |
| 打印版 PDF | 按需通过 `md2tex/` 生成 |

### 4. 转换为 PDF（可选，不需要 AI 助手）

```bash
# Cheat Sheet（紧凑，考场用）
python md2tex/md2tex-tight.py "Cheat Sheet.md" -o "cheatsheet.tex"

# 逐课讲解 / 其他长 Markdown（阅读版，可打印可批注）
python md2tex/md2tex-read.py "第01课-xxx.md" -o "第01课-xxx.tex"
```

将 `.tex` 上传到 [Overleaf](https://www.overleaf.com)，编译器选 **XeLaTeX**，直接编译。

---

## 使用技能：完整流程

1. **你提供** 课件文件（PDF/PPT）、作业文件、考试约束
2. **技能会询问** 复习范围：哪些章节、考试日期、哪些内容你认为是重要 / 不重要的、以及逐课讲解要中文还是全英文
3. **产出** 逐课讲解——每章一个文件
4. **产出** `Cheat Sheet.md`——基于上述讲解整理的全英文参考
5. **（可选）** 要求打印版。Cheat Sheet 可选 **紧凑版**（A4 横向，5pt，7 栏——密度最大）或 **宽松版**（A4 竖向，7pt，2 栏——好读）；逐课讲解则用 `md2tex-read.py`
6. **你上传** `.tex` 到 Overleaf（XeLaTeX）编译

> 第 4 步之后，你可以把已经掌握的内容从 Cheat Sheet 中删掉再导出——打印出来的就是你的精简版。

### 灵活覆盖范围

可将任意内容标记为"不重要"（case study、整章、某些作业题）。技能仍会保留该知识点本身以保证 Cheat Sheet 完整，但跳过其中的例题与推导。

### 语言模式

- **默认**——中文叙述，术语保留英文
- **全英文**——说"全英文讲解"（或"两种语言都要"即中英各一份）。术语严格采用你英文课件中的原词。用意：培养用考试语言思考的习惯。

---

## 项目结构

```
exam-review/
├── README.md                 ← 英文版
├── README.zh.md              ← 本文件（中文版）
├── LICENSE                   ← MIT
├── .gitignore
├── skill/                    ← AI 助手 skill
│   └── SKILL.md              ← 主 skill 文件（AI 执行指令）
├── md2tex/                   ← 转换工具
│   ├── md2tex-read.py        ← 阅读版式（逐课讲解，A4）
│   ├── md2tex-tight.py       ← 紧凑版式（Cheat Sheet，A4 横向）
│   ├── md2tex-loose.py       ← 宽松版式（Cheat Sheet，A4 竖向）
│   ├── README.md             ← 工具用法、规格与已知限制
│   └── examples/
│       ├── sample.md         ← 示例 Cheat Sheet（Markdown）
│       └── sample.tex        ← 生成的输出（XeLaTeX）
```

---

## MD 转 PDF：输出规格（紧凑）

下表为 **紧凑版** Cheat Sheet（`md2tex-tight.py`）的默认值。`md2tex-read.py` 以可读性优先：A4 竖向、8pt、2 栏、表格就地排版。

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
# Cheat Sheet——紧凑版（默认）或宽松版
python md2tex/md2tex-tight.py my_cheatsheet.md -o my_cheatsheet.tex
python md2tex/md2tex-loose.py my_cheatsheet.md -o my_cheatsheet.tex

# 逐课讲解 / 其他长 Markdown——阅读版
python md2tex/md2tex-read.py my_notes.md -o my_notes.tex

# 自定义栏数
python md2tex/md2tex-tight.py my_cheatsheet.md -o out.tex --columns 5

# 试用附带的示例
python md2tex/md2tex-tight.py md2tex/examples/sample.md -o sample.tex
```

---

## 许可证

[MIT](./LICENSE)
