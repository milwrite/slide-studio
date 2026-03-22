# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Important Rules

- **Always use the frontend-design skill when making changes that impact aesthetics.** This applies to any visual/styling modifications — typography, spacing, backgrounds, component styling, layout adjustments.
- **All changes must be accessible by default.** Every new slide, component, or interactive element must meet WCAG 2.1 AA. This is not optional and not a follow-up task.
- **Never use Co-Authored-By lines in commits.**

## Repository Structure

```
slide-decks/
├── system-prompting/      Workshop 1 (live: cuny-ai-lab.github.io/system-prompting/)
├── knowledge-collections/ Workshop 2 (live: cuny-ai-lab.github.io/knowledge-collections/)
└── cail-workshops/        Separate nested git repo (github.com/CUNY-AI-Lab/workshops.git)
                           Archive of older workshops; has its own .git, not a submodule
```

**Remotes:**
- `milwrite/slide-decks` — personal working repo (all development happens here)
- `cuny-ai-lab/[deck-name]` — production org repos (one per deck, deploys via GitHub Pages from `main`)

**Deploy flow:** develop in `milwrite/slide-decks`, then push the subdirectory to the corresponding `cuny-ai-lab` repo via `git subtree push`.

## Shared Deck Engine

Both active decks run a custom vanilla-JS deck engine. No framework, no build step, no npm. Open `index.html` directly in a browser.

**JS load order** (bottom of `index.html`):
1. `js/tabs.js` — tab component
2. `js/carousel.js` — auto-advancing screenshot carousel with ARIA labels and prev/next arrows
3. `js/scrubber.js` — scrubber timeline + ARIA slider; exposes `updateScrubber(current, total)`
4. `js/deck-engine.js` — core navigation + focus management + live-region announcements; exposes `window.deckEngine`, `window.goTo`, `window.next`, `window.prev`

**Inline script** (before JS tags): `copyTemplate(id)` — clipboard copy for exercise templates.

**Known divergence:** the two `deck-engine.js` files use different live-region IDs (`#slide-announcer` in system-prompting, `#slide-announce` in knowledge-collections). Do not share JS files between decks until IDs are reconciled.

## Style Guide

### Typography (Google Fonts CDN)

| Role | Family |
|---|---|
| Headings | `Fraunces` (variable, serif) |
| Body | `DM Sans` (variable, sans-serif) |
| Code / prompts | `JetBrains Mono` |

### Color Design Tokens (`:root` in `css/styles.css`)

| Token | Purpose |
|---|---|
| `--bg-light` | Light slide background |
| `--bg-panel` | Stage panel background |
| `--dark-bg` | Dark slide / section background |
| `--dark-bg-lighter` | Elevated dark surface |
| `--code-bg` | Code block background |
| `--text-dark` | Primary body text |
| `--text-muted` | Secondary / muted text |
| `--light-text` | Text on dark backgrounds |
| `--light-text-50` | Dimmed text on dark BG — **must be >= 0.65 opacity** (WCAG AA) |
| `--accent-navy` | Navy accent |
| `--accent-blue` | Blue accent |
| `--accent-cyan` | Primary cyan accent (`#4ECDC4`) |
| `--accent-light-blue` | Light blue accent |
| `--accent-red` | Red / warning |
| `--accent-amber` | Amber / caution |
| `--accent-green` | Green / success |

### Slide Layout Classes

| Class | Description |
|---|---|
| `layout-split` | Two-column: `.content` (left, light) + `.stage` (right, panel) |
| `layout-content` | Single-column, light background |
| `layout-full-dark` | Centered, dark background (roadmaps, model grids) |
| `layout-divider` | Section break — dark, centered large `<h2>`, `.divider-line` accent bar |
| `layout-grid` | Dark background with `.grid-2` card layout |

### Component Classes

