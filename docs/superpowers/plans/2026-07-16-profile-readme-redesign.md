# Profile README Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the templated profile README with a restrained, professional, self-owned design anchored by one bespoke animated SVG header, real linked projects, a trimmed stack, and the kept GitHub activity.

**Architecture:** Two files. `assets/header.svg` is a hand-authored, SMIL-animated banner committed in-repo (no template service). `README.md` is rewritten to a clean editorial layout that references the banner and keeps only the streak card as its single dynamic widget.

**Tech Stack:** SVG (SMIL animation), GitHub-flavored Markdown, shields.io (trimmed), github-readme-streak-stats.

## Global Constraints

- Palette: `#0A192F` (deep navy), `#132A4D`/`#1F4068` (mid navy), `#1F6FEB` (accent blue), `#FFFFFF` text, `#C7D0D9` muted, `#8AA0B8` faint.
- No JS/CSS (GitHub strips them). Animation must be SMIL inside the SVG, with a graceful static fallback if the image proxy freezes it.
- Aesthetic: restrained/editorial. No terminal look, no maximalist widgets (no trophies/snake/multi-card stats).
- Keep GitHub activity (streak card) and the contact row.
- Header text must stay legible at mobile width.

---

### Task 1: Bespoke animated header SVG

**Files:**
- Create: `assets/header.svg`

**Interfaces:**
- Produces: an SVG at `assets/header.svg`, `viewBox="0 0 1200 300"`, `width="100%"`, referenced by README Task 2 as `assets/header.svg`.

- [ ] **Step 1: Write the SVG**

Create `assets/header.svg` with exactly:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 300" width="100%" role="img" aria-label="Riad Attou — AI & Data Science Engineer">
  <defs>
    <linearGradient id="bg" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#0A192F"/>
      <stop offset="0.55" stop-color="#132A4D"/>
      <stop offset="1" stop-color="#0A192F"/>
    </linearGradient>
    <linearGradient id="sheen" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0" stop-color="#1F6FEB" stop-opacity="0"/>
      <stop offset="0.5" stop-color="#1F6FEB" stop-opacity="0.9"/>
      <stop offset="1" stop-color="#1F6FEB" stop-opacity="0"/>
      <animate attributeName="x1" values="-0.7;0.7;-0.7" dur="7s" repeatCount="indefinite"/>
      <animate attributeName="x2" values="0.3;1.7;0.3" dur="7s" repeatCount="indefinite"/>
    </linearGradient>
  </defs>

  <rect width="1200" height="300" rx="18" fill="url(#bg)"/>

  <!-- faint data-point constellation (evokes data, stays subtle) -->
  <g stroke="#1F6FEB" stroke-opacity="0.14" stroke-width="1">
    <line x1="930" y1="150" x2="1010" y2="90"/>
    <line x1="1010" y1="90" x2="1090" y2="140"/>
    <line x1="1010" y1="90" x2="1000" y2="200"/>
    <line x1="1000" y1="200" x2="1090" y2="140"/>
    <line x1="930" y1="150" x2="1000" y2="200"/>
  </g>
  <g fill="#1F6FEB">
    <circle cx="930" cy="150" r="3" fill-opacity="0.35">
      <animate attributeName="fill-opacity" values="0.2;0.5;0.2" dur="6s" repeatCount="indefinite"/>
    </circle>
    <circle cx="1010" cy="90" r="3.5" fill-opacity="0.45">
      <animate attributeName="fill-opacity" values="0.25;0.6;0.25" dur="7.5s" repeatCount="indefinite"/>
    </circle>
    <circle cx="1090" cy="140" r="2.5" fill-opacity="0.3">
      <animate attributeName="fill-opacity" values="0.15;0.45;0.15" dur="8s" repeatCount="indefinite"/>
    </circle>
    <circle cx="1000" cy="200" r="2.5" fill-opacity="0.3">
      <animate attributeName="fill-opacity" values="0.2;0.5;0.2" dur="6.8s" repeatCount="indefinite"/>
    </circle>
  </g>

  <!-- name -->
  <text x="72" y="140" fill="#FFFFFF" font-family="'Segoe UI', Helvetica, Arial, sans-serif" font-size="66" font-weight="700" letter-spacing="1">Riad Attou</text>

  <!-- animated accent underline: base + moving sheen highlight -->
  <rect x="74" y="162" width="168" height="4" rx="2" fill="#1F6FEB" fill-opacity="0.28"/>
  <rect x="74" y="162" width="168" height="4" rx="2" fill="url(#sheen)"/>

  <!-- subtitle -->
  <text x="74" y="206" fill="#C7D0D9" font-family="'Segoe UI', Helvetica, Arial, sans-serif" font-size="24" letter-spacing="0.4">AI &amp; Data Science Engineer · Machine Learning · Data</text>

  <!-- affiliation -->
  <text x="74" y="240" fill="#8AA0B8" font-family="'Segoe UI', Helvetica, Arial, sans-serif" font-size="17" letter-spacing="0.4">Centrale Lyon × Xi'an Jiaotong University</text>
