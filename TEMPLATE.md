# CUNY AI Lab Slide Deck Template

Generic, fillable template for building faculty workshop slide decks following the CUNY AI Lab workshop series pattern.

## Two Template Variants

| Variant | Directory | Engine | Best For |
|---------|-----------|--------|----------|
| **Custom Deck** | `template/` | Hand-built JS (deck-engine, carousel, scrubber, tabs) | Multi-week series decks with screenshot carousels, prompt progressions, copy-able exercise templates |
| **Reveal.js** | `template-reveal/` | Reveal.js 4.5.0 (CDN) | Single-session workshops, live coding demos, vertical slide groups, timing badges for facilitators |

Both variants include: `index.html` (source of truth), `SLIDES.md` (markdown mirror), `CLAUDE.md` (agent guidance).

**Use these templates when:**
- Creating a new workshop deck (pick the variant that fits)
- Building additional workshop variations
- Standardizing new decks to match existing CUNY AI Lab architecture

---

## Project Overview

**Title:** [WORKSHOP NAME]  
**Series:** Part [N] of a 3-part CUNY AI Lab faculty series (system prompts → knowledge collections → agentic tools)  
**Type:** Single-page HTML slide deck, no build system, no npm dependencies  
**View:** Open `index.html` directly in a browser or visit `https://cuny-ai-lab.github.io/[REPO-NAME]/`

---

## Quick Start

```bash
# Clone
git clone https://github.com/CUNY-AI-Lab/[REPO-NAME].git
cd [REPO-NAME]

# View
open index.html

# Navigate
# - Arrow keys / spacebar to advance
# - Esc for overview
# - Home/End for first/last slide
# - Hash routing: index.html#/5 jumps to slide 5

# Deploy
# Push to main — GitHub Pages rebuilds automatically
git push origin main
```

---

## Architecture

### Entry Point: `index.html`

All slides live as `.slide` divs inside a `#deck` container. Slide content is the source of truth; `SLIDES.md` is a markdown mirror kept in sync manually after edits.

### JavaScript Modules (in load order)

Located in `js/`:

1. **`tabs.js`** — Tab component (for multi-pane content)
2. **`carousel.js`** — Auto-advancing image carousel (for screenshot sequences)
   - Exposes: `window.startCarousel(id)`, `window.pauseCarousel(id)`
   - Auto-generates dots and nav arrows
   - Supports ARIA labels for screen readers
3. **`scrubber.js`** — Timeline scrubber in the nav bar
   - Exposes: `window.updateScrubber(current, total)`
   - ARIA slider with keyboard support (Arrow, Home, End)
4. **`deck-engine.js`** — Core navigation engine
   - Exposes: `window.deckEngine`, `window.goTo(n)`, `window.next()`, `window.prev()`
   - Handles focus management, live-region announcements, step reveals

### CSS Architecture

Located in `css/`:

- **`styles.css`** — Layout, components, typography, design tokens (CSS custom properties), utilities (`.sr-only`, `.skip-link`, focus-visible styles)
- **`responsive.css`** — Breakpoint overrides (1024px, 768px, 480px)
- **`animations.css`** — `@keyframes` definitions, `prefers-reduced-motion` overrides

### Inline Scripts in `index.html`

- **`copyTemplate(id)`** — Clipboard copy utility for drafting-station templates (exercises, prompts)

### Images

All image assets in `images/`. Screenshots follow naming: `slide{N}-{letter}.png` (e.g., `slide4-a.png`).

---

## Slide Layouts

Use one layout class per slide:

- **`layout-split`** — Two-column: `.content` (left, light bg) + `.stage` (right, panel/accent)
- **`layout-content`** — Single-column, light background (main content area)
- **`layout-full-dark`** — Centered, dark background (titles, roadmaps, closing)
- **`layout-divider`** — Section break: dark bg, centered large heading
- **`layout-grid`** — Grid layout with `.grid-2` card grid (light bg)

---

## Progressive Reveal & Interactivity

### Step Reveal (Click-to-Advance Content)

Elements with `class="step-hidden" data-step` are hidden until clicked:

```html
<h2>My Heading</h2>
<ul class="step-hidden" data-step>
  <li>Item 1 (hidden until clicked)</li>
  <li>Item 2</li>
</ul>
```

- Managed by `deck-engine.js`
- Resets when leaving the slide
- Typical use: building up bullet lists, revealing answers

### Stream Bullets (Animated Entry)

`<ul class="stream-list">` items animate in with staggered delay (200ms + 250ms per item) when the slide becomes active:

```html
<ul class="stream-list">
  <li>Animates in first (250ms)</li>
  <li>Animates in second (500ms)</li>
  <li>Animates in third (750ms)</li>
</ul>
```