| Class | Usage |
|---|---|
| `.label`, `.label-cyan`, `.label-blue` | Small-caps badge before `<h2>` |
| `.card`, `.card-cyan`, `.card-navy` | Content card group |
| `.tip-box` | Instructional callout box |
| `.prompt-block`, `.prompt-bad/mid/good` | Prompt or collection quality comparison block |
| `.prompt-label` | Badge on a prompt block |
| `.drafting-station`, `.template-block` | Copyable template with Copy button |
| `.roadmap`, `.roadmap-item`, `.roadmap-item.active` | Session roadmap widget |
| `.model-grid`, `.model-card` | Model or document-type card grid |
| `.carousel`, `.carousel-item`, `.carousel-dots` | Screenshot carousel |
| `.theater-diagram`, `.theater-backstage/curtain/frontstage` | "Behind the curtain" flow metaphor |
| `.flow-node`, `.flow-connector-line` | Generic flow diagram nodes + arrows |
| `.progression-dots`, `.dot-bad/mid/good/active` | Quality progression indicator row |
| `.stream-list` | Staggered bullet animation on slide entry |
| `.step-hidden` + `data-step` | Progressive step reveal (forward advance) |
| `.comparison`, `.comparison-panel`, `.panel-muted` | Side-by-side comparison panels |
| `.sr-only` | Visually hidden, screen-reader accessible |
| `.skip-link` | Skip-to-content keyboard shortcut |
| `.logo-watermark` | Background logo; hidden on title and closing slides by JS |

### Slide HTML Template

```html
<!-- ==========================================
     SLIDE N — TITLE
     ========================================== -->
<div class="slide layout-split" role="group" aria-roledescription="slide"
     aria-label="Slide N: Title" data-slide="kebab-name" tabindex="-1">
  <div class="content">
    <span class="label label-cyan">Section Label</span>
    <h2>Heading</h2>
    <!-- body content -->
  </div>
  <div class="stage">
    <!-- carousel, code block, diagram, or screenshot -->
  </div>
</div>
```

All four attributes required on every slide: `role="group"`, `aria-roledescription="slide"`, `aria-label="Slide N: Title"`, `tabindex="-1"`. `data-slide` uses kebab-case nouns (no numbers). `aria-label` format: `"Slide N: Title"` (sequential number + colon + space + title).

### Carousel Pattern (`layout-split` stage)

```html
<div class="carousel" data-interval="8000">
  <div class="carousel-item active">
    <img src="images/slide{N}-a.png" alt="Descriptive alt text">
    <div class="carousel-caption"><strong>Step Title</strong><br>Explanatory text.</div>
  </div>
  <!-- additional .carousel-item divs -->
  <div class="carousel-dots"></div>
</div>
```

First item must have `class="carousel-item active"`. Image naming: `slide{N}-{letter}.png`.

### HTML Entity Conventions

- Always use named HTML entities, never raw Unicode for typographic characters
- `&rsquo;` `&ldquo;` `&rdquo;` — smart quotes in displayed text
- `&rarr;` `&hellip;` `&amp;` — as needed
- **No em dash (`&mdash;` or `—`) in displayed slide text.** Rewrite to avoid, or use a colon (`: `) or ` -- ` as separator

### No Em Dashes in Slide Content

The no-em-dash rule applies to all text rendered visibly on slides (heading text, body text, bullets, labels, captions). It does NOT apply to HTML comments, SLIDES.md heading metadata, or file-level documentation. In displayed content, replace `—` with:
- A colon: `One Document, One Purpose: the model retrieves better from focused documents`
- Or restructure the sentence to avoid the dash entirely

## SLIDES.md Format Specification

Each deck has a `SLIDES.md` file — a markdown mirror of `index.html` content. `index.html` is always the source of truth.

**Header block:**
```markdown
# [Workshop Title] -- Slide Deck Content

> CUNY AI Lab Sandbox Workshop
> Source: `index.html`
> Upstream: <https://github.com/CUNY-AI-Lab/[repo-name]>
```

