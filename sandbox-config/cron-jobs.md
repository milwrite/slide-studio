# Cron Jobs: CAIL Slide Deck Builder Pipeline

Five scheduled jobs that support the workspace configuration workflow in OpenWebUI. These map to the five stages of the slide deck lifecycle: sourcing, validation, quality, sync, and delivery.

---

## Job 1: Template Sync Check (Weekly, Monday 9 AM ET)

**OpenWebUI mapping:** Knowledge Collection freshness
**Purpose:** Pull latest from CUNY-AI-Lab org repos and compare against `milwrite/slide-decks/template/`. Flag any drift so the Knowledge Collection stays current.

```json
{
  "name": "cail-template-sync",
  "schedule": { "kind": "cron", "expr": "0 9 * * 1", "tz": "America/New_York" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Check template sync status for the CAIL Slide Deck Builder. Pull latest from CUNY-AI-Lab/system-prompting and CUNY-AI-Lab/knowledge-collections on GitHub. Compare against /home/milwrite/slide-decks/template/ and the two subdirectories. Report any files where the org repos have newer content. If drift exists, list the changed files and suggest whether to update the template or the org repo. Post findings to #orchestra.",
    "timeoutSeconds": 300
  },
  "delivery": { "mode": "announce", "channel": "discord", "to": "channel:1467738838846799953" }
}
```

---

## Job 2: Knowledge Collection Validation (Weekly, Monday 10 AM ET)

**OpenWebUI mapping:** Model > Knowledge binding integrity
**Purpose:** Verify that the files in `template/` match what should be uploaded to the OpenWebUI Knowledge Collection. Check file counts, sizes, and content hashes. Catch cases where someone edited the template locally but forgot to re-upload.

```json
{
  "name": "cail-knowledge-audit",
  "schedule": { "kind": "cron", "expr": "0 10 * * 1", "tz": "America/New_York" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Audit the CAIL Deck Template knowledge collection files. Check /home/milwrite/slide-decks/template/ for: (1) index.html exists and has [BRACKETED PLACEHOLDER] markers or real content, (2) SLIDES.md exists and has matching slide count, (3) CLAUDE.md exists with architecture reference, (4) css/ has styles.css + responsive.css + animations.css, (5) js/ has deck-engine.js + carousel.js + scrubber.js + tabs.js. Report file sizes and last-modified dates. Flag any missing files or unexpected changes.",
    "timeoutSeconds": 180
  },
  "delivery": { "mode": "announce", "channel": "discord", "to": "channel:1467738838846799953" }
}
```

---

## Job 3: Accessibility Audit (Weekly, Wednesday 2 PM ET)

**OpenWebUI mapping:** Model output quality (WCAG compliance)
**Purpose:** Scan all active slide decks in `milwrite/slide-decks/` for accessibility issues. Check aria attributes, alt text, focus management, color contrast references, and reduced-motion support.

```json
{
  "name": "cail-a11y-audit",
  "schedule": { "kind": "cron", "expr": "0 14 * * 3", "tz": "America/New_York" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Run an accessibility audit on all slide decks in /home/milwrite/slide-decks/. For each subdirectory containing an index.html (system-prompting/, knowledge-collections/, template/): (1) Check every .slide div has role='group', aria-roledescription='slide', and aria-label with correct numbering. (2) Verify #slide-announcer exists with aria-live='polite'. (3) Check all img tags have alt attributes. (4) Verify .sr-only and .skip-link utilities exist in CSS. (5) Check css/animations.css has prefers-reduced-motion media query. (6) Flag any decorative emoji missing aria-hidden='true'. Report issues by deck and slide number.",
    "timeoutSeconds": 300
  },
  "delivery": { "mode": "announce", "channel": "discord", "to": "channel:1467738838846799953" }
}
```

---

## Job 4: SLIDES.md Drift Detection (Daily, 8 AM ET)

**OpenWebUI mapping:** System prompt instruction compliance (sync rule)
**Purpose:** Compare `index.html` slide content against `SLIDES.md` for each deck. The system prompt mandates these stay in sync; this job catches drift before it compounds.

```json
{
  "name": "cail-slides-md-sync",
  "schedule": { "kind": "cron", "expr": "0 8 * * *", "tz": "America/New_York" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Check SLIDES.md sync status for all decks in /home/milwrite/slide-decks/. For each subdirectory with both index.html and SLIDES.md: (1) Count slides in index.html (number of .slide divs). (2) Count slide sections in SLIDES.md (## Slide N headers). (3) Compare slide titles between the two files. (4) Flag any count mismatch or title drift. Only report if issues are found; stay silent if everything is in sync.",
    "timeoutSeconds": 180
  },
  "delivery": { "mode": "announce", "channel": "discord", "to": "channel:1467738838846799953" }
}
```

---

## Job 5: Workshop Prep Pipeline (Friday 3 PM ET)

**OpenWebUI mapping:** Prompt suggestions + Model card readiness
**Purpose:** Ahead of the weekend, check if any upcoming workshops need deck preparation. Review the CUNY-AI-Lab/workshops repo for new or updated workshop entries. Flag workshops that have an outline but no slide deck yet. Suggest which template layout patterns would fit.

```json
{
  "name": "cail-workshop-prep",
  "schedule": { "kind": "cron", "expr": "0 15 * * 5", "tz": "America/New_York" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Workshop prep check for CUNY AI Lab. (1) Fetch the latest state of https://github.com/cuny-ai-lab/workshops — list all workshop directories and their current status (has slides? has outline? has resource.md?). (2) Cross-reference against /home/milwrite/slide-decks/ to see which workshops already have slide decks built. (3) For any workshop with an outline.md but no corresponding slide deck, suggest which template layouts would fit their content and estimate the number of slides needed. (4) Report to #orchestra with a summary table.",
    "timeoutSeconds": 300
  },
  "delivery": { "mode": "announce", "channel": "discord", "to": "channel:1467738838846799953" }
}
```

---

## Installation

To activate these cron jobs, run each through the OpenClaw `cron add` action. Example for Job 1:

```
cron add --job '{ "name": "cail-template-sync", "schedule": { "kind": "cron", "expr": "0 9 * * 1", "tz": "America/New_York" }, ... }'
```

Or ask Quimbot/Petrarch to set them up in a main session.

## Mapping to OpenWebUI Workspace Features

| Cron Job | OpenWebUI Feature | What It Validates |
|----------|------------------|-------------------|
| Template Sync | Knowledge Collection | Files are current with upstream |
| Knowledge Audit | Knowledge > Files | Upload integrity, nothing missing |
| A11y Audit | Model output quality | Generated decks meet WCAG 2.1 AA |
| SLIDES.md Drift | System prompt rule #5 | index.html ↔ SLIDES.md sync enforced |
| Workshop Prep | Prompt suggestions | New workshops get recommended patterns |
