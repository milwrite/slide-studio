# Sample Prompts for CAIL Slide Deck Builder

## 1. Generate a Complete Deck from Scratch

```
Create a 90-minute workshop deck on "Designing AI-Assisted Writing Assignments" for the CUNY AI Lab faculty series.

Workshop details:
- Part 2 of a 3-part series: (1) System Prompting, (2) This workshop, (3) Agentic Tools
- Date: April 7, 2026
- Authors: Dr. Sarah Chen, Zach Muhlbauer
- Series: AI Literacy for Faculty

Slide content:
---SLIDE 1 (title)---
title: Designing AI-Assisted Writing Assignments
subtitle: A Workshop for the CUNY AI Lab Sandbox

---SLIDE 2 (roadmap)---
session_1: March 16 | System Prompting | Crafting instructions that shape AI behavior
session_2: April 7 - This Week | Writing Assignments | Designing assignments that integrate AI thoughtfully
session_3: April 21 | Agentic Tools | Building custom tools and automated workflows

---SLIDE 3 (split)---
label: Context
title: Why Redesign Assignments?
body: Student AI use is already widespread. Rather than policing tool access, faculty can redesign assignments so AI becomes part of the learning process instead of a shortcut around it.
quote: The goal is not to prevent AI use but to make the assignment worth doing even with AI available.
key_distinction: "AI-proof" assignments are a losing strategy. "AI-integrated" assignments teach critical thinking about tool use.
stage: Diagram showing assignment redesign spectrum from "ban AI" to "require AI"

[continue for remaining slides...]
```

## 2. Edit a Single Slide (Copyedit)

```
On slide 7, change the discussion prompt from:
"What challenges might arise when using AI in your discipline?"
to:
"How does AI-generated text differ from student-generated text in your field? What would you look for?"
```

## 3. Add a New Slide

```
Add a new slide between slides 8 and 9. It should be a layout-split with:
- Label: "Case Study"
- Title: "History Department Example"
- Left panel: A before/after comparison of a primary source analysis assignment
- Right panel: Student work samples showing the difference

Use step-reveal so the "after" version appears on click.
```

## 4. Swap Theme Colors

```
Change the label color scheme from cyan to amber for all slides in Part II (slides 7-11). Keep Part I and Part III as cyan.
```

## 5. Generate SLIDES.md from Existing Deck

```
Here is my current index.html:
[paste full file]

Generate the matching SLIDES.md markdown mirror for this deck.
```

## 6. Structural Review

```
Review my deck structure and suggest improvements:
- Are the section dividers well-placed?
- Does the exercise section have enough scaffolding?
- Are interactive patterns (step reveal, carousel, stream list) used effectively?
- Any accessibility issues?
```

## 7. Quick Content Fill

```
I have a workshop on "Critical Play: Games That Expose AI Limitations" and I need slides 3-5 filled.

Slide 3: Explain what "critical play" means in the context of AI literacy. The concept comes from Mary Flanagan's work on games as sites of cultural production.

Slide 4: Show three game mechanics that expose AI weaknesses: (1) rapid context switching, (2) creative constraint puzzles, (3) adversarial prompt chains. Use a carousel with placeholder screenshots.

Slide 5: Before/after comparison of a generic quiz vs. a critical play exercise. Step reveal for the "after" version.
```
