# CUNY AI Lab Sandbox: Slide Deck Builder

## What This Is

A workspace configuration for the CUNY AI Lab Sandbox (chat.ailab.gc.cuny.edu) that turns Kimi k2.5 into a slide deck authoring tool. Users describe their workshop content in natural language; the model fills out a pre-built HTML template using line-level edits.

The whole system runs inside OpenWebUI's Workspace: a custom Model wraps Kimi k2.5, binds a Knowledge Collection (the template files), and uses a system prompt that teaches the model how to read and edit the template's placeholder schema.

## Core Components

### 1. Custom Model ("CAIL Slide Deck Builder")

OpenWebUI Model Builder fields:

| Field | Value |
|-------|-------|
| **Name** | CAIL Slide Deck Builder |
| **ID** | cail-slide-deck-builder |
| **Base Model** | kimi-k2.5 |
| **Description** | Generates and edits CUNY AI Lab workshop slide decks from natural language descriptions. Produces a single index.html file following the CAIL template architecture. |
| **Tags** | workshops, slides, cail |
| **Visibility** | Private (CAIL team) |

### 2. Knowledge Collection ("CAIL Deck Template")

Upload these files from `slide-decks/template/` as a Knowledge Collection:

**Core (Full Context injection):**
- `index.html` — the 16-slide placeholder template (source of truth)
- `SLIDES.md` — markdown mirror of all slide content
- `CLAUDE.md` — architecture reference, layout classes, conventions

**CSS themes (Focused Retrieval):**
- `css/styles.css` — design tokens, layout classes, components
- `css/responsive.css` — breakpoint overrides
- `css/animations.css` — keyframes, reduced-motion

**JS interactivity (Focused Retrieval):**
- `js/deck-engine.js` — navigation, keyboard, fragments
- `js/carousel.js` — screenshot carousels
- `js/scrubber.js` — timeline slider
- `js/tabs.js` — tabbed content panels

The model can query these via `#cail-deck-template` in the chat or through the bound Knowledge auto-injection.

### 3. System Prompt

See `system-prompt.md` in this directory. Key behaviors:
- Understands the `[BRACKETED PLACEHOLDER]` convention
- Outputs precise line edits: `REPLACE line N: old → new`
- Can generate complete filled index.html when given enough content
- Maintains SLIDES.md in sync with index.html changes
- Enforces WCAG 2.1 AA accessibility requirements
- Follows the CAIL visual language (Fraunces headings, DM Sans body, dark-panel stages)

### 4. Prompt Suggestions (starter chips)

These appear when a user opens a new chat:

1. "Create a new workshop deck about [topic] for a 90-minute CAIL session"
2. "Edit slide 5: change the comparison to show [X] vs [Y]"
3. "Add a new section divider between slides 8 and 9"
4. "Replace all placeholder text with content about [workshop topic]"
5. "Show me the current SLIDES.md content for my deck"

### 5. Capabilities

| Capability | Setting |
|------------|---------|
| Knowledge | Bound to "CAIL Deck Template" (Full Context for index.html, SLIDES.md, CLAUDE.md) |
| Web Search | OFF (template is self-contained) |
| Image Generation | OFF |
| Code Interpreter | ON (for rendering previews if available) |

---

## Slide Content Input Format

When a user provides workshop content, the model expects this structure:

```
WORKSHOP: [Title]
DATE: [Date]
AUTHOR: [Names]
SERIES: [Series name, e.g. "AI Literacy for Faculty"]

---SLIDE 1 (title)---
title: [Main title]
subtitle: [Subtitle line]

---SLIDE 2 (roadmap)---
session_1: [Date] | [Title] | [Description]
session_2: [Date] | [Title] | [Description]
session_3: [Date] | [Title] | [Description]

---SLIDE 3 (split)---
label: [Section label, e.g. "The Basics"]
title: [Topic title]
body: [Paragraph text]
quote: [Analogy or key framing]
key_distinction: [Clarifying note]
stage: [Description of visual for right panel]

---SLIDE N (divider)---
part: [Part number]
title: [Section title]
subtitle: [One-liner]

---SLIDE N (exercise)---
step: [N of M]
title: [Step title]
instructions: [What to do]
actions:
  - [Action 1]
  - [Action 2]
template: |
  [Copyable template text with blank fields]
```

The model maps each slide type to the correct layout class from the template.

---

## Edit Protocol

For minor changes (copyedits, single-slide updates), the model uses a line-edit format:

```
EDIT index.html

SLIDE 3, line 87:
  OLD: <h2>[Topic Title]</h2>
  NEW: <h2>What Are System Prompts?</h2>

SLIDE 3, line 88:
  OLD: [Introductory paragraph explaining the concept...]
  NEW: A system prompt is a set of instructions that shapes how an AI model responds...

SYNC SLIDES.md
  Slide 3 title: "What Are System Prompts?"
```

This protocol lets users request small fixes ("change the title on slide 7 to X") without regenerating the whole file.

---

## Reference Workshop Repos

These are the source-of-truth decks the template derives from:

| Workshop | Repo | Live URL |
|----------|------|----------|
| System Prompting | CUNY-AI-Lab/system-prompting | cuny-ai-lab.github.io/system-prompting |
| Knowledge Collections | CUNY-AI-Lab/knowledge-collections | cuny-ai-lab.github.io/knowledge-collections |
| Creative Coding | CUNY-AI-Lab/creative-coding | cuny-ai-lab.github.io/creative-coding |
| Gen Dev Foundations | CUNY-AI-Lab/gen-dev-foundations | cuny-ai-lab.github.io/gen-dev-foundations |
| Vibecoding Prototypes | CUNY-AI-Lab/vibe-coding-prototypes | cuny-ai-lab.github.io/vibe-coding-prototypes |
| CUNY AI Intro | zmuhls/cuny-ai-intro | (standalone presentation) |

The aggregator repo `milwrite/slide-decks` contains all org repos as subdirectories plus the `template/` directory.

---

## Layout Reference (Quick)

| Layout Class | Use Case | Panels |
|-------------|----------|--------|
| `title-slide` | Opening/closing | Centered, logo + title + meta |
| `layout-full-dark` | Roadmap, takeaways | Dark bg, centered |
| `layout-split` | Concept + visual, exercise + template | Left content + right stage |
| `layout-content` | Text-heavy, discussion | Full-width, light bg |
| `layout-divider` | Section breaks | Dark, large heading |
| `layout-grid` | Card grids, comparisons | Grid-2 cards |

## Interactive Patterns

| Pattern | HTML | Trigger |
|---------|------|---------|
| Step reveal | `class="step-hidden" data-step` | Forward advance |
| Stream bullets | `<ul class="stream-list">` | Slide activation |
| Carousel | `<div class="carousel" data-interval="8000">` | Auto-advance |
| Prompt progression | `.prompt-bad` / `.prompt-mid` / `.prompt-good` | Step reveal |
| Copyable template | `<pre id="template-N">` + `copyTemplate()` | Button click |
| Tabs | Tab component via `js/tabs.js` | Click |
