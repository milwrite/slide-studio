# Composing System Prompts — Slide Deck Content

> CUNY AI Lab Sandbox Workshop
> Source: `index.html`
> Upstream: <https://github.com/CUNY-AI-Lab/system-prompting>

---

## Slide 1 — Title (`title-slide`)

**Composing System Prompts**
A Workshop for the CUNY AI Lab Sandbox
March 16, 2026

---

## Slide 2 — Workshop Roadmap (`layout-full-dark`)

**Three Weeks, Three Skills**

| Date | Session | Description |
|------|---------|-------------|
| March 16 &mdash; This Week | Composing System Prompts | Learn how system prompts can help orchestrate and constrain AI models to address specific course goals |
| March 23 | Curating Knowledge Collections | Upload syllabi, readings, and relevant sources to ground AI models in course materials |
| March 30 | Customizing Skills & Tools | Build specialized skills, tools, and workflows tailored to your courses |

---

## Slide 3 — What Is a System Prompt? (`layout-split`)

**Label:** The Basics

A **system prompt** is a set of instructions you write *before* a conversation begins. The user never sees it, but it shapes every response the AI gives.

> Think of it as the **setup instructions** for a specialized tool: what task it handles, how it should operate, what guardrails to follow, and what's out of scope.

**Key distinction:** The *user prompt* is what someone types in the chat. The *system prompt* is what you, the instructor, compose behind the scenes to guide the AI's behavior across the entire conversation.

---

## Slide 4 — Where It Lives (`layout-split`)

**Label:** Open WebUI

In the CUNY AI Lab Sandbox, you'll find the system prompt field in two places:

1. **Chat-Level System Prompt** — Click the settings gear at the top of any new conversation to set a prompt for that session.
2. **Model Configuration** — Go to **Workspace → Models** to create a custom model with a permanent system prompt baked in.

**Note:** Next time, we'll create custom models that combine your system prompt with a knowledge collection of course materials to ground the AI in your teaching context.

**Carousel:**
- `4-a.png` — Chat-level System Prompt field
- `4-b.png` — Workspace → Models page
- `4-c.png` — Model editor with System Prompt field under Model Params

---

## Slide 5 — Why System Prompts Matter (`layout-content`)

**Label:** Why Bother?

You're building a revision support tool for your composition course. A student opens it and types: **"I'm stuck on my introduction."**

**Without System Prompt** *(shown first)*
"Here's a strong introduction you could use: 'Throughout history, the question of racial identity has been central to American culture. In this essay, I will argue that...'"
*Writes the introduction for them. The student learns nothing.*

**With System Prompt** *(revealed on advance)*
"Let's work through this. What's the main argument you want your reader to encounter first? Try telling me in one sentence what your paper is really about. Don't worry about making it perfect yet."
*Prompts the student to think. The writing stays theirs.*

---

## Slide 6 — Models in the Sandbox (`layout-full-dark`)

**Label:** Your Toolkit

All open-weight models. No data retained on external servers. Each has different constraints and affordances.

| Model | Description |
|-------|-------------|
| DeepSeek V3.2 | Large model; strong at working through complex, multi-step tasks |
| Kimi K2.5 | Reads text, images, and video; good for visual and long-document tasks |
| GLM 5 | Large model; extended step-by-step reasoning and tool use |
| gpt-oss-120b | OpenAI's open-weight model; lightweight and efficient |
| Qwen3 235B | Large model; strong multilingual support across 100+ languages |
| Gemma 3 27B | Smaller, faster model; good for everyday tasks and quick responses |
| Llama 3.1 70B | Mid-size model; reliable general-purpose option |

---

## ~~Slide 7 — Section Divider~~ (removed — "Part I" label folded into slide 8)

---

## Slide 7 — Composition: The Vague Prompt (`layout-content`, Part I &mdash; Example 1)

**Section label:** Part I &mdash; Example 1: Composition & Writing
**Label:** Composition & Writing · progression 1/3 (bad)

**Weak:** `Help students write better.`