Use for emphasis on key points that deserve visual choreography.

### Carousel

For screenshot sequences in a `.stage` panel:

```html
<div class="carousel" data-interval="8000">
  <div class="carousel-item active">
    <img src="images/slide4-a.png" alt="Descriptive alt text">
    <div class="carousel-caption">
      <strong>Step Title</strong><br>
      Descriptive sentence mentioning UI elements in <strong>bold</strong>.
    </div>
  </div>
  <div class="carousel-item">
    <img src="images/slide4-b.png" alt="Descriptive alt text">
    <div class="carousel-caption">
      <strong>Step Title</strong><br>
      Descriptive sentence.
    </div>
  </div>
  <div class="carousel-dots"></div>
</div>
```

`carousel.js` auto-generates dots and arrows. First item must have `class="carousel-item active"`.

---

## Accessibility (WCAG 2.1 AA)

### Required on Every `.slide` div

```html
<div class="slide" role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1">
  <!-- content -->
</div>
```

When adding a new slide:
1. Include all four attributes (`role`, `aria-roledescription`, `aria-label`, `tabindex`)
2. Renumber all subsequent slides

### Deck & Navigation

- **`#deck`:** `role="region" aria-roledescription="slide deck"`
- **Nav bar:** `<nav aria-label="Slide navigation">`
- **`#slide-announcer`:** `aria-live="polite"` — updated by `deck-engine.js` on every navigation

### Scrubber (Timeline)

- `role="slider"` with full ARIA value attributes
- Keyboard support: Arrow keys (prev/next), Home (first), End (last)
- Updated by `updateScrubber(current, total)` on navigation

### Content Guidelines

- **Decorative content:** Emoji and decorative SVGs must have `aria-hidden="true"`
- **Hidden content:** `.step-hidden` uses `opacity: 0` + `visibility: hidden` (hidden from screen readers)
- **Screen-reader-only text:** Use `.sr-only` utility
- **Text contrast:** `--light-text-50` is `0.65` opacity (meets AA on dark backgrounds); never use `opacity < 0.65` on text over dark backgrounds
- **Reduced motion:** `css/animations.css` includes `prefers-reduced-motion: reduce` media query disabling all animations/transitions

---

## Design Tokens (CSS Custom Properties)

All in `css/styles.css`:

### Colors

**Backgrounds:**
- `--bg-light` — Main slide background
- `--bg-panel` — Right panel (layout-split)
- `--dark-bg` — Dark theme background
- `--dark-bg-lighter` — Lighter dark variant
- `--code-bg` — Code block background

**Text:**
- `--text-dark` — Primary text on light bg
- `--text-muted` — Secondary text
- `--light-text` — Text on dark bg
- `--light-text-50` — Muted text on dark bg

**Accents:**
- `--accent-navy`, `--accent-blue`, `--accent-cyan`, `--accent-light-blue`, `--accent-red`, `--accent-amber`, `--accent-green`

### Typography

- **Headings:** `Fraunces` (Google Fonts)
- **Body:** `DM Sans` (Google Fonts)
- **Code/Prompts:** `JetBrains Mono` (Google Fonts)

All loaded from CDN; no local font files.

---

## Conventions & Style Rules

### Content

- **No em dashes** (`—`) anywhere in slide content — use periods, commas, colons, semicolons, or parentheses instead
- **Prompt blocks** use `.prompt-bad`, `.prompt-mid`, `.prompt-good` with matching `.prompt-label` badges
- **Hands-on exercises:** Template in the stage panel first, "Your turn" tip-box after
- **Logo watermark:** Hidden on first (title) and last (closing) slides automatically by `deck-engine.js`

### Maintenance

- **`SLIDES.md` must be kept in sync** with `index.html` after every content edit (manual mirror)
- **Commit messages:** Short, lowercase, no sign-offs, no Co-Authored-By lines
- **Changes affecting aesthetics:** Always use the frontend-design skill

### Coding Style

- **No build step**, no linting, no tests — `index.html` is the source of truth
- **No npm dependencies** — all JS is vanilla, all CSS is hand-written
- **HTML-first:** Content in the HTML; styling/interaction in CSS/JS

---

## File Structure

```
[REPO-NAME]/
├── index.html              # Single source of truth (all slides)
├── CLAUDE.md               # Guidance for Claude Code (copy from sibling, customize)
├── SLIDES.md               # Markdown mirror of slide content (manual sync)
├── .gitignore              # Ignore node_modules, etc. (if using git)
├── css/
│   ├── styles.css          # Layout, components, tokens, utilities
│   ├── responsive.css      # Breakpoint overrides
│   └── animations.css      # @keyframes, prefers-reduced-motion
├── js/
│   ├── tabs.js             # Tab component
│   ├── carousel.js         # Image carousel
│   ├── scrubber.js         # Timeline scrubber
│   └── deck-engine.js      # Core navigation
└── images/
    ├── logo-horizontal.png # CUNY AI Lab logo (for watermark)
    ├── slide2-a.png        # Screenshot/diagram assets
    └── ...
```

