# md2tex — Markdown Cheat Sheet → XeLaTeX

Convert a Markdown Cheat Sheet into a single self-contained `.tex` file. Upload to Overleaf (XeLaTeX) and compile.

Two layout modes are available:

| Mode | File | Layout | Best for |
|------|------|--------|----------|
| **Tight** | `md2tex-tight.py` | A4 landscape, 3mm margins, **5pt**, 7 columns, >=4 cols = large table | Maximum content per page, exam hall |
| **Loose** | `md2tex-loose.py` | A4 portrait, 1cm margins, **7pt**, 2 columns, >=6 cols = large table | Comfortable reading, revision desk |

---

## Quick start

### Tight layout (compact)

```bash
python md2tex-tight.py "Cheat Sheet.md" -o "cheatsheet.tex"
```

### Loose layout (generous)

```bash
python md2tex-loose.py "Cheat Sheet.md" -o "cheatsheet.tex"
```

### Custom columns

```bash
python md2tex-tight.py input.md -o out.tex --columns 5    # override default 7
python md2tex-loose.py input.md -o out.tex --columns 3    # override default 2
```

---

## Comparison

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

After the skill generates `Cheat Sheet.md`, tell your agent:

| You say | The agent runs |
|---------|---------------|
| *"转成 LaTeX"* / *"输出 PDF"* | `md2tex-tight.py` (tight, default) |
| *"转成宽松版 LaTeX"* / *"宽松排版"* | `md2tex-loose.py` (loose) |

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

- Emoji preserved as-is (XeLaTeX + Verdana usually renders them)
- Code blocks (`` ``` ``) are monochrome, no syntax highlighting
- Nested lists: one level only
- Anchor links `[text](#ref)` → text only (link dropped)