**What goes wrong?**
- No role assignment to contextualize the model for specific workflows or domain knowledge
- No boundaries or pedagogical guidance to constrain the model from doing work for students
- No success criteria for the model to optimize toward

---

## Slide 8 — Composition: Getting Warmer (`layout-content`)

**Label:** Composition & Writing · progression 2/3 (mediocre)

**Getting There:**
```
You are a writing scaffold for a college composition course. Help students develop their essays by breaking revision into structured steps. Ask them to identify their thesis before giving feedback. Don't write essays for them.
```

**What improved?**
- Assigns a role and disciplinary context
- Includes a basic pedagogical move
- Sets one boundary

**What's still missing?**
- No procedural instructions for *how* to give feedback
- No awareness of student population or course level
- No edge-case handling

---

## Slide 9 — Composition: A Prompt That Fosters Revision (`layout-content`)

**Label:** Composition & Writing · progression 3/3 (strong)

```
You are a writing scaffold for an English 101 composition course at a public urban university. Students are drafting a position paper on rhetoric in popular media and must revise their first draft in preparation for their final submission.

The core problem: students treat revision as proofreading, fixing grammar and word choice, rather than rethinking argument, structure, and evidence. They lack a process for examining whether their ideas are clear, well-organized, and sufficiently supported. This tool scaffolds the move from surface-level fixes to substantive revision.

Procedure:
1. Request the assignment prompt and student draft before responding.
2. Identify the highest-priority concerns (thesis clarity, structure, evidence) before surface-level issues.
3. For each concern, ask the student a question rather than providing a fix.

Constraints:
- Never generate text that could substitute for the student's own writing.
- If asked to "just fix it," redirect toward a specific revision step.
- Do not grade or evaluate.
- Tone: Warm and direct. Use "I notice..." and "What if you tried..."
```

---

## ~~Slide 11 — Section Divider~~ (removed — "Example 2" label folded into slide 10)

---

## Slide 10 — History: The Vague Prompt (`layout-content`, Example 2)

**Section label:** Example 2: Primary Source Analysis
**Label:** History · progression 1/3 (bad)

**Weak:** `Analyze historical documents.`

**What goes wrong?**
- No methodological framework
- No period or geographic focus
- No guidance on handling hallucinated facts or invented sources

---

## Slide 11 — History: Getting Warmer (`layout-content`)

**Label:** History · progression 2/3 (mediocre)

**Getting There:**
```
You are a history source-analysis tool. Help students analyze primary sources from American history. Ask them to consider the author, audience, and context of each document. Don't just summarize the document for them.
```

**What improved?**
- Assigns a role and disciplinary scope
- References a real methodology
- Sets a boundary against summarization

**What's still missing?**
- No procedural steps for guiding analysis
- No handling of uncertainty or AI limitations
- No attention to historiographical perspective

---

## Slide 12 — History: A Prompt That Fosters Historical Thinking (`layout-content`)

**Label:** History · progression 3/3 (strong)

```
You are a source-analysis tool for an undergraduate U.S. history survey covering the period from Reconstruction through the Civil Rights Movement.

The core problem: students extract facts from sources rather than analyzing them as constructed arguments shaped by author, audience, and context.

Procedure (based on Wineburg's historical thinking heuristics):
1. Ask the student to identify the source before proceeding.
2. Guide them through the four moves below, one at a time. Never jump ahead.
3. After each move, ask why that detail matters and prompt them to ground their response in specific passages.
4. After all four moves, ask the student to synthesize: what does the full picture reveal about this historical moment?

Four Moves:
- Sourcing — Before reading: who created this, when, and why?
- Contextualization — What was happening at the time and place this was produced?
- Close Reading — What does the text actually say, and what does it leave out?
- Corroboration — How does this source compare to others from the period?

Constraints:
- Never offer guidance before the student has attempted an answer.
- If unsure about a historical fact, say so. Never invent dates, names, or events.
- Never provide a complete analysis. Ask the next question a historian would ask.
- Tone: Patient and curious.
```

---

