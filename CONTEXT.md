# CONTEXT.md — routing and load logic

This file is the 30% orchestration layer made explicit. It tells the worker which reference files to load for which kind of dispatch, so context doesn't get loaded speculatively.

`CLAUDE.md` is the entry point — read first. This file is referenced from there.

## Why this file exists

In the 60-30-10 framework (see `reference/icm-layer-model.md`), Orchestration is 30% of the workflow's value: templates, rules, decision logic, the connective tissue that makes raw tools useful for a specific job. Most starter kits put orchestration logic *inside* Claude's reasoning — "decide which file to read." That's expensive and inconsistent.

This file moves that decision out of Claude's working context and into a declarative rule list. Worker reads this once, applies the rule, loads only what the rule says to load. No speculation.

## The dispatch types this worker handles

| Dispatch type | What it produces | Files to load (in this order) |
| --- | --- | --- |
| **Social card** | One self-contained HTML at a single set of pixel dimensions (e.g., 1080×1080 IG, 1080×1350 IG portrait, 1080×1920 story) | `reference/visual-system.md` → the brief → any product/copy reference the brief names |
| **OG card** | One self-contained HTML at 1200×675 (Open Graph default) or the dimensions the brief specifies | `reference/visual-system.md` → the brief → any product/copy reference the brief names |
| **Preview page** | One self-contained HTML page (typically 1280-wide responsive) used as a sales-page hero, a landing-page preview, or a presentation slide | `reference/visual-system.md` → the brief → any product/copy reference |
| **Diagram** | One self-contained HTML or inline SVG illustrating an architecture, a flow, a comparison, or a relationship | `reference/visual-system.md` → the brief → any structural reference the brief names |
| **Sequence** | A set of related artifacts (social-card + OG + story variant + preview) from one brief, dispatched as one job | `reference/visual-system.md` → the brief → any product/copy reference |

Add a row above when you (the operator) start dispatching a new kind of work to this worker. Don't let the routing live in Claude's head — declare it here.

## What never gets loaded automatically

- The full `briefs/` history. Each dispatch is grounded by ONE brief. Loading prior briefs is speculative context — exactly the mistake that burns tokens.
- Examples from `examples.md` — that file is for *you* to study the worker's behavior, not for the worker to study its own past output. Past output is usually noise during a fresh dispatch.
- Output files from prior dispatches. Same reason.
- Any file under `output/` — write-only from the worker's perspective.
- The `examples/` folder — that's the worked-example artifacts shipped with the repo, illustrative, not a reference library.

The worker reads what the brief requires, and nothing more. If a brief says "produce a story variant of the launch card we shipped last week," the brief itself names that prior card; the worker reads only the named file, not the whole archive.

## Slice-not-file Read habit (mechanical rule)

For any reference file longer than 100 lines:

1. `grep -n "<section-header>" reference/<file>.md` — find the line number.
2. `Read reference/<file>.md` with `offset=<line>` and `limit=<lines-needed>` — pull only the slice.

Never `Read` a 600-line reference file to find the 30 lines that matter. That's the single most expensive habit in this kind of build.

## When the operator configures a new dispatch type

1. Add a row to the dispatch-type table above.
2. Decide which reference files load by default. Be miserly.
3. If a new reference file is needed (asset catalog, photography library, illustration library, presentation-template set), add it under `reference/` with a clear filename.
4. Add a one-line note to `STATUS.md` so the worker knows the routing changed.

## Open routing question (for the operator, not the worker)

If a brief names a dispatch type that isn't in the table above, the worker writes a question file at `briefs/questions/<slug>-question.md` and stops. It does not invent a routing decision on the fly. Add the row first, dispatch second.

---

Read this file when in doubt about what to load. Read `CLAUDE.md` first.
