# Knowledge Collection: cail-deck-template

## Setup in OpenWebUI

Navigate to **Workspace > Knowledge > + New Collection**

| Field | Value |
|-------|-------|
| Name | CAIL Deck Template |
| Description | Complete HTML slide deck template for CUNY AI Lab workshops. Contains a 16-slide placeholder index.html, SLIDES.md mirror, CLAUDE.md architecture guide, themed CSS, and interactive JS modules. |

## Files to Upload

### Full Context (click to toggle after attaching to model)

These files should be injected in their entirety into every conversation:

| File | Source Path | Purpose |
|------|------------|---------|
| `index.html` | `template/index.html` | 16-slide placeholder template, source of truth |
| `SLIDES.md` | `template/SLIDES.md` | Markdown mirror of all slide content |
| `CLAUDE.md` | `template/CLAUDE.md` | Architecture reference, layout classes, conventions |

### Focused Retrieval (RAG chunks)

These files are queried via RAG when the model needs specific CSS or JS details:

| File | Source Path | Purpose |
|------|------------|---------|
| `styles.css` | `template/css/styles.css` | Design tokens, layout classes, component styles |
| `responsive.css` | `template/css/responsive.css` | Breakpoint overrides (1024, 768, 480px) |
| `animations.css` | `template/css/animations.css` | Keyframes, reduced-motion override |
| `deck-engine.js` | `template/js/deck-engine.js` | Core navigation, keyboard, fragments, focus management |
| `carousel.js` | `template/js/carousel.js` | Auto-advancing screenshot carousels |
| `scrubber.js` | `template/js/scrubber.js` | Timeline slider in nav bar |
| `tabs.js` | `template/js/tabs.js` | Tabbed content panels |

### Optional Reference Files

For additional context, add the TEMPLATE.md spec from the repo root:

| File | Source Path | Purpose |
|------|------------|---------|
| `TEMPLATE.md` | `TEMPLATE.md` (repo root) | Full template specification with all patterns |

## Binding to Model

After creating the collection:
1. Go to **Workspace > Models > CAIL Slide Deck Builder > Edit**
2. Under **Knowledge**, click **+ Add Knowledge**
3. Select **CAIL Deck Template**
4. Click on `index.html`, `SLIDES.md`, and `CLAUDE.md` entries to toggle them to **Full Context** (icon changes from magnifying glass to document)
5. Leave CSS and JS files as **Focused Retrieval** (magnifying glass icon)

## Updating the Collection

When the template is updated upstream:
1. Replace the changed files in the Knowledge Collection
2. OpenWebUI re-indexes automatically
3. No model reconfiguration needed

## Additional Collections (per workshop)

For specific workshops, create separate Knowledge Collections with the course syllabus, readings, and source materials. Attach those alongside the base template collection:

| Collection Name | Contents |
|----------------|----------|
| System Prompting Content | Syllabus, prompt examples, discipline-specific materials |
| Knowledge Collections Content | RAG documentation, workshop exercises, evaluation rubrics |
| Creative Coding Content | Code examples, art history references, tool documentation |