---

## Common Patterns

### Pattern 1: Title Slide

```html
<div class="slide layout-full-dark" role="group" aria-roledescription="slide" aria-label="Slide 1: Title" tabindex="-1">
  <div class="content">
    <h1>[Workshop Title]</h1>
    <p class="subtitle">[Subtitle or description]</p>
  </div>
</div>
```

### Pattern 2: Two-Column Split with Screenshot Carousel

```html
<div class="slide layout-split" role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1">
  <div class="content">
    <h2>[Title]</h2>
    <ul class="stream-list">
      <li>Point 1</li>
      <li>Point 2</li>
    </ul>
  </div>
  <div class="stage">
    <div class="carousel" data-interval="8000">
      <div class="carousel-item active">
        <img src="images/slide-a.png" alt="[alt text]">
        <div class="carousel-caption"><strong>Step 1</strong><br>[Description]</div>
      </div>
      <!-- more items -->
      <div class="carousel-dots"></div>
    </div>
  </div>
</div>
```

### Pattern 3: Three-Part Example (Weak → Getting Warmer → Strong)

```html
<div class="slide layout-grid" role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1">
  <h2>[Title]</h2>
  <div class="grid-2">
    <div class="card bad">
      <h3 class="prompt-label prompt-bad">Weak</h3>
      <p>[Example text]</p>
    </div>
    <div class="card mid">
      <h3 class="prompt-label prompt-mid">Getting Warmer</h3>
      <p>[Example text]</p>
    </div>
    <div class="card good">
      <h3 class="prompt-label prompt-good">Strong</h3>
      <p>[Example text]</p>
    </div>
  </div>
</div>
```

### Pattern 4: Exercise with Copyable Template

```html
<div class="slide layout-split" role="group" aria-roledescription="slide" aria-label="Slide N: Exercise" tabindex="-1">
  <div class="content">
    <h2>Exercise: Build Your [Thing]</h2>
    <p>[Instructions]</p>
  </div>
  <div class="stage">
    <div class="template-box">
      <pre id="template-1"><code>[Template text here]</code></pre>
      <button onclick="copyTemplate('template-1')" class="copy-btn">Copy to Clipboard</button>
    </div>
  </div>
</div>
```

---

## Deployment

### GitHub Pages Setup

```bash
# 1. Create repo on GitHub: https://github.com/CUNY-AI-Lab/[REPO-NAME]

# 2. Clone locally
git clone https://github.com/CUNY-AI-Lab/[REPO-NAME].git
cd [REPO-NAME]

# 3. Add files (index.html, css/, js/, images/, CLAUDE.md, SLIDES.md, .gitignore)

# 4. Commit & push
git add .
git commit -m "init: create slide deck"
git push origin main

# 5. Enable GitHub Pages in Settings > Pages:
#    - Source: Deploy from a branch
#    - Branch: main, folder: / (root)
#    - Save

# 6. Site live at https://cuny-ai-lab.github.io/[REPO-NAME]/
```

No build step required. GitHub Pages serves `index.html` directly.

---

## Quick Customization Checklist

- [ ] Rename `[REPO-NAME]` throughout documentation and config
- [ ] Update `index.html` `<title>` and meta tags
- [ ] Copy and customize `CLAUDE.md` from sibling deck
- [ ] Create slide structure in `index.html` (see patterns above)
- [ ] Add CSS customizations to `css/styles.css` if needed (colors, fonts, spacing)
- [ ] Add screenshot images to `images/` with `slide{N}-{letter}.png` naming
- [ ] Populate `SLIDES.md` with markdown version of content (manual sync after edits)
- [ ] Push to GitHub and enable Pages
- [ ] Test in browser: open `index.html` locally, verify navigation and accessibility
- [ ] Verify GitHub Pages URL is live and accessible

---

## Resources

**Sibling decks** (for reference):
- System-Prompting: https://github.com/CUNY-AI-Lab/system-prompting
- Knowledge-Collections: https://github.com/CUNY-AI-Lab/knowledge-collections

**Accessibility:**
- WCAG 2.1: https://www.w3.org/WAI/WCAG21/quickref/
- ARIA authoring practices: https://www.w3.org/WAI/ARIA/apg/

**Typography:**
- Google Fonts: https://fonts.google.com
