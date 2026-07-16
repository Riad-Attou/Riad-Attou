# Profile README Redesign — Design

Date: 2026-07-16
Repo: `Riad-Attou/Riad-Attou` (GitHub profile README)

## Problem

The current profile README reads as a template. It uses the widely-copied
stack of `capsule-render` waving banner, `readme-typing-svg`, three walls of
`shields.io` badges, and a streak card. The individual pieces are fine, but the
combination is generic — thousands of student profiles look identical. The
owner wants it to feel **distinctive and personal**, while keeping the GitHub
activity element.

An earlier exploration of louder directions (terminal/neofetch aesthetic,
animated neural-net hero) was rejected as "too much / too nerdy." The chosen
direction is **restraint**: a calm, professional, editorial profile that stands
out precisely because it does not shout.

## Goals

- Replace the templated look with a self-owned visual identity.
- Keep it professional and recruiter-friendly (owner is seeking AI/DS internships).
- Keep GitHub activity.
- Low ongoing maintenance.

## Non-goals

- No terminal/hacker aesthetic.
- No maximalist widget pile (no trophies, snake, WakaTime, multi-card stats).
- No new external service dependencies beyond what already renders reliably.

## Design principles

1. **Restraint over volume** — fewer, better elements; generous whitespace.
2. **Own the identity** — the one bespoke element (header SVG) lives in the repo,
   not a template service.
3. **Substance is linkable** — projects point to real repositories.
4. **Fast to read** — a recruiter should grasp who/what/how in ~5 seconds.

## Palette

Keep the existing navy/blue brand for continuity and professionalism:

- Deep navy `#0A192F`
- Mid navy `#1F4068`
- Accent blue `#1F6FEB`
- Text on dark `#FFFFFF` / muted `#C7D0D9`

## Layout (top to bottom)

### 1. Header — bespoke animated SVG (self-hosted)

- File committed at `assets/header.svg`, referenced from the README.
- Content: name **Riad Attou** + one-line identity
  ("AI & Data Science Engineer · ML · Data"). Subtle navy→blue gradient
  background matching the palette.
- **Subtle animation** (chosen): understated motion only — a slowly drifting
  accent line / gentle gradient-stop shift. No typing effect, no bounce.
- Implemented with **SMIL** (`<animate>` / `<animateTransform>`) rather than CSS
  keyframes, because SMIL animates more reliably through GitHub's image proxy
  (camo). The banner must still look complete and intentional if the animation
  is frozen (graceful static fallback).
- Sized responsively (viewBox + `width="100%"`), so it scales on desktop and
  mobile.

**Rendering note / risk:** GitHub serves repo SVGs through camo. Referencing the
SVG so it is served with `image/svg+xml` (relative repo path, which GitHub
rewrites to the raw blob URL) is required for animation. Verification step below
confirms it animates on the live profile; if camo freezes it, the static
fallback still looks correct, so this is low-risk.

### 2. About — three tight lines

Concise prose, no badge row. Example shape:

- One line: who — AI/Data Science engineering student, Centrale Lyon × Xi'an
  Jiaotong University (styled bold text, not badges).
- One line: what — designs and ships ML systems end-to-end (data → model →
  evaluation).
- One line: edge — emphasis on performance, interpretability, real-world use.

### 3. Currently — 2–3 bullets

- Seeking AI / Data Science internship.
- Focus: ML engineering & predictive modeling.
- Open to research and industry collaboration.

### 4. Selected Projects — real, linked

Replace the two unlinked, generic blurbs ("table tennis ML", "decision
systems") with concrete repositories, each as: **title → one line (what +
method/result) → link**. Proposed set (final selection confirmed with owner):

1. **Myeloid leukemia survival prediction** — `qrt_ml`. Predicts overall
   survival from medical data; QRT 2024–2025 Data Challenge with Institut
   Gustave Roussy. (Flagship — real challenge, medical ML.)
2. **Go AI** — `go-ai-app`. Full Go game (Pygame) with a Minimax +
   alpha-beta-pruning opponent and tutorial mode.
3. **Ant Colony Optimization** — `ant-colony-optimization`. Metaheuristic solver
   for graph optimization problems, with visualization.

Optional 4th slot for `hydrological-experiments` (XJTU Smart Water Lab) if a
fourth is wanted. The current "table-tennis service–return" research may be kept
if it is a real, ongoing project the owner wants featured — needs a link
(riadattou.com or a repo). **Open item for owner review.**

### 5. Tech stack — trimmed badge row (chosen)

One clean, single row of ~7–8 core tools instead of three walls. Proposed core:
`Python`, `PyTorch`, `scikit-learn`, `Pandas`, `NumPy`, `SQL`, `C`,
`TypeScript`. Keep the flat `for-the-badge` style but drop the long tail
(OCaml, MATLAB, Dash, Firebase, GitLab, etc.) to reduce wall effect. Final list
confirmed with owner.

### 6. GitHub activity — kept, minimal

Keep the existing streak card (`github-readme-streak-stats`), themed to match
the navy palette. No additional stat cards or animations — this is the one
dynamic widget, deliberately alone.

### 7. Connect + CTA — kept, cleaned

Tidy single row: Website (riadattou.com) · Google Scholar · LinkedIn · Email.
One-line closing CTA ("Open to AI/ML internships and research collaborations").
Remove the footer waving banner.

## What gets removed

- `capsule-render` waving header **and** footer.
- `readme-typing-svg` typing animation.
- Two of the three badge walls (languages/ML/web collapsed to one trimmed row).
- Generic unlinked project blurbs (replaced with linked real repos).

## Files touched

- `assets/header.svg` — new bespoke animated banner.
- `README.md` — rewritten to the layout above.

## Verification

1. **SVG validity** — open `assets/header.svg` in a browser locally; confirm it
   renders, text is legible, animation is subtle, and the frozen state still
   looks complete.
2. **Markdown render** — preview `README.md` (local markdown preview) to confirm
   structure, links, and badge row layout.
3. **Live profile** — after push, load `github.com/Riad-Attou` to confirm the
   header SVG renders and animates through camo, project links resolve, and the
   streak card loads. If the animation is frozen by camo, confirm the static
   fallback looks intentional (acceptance criterion, not a failure).
4. **Mobile** — check the profile on a narrow viewport; header scales, no
   horizontal overflow.

## Open items for owner review

1. Final project selection (default: qrt_ml, go-ai-app, ant-colony-optimization;
   keep table-tennis research only if a link exists).
2. Final tech-stack list (default 8 above).
3. Whether to feature `hydrological-experiments` as a 4th project.
