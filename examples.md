# examples.md

Worked examples for this worker. Each example pairs a brief (under `briefs/`) with the artifacts the worker produced from it (under `examples/`). Studying these tells you what the worker is good at, what to expect, and where the boundary is.

You can reproduce any example by dispatching the paired brief with the default visual-system configuration intact. The composition is deterministic-enough that the output will be substantially the same — minor pixel-level variation, same dimensions, same palette, same copy verbatim, same accessibility floor honored.

---

## Example 1 — FocusBlock launch sequence (industry-neutral)

**Paired brief:** [`briefs/2026-05-14-example.md`](briefs/2026-05-14-example.md)

**Job:** A three-artifact launch sequence for a focus-timer / deep-work scheduler called FocusBlock. The artifacts are a 1080×1080 social card, a 1200×675 OG card, and a 1280×900 preview page. Visual system: default in `reference/visual-system.md`.

**Worker's pre-flight check (transcribed):**

1. Brief is loaded and complete — six sections present.
2. Visual system is loaded — default working set, not the placeholder marker.
3. Domain context is loaded — product facts, copy text, dimensions, and asset constraints are all in the brief.
4. Dimensions and format are explicit — three artifacts at 1080×1080, 1200×675, 1280×900, all HTML.
5. Refusal gates are clear — no high-harm domain, no third-party identity, no text-on-asset claims requiring external sources. Brief is in-contract.

All five pass. Producing.

**Worker's output (the artifacts):**

| Artifact | File | Dimensions |
| --- | --- | --- |
| Social card | [`examples/social-1080.html`](examples/social-1080.html) | 1080×1080 |
| OG card | [`examples/og-1200x675.html`](examples/og-1200x675.html) | 1200×675 |
| Preview page | [`examples/preview.html`](examples/preview.html) | 1280×900 |

Open each file in a browser to see the composition. To rasterize any of them to PNG for a platform that demands a PNG:

```bash
# macOS / Linux with Chromium installed
chromium --headless --no-sandbox \
  --screenshot=output/social.png \
  --window-size=1080,1080 \
  examples/social-1080.html
```

That rasterization step is infrastructure (60%), not AI (10%). The worker doesn't run it; you do, from your shell or your CI.

**Worker's self-verification (transcribed):**

| Success criterion | Pass / Fail | Notes |
| --- | --- | --- |
| Pixel dimensions exactly as specified across all three | Pass | `body { width; height; }` set explicitly per artifact. |
| Every color used is from the palette table | Pass | All five tokens (`--bg`, `--ink`, `--muted`, `--accent-warm`, `--rule`) come from `reference/visual-system.md`. No off-palette hex codes introduced. |
| Every font in use is in the typography table | Pass | System sans stack for display + body; system mono stack for labels + CTAs. No `<link>` to external fonts. |
| Body-on-background contrast ≥ 4.5:1 | Pass | `--ink #111111` on `--bg #f6f5f0` measures ~19:1; `--muted #6a6862` on `--bg` measures ~5.0:1. Both above AA. |
| Headline-on-background contrast ≥ 3:1 | Pass | Headline uses `--ink` on `--bg`, same ~19:1 ratio. |
| All visible text is real HTML text | Pass | No `<img>` tags carrying text. Headlines, kickers, sub copy, swatch labels, CTAs all composed in HTML. |
| Zero external network requests | Pass | `grep -E "https?://" examples/*.html` returns only the displayed CTA text (`focusblock.example/download`), which is plain text, not a fetched resource. No `<link>`, no `<script src>`, no external `<img src>`. |
| Files load via `file://` | Pass | All three artifacts open standalone in a browser with no network tab activity. |
| Copy verbatim across all three artifacts | Pass | Kicker `LAUNCH · v1.0`, headline `Schedule the focus. Stop renting the willpower.`, sub, CTA — all identical wording across artifacts. |
| Social card legible at thumbnail (160px wide) | Pass | Verified by browser zoom to 15%: headline still scannable, kicker still visible, CTA recognizable as a URL. |

All criteria pass. Handing back.

**`STATUS.md` line written:**

> 2026-05-14 — focusblock-launch-visuals — shipped. Three artifacts: social-1080.html, og-1200x675.html, preview.html. All dimensions correct, palette in-table, contrast above AA, zero external fetches. Visual system: default.

---

## What this example demonstrates

- **Brief-as-contract, multi-artifact edition.** One brief produces three artifacts because the brief defined the set. No "should I also make a story version?" conversation. The brief said three; the worker made three.
- **Cross-artifact consistency by configuration, not by memory.** Each artifact reads the same `reference/visual-system.md`, so the palette and type match without the worker having to remember its own past output. If the campaign needs to extend to a story variant later, the operator writes a new brief; the visual system is already shared.
- **HTML as source, PNG as infrastructure.** The artifact files are HTML. The worker does NOT call a rasterizer, save a PNG, or attempt to produce binary output. Rasterization is one shell command on the operator side, runnable any time.
- **Pre-flight checklist explicitly cleared before composition.** Five gates checked, all passed, then the worker built. Real dispatches will sometimes trip a gate — this example shows the clean path; the question-file path is illustrated below.
- **Self-verification against grep-able criteria.** The worker checked dimensions, palette tokens, contrast ratios, and external-fetch count against the brief's success criteria before declaring shipped. The criteria were named in the brief; the worker just ran them.

## What a "stop and write a question file" example would look like

We're not shipping a second worked example in this starter — the architecture is the same; the demonstration would just show a question file instead of artifacts. For reference, the trigger is one of the five refusal gates in `rules.md`. If the brief above had asked for the social card to render a `4.9 ★ on the App Store · 12,000 reviews` badge — and the brief had not provided a source for those numbers — the worker would have stopped and written:

> `briefs/questions/focusblock-launch-visuals-question.md`:
>
> Brief: `briefs/2026-05-14-example.md`
>
> What's missing: The brief asks the social card to render: `4.9 ★ on the App Store · 12,000 reviews`. This claim appears as readable text on the artifact and requires a source the brief does not provide. The brief's "Product context" section does not include rating or review-count data.
>
> What's needed: Either (a) add the source for the rating and review-count numbers (App Store listing URL, dated screenshot, or similar), or (b) confirm the badge is illustrative and authorize an on-asset label ("illustrative · pre-launch comp"), or (c) remove the badge from the brief.
>
> Status of partial work: none, stopped before producing.

The worker would update `STATUS.md` with a "blocked" line and stop. The operator would either ground the claim, mark it illustrative, or remove it — then re-dispatch.

This is the path you should expect for a meaningful share of real dispatches when you're still calibrating your brief-writing habits. It's working as intended.

---

Last updated: 2026-05-14 (initial worked example).
