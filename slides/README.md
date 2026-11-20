# OSU / Soundbendor Beamer Template (v2)

A Metropolis-based Beamer style for Oregon State University research talks and
course slides. All styling lives in `sty/osu.sty`, so a deck only sets options
and writes content.

```
sty/osu.sty          the style (options, logos, commands, boxes, code, pseudocode)
sty/osucolors.sty    OSU brand palette (can be shared with the poster template)
sty/logo/            OSU, OSU-Cascades and Soundbendor logos
main.tex             minimal starter deck
research.tex         research-mode example (Corvallis, with lab logo)
teaching.tex         teaching-mode example (Cascades, with course metadata)
refs.bib             example bibliography for research.tex (the entries are jokes)
```

## Build

```bash
latexmk -pdf research.tex
```

On the OSU HPC, the system TeX Live (2018) is too old. Put TeX Live 2022 first on your PATH:

```bash
export PATH=/usr/local/apps/latex/2022/bin/x86_64-linux:$PATH
```

On Overleaf, upload the folder and compile with pdfLaTeX. XeLaTeX and LuaLaTeX also work.

## Minimal deck

```latex
\documentclass[10pt,aspectratio=169,xcolor=table]{beamer}
\usepackage[campus=corvallis, mode=research]{sty/osu}
\title{...} \subtitle{...} \author{...} \date{\today}
\begin{document}
\maketitle
...
\end{document}
```

Teaching mode uses course metadata in place of subtitle, author and date:

```latex
\usepackage[campus=cascades, mode=teaching]{sty/osu}
\title{Loops}
\course{CS 101}{Introduction to Computer Science}   % subtitle + footer
\instructor{Prof. Benny Beaver}                     % author
\courseterm{Fall 2026}                              % date
```

`xcolor=table` is only needed for `\rowcolors`. `aspectratio=169` is optional, and every logo position works at 4:3 too.

## Options

| Option | Values (default first) | Effect |
|---|---|---|
| `campus` | `corvallis`, `cascades` | Which OSU logo appears on the title page and in the title bar |
| `mode` | `research`, `teaching` | Title-page layout and frame decorations |
| `lab` | `auto`, `true`, `false` | Soundbendor logo; `auto` shows it in research mode only |
| `framelogo` | `true`, `false` | OSU logo in the frame-title bar |
| `footer` | `true`, `false` | Teaching: "CS 101 · Title" bottom-left |
| `bullets` | `square`, `circle` | Itemize bullet family |
| `enumerate` | `circle`, `arabic`, `Alph`, `alph`, `roman`, `Roman` | Default enumerate label (`circle` = numbers in filled Beaver circles, as in v1). Mini-templates like `[a)]` stay plain |
| `font` | `fira`, `default` | Fira Sans (the intended Metropolis font) or Computer Modern Sans |
| `code` | `Python`, `C++`, `Java`, any listings language | Default code language |
| `todo` | `true`, `false` | Show or hide `\todo{}` notes |
| `logopath` | `sty/logo/` | Where the logo files live |
| `legacy` | `false`, `true` | Also define the old names `\defn`, `\key`, `\sign` |
| `metropolis` | `{...}` | Passed to Metropolis, e.g. `metropolis={progressbar=frametitle}` |

The v1 commands `\campus{Cascades}` and `\presentmode{teaching}` still work in the preamble.

### What each mode shows

| | Research | Teaching |
|---|---|---|
| Title page | OSU logo top-right, Soundbendor bottom-centre | OSU logo bottom-right |
| Frames with a title | OSU logo in the title bar, Soundbendor bottom-left | OSU logo in the title bar, course ID bottom-left |

`\researchonly{...}` and `\teachingonly{...}` let one slide file serve both decks.

## Text highlighting

| Command | Style | Use for | v1 name |
|---|---|---|---|
| `\term{}` | Beaver, bold | a term being defined | `\defn` |
| `\keyword{}` | Pine Stand, slanted | technical vocabulary | `\keyword` |
| `\keyidea{}` | Stratosphere, bold | the one idea to remember | `\key` |
| `\var{}` | Pine Stand, italic | a variable named in prose | `\var` |
| `\sym{}` | Pine Stand, bold | symbols, operators, signs | `\sign` |
| `\mat{}` | Hop Bine | matrices, tensors | `\mat` |
| `\stress{}` | Beaver, slanted | spoken emphasis | `\mark` (see below) |
| `\code{}` | Stratosphere, mono | short inline code | new |
| `\hl{}` | Candela background | highlighter pen | new |
| `\badge[color]{}` | small filled tag | "new", "accepted", ... | new |
| `\todo{}` | yellow note | drafts; `todo=false` hides them all | new |

Overlay-aware helpers:
- `\sout<2->{text}` strikes the text through from slide 2.
- `\blank<2->{answer}` shows a blank line until slide 2, then the answer.
- `\correctchoice<2->{text}` marks the right answer in a `choices` list from slide 2.

## Callout boxes

Each box takes an optional title and an optional overlay spec, for example
`\begin{answer}<2->[Solution] ... \end{answer}`.

| Teaching | Research | General |
|---|---|---|
| `objectives` | `rq` (numbered RQ1, RQ2, ...) | `aside` |
| `question` | `hypothesis` (numbered H1, H2, ...) | `funfact` |
| `answer` | `contribution` | `roundbox` (plain frame, from v1) |
| `tryit` | `finding` | `bigquote[attribution]` |
| `pitfall` | `limitation` | |
| `takeaway` | `futurework` | |
| `quiz` + `choices`, `\choice`, `\correctchoice` | | |

To make your own box: `\osunewbox{envname}{color}{\faIcon}{Default Title}`.

