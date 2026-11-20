# Soundbendor-Lab-Template

Soundbendor Lab templates for presentations and posters, in Oregon State University colors.

| Folder | What | Build |
|---|---|---|
| [`slides/`](slides/) | Beamer (Metropolis) style for research talks and course slides | `latexmk -pdf research.tex` |
| [`poster/`](poster/) | `beamerposter` research poster (A0 or 4'×3') | `latexmk -lualatex poster.tex` |

Example output: [research talk](slides/research.pdf) · [course lecture](slides/teaching.pdf) · [poster](poster/poster.pdf)

## Slides in one minute

```latex
\documentclass[10pt,aspectratio=169,xcolor=table]{beamer}
\usepackage[campus=corvallis, mode=research]{sty/osu}   % or cascades / teaching
\title{...} \author{...}
\begin{document}
\maketitle
...
\end{document}
```

Start from [`slides/main.tex`](slides/main.tex). [`slides/README.md`](slides/README.md) lists every option, command and box.

## Shared colors

Both templates use the same OSU palette file: `slides/sty/osucolors.sty` and
`poster/style/osucolors.sty` are identical copies. If you change a color, change both.

## Requirements

TeX Live 2022 or newer, or Overleaf. On the OSU HPC, the default TeX Live (2018) is too old:

```bash
export PATH=/usr/local/apps/latex/2022/bin/x86_64-linux:$PATH
```

## Notes

- The teaching example has a button that plays *Loop Invariants* by J. P. Dougherty.
  The audio file is not in this repository. Put `loop-invariants.mp3` in `slides/media/` to use it.
- The OSU and OSU-Cascades logos are university marks; follow OSU brand guidelines when using them.
