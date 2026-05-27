# `viz/` — nanochat visualization workspace

## Role

This folder hosts **standalone, browser-based visualizations** of nanochat's
internals. Their job is to make the code in `nanochat/` easier to learn and
reason about — they are not infrastructure, they are explainers.

Each visualization is a single self-contained artifact (one HTML file per topic)
that someone can open with a double-click (or via `python -m http.server 8000`
for environments where local `file://` blocks CDN fetches).

## Audience

ML practitioners and researchers reading nanochat for the first time, plus
people who already know the code and want a visual reference to point others at.
Assume the reader has a working knowledge of PyTorch and basic transformer
ideas (attention, MLP, softmax), but do not assume familiarity with the more
nanochat-specific bits (GQA, ResFormer-style value residuals, half-split rotary,
QK-norm, smear, backout, x0/resid lambdas, softcap, etc.).

## Scope rules

- **Mirror the code, don't invent variants.** Diagrams and prose must reflect
  what `nanochat/` actually does at the SHA where the visualization was last
  updated. If you change a viz, point at the matching nanochat code.
- **Verbatim code excerpts.** Code panels in a visualization must be exact
  quotes from `nanochat/` (preserve whitespace and comments). Add a small
  citation line indicating the source file and line range.
- **Numerical demos use toy values.** Use deterministic, hand-tuned numbers
  small enough to render legibly. Do not embed real model weights.
- **One visualization, one HTML file.** Keep each viz self-contained so it can
  be shared as a single link/file. No build step.
- **No private data, no secrets, no telemetry.** These files may be embedded
  in blog posts or sent over Slack — assume a public audience.

## Stack conventions

- HTML5, CSS3, modern ES2020+ JavaScript. No transpile step.
- CDN libraries are allowed and encouraged for quality where they pay off:
  - **D3 v7** for SVG/data binding and scales
  - **KaTeX** for typeset math
  - **highlight.js** (or Prism) for syntax-highlighted code
  - Google Fonts (Inter + JetBrains Mono) for typography
- Prefer SVG over Canvas unless dataset size makes Canvas necessary.
- Animations should be optional/respectful — honor `prefers-reduced-motion`.

## Visual design

- Dark editorial palette by default. Background `#0c0d12`, surface `#15171f`,
  border `#252836`, text `#e6e7ef`, muted `#8a8d9f`.
- Accent gold `#f0b840` for primary, with role colors: Q=`#5aa9ff`,
  K=`#5ed492`, V=`#f06aa3`, attention=`#a855f7`, gate=`#fb923c`.
- Generous whitespace, ~1100px max content width.
- Math-first: lead with the formula or shape, then explain.

## Update policy

When a meaningful piece of nanochat's inference path changes in `nanochat/`,
update the corresponding viz in the same PR (or the next opportunity).
"Meaningful" = changes a stage in the pipeline, a shape, a formula, or a
hyperparameter that the viz exposes. Cosmetic refactors don't need viz updates.

## Inventory

| File | What it explains | Primary sources |
|---|---|---|
| `inference_visualization.html` | End-to-end forward pass of nanochat's GPT, from token IDs to next-token sampling | `nanochat/gpt.py`, `nanochat/engine.py` |

Add new viz files here as they land.