## ~~Slide 15 — Section Divider~~ (removed — "Example 3" label folded into slide 13)

---

## Slide 13 — Literature: The Vague Prompt (`layout-content`, Example 3)

**Section label:** Example 3: Close Reading & Literary Analysis
**Label:** Literature & Cultural Studies · progression 1/3 (bad)

**Weak:** `Help with literary analysis.`

**What goes wrong?**
- Defaults to plot summary
- No theoretical or critical framework
- No requirement for textual evidence

---

## Slide 14 — Literature: Getting Warmer (`layout-content`)

**Label:** Literature & Cultural Studies · progression 2/3 (mediocre)

**Getting There:**
```
You are a close-reading scaffold. Help students analyze literary texts by focusing on themes, symbolism, and narrative techniques. Don't just summarize the plot. Ask students to point to specific passages.
```

**What improved?**
- Names specific analytical categories
- Addresses the plot-summary problem
- Requires textual evidence

**What's still missing?**
- No procedural steps for scaffolding analysis
- No critical or theoretical framework
- No attention to cultural context

---

## Slide 15 — Literature: A Prompt That Fosters Close Reading (`layout-content`)

**Label:** Literature & Cultural Studies · progression 3/3 (strong)

```
You are a close-reading tool for an introductory English course focused on cultural studies and literary analysis.

The core problem: students default to summarizing content or importing biographical and historical context rather than attending closely to how the text works — how language, form, imagery, and internal tension generate meaning within the artifact itself.

Procedure:
1. Ask what the student notices about the language in their chosen passage.
2. Prompt them to examine specific textual features (word choice, imagery, syntax, point of view) and how they create meaning.
3. Ask how the passage connects to the work's larger themes and cultural moment.
4. Guide them toward an interpretive claim grounded in textual evidence.

Framework:
- Treat the text as a self-contained object. Bracket authorial intent and historical context.
- Look for tension, irony, paradox, and ambiguity as sites of meaning.
- Once a close reading is underway, invite students to reflect on the method itself.

Constraints:
- Facilitate multiple interpretations grounded in textual evidence. Do not prescribe a correct reading.
- If a student reaches for biographical context, redirect: "What in the language itself supports that reading?"
- Tone: Encouraging and accessible. Affirm observations, then push deeper.
```

---

## ~~Slide 19 — Section Divider~~ (removed — "Part II" label folded into slide 16)

---

## Slide 16 — Core Components of a System Prompt (`layout-content`, Part II)

**Section label:** Part II: The Building Blocks
**Label:** Structure

Each system prompt is built from modular components. We'll draft yours one piece at a time.

1. **Context & Problem** — What course, what students, what learning challenge?
2. **Procedure** — What steps should the tool follow?
3. **Constraints** — What should it refuse to do, and how should it redirect?
4. **Tone** — What register and affect should it use with your students?
5. **Output Format** — How should it structure its responses?

---

## Slide 17 — Component 1: Context & Problem (`layout-split`)

**Label:** Component 1

Name the tool, the course, the students, and the specific learning challenge. Everything else follows from this.

- What kind of tool is this?
- Who are your students?
- What learning challenge does it address?

**Template (copyable):**
```
You are a [tool type] for [course name].
Students are [relevant context].

The core problem: [specific learning challenge].
```

**Your turn:** Write 2-3 sentences naming what the tool does, who the students are, and what learning challenge it addresses.

---

## Slide 18 — Component 2: Procedure (`layout-split`)

**Label:** Component 2

Tell the tool what to do, step by step. Numbered steps give the model a clear sequence rather than a loose set of suggestions.

- What should the tool request before responding?
- What should it prioritize?
- How should it respond to each student input?

**Template (copyable):**
```
Procedure:
1. Ask the student for [specific input] before responding.
2. Identify [priority concern] before addressing [secondary concerns].
3. For each issue, [specific action, e.g. ask a question rather than fix it].
```

**Your turn:** Write 2-4 numbered steps.

---

## Slide 19 — Component 3: Constraints (`layout-split`)

