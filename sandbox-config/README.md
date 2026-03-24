# sandbox-config/

OpenWebUI Workspace configuration for the **CAIL Slide Deck Builder** running on the CUNY AI Lab Sandbox (chat.ailab.gc.cuny.edu) with Kimi k2.5.

## Files

| File | Purpose |
|------|---------|
| `SPEC.md` | Full system specification: components, input format, edit protocol, reference repos |
| `system-prompt.md` | The system prompt to paste into the Model Builder |
| `model-file.json` | Model configuration (name, description, params, capabilities) |
| `knowledge-collection.md` | Instructions for creating and populating the Knowledge Collection |
| `sample-prompts.md` | Seven example prompts demonstrating all usage modes |
| `SKILL.md` | Agent skill for precise line edits on single index.html files |
| `cron-jobs.md` | Five scheduled jobs covering sync, validation, a11y, drift, and prep |

## Setup Order

1. Create the Knowledge Collection per `knowledge-collection.md`
2. Create the Model per `model-file.json` (or paste the system prompt from `system-prompt.md`)
3. Bind the Knowledge Collection to the Model
4. Set up cron jobs per `cron-jobs.md`
5. Test with prompts from `sample-prompts.md`

## Architecture

```
User → OpenWebUI Chat → CAIL Slide Deck Builder (Kimi k2.5)
                              ↓
                    Knowledge Collection (RAG)
                    ├── index.html (Full Context)
                    ├── SLIDES.md  (Full Context)
                    ├── CLAUDE.md  (Full Context)
                    ├── css/*      (Focused Retrieval)
                    └── js/*       (Focused Retrieval)
                              ↓
                    Output: filled index.html + SLIDES.md
```