</svg>
```

- [ ] **Step 2: Verify the SVG is well-formed XML**

Run: `python3 -c "import xml.dom.minidom,sys; xml.dom.minidom.parse('assets/header.svg'); print('well-formed')"`
Expected: prints `well-formed` (no exception).

- [ ] **Step 3: Rasterize a preview to eyeball it (if a renderer is available)**

Run (try in order, stop at first that works):
`rsvg-convert assets/header.svg -o /tmp/header.png 2>/dev/null && echo rsvg-ok || (python3 -c "import cairosvg; cairosvg.svg2png(url='assets/header.svg', write_to='/tmp/header.png')" 2>/dev/null && echo cairosvg-ok || echo "no-renderer: skip visual, rely on live GitHub check")`
Expected: `/tmp/header.png` created, OR `no-renderer` message.
If a PNG was produced, open it and confirm: name legible, subtitle + affiliation readable, navy gradient, subtle blue accent — nothing gimmicky. The frozen (non-animated) state must already look complete.

- [ ] **Step 4: Commit**

```bash
git add assets/header.svg
git commit -m "Add bespoke animated SVG header for profile README"
```

---

### Task 2: Rewrite README.md

**Files:**
- Modify: `README.md` (full rewrite)

**Interfaces:**
- Consumes: `assets/header.svg` from Task 1.

- [ ] **Step 1: Replace README.md with the new layout**

Overwrite `README.md` with exactly:

```markdown
<p align="center">
  <img src="assets/header.svg" alt="Riad Attou — AI & Data Science Engineer" width="100%">
</p>

### About

I'm an AI & Data Science engineering student at **Centrale Lyon × Xi'an Jiaotong University**. I design and ship machine-learning systems end to end — from data exploration to modeling and evaluation — with a bias toward performance, interpretability, and real-world usability.

### Currently

- 🎯 Seeking an **AI / Data Science internship**
- 🧠 Focused on ML engineering & predictive modeling
- 🤝 Open to research and industry collaboration

### Selected Projects

