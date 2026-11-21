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
| `font` | `fira`, `default` | Fira Sans Light with Regular for bold (the Metropolis look, same under pdfLaTeX, XeLaTeX and LuaLaTeX) or Computer Modern Sans |
| `code` | `Python`, `C++`, `Java`, any listings language | Default code language |
| `todo` | `true`, `false` | Show or hide `\todo{}` notes |
| `logopath` | `sty/logo/` | Where the logo files live |
| `legacy` | `false`, `true` | Also define the old names `\defn`, `\key`, `\sign` |
| `metropolis` | `{...}` | Passed to Metropolis, e.g. `metropolis={progressbar=frametitle}` |
| `lang` | `en-US`, any language tag | Document language written into the PDF, so screen readers use the right voice |
| `linkline` | `true`, `false` | Underline web links, so they are not marked by colour alone |
| `blockskip` | `4pt`, any length | Space above and below every box, block and code listing. Teaching decks read better with more air, e.g. `blockskip=12pt` |
| `bib` | file name(s) | Load biblatex with this `.bib` file (see [Bibliography](#bibliography)) |
| `cite` | `numeric`, `hidden`, `inline` | Citation style: [1], none, or (Author, Year) |
| `itemsep` | `4pt`, any length | Space between list items; nested lists get half. `itemsep=0pt` gives the tight v1 lists |

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
| `\qrlink[size]{url}` | QR code that is also a link, with the address printed underneath |
| `\fullbleed{img/file}` | Full-page image (use in a `[plain]` frame) |
| `\figalt{description}{\includegraphics{...}}` | Image with alternative text for screen readers |
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

Give the `.bib` file and a citation style as options. The style loads biblatex for you:

```latex
\usepackage[campus=corvallis, mode=research, bib=refs.bib, cite=numeric]{sty/osu}
...
\cite{whiskers2024attention}      % in the slides
...
\referencesframe                  % last slide; or \referencesframe[Further Reading]
```

| `cite=` | On the slide | Reference list |
|---|---|---|
| `numeric` (default) | **[1]** in small bold Beaver | numbered [1], [2], ... in citation order (IEEE) |
| `hidden` | nothing (the space before `\cite` is removed too) | alphabetical by author, no numbers |
| `inline` | (Whiskers et al., 2024) in small Beaver | alphabetical by author (author–year) |

To switch styles, change the option and rebuild; the slides keep using `\cite`.
In `hidden` mode every cited entry still appears in the reference list. That
suits talks where citations would clutter the slides. `\parencite`, `\textcite`
and the other biblatex citation commands work in every mode.

In every mode the list has a little space between entries, bold authors, titles
in Stratosphere (with their quotation marks), and DOIs and URLs as links.
`bib` takes several files as well: `bib={refs.bib,more.bib}`.
`\referencesframe` breaks onto extra slides when the list is long.

If you load biblatex yourself instead (`\usepackage[style=...]{biblatex}`), the
formatting still applies; leave out `bib`.

## Accessibility

Beamer cannot produce a fully tagged PDF, so a deck cannot meet WCAG 2.1 AA on
its own. The style handles what it can:

- **Contrast (1.4.3, 1.4.11).** All text colours reach 4.5:1 on white: the
  title bar is full Beaver (4.6:1), and footnotes, `\slidecite`, quote
  attributions and the teaching footer use `osunote` (black at 60 %, 5.7:1).
  `\mat`, code strings and box titles use darker versions of their palette colours.
  Light palette colours (Luminance, Candela, Solar Flare, Seafoam, ...) are
  for fills, never for text.
- **Not colour alone (1.4.1).** Web links are underlined. Plot lines differ
  by marker and dash pattern as well as colour. `\correctchoice` adds a check
  mark, and `\sout` strikes text through.
- **PDF metadata (2.4.2, 3.1.1).** The title and author come from `\title`
  and `\author`, the language from `lang`, and viewers show the title instead
  of the file name.
- **Readable text.** Under pdfLaTeX every glyph is mapped to Unicode, so text
  copies, searches and is read aloud correctly. Box icons, frame logos and
  the quote mark are marked as decorative, so screen readers skip them. The
  `\correctchoice` check is read as "(correct)", and the title-page logos have
  alternative text.
- **Size.** Nothing the style places is smaller than `\scriptsize`, including
  references, the footer and `\slidecite`.

What each deck still needs:

- Wrap figures in `\figalt{what it shows}{\includegraphics{...}}`.
- Give audio and video a text version. The song slide shows its lyrics.
- Don't use colour as the only signal in your own figures. Label bars directly
  (`nodes near coords`) or add a legend with patterns.
- Keep text at `\small` or larger where you can, and don't put text on light fills.
- Publish an overlay-free copy for screen-reader users. Overlays repeat each
  frame several times, and the `handout` option collapses them to one page per frame:
  `\documentclass[handout,10pt,aspectratio=169,xcolor=table]{beamer}`.
- For a fully accessible version, share the slide text as HTML or Markdown
  next to the PDF. Real PDF tagging (`\DocumentMetadata{tagging=on}`) needs a
  newer LaTeX than TeX Live 2022, and Beamer support for it is still incomplete.

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
  - Fira Sans Light under pdfLaTeX too (v1 fell back to Computer Modern Sans there), so every engine gives the same look.
  - `paddletail` and `bucktooth` colors.
  - OSU pgfplots color cycle.
  - Unnumbered "References" continuation frames.
  - List items have 4pt between them (v1: 0pt), set with `itemsep`.
  - Accessibility: contrast-safe text colours, underlined links, PDF language and title, Unicode text, spoken icon labels, `\figalt`.

## Poster template

The poster is a separate project in `../poster/` (beamerposter: different
geometry, headline, blocks and font scaling, so merging would add little). It now shares
the palette: `style/osucolors.sty` there is an identical copy of `sty/osucolors.sty` here,
so if you change a color, copy the file to both places. Its theme file is `style/osuposter.sty`
(renamed from `osu.sty`), and `style/colorosu.sty` holds only the poster's beamer color theme.
Build it with `latexmk -lualatex poster.tex`.