**Label:** Component 3

Define what the tool should not do and how it redirects when students push against those limits.

- What will students ask it to do *for* them?
- How should it redirect instead?
- What uncertainty should it name explicitly?

**Template (copyable):**
```
Constraints:
- Never [specific output to avoid].
- If asked to [common student request], redirect by [specific alternative].
- If uncertain about [domain content], say so explicitly.
```

**Your turn:** Write 2-3 constraints that keep the tool from doing work students should do themselves.

---

## Slide 20 — Component 4: Tone (`layout-split`)

**Label:** Component 4

One sentence on tone shapes how the tool communicates with every student it encounters.

- What register fits your students?
- Should it feel warm, direct, encouraging?
- Are there phrases that model the right affect?

**Template (copyable):**
```
Tone: [Adjective and adjective]. Use phrases like "[example phrase]" and "[example phrase]."
```

**Your turn:** Add one sentence describing the tone.

---

## Slide 21 — Component 5: Output Format (`layout-split`)

**Label:** Component 5

Optional, but useful when consistent structure helps students know what to expect.

- Should each response end with a question?
- Should it follow a fixed structure?
- What length is appropriate?

**Template (copyable):**
```
Format each response as:
Observation: [what you notice]
Focus: [one thing to work on]
Next step: [a specific, actionable suggestion]
Question: [something for the student to consider]
```

**Your turn:** If it fits your tool, add a short format spec. Not every prompt needs one.

---

## ~~Slide 26 — Section Divider~~ (removed — "Part III" label folded into slide 22)

---

## Slide 22 — Advanced Strategies (`layout-grid`, Part III)

**Section label:** Part III: Advanced Strategies & Tips

- **Conditional Behavior** — "If the student submits a draft, focus on structure before style. If they ask a yes/no question, reframe it as an open one."
- **Conversational Brevity** — "Respond to one thing at a time. Ask one question, wait for the student's response, then proceed."
- **Epistemic Guardrails** — "If you are not certain about a factual claim, explicitly state your uncertainty. Never fabricate citations or attribute quotes."
- **Multilingual Support** — "If a student writes in a language other than English, respond in that language. Offer to discuss concepts in both languages."

---

## Slide 23 — Common Pitfalls (`layout-content`)

**Label:** Watch Out

- **Too Long & Too Detailed** — A 2,000-word system prompt can confuse the model. Prioritize the most important instructions.
- **Contradictory Instructions** — "Always give detailed feedback" + "Keep responses under 50 words" = conflicting behavior.
- **Forgetting the Student's Perspective** — Test it by asking the kinds of questions your students actually ask.
- **Set It and Forget It** — System prompts need iteration. Use them, notice what breaks, revise.

---

## Slide 24 — The Road Ahead (`layout-full-dark`)

| Date | Session | Description |
|------|---------|-------------|
| March 16 &mdash; Today | System Prompts ✓ | Wrote and tested a system prompt grounded in your course objectives |
| March 23 &mdash; Next Week | Knowledge Collections | Upload course materials to ground AI models in your specific assignments and readings |
| March 30 | Skills & Tools | Build custom skills, tools, and workflows tailored to your courses |

---

## Slide 25 — Closing

**Let's Build One Together**

1. Pick one assignment from your course
2. Write a system prompt using the five components
3. Test it with a real student question
4. Iterate and refine

ailab.gc.cuny.edu

---

_Last synced: 2026-03-22. Upstream: CUNY-AI-Lab/system-prompting @ main. Update both files together._

---

## Changelog

### 2026-03-22 (milwrite/slide-decks sandbox)
- Added `data-slide` attribute reference (proposed; not yet in upstream HTML)
- Roadmap description updated: replaced anthropomorphic framing with faculty-centered language
- Section dividers collapsed: slides 7, 11, 15, 19, 26 dropped; section labels folded into subsequent content slides
- Road Ahead description (March 16 "Today" row): updated to active past tense ("Wrote and tested...") replacing "Defining how the AI thinks and teaches"
