# exam-review

**Generate structured exam review materials + convert to printable LaTeX PDF.**

A two-component toolkit for students who need to turn lecture slides and homework into organized, exam-ready review materials — and optionally print them as a compact Cheat Sheet.

---

## Components

| Component | What it does | Who it's for |
|-----------|-------------|--------------|
| [`skill/`](./skill/) | AI agent skill — reads lecture PDFs and HW files, generates per-lecture explanations in mixed Chinese/English, plus a full English Cheat Sheet | AI agent users |
| [`md2tex/`](./md2tex/) | Python CLI tool — converts any Markdown Cheat Sheet into a single self-contained `.tex` file that compiles on Overleaf (XeLaTeX) to a space-efficient PDF | Anyone with a Cheat Sheet in Markdown |

They work **together**: the skill produces the Cheat Sheet, and `md2tex` renders it. But each is also usable independently.

---

## Quick Start

### 1. Generate review materials (requires an AI agent)

Tell your AI agent:

> *"Download and install this skill: https://github.com/SY-TANG-ai/exam-review"*

Then say:

> *"Help me review this course, here are the lecture files"*

The skill produces: per-lecture explanations → `Cheat Sheet.md`

### 2. Convert to PDF (no AI agent needed)

```bash
python md2tex/md2tex.py "Cheat Sheet.md" -o "cheatsheet.tex"
```

Upload `cheatsheet.tex` to [Overleaf](https://www.overleaf.com), set compiler to **XeLaTeX**, and compile.

> Don't use an AI agent? `md2tex` works on any Markdown file — just write your own Cheat Sheet.

---

## Project Structure

```
exam-review/
├── README.md                 ← This file
├── README.zh.md              ← 中文版
├── LICENSE                   ← MIT
├── .gitignore
├── skill/                    ← AI agent skill
│   ├── SKILL.md              ← Main skill file (instructions for the AI)
│   ├── README.md             ← Skill installation & usage guide
│   └── design-notes.md       ← Design rationale & decisions
├── md2tex/                   ← Conversion tool
│   ├── md2tex.py             ← CLI script (Python 3)
│   ├── README.md             ← Tool usage, specs & known limitations
│   └── examples/
│       ├── sample.md         ← Example Cheat Sheet (Markdown)
│       └── sample.tex        ← Generated output (XeLaTeX)
```

---

## MD to PDF: Output specs

| Spec | Value |
|------|-------|
| Page | A4 landscape, 3mm margins all around |
| Font | Verdana (sans-serif), **5pt** uniform |
| Columns | 7 default (configurable via `--columns`) |
| Line height | Same as font size (5pt) |
| Colors | Blue `##` / Red `###` / Orange `####` / Green **bold** / Purple `` `code` `` |
| Tables (≤3 cols) | Fixed-ratio `p{...}` columns with auto line wrap |
| Tables (≥4 cols) | Side-by-side in `table*` after `\end{multicols}` |
| Math | `$$...$$` → `\[...\]` inline within columns |
| Chinese | Supported via `ctex` package |

For the full breakdown, see [`md2tex/README.md`](./md2tex/).

---

## Usage examples

```bash
# Basic conversion
python md2tex/md2tex.py my_cheatsheet.md -o my_cheatsheet.tex

# Custom column count
python md2tex/md2tex.py my_cheatsheet.md -o out.tex --columns 5

# Try the included sample
python md2tex/md2tex.py md2tex/examples/sample.md -o sample.tex
```

---

## License

[MIT](./LICENSE)
