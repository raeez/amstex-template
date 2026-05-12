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

## Code-writing discipline — repo application

Per `~/ecosystem/INVARIANTS.md §XIII`. Twelve rules instantiated for latex-template (shared `raeez-math-template.sty`; consumer harness; symlink contract per `INVARIANTS.md §XII`):

1. **Think Before Coding.** Every typography / theorem-environment / claim-status / TikZ edit names the affected manuscript (or "all consumers"). Every macro addition documents the consumer surface it changes.
2. **Simplicity First.** No speculative environments; consumers earn additions. No parallel typography branches. No fork in a consumer — repair here.
3. **Surgical Changes.** A TikZ style edit does not touch theorem environments and vice versa. A claim-status macro change does not opportunistically rewrite anchor logic. The `\mainpreambleonly` harness guard stays untouched unless intentionally changing harness compatibility.
4. **Goal-Driven Execution.** Success = `make check` clean, `make install-consumer-symlinks` no-op (all consumers correctly linked), `scripts/check-template-harness` clean, every consumer manuscript builds.
5. **Use the model only for judgment calls.** Template-vs-consumer ownership is deterministic per `INVARIANTS.md §XII`. No LLM in the loop on which side owns typography.
6. **Token budgets are not advisory.** Small repo; standard.
7. **Surface conflicts, don't average them.** Template authority is here per `INVARIANTS.md §XII`; consumers do not fork. If a consumer needs custom typography, the repair lands here, never in the consumer.
8. **Read before you write.** Read consumer-manuscript loading conventions (`\documentclass` → `\usepackage{raeez-math-template}` → `\ifdefined\mainpreambleonly`) before adding env or macro that consumers will load. Cross-check with `MATH_TEMPLATE_CONSUMERS` in `~/ecosystem/repo-roster.sh`.
9. **Tests verify intent.** `scripts/check-template-harness` confirms consumer integrity; `make check` is the load-bearing gate. A green build that breaks a consumer is broken.
10. **Checkpoint after every significant step.** After each consumer-affecting change, restate which consumer manuscripts need a sanity-build (per `MATH_TEMPLATE_CONSUMERS` list).
11. **Match the codebase's conventions, even if you disagree.** memoir / EB Garamond / theorem / claim-status / anchor / TikZ template. Existing macro naming. No parallel naming schemes.
12. **Fail loud.** Announce every consumer-build break immediately. Never push a typography change without running `make check` and at least one consumer build. Never overwrite a real consumer file via `install-consumer-symlinks` — surface the conflict.
