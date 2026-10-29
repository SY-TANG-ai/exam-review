# exam-review

**Generate structured exam review materials + convert them to printable LaTeX PDF.**

An AI agent skill plus a Markdown→LaTeX converter. Turns lecture slides and homework into exam-ready review material: per-lecture walkthroughs, a full English Cheat Sheet, and optionally a printable PDF.

---

## Components

| Component | What it does |
|-----------|-------------|
| [`skill/`](./skill/) | AI agent skill — reads lecture PDFs and HW files, generates per-lecture explanations plus a full English Cheat Sheet |
| [`md2tex/`](./md2tex/) | Python CLI — converts Markdown into a self-contained `.tex` for Overleaf (XeLaTeX). Three layouts: reading, tight, loose |

They work **together** — the skill produces the Cheat Sheet, `md2tex` prints it. Each is also usable alone.

---

## Quick Start

### 1. Install the skill

Tell your AI agent:

> *"Download and install this skill: https://github.com/SY-TANG-ai/exam-review"*

Most agents can install from a GitHub URL automatically. If yours can't, clone the repo and place the `skill/` directory into your agent's skills folder.

To verify, say *"帮我复习"* — the skill should start by asking for your lecture files.

### 2. Trigger it

Any of these will do:

- *"帮我复习这门课"* / *"Help me review this course"*
- *"生成考试复习资料"* / *"Generate exam review materials"*
- *"整理一下这些课件"* / *"Organize these lecture slides"*
- *"期末考试"* / *"Final exam prep"*
- *"期中复习"* / *"Midterm review"*

### 3. What you get

| Stage | Output |
|-------|--------|
| Per-lecture explanations | Narrative walkthrough per chapter — Chinese with English terminology by default, or all-English if you ask. Beginner-friendly, covers **every** concept, formula, definition, condition and special case from the source |
| `Cheat Sheet.md` | Full English reference. No examples, no filler — knowledge points, definitions, formulas, conditions only |
| Printable PDF | On request, via `md2tex/` |

### 4. Convert to PDF (optional, no AI agent needed)

```bash
# Cheat Sheet (compact, for the exam hall)
python md2tex/md2tex-tight.py "Cheat Sheet.md" -o "cheatsheet.tex"

# Lecture notes / any long Markdown (readable, for annotation)
python md2tex/md2tex-read.py "第01课-xxx.md" -o "第01课-xxx.tex"
```

Upload the `.tex` to [Overleaf](https://www.overleaf.com), set the compiler to **XeLaTeX**, compile.

---

## Using the skill: full workflow

1. **You provide** lecture files (PDF/PPT), homework files, and exam constraints
2. **The skill asks** about scope: which lectures, exam date, topics you consider important / unimportant, and whether you want the explanations in Chinese or all English
3. **It produces** per-lecture explanations — one file per chapter
4. **It produces** `Cheat Sheet.md` — a full English reference built from those explanations
5. **(Optional)** Ask for a printable version. For Cheat Sheets you pick **tight** (A4 landscape, 5pt, 7 columns — maximum density) or **loose** (A4 portrait, 7pt, 2 columns — comfortable). For lecture notes, `md2tex-read.py` is used
6. **You upload** the `.tex` to Overleaf (XeLaTeX) and compile

> After step 4 you can delete topics you already know from the Cheat Sheet, then export — your trimmed version is what gets printed.

### Flexible coverage

Mark any topic as "unimportant" (case studies, a whole lecture, certain homework questions). The skill still keeps the knowledge point itself so the Cheat Sheet stays complete, but skips the worked examples and derivations.

### Language modes

- **Default** — Chinese narrative, English terminology
- **All-English** — say "全英文讲解" (or "两种语言都要" for both). Uses the exact wording from your English course materials. Purpose: build the habit of thinking in the exam language.

---

## Project Structure

```
exam-review/
├── README.md                 ← This file
├── README.zh.md              ← 中文版
├── LICENSE                   ← MIT
├── .gitignore
├── skill/                    ← AI agent skill
│   └── SKILL.md              ← Main skill file (instructions for the AI)
├── md2tex/                   ← Conversion tool
│   ├── md2tex-read.py        ← Reading layout (lecture notes, A4)
│   ├── md2tex-tight.py       ← Tight layout (Cheat Sheet, A4 landscape)
│   ├── md2tex-loose.py       ← Loose layout (Cheat Sheet, A4 portrait)
│   ├── README.md             ← Tool usage, specs & known limitations
│   └── examples/
│       ├── sample.md         ← Example Cheat Sheet (Markdown)
│       └── sample.tex        ← Generated output (XeLaTeX)
```

---

## MD to PDF: Output specs

Defaults below are for the **tight** Cheat Sheet layout (`md2tex-tight.py`). `md2tex-read.py` favours readability instead: A4 portrait, 8pt, 2 columns, tables kept in place.

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
# Cheat Sheet — tight (default) or loose
python md2tex/md2tex-tight.py my_cheatsheet.md -o my_cheatsheet.tex
python md2tex/md2tex-loose.py my_cheatsheet.md -o my_cheatsheet.tex

# Lecture notes / any long Markdown — reading layout
python md2tex/md2tex-read.py my_notes.md -o my_notes.tex

# Custom column count
python md2tex/md2tex-tight.py my_cheatsheet.md -o out.tex --columns 5

# Try the included sample
python md2tex/md2tex-tight.py md2tex/examples/sample.md -o sample.tex
```

---

## License

[MIT](./LICENSE)
