# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

> **Inherits `~/ecosystem/INVARIANTS.md`.** That file holds the canonical ecosystem rules: destructive-git forbidden-command list, multi-agent worktree concurrency, standalone-documents discipline, Russian-school voice, every-file-into-the-repo rule, commits-carry-no-LLM-attribution, deep-semantic-merges, intelligence propagation, mathematical-repair doctrine. Read it first. Repo-local rules follow.

---

## Repo-local

This repository owns the shared mathematical LaTeX surface for the chiral bar-cobar series and successor mathematical-physics manuscripts. Treat it as infrastructure, not as a disposable paper scaffold.

## Authority

- `raeez-math-template.sty` is the deployed package.
- `main.tex` is the specimen and regression document.
- `Makefile` is the public command surface.
- `scripts/install-consumer-symlinks` deploys the package into consumers.
- `scripts/check-template-harness` verifies the cross-repo contract.

Consumers must use the shared package through a root-level symlink:

```text
raeez-math-template.sty -> ../latex-template/raeez-math-template.sty
```

Each consumer main file loads it as the first nonblank, noncomment command after `\documentclass`, and keeps the `\ifdefined\mainpreambleonly` guard for harness compilation.

## Consumers

The current checked consumers are:

- `~/chiral-bar-cobar`
- `~/cbc`
- `~/chiral-bar-cobar-vol2`
- `~/chiral-bar-cobar-vol2-kz-bv-20260424`
- `~/chiral-bar-cobar-vol4`
- `~/calabi-yau-quantum-groups`
- `~/igusa-cusp-form`
- `~/topological-strings`

If a manuscript needs a theorem environment, typography rule, package option, or engine workaround that belongs to the series rather than the single manuscript, add it here and deploy it. Do not fork the style file in a consumer.

## Verification

After any template or deployment-script change, run:

```sh
make install-consumer-symlinks
make check
```

For a narrower harness-only check, run:

```sh
scripts/check-template-harness
```

The install script may repair symlinks. It must not overwrite a real consumer file. A consumer that cannot accept the shared symlink is a blocker, not a reason to clone the template locally.
