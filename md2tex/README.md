# md2tex — Markdown → XeLaTeX

Convert Markdown into a single self-contained `.tex` file. Upload to Overleaf (XeLaTeX) and compile.

Three layout modes:

| Mode | File | Layout | Best for |
|------|------|--------|----------|
| **Read** | `md2tex-read.py` | A4 portrait, 1.3cm margins, **8pt**, 2 columns, all tables in place | 逐课讲解 / any long-form Markdown you want to read and annotate on paper |
| **Tight** | `md2tex-tight.py` | A4 landscape, 3mm margins, **5pt**, 7 columns, >=4 cols = large table | Cheat Sheet — maximum content per page, exam hall |
| **Loose** | `md2tex-loose.py` | A4 portrait, 1cm margins, **7pt**, 2 columns, >=6 cols = large table | Cheat Sheet — comfortable reading, revision desk |

---

## Quick start

### Reading layout (逐课讲解)

```bash
python md2tex-read.py "第01课-xxx.md" -o "第01课-xxx.tex"
```

### Tight layout (compact Cheat Sheet)

```bash
python md2tex-tight.py "Cheat Sheet.md" -o "cheatsheet.tex"
```

### Loose layout (generous Cheat Sheet)

```bash
python md2tex-loose.py "Cheat Sheet.md" -o "cheatsheet.tex"
```

### Custom columns

```bash
python md2tex-read.py  input.md -o out.tex -c 1 --fontsize 9
python md2tex-tight.py input.md -o out.tex --columns 5    # override default 7
python md2tex-loose.py input.md -o out.tex --columns 3    # override default 2
```

---

## Read mode specifics

`md2tex-read.py` is tuned for revision notes, not cheat sheets:

| Feature | Behaviour |
|---------|-----------|
| Tables | Stay in place (never moved to the end); column widths auto-sized by content, font auto-shrinks for 4+ columns |
| Symbols | `✓ ✗ ✅ ❌ ⭐ ★ ⇒ ⟹ ⟺ ● ◉ ◇ ◆ ≤ ≥ ≠ ≈ ∞` → math-safe forms |
| Decorative marks | `📌 🔴 🟡 🟢 🔵 ⚪ 🎯 ❗` stripped (heading/text still carries the meaning) |
| `⚠️` in a heading | removed; elsewhere → `注意：` |
| `==highlight==` | delimiters removed |
| Box drawing in code blocks | `─ │ ├ └` → ASCII `- \| +` |
| Obsidian callouts | `> [!note] text` → quote text only |
| Code | fenced and inline code are protected — `==`, `*`, `_`, `~`, `^` stay literal |
| Fonts | Verdana → DejaVu Sans → TeX Gyre Heros fallback (compiles on Overleaf without Verdana) |

---

## Comparison (cheat-sheet modes)

| Spec | Tight | Loose |
|------|-------|-------|
| Page | A4 landscape | A4 portrait |
| Margins | 3mm all around | 1cm all around |
| Columns | 7 default (configurable) | 2 default (configurable) |
| Font size | **5pt**, lineskip 5pt | **7pt**, lineskip 9pt |
| Table spacing | `\tabcolsep` 0.1pt, `\arraystretch` 0.9 | `\tabcolsep` 1pt, `\arraystretch` 1.0 |
| Paragraph gap | 0pt | 2pt |
| Large table threshold | >=4 columns | >=6 columns |
| List spacing | 0pt | 1pt itemsep, topsep |
| File | `md2tex-tight.py` | `md2tex-loose.py` |

### Shared across both

| Feature | Value |
|---------|-------|
| Font | Verdana (sans-serif), uniform size |
| Colors | Blue `##` / Red `###` / Orange `####` / Green **bold** / Purple `` `code` `` |
| Math | `$$...$$` → `\[...\]`, inline within columns |
| Chinese | Supported via `ctex` package |
| Arrows | Unicode `→` → `$\rightarrow$`, ASCII `->` → `$\rightarrow$` |
| Subscripts | Unicode `₀₁₂...` → `\textsubscript{0}` etc. |
| Large tables | Placed after `\end{multicols}` in `table*` + `minipage`, side-by-side |

---

## Usage with exam-review skill

The skill calls these itself when you ask for a printable version:

| You say | The agent runs |
|---------|---------------|
| *"把第01课转成 PDF / 打印版"* / *"转成 LaTeX"*（对逐课讲解或你自己的 .md） | `md2tex-read.py` |
| *"Cheat Sheet 转成 LaTeX"* / *"输出 PDF"* | `md2tex-tight.py` (tight, default) |
| *"Cheat Sheet 转成宽松版"* / *"宽松排版"* | `md2tex-loose.py` (loose) |

---

## In Overleaf

1. New project → **Blank Project**
2. Upload the `.tex` file
3. Menu → **Compiler** → **XeLaTeX**
4. **Recompile**

Adjust layout parameters directly in the `.tex` file.

---

## Requirements

- Python 3.7+
- No third-party Python packages
- Overleaf: XeLaTeX + `ctex` (built-in)

---

## Known limitations

- Read mode strips decorative emoji; cheat-sheet modes keep them as-is
- Code blocks (`` ``` ``) are monochrome, no syntax highlighting
- Nested lists: one level only
- Anchor links `[text](#ref)` → text only (link dropped)