**[Table-tennis service–return analysis](https://scholar.google.com/citations?user=GLPha1AAAAAJ&hl=en)** — Research project. Clustering and predictive modeling of elite service–return patterns, with end-to-end ML pipelines and statistical hypothesis testing.

**[Myeloid leukemia survival prediction](https://github.com/Riad-Attou/qrt_ml)** — Predicts patients' overall survival from medical data. QRT 2024–2025 Data Challenge, in collaboration with Institut Gustave Roussy.

**[Go AI](https://github.com/Riad-Attou/go-ai-app)** — A full game of Go in Pygame with a Minimax + alpha-beta-pruning opponent, tutorial mode, and automatic scoring.

**[Ant Colony Optimization](https://github.com/Riad-Attou/ant-colony-optimization)** — Metaheuristic solver for graph optimization problems, with visualization.

### Tech Stack

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white">
  <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white">
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white">
  <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white">
  <img src="https://img.shields.io/badge/SQL-336791?style=for-the-badge&logo=postgresql&logoColor=white">
  <img src="https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
</p>

### GitHub Activity

<p align="center">
  <a href="https://git.io/streak-stats"><img src="https://github-readme-streak-stats.herokuapp.com?user=Riad-Attou&theme=github-dark-blue&hide_border=true&mode=weekly&background=45%2C0A192F%2C1F4068" alt="GitHub Streak" /></a>
</p>

### Connect

<p align="center">
  <a href="https://riadattou.com/en"><img src="https://img.shields.io/badge/Website-riadattou.com-1F6FEB?style=for-the-badge"></a>
  <a href="https://scholar.google.com/citations?user=GLPha1AAAAAJ&hl=en"><img src="https://img.shields.io/badge/Google%20Scholar-Research-1F6FEB?style=for-the-badge&logo=googlescholar&logoColor=white"></a>
  <a href="https://www.linkedin.com/in/riad-attou"><img src="https://img.shields.io/badge/LinkedIn-Riad%20Attou-1F6FEB?style=for-the-badge&logo=linkedin&logoColor=white"></a>
  <a href="mailto:attou.rd@gmail.com"><img src="https://img.shields.io/badge/Email-attou.rd%40gmail.com-1F6FEB?style=for-the-badge&logo=gmail&logoColor=white"></a>
</p>

<p align="center"><i>Open to AI / ML engineering internships and research collaborations.</i></p>
```

- [ ] **Step 2: Verify structure and links**

Run: `python3 - <<'PY'
import re
t=open('README.md').read()
must=['assets/header.svg','qrt_ml','go-ai-app','ant-colony-optimization','GLPha1AAAAAJ','riadattou.com','linkedin.com/in/riad-attou','streak-stats']
missing=[m for m in must if m not in t]
banned=['capsule-render','readme-typing-svg','OCaml','MATLAB']
present=[b for b in banned if b in t]
print('MISSING:',missing or 'none')
print('SHOULD-BE-GONE-BUT-PRESENT:',present or 'none')
PY`
Expected: `MISSING: none` and `SHOULD-BE-GONE-BUT-PRESENT: none`.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "Rewrite profile README: editorial layout, real linked projects, trimmed stack"
```

---

### Task 3: Live verification (after merge/push to `main`)

**Files:** none (verification only).

This is the real acceptance check — it can only run once the branch is on GitHub's default branch (do it after the finishing-a-development-branch step merges to `main`).

- [ ] **Step 1: Load the live profile**

Open `https://github.com/Riad-Attou` in a browser.

- [ ] **Step 2: Confirm acceptance criteria**

- Header SVG renders; text legible on desktop.
- Animation plays (subtle sheen on the underline + gently pulsing points). **If frozen by GitHub's image proxy, confirm the static state still looks intentional — this is acceptable, not a failure.** If a moving animation is required and it's frozen, switch the README `img src` from `assets/header.svg` to the raw URL `https://raw.githubusercontent.com/Riad-Attou/Riad-Attou/main/assets/header.svg` and re-push.
- All four project links resolve.
- Streak card loads.
- Connect badges + links work.

- [ ] **Step 3: Confirm mobile**

Open the profile on a narrow viewport (or DevTools device mode). Header scales, no horizontal overflow, subtitle still readable.

---

## Self-Review

**Spec coverage:** Header SVG (T1), About/Currently/Projects/Stack/Activity/Connect + removals (T2), all four verification points — SVG validity, markdown render, live profile, mobile (T1 S2–3, T2 S2, T3). Palette, no-JS/CSS, restraint, kept activity — all in Global Constraints and enforced by T2 S2's banned-list check. Covered.

**Placeholder scan:** No TBD/TODO; both files' full content is inline. Clean.

**Type consistency:** README references `assets/header.svg` exactly as Task 1 produces it. Scholar id `GLPha1AAAAAJ`, repo slugs, and streak URL are consistent between plan and verification checks.
