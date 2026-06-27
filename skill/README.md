# exam-review Skill

An AI agent skill that generates structured exam review materials from lecture PDFs and homework files.

## Features

- **Per-lecture explanations** — narrative walkthroughs in Chinese with English terminology, beginner-friendly
- **Full English Cheat Sheet** — exam-ready, concise, covers every knowledge point
- **No gaps** — every concept, formula, definition, condition, and special case from the source materials is included
- **Flexible coverage** — you can mark specific topics as "important" or "unimportant"; the skill adjusts depth accordingly
- **LaTeX export** — after reviewing the Cheat Sheet, ask the agent to convert it to a printable PDF via `md2tex/`

## Installation

Tell your AI agent:

> *"Download and install this skill: https://github.com/SY-TANG-ai/exam-review"*

Most AI agents can download and install a skill from a GitHub URL automatically.

### Manual install (if your agent doesn't support auto-install)

Clone the repo and place the `skill/` directory into your agent's skills folder.

### Verification

After installation, say to your agent:

> *"帮我复习"*

If the skill loads correctly, it will begin by asking for your lecture files.

## How to trigger

Say any of the following to your agent:

- *"帮我复习这门课"* / *"Help me review this course"*
- *"生成考试复习资料"* / *"Generate exam review materials"*
- *"整理一下这些课件"* / *"Organize these lecture slides"*
- *"期末考试"* / *"Final exam prep"*
- *"期中复习"* / *"Midterm review"*

## Workflow

1. **You provide** lecture files (PDF/PPT), homework files, and exam constraints
2. **The skill produces** per-lecture explanations (Chinese narrative + English terms)
3. **The skill produces** `Cheat Sheet.md` — a full English reference, no examples, no filler
4. **(Optional)** You say *"转成 LaTeX"* or *"输出 PDF"*
5. The skill will ask: **"紧凑排版 (tight) 还是宽松排版 (loose)?"**
6. **Tight** → runs `md2tex-tight.py` (A4 landscape, 5pt, 7 cols — maximum density)
7. **Loose** → runs `md2tex-loose.py` (A4 portrait, 7pt, 2 cols — comfortable reading)
8. **You upload** the `.tex` file to Overleaf (XeLaTeX) and compile

> 💡 After step 3, you can also delete topics from the Cheat Sheet that you already know, then ask for the LaTeX export — your custom trimmed version is what gets printed.

## Design notes

See [`design-notes.md`](./design-notes.md) for the full rationale behind:

- Why per-lecture explanations mix Chinese and English
- Why the Cheat Sheet is all English
- Why knowledge points are embedded in chapters rather than separate files
- What each template section is for