**Slide heading format:**
```markdown
## Slide N -- Title (`layout-class-name`)
```
- Double hyphen separator ` -- ` (not single hyphen, not em dash)
- Layout class in backtick parens
- Sequential numbering matching actual HTML slide positions

**Removed or merged slides:**
```markdown
## ~~Slide N -- Section Divider~~ (merged into Slide M)
```

**Metadata annotations** (as bold inline text within slide entries):
- `**Label:** The Basics` — section label shown on the slide
- `**Carousel:** - slide4-a.png ...` — carousel image inventory
- `**Layout note:** ...` — any non-obvious layout or structure details

**Footer:**
```markdown
---

_Last synced: YYYY-MM-DD. Upstream: CUNY-AI-Lab/[repo] @ main. Update both files together._

## Changelog

### YYYY-MM-DD
- Description of what changed
```

**Sync rule:** After every content edit to `index.html`, update SLIDES.md in the same commit. After every SLIDES.md edit, verify the HTML matches.

## Accessibility Standards

These apply to both active decks:

- WCAG 2.1 AA required on all changes
- Every `.slide` div: `role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1"`
- `#deck` container: `role="region" aria-roledescription="slide deck"`
- Live region (`aria-live="polite"`) updated on every slide navigation
- Decorative images: `alt="" aria-hidden="true"`
- Decorative emoji: `aria-hidden="true"` on the containing span
- Decorative SVG arrows: `aria-hidden="true" focusable="false"`
- `.step-hidden` uses both `opacity: 0` and `visibility: hidden` (hidden from screen readers)
- `.sr-only` is the utility for visually-hidden, screen-reader-accessible text
- Skip link: `<a href="#deck" class="skip-link">Skip to slides</a>` immediately after `<body>`
- `--light-text-50` must use >= 0.65 opacity for AA contrast on dark backgrounds
- `prefers-reduced-motion` in `css/animations.css` disables all animations and transitions

## CSS Files

Each deck carries its own copies (not shared):
- `css/styles.css` — layout, components, typography, design tokens (`:root` variables), `.sr-only`, focus-visible styles
- `css/responsive.css` — breakpoint overrides (1024px, 768px, 480px)
- `css/animations.css` — `@keyframes` definitions, `prefers-reduced-motion` override

## Commit Conventions

- Short, lowercase, no sign-off
- No `Co-Authored-By` lines
- Format: `type(scope): description` (e.g., `fix(knowledge-collections): remove orphaned fragment after closing slide`)
- When a change touches both decks, use the repo-level scope or omit the scope

## Conventions

- No em dashes (`—`) anywhere in slide content
- `SLIDES.md` must be kept in sync with `index.html` after every content edit
- Logo watermark uses `alt="" aria-hidden="true"` (decorative treatment)
- Logo watermark is hidden on title (first) and closing (last) slides via `deck-engine.js`
- Hands-on exercise slides: template in the stage panel first, "Your turn" tip-box after
- Prompt/collection examples frame the instructor as the builder/curator, not students
- System prompt examples frame AI as a tool the instructor builds, not something students use

## Cross-Workshop Backport Tracker

Improvements in one deck that should be applied to the sibling:

| Improvement | Source | Target | Status |
|---|---|---|---|
| `--light-text-50` raised to `0.65` opacity | knowledge-collections | system-prompting | pending |
| `tabindex="-1"` on all slides | knowledge-collections | system-prompting | pending |
| `data-slide` named attribute on all slides | knowledge-collections | system-prompting | pending |
| More targeted `animations.css` reduced-motion overrides | knowledge-collections | system-prompting | pending |
| Skip link `<a href="#deck" class="skip-link">` | system-prompting | knowledge-collections | pending |
| Logo watermark `alt="" aria-hidden="true"` (decorative) | system-prompting | knowledge-collections | pending |
| Live-region ID reconciliation (`slide-announcer` vs `slide-announce`) | — | both | pending |
