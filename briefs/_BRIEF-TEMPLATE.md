# Brief — <DATE>-<SLUG>

The brief is the worker's contract. Copy this template to `briefs/<YYYY-MM-DD>-<slug>.md` and fill it in. If a section is genuinely N/A, write "N/A — [one-line reason]" rather than deleting the heading.

---

## 1. What to produce

The deliverable list, in concrete terms. Every artifact gets a type, pixel dimensions, and format (HTML). The worker should be able to grep this section and know exactly what to make.

**Examples of complete answers:**
- "One social card, 1080×1080, HTML."
- "One OG card, 1200×675, HTML. Plus one social square 1080×1080, HTML. Same campaign — see Section 3 for cross-artifact consistency rules."
- "One preview page, 1280px-wide responsive, HTML. Hero + three feature sections + footer."

**Examples of incomplete answers:**
- "Some visuals for the launch." (No artifact types, no dimensions, no format.)
- "A social card." (Which platform? What dimensions?)

---

## 2. Audience

Who sees this. On what platform. What they already know. What they care about.

A useful answer is two or three sentences. A useless answer is "everyone" or "our followers."

**Example:** "Engineers and indie founders on LinkedIn and Hacker News. They scroll fast, they distrust marketing aesthetics, they respond to specific concrete claims and visible structure. The artifact will be seen at small thumbnail size first; if it doesn't communicate the core idea at thumbnail, it loses them."

---

## 3. Format and constraints

Copy text (verbatim). Asset references (named files in `reference/`). Accessibility floor. Structural rules. Anything the worker would otherwise have to guess.

**Examples:**

**Copy:**
- Kicker (above headline): `LAUNCH · v1.0`
- Headline: `Most productivity apps optimize for willpower. FocusBlock optimizes for scheduling.`
- Sub (below headline, if used): `A focus-timer that learns your energy patterns and books focus on your calendar.`
- CTA / URL (bottom-of-frame): `focusblock.example/download`

**Assets:**
- No logo or product photography for this dispatch. Composition is pure type + grid.

**Accessibility floor:**
- Body-on-background contrast ≥ 4.5:1 (WCAG AA for body text).
- Headline-on-background contrast ≥ 3:1 (WCAG AA for large text).
- All visible text is real text in the HTML, not rasterized into images.

**Structural:**
- Social square 1080×1080: kicker top-left, headline center-left flush, CTA bottom-left. 12-col grid markers visible in hero band.
- OG 1200×675: kicker top-left, headline left-aligned dominating the frame, CTA bottom-right.
- Cross-artifact: kicker copy and CTA copy verbatim identical across all artifacts in the sequence; headline can rewrap per dimensions but the wording stays the same.

---

## 4. Visual-system override (or default)

Either:
- "Use the default visual system in `reference/visual-system.md`." (Most common.)
- "Use the visual system in `reference/visual-system-<variant>.md`." (For when you maintain multiple visual configurations — different brands, different campaigns.)
- "Override the default with the following clauses: …" (For one-off tweaks. Use sparingly; if you're overriding repeatedly, edit the visual-system file instead.)

---

## 5. Success criteria

What "done" looks like, in concrete terms the worker can verify itself against before handing back.

**Examples:**
- "Pixel dimensions exactly as specified. Every color used is from `reference/visual-system.md`'s palette table. Every font in use is in the typography table. Body-on-background contrast ≥ 4.5:1, headline-on-background contrast ≥ 3:1. All visible text composed as real HTML text, not images. File loads via `file://` with no external network requests. Kicker and CTA copy verbatim per Section 3."
- "Renders cleanly at thumbnail size (160px wide preview) — headline still legible, kicker still scannable, CTA visible. Tested by viewing the HTML at 1080×1080 then zooming the browser to 15%."

Make this list grep-able. The worker will check each item before declaring shipped.

---

## 6. Refusal context (if relevant)

If this brief touches a domain where the worker needs extra care — text-on-asset claims that need sources, third-party logos or trademarks, real-person likenesses, high-harm domains, regulated-product imagery — note that here. The worker uses this section to decide whether to apply any of the refusal gates from `rules.md`.

If the brief is in a high-harm domain (medical, financial advice targeting vulnerable audiences, mental-health crisis, regulated-product imagery — firearms, gambling, alcohol, tobacco, prescription pharma), this section MUST include the line:

> operator-authorized: I have reviewed the regulatory and platform-policy context for this domain, and I authorize the worker to produce artifacts here.

Without that line, the worker refuses high-harm content.

Otherwise, leave this section empty or write "N/A — no third-party identity, no high-harm domain, no text-on-asset claims requiring external sources."

---

## Notes for the operator (delete this section before dispatch)

- A complete brief should fit in one screen. If yours is sprawling, you're orchestrating in the wrong place.
- The brief is permanent. Once dispatched, it lives in `briefs/` as the contract for what was asked. Don't edit it after the fact.
- If you find yourself writing the brief twice for similar dispatches, the duplicated content probably belongs in a `reference/` file, not in every brief.
- Copy text goes in the brief verbatim. Do not paraphrase; the worker uses what the brief says.
