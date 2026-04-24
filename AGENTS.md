# AGENTS.md — latex-template

> **Inherits `~/ecosystem/INVARIANTS.md`** — canonical ecosystem rules (model-agnostic): destructive-git forbidden list, multi-agent worktree concurrency, standalone-documents discipline, Russian-school voice, every-file-into-the-repo rule, no-LLM-attribution in commits, deep-semantic-merges, intelligence propagation, open-source whitelist, mathematical-repair doctrine.
> **Inherits `~/ecosystem/AGENTS-HARNESS.md`** — canonical Codex / GPT-5-family harness calibration: reasoning-effort per task class, agentic eagerness, tool-use discipline, tool preambles, persistence and stop conditions, verbosity control, uncertainty handling, long-context outlining, self-reflection rubric, scope discipline, error-handling, git-and-worktree restatement for Codex defaults, frontend quality, no-LLM-commit-attribution, voice.
> **Mirrors the repo's `CLAUDE.md`** on substance. Before editing code in this repo, read `./CLAUDE.md` — it carries the repo-local layout, commands, doctrine, and conventions. `AGENTS.md` and `CLAUDE.md` must not diverge in facts; they may differ in structure and voice.
>
> **Load order.** `INVARIANTS.md` → `AGENTS-HARNESS.md` → this repo's `CLAUDE.md` → this file's repo-local section (if any). The closest `AGENTS.md` in the directory tree wins per `agents.md`; explicit principal chat instructions outrank everything.
>
> **Model target.** Deepest host-exposed GPT-5.5 / GPT-5-Codex-family model, `reasoning_effort=high` or `xhigh` for non-trivial work (Pro-class). Terse, declarative voice per `INVARIANTS.md §IV`. No LLM attribution on commits (`INVARIANTS.md §VI`).

---

## Repo-local (Codex / GPT-5 specific)

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