The standard Beamer blocks (`block`, `alertblock`, `exampleblock`, `definition`,
`theorem`, `example`, `proof`) are drawn as rounded boxes with a thin grey outline
and a soft shadow, so they stay visible with `metropolis={block=transparent}` too.

### Confusion matrix

```latex
\begin{confusionmatrix}{Cat (actual)}{CatNet (predicted)}{stays, leaves, glares}
  \cmrow{stays}{412, 31, 7}
  \cmrow{leaves}{38, 519, 2}
  \cmrow{glares}{11, 4, 976}
\end{confusionmatrix}
```

Booktabs rules, bold header row, and the correct (diagonal) cells bold on a light Pine Stand background.
Other tables: use booktabs (`\toprule`, `\midrule`, `\bottomrule`) with a bold header row, as in the examples.

## Other helpers

| Command | What it does |
|---|---|
| `\bignumber[color]{73\%}{caption}` | Large statistic callout |
| `\slidecite{Author, 2025}` | Small source note, bottom-right |
| `\qrlink[size]{url}` | QR code that is also a link |
| `\fullbleed{img/file}` | Full-page image (use in a `[plain]` frame) |
| `\mediabutton{https://...}{Play}` or `\mediabutton{media/file.mp3}{Play}` | Play button. A web link works in every viewer. A local file must sit next to the PDF and needs a desktop viewer (Acrobat, Okular, Skim); browser viewers block file links |
| `\codefile[opts]{file.py}` | Include a source file in the code box |
| `\osupalette`, `\colorswatch{color}{Label}` | Palette reference slide |
| `\node[osunode=pinestand]`, `\draw[osuarrow]` | TikZ styles for diagrams |
| pgfplots | Plots and bar charts use the OSU color cycle automatically |

Math: `\argmin`, `\argmax`, `\argminl`, `\argmaxl` (limits forced inline),
`\E`, `\Var`, `\Cov`, `\R`, `\N`, `\Z`, `\abs{}`, `\norm{}`, `A\T` (transpose),
`\sgn`, `\softmax`, and `\given` (use it inside `\left( \right)`).

## Code and pseudocode

- Every `lstlisting` is drawn in the grey rounded box automatically. Its frame needs `[fragile]`.
- `[style=numbered]` adds line numbers.
- Put `\end{lstlisting}` at column 0. If it is indented, the box gets an extra blank line.
- To escape to LaTeX inside code, use `(* ... *)`. v1 used `@`, which broke Python decorators.
- For pseudocode, use algorithm2e with `\begin{algorithm}[H]`. The algorithm title (`\caption{...}`) is bold Beaver small caps, keywords are Stratosphere, comments (`\tcp`) are Pine Stand, and function names from `\SetKwFunction{FTrain}{Train}` are Beaver small caps. The v1 `\Input`, `\Output`, `\Local`, `\Fn`, `\Unless` and `\Repeat` are kept.

## Bibliography

Load biblatex in the deck as usual; IEEE style is recommended. The style detects biblatex and formats:

- in-text citations: small, bold, Beaver, e.g. **[1]**
- reference list: numbered [1], [2], ... with a little space between entries;
  authors bold, titles in Stratosphere (IEEE's quotation marks kept), DOIs and URLs as links

```latex
\usepackage[style=ieee]{biblatex}
\addbibresource{refs.bib}
...
\begin{frame}[allowframebreaks]{References}\printbibliography[heading=none]\end{frame}
```

## Changes from v1 (`sty/osu.tex`)

- It is now a package with options (`\usepackage[...]{sty/osu}`) instead of `\input{sty/osu.tex}`.
- The old `\ifdef{\titleResearch}` block in `main.tex` is gone. `\course`, `\instructor` and `\courseterm` replace it.
- **Bug fixes:**
  - The forked file redefined `\mark`, which is a TeX primitive used for page marks. That command is now `\stress`.
  - `algorithm2e` was loaded twice with different options.
  - `\renewcommand{\theenumi}{\Alph...}` never took effect, because beamer resets it at `\begin{document}`. The v1 `items[circle]` setting is what actually styled enumerate (numbers in circles); that is the default again. Use `enumerate=Alph` for letters.
  - The `circlealt` bullets could never be selected; now `bullets=circle` selects them.
  - The second `\makeatletter` should have been `\makeatother`.
- **Removed:**
  - `media9` and the `APlayer.swf` Flash player. Flash no longer runs in any PDF viewer; `\mediabutton` replaces it.
  - The `subfigure` package, which is obsolete. Use `subcaption` with `\begin{subfigure}{0.48\linewidth}`.
  - `xpatch`, `color`, the duplicate `ragged2e`, and the `\lst@CommentDM@L` internal patch.
- `mdframed` is replaced by `tcolorbox`. `roundbox` and the code box keep the same look.
- New:
  - `\alert` is Beaver (v1 used the lighter Solar Flare).
  - All-white Cascades logo for the orange title bar (`OSU-cascades-allwhite.png`).
  - Fira Sans under pdfLaTeX.
  - `paddletail` and `bucktooth` colors.
  - OSU pgfplots color cycle.
  - Unnumbered "References" continuation frames.

## Poster template

The poster is a separate project in `../poster/` (beamerposter: different
geometry, headline, blocks and font scaling, so merging would add little). It now shares
the palette: `style/osucolors.sty` there is an identical copy of `sty/osucolors.sty` here,
so if you change a color, copy the file to both places. Its theme file is `style/osuposter.sty`
(renamed from `osu.sty`), and `style/colorosu.sty` holds only the poster's beamer color theme.
Build it with `latexmk -lualatex poster.tex`.
