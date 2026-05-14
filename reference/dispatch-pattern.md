# reference/dispatch-pattern.md

## The role boundary

**The orchestrator dispatches. The worker builds. Crossing the boundary is the bug.**

That's the whole rule. The rest of this file is what it looks like in practice.

## What an orchestrator does

- Picks the work. (Which campaign, which audience, which deadline.)
- Writes the brief. (A six-section file at `briefs/<date>-<slug>.md`.)
- Dispatches the worker with one paste prompt. ("Read the brief at briefs/2026-XX-XX-slug.md and execute.")
- Verifies the artifact came back matching the brief — dimensions, palette, copy, accessibility floor.
- Runs the rasterization step if a PNG/JPG is required for the publication platform.
- Moves the artifact to its destination (the platform where it gets published).
- Logs the dispatch outcome.

Orchestration is the 30% layer in the 60-30-10 framework. Its output is briefs and routing decisions, not artifacts.

## What the worker does

- Reads the brief.
- Reads the visual-system configuration.
- Loads only the reference slices the brief names (a logo file, a product photography note, a previous campaign artifact for the brief to reference, etc.).
- Runs the pre-flight checklist.
- Produces the self-contained HTML artifact(s).
- Verifies against the brief's success criteria — dimensions correct, palette correct, copy text verbatim, accessibility checks pass, no external fetches.
- Updates `STATUS.md`.
- Hands back.

The worker's output is artifacts. Not briefs, not calendars, not routing decisions, not other workers' work.

## What "crossing the boundary" looks like

Markers, in order of severity:

1. **The orchestrator Reads any file longer than 100 lines without an offset + limit.** Single most reliable signal. If the orchestrator is Reading whole reference docs, it's hoarding context it won't dispatch with.
2. **The orchestrator opens the worker's `reference/` "to check."** No. The worker reads its own reference. The orchestrator points to it.
3. **The orchestrator produces any artifact other than a brief.** Sketched layouts, half-built CSS, "let me mock it up quickly" — all worker territory. If the orchestrator is generating HTML, it has crossed.
4. **The worker writes its own brief.** "Let me figure out what would look good here" — that's orchestration work. The worker stops and asks for a real brief.
5. **The worker extends scope.** "I also made a story variant" or "here's an OG card that pairs with it" — that's the worker picking work, which is orchestration. The worker delivers what the brief asked for, nothing more.
6. **The worker proposes multiple concepts.** "I made three versions for you to choose from" — that's the worker pushing concept-selection back to the orchestrator after the orchestrator already dispatched. If the brief was complete, one composition is the answer.
7. **The dispatch step takes longer than the worker's actual job.** If briefs take fifteen minutes and the worker ships in five, the orchestrator is doing 60% of the work twice.

If any of these show up, the fix is not to make either side faster. It's to make each side do less.

## The brief-as-contract

The brief is the only context the worker needs. If a brief is complete, the worker can cold-start, produce, and hand back without any conversation. If a brief is incomplete, the worker stops and writes a question file — does not "do its best."

A complete brief has six sections:

1. **What to produce** — the deliverable list, in concrete terms: artifact type + pixel dimensions + format (HTML).
2. **Audience** — who sees this, on what platform, what they already know, what they care about.
3. **Format and constraints** — copy text (verbatim), assets to use (filenames in `reference/`), accessibility floor, structural requirements.
4. **Visual-system override (or default)** — which visual-system config to use, or "use the default in `reference/visual-system.md`."
5. **Success criteria** — what "done" looks like, grep-able.
6. **Refusal context if relevant** — any claims, third-party identity, real-person depictions, or domains where the worker should be especially careful.

See `briefs/_BRIEF-TEMPLATE.md` for the actual template. The template is the contract shape; the brief's content varies per dispatch.

## Why this matters in practice

Most starter kits fuse orchestrator and worker into one Claude session. The reader pastes a vague intent ("make a social card for the launch"), Claude has to guess dimensions, audience, copy, palette, structural choices, and then produce, all in one shot. The Claude session ends up Reading everything in the repo to compensate for the missing brief.

Two failure modes follow:

1. **Token burn.** The session loads context speculatively, because nothing tells it what it actually needs.
2. **Inconsistent output.** With no brief to verify against, the artifact's "rightness" is a vibe. Some days it lands; some days it doesn't. The variance is the cost of the boundary not being enforced.

Splitting orchestrator from worker means the worker's job becomes mechanical: given a complete brief, produce an artifact that matches the success criteria. The variance moves where it belongs — into the brief, where the orchestrator can iterate cheaply.

## When you're acting as your own orchestrator

You probably are, while you're getting started. That's fine — the boundary still applies. Practical discipline:

- **Write the brief in a separate session or before opening the worker.** Don't write the brief inside the same Claude session that will produce the artifact. Use a markdown file. Iterate on it. When it's complete, dispatch.
- **Don't carry on a "what should we make today" conversation with the worker.** That's orchestration. Either you do it in your head, or you build a separate orchestrator session that talks to a campaign calendar.
- **Watch for the marker symptoms above.** When you see one in your own workflow, treat it the same way you'd treat a code smell.

## When to build a real orchestrator

You build a separate orchestrator session — its own folder, its own `CLAUDE.md`, its own job — when:

- You're dispatching the same kind of work routinely (daily, weekly).
- The brief-writing itself has become the bottleneck.
- You want a calendar-aware system that proposes briefs based on what's scheduled.

The orchestrator's CLAUDE.md says, in essence: "Read today's row from the calendar. Write a brief per worker that's due. Output the paste prompts. Stop." That's the whole job. The orchestrator doesn't produce artifacts. The orchestrator doesn't load worker contexts. The orchestrator points.

The other two repos in the operator-stack series (`your-content-worker`, `your-animation-worker`) follow the same pattern. Once you have all three, you can build one orchestrator that dispatches to all three from a single morning routine.

---

Last updated: 2026-05-14.
