# Mathematical LaTeX Template

Canonical LaTeX template for the chiral bar-cobar trilogy and successor
mathematics repositories.

The deployed source is `raeez-math-template.sty`. It centralizes the
shared monograph surface:

- Engine-aware Garamond typography:
  pdfLaTeX uses EB Garamond text plus `newtxmath`'s EB Garamond metrics;
  LuaLaTeX/XeLaTeX prefer a complete Adobe Garamond Premier Pro family
  for the paper-first body face, with EB Garamond as the complete open
  fallback and `Garamond-Math.otf` for math.
- `microtype` protrusion, expansion, small-cap tracking, and local kerning.
- A local OpenType protrusion table for LuaLaTeX/XeLaTeX, avoiding upstream
  glyph-name drift while preserving optical margin alignment for Garamond.
- Page geometry, display spacing, list spacing, footnote rhythm, widow
  control, and section/chapter hierarchy.
- Quiet running heads, restrained table rules, tuned table/footnote surfaces,
  and PDF title/author metadata.
- Theorem-like environments and stable `hyperref` anchors.
- Annals/archive claim-status switching.
- Unicode fallbacks for `pdflatex` fragments.
- `graphicx`, `float`, `amsrefs`, and legacy theorem-name compatibility
  for older mathematical-physics manuscripts.
- Shared TikZ styles for modular-graph visual calculus.

The consuming manuscript owns its title page, mathematical notation, and
chapter graph. The template owns typography and structural LaTeX.

Current consumers: `~/chiral-bar-cobar`, `~/cbc`,
`~/chiral-bar-cobar-vol2`, `~/chiral-bar-cobar-vol2-kz-bv-20260424`,
`~/chiral-bar-cobar-vol4`, `~/calabi-yau-quantum-groups`,
`~/igusa-cusp-form`, and `~/topological-strings`.

## Use

Symlink the style into the consuming repo root:

```sh
ln -s ../latex-template/raeez-math-template.sty raeez-math-template.sty
```

Then load it immediately after the document class:

```tex
\documentclass[11pt]{memoir}
\usepackage{raeez-math-template}
```

## Font Profiles

The default profile is `auto`. Under LuaLaTeX/XeLaTeX, `auto` chooses the
best complete print family available in this order: Garamond Premier Pro,
Adobe Garamond Pro, then EB Garamond. Under pdfLaTeX, all profiles use the
portable EB Garamond/newtxmath baseline.

Force a profile at package load:

```tex
\usepackage[premier]{raeez-math-template}     % Garamond Premier Pro
\usepackage[adobepro]{raeez-math-template}    % Adobe Garamond Pro
\usepackage[adobepropreview]{raeez-math-template} % Adobe upright preview
\usepackage[ebgaramond]{raeez-math-template}  % portable open fallback
```

Equivalent aliases are supported for driver scripts and experiments:
`font=premier`, `garamondpremier`, `adobepremier`,
`font=adobepro`, `adobe-garamond-pro`, `agaramondpro`,
`font=adobepropreview`, `adobe-preview`, `adobepreview`,
`font=ebgaramond`, and `eb`.

Force a profile without editing the manuscript:

```sh
lualatex '\def\raeezfontprofile{premier}\input{main.tex}'
lualatex '\def\raeezfontprofile{adobepro}\input{main.tex}'
lualatex '\def\raeezfontprofile{adobepropreview}\input{main.tex}'
lualatex '\def\raeezfontprofile{ebgaramond}\input{main.tex}'
```

The specimen targets expose the same flag:

```sh
make specimen-luatex FONT_PROFILE=premier
make specimen-luatex FONT_PROFILE=adobepro
make specimen-luatex FONT_PROFILE=adobepropreview
make specimen-luatex FONT_PROFILE=ebgaramond
```

The Adobe profiles are strict by design. `premier` requires real
`GaramondPremrPro`, `GaramondPremrPro-It`, `GaramondPremrPro-Smbd`, and
`GaramondPremrPro-SmbdIt` faces. `adobepro` requires
`AGaramondPro-Regular`, `AGaramondPro-Italic`, and either the Semibold pair
or the Bold pair. The template refuses to synthesize italic or bold faces for
paper builds.

`adobepropreview` is intentionally not a final paper profile. It exists for
live comparison on machines that only have `AGaramondPro-Regular`: upright text
uses Adobe Garamond Pro while italic and bold use real EB Garamond faces. This
gives a fair read of Adobe Garamond's page colour without fake slanting or fake
weight, but final paper typography should use `adobepro` only after the full
Adobe family is installed.

For pdfLaTeX, the EB Garamond math path uses newtx's Garamond math letters,
smaller large operators, the STIX-derived `vvarbb` blackboard alphabet, and
the Garamond `alth` math-h variant. `subscriptcorrection` is available as an
explicit option for papers that do not load `xy`/`xymatrix` material:

```tex
\usepackage[subscriptcorrection]{raeez-math-template}
```

## Design Discipline

The template is built around a narrow hierarchy:

- Conceptual hierarchy lives in theorem names, labels, proof dependencies,
  and mathematical notation.
- Visual hierarchy is deliberately restrained: tracked small caps for
  chapters/sections/theorem heads, italic subsection heads, quiet captions,
  and no decorative boxing.
- Page colour is controlled by Garamond text, matched math italics,
  conservative operator sizing, microtype expansion,
  and nonzero but modest line spread.
- The default text block is widened enough for real mathematical displays but
  kept inside a paper-reading measure; the template rejects overfull specimen
  logs in the harness instead of hiding them.
- Display equations sit in the paragraph rhythm; they are neither cramped nor
  poster-sized.
- The default output is print-serious: hidden PDF link borders, stable
  bookmarks, old-style proportional figures in prose, and tabular figures only
  where the manuscript asks for them.

The Adobe path is deliberately strict. It activates Garamond Premier Pro as
the body face only when real Regular, Italic, Semibold, and Semibold Italic
faces are installed. The template does not synthesize fake italic or fake
bold. If only `GaramondPremrPro` and `GaramondPremrPro-Disp` are present,
LuaLaTeX keeps EB Garamond for body text and uses the Premier Display face
only for high-level display headings.

The executable specimen is `main.tex`; compile it after typography changes:

```sh
pdflatex -interaction=nonstopmode -halt-on-error main.tex
```

LuaLaTeX is supported as a higher-fidelity OpenType path when all consuming
macros are engine-clean:

```sh
lualatex -interaction=nonstopmode -halt-on-error main.tex
```

For archive builds that should show claim-status tags:

```sh
pdflatex '\def\archivebuild{1}\input{main.tex}'
```

By default the public/Annals build suppresses claim-status tags.

## Harness

The harness is intentionally concrete. It checks the template as typography,
as a TeX package, and as a deployed cross-repo contract.

```sh
make install-consumer-symlinks
make check
```

`make install-consumer-symlinks` installs or repairs the shared
`raeez-math-template.sty` symlink in every known mathematics consumer. It
refuses to overwrite a real file.

`make check` compiles the specimen twice under pdfLaTeX, compiles it twice
under LuaLaTeX when available, probes the switchable font profiles, emits a
short embedded-font report when `pdffonts` is installed, rejects overfull
specimen/probe logs, rejects specimen/probe font and microtype warnings, fails
on Type 3 or unembedded specimen fonts, and compiles a minimal theorem-bearing
preamble document inside each consumer. That keeps typography changes honest
against the actual deployment graph.
