# your-design-worker

A visual-asset Claude worker, ICM-structured, fork-ready. Dispatched a brief, produces self-contained HTML artifacts — social cards, OG cards, preview pages, diagrams — composed against a visual system you configure. MIT licensed.

Live landing page (once you've pushed and enabled Pages): `https://<your-username>.github.io/your-design-worker/`

---

## Why this exists

There are other Claude-worker starter kits. This one is opinionated about five things, each enforced by the architecture rather than by README aspiration:

1. **ICM rigor.** The three always-relevant files — `CLAUDE.md`, `CONTEXT.md`, `STATUS.md` — plus `identity.md`, `rules.md`, `examples.md`, and `reference/`. Not "a CLAUDE.md and hope." Structural, named, enforced.
2. **60-30-10 layer separation.** Infrastructure / Orchestration / AI, in the corrected definition (see `reference/icm-layer-model.md`). AI is the 10%; the other 90% lives in scripts, files, and declarative rules outside Claude. This worker's specific application: **HTML is source, PNG is infrastructure** — rasterization is one shell command, not an AI step.
3. **Brief-as-contract dispatch.** Clean role boundary between orchestrator and worker. The worker is meant to be called, not driven free-form. The brief is durable, audit-trail-friendly, and complete-or-refused — no "I'll do my best with what's missing."
4. **Pages-ready landing.** Every repo IS its own marketing surface. Push it, enable GitHub Pages from `/docs`, public URL in sixty seconds.
5. **Tied to a real article series.** The architecture is documented in the operator-AI series, starting with "I burned 800,000 tokens on one daily routine." Read the article for the receipts; read this repo for the code that runs the fix.

The article: [Article 1 — destination TBD]. Update this link once the Medium URL is live.

---

## What this worker does

| Job | Input | Output |
| --- | --- | --- |
| Social card | Brief naming dimensions, copy, audience | One self-contained HTML at the named dimensions |
| OG card | Brief naming dimensions, copy, target URL | One self-contained HTML at 1200×675 default |
| Preview page | Brief naming structural sections, copy | One self-contained responsive HTML page |
| Diagram | Brief naming the relationship to illustrate | One self-contained HTML with inline SVG |
| Sequence | Brief naming a campaign and the artifact set | A set of self-contained HTML artifacts, internally consistent |

Full job definitions in [identity.md](identity.md). Routing logic in [CONTEXT.md](CONTEXT.md). Worked example with three artifacts produced from one brief in [examples.md](examples.md) and [examples/](examples/).

---

## Why HTML, not PNG

The worker's native output is self-contained HTML — inline CSS, system-stack fonts, no external dependencies. That is a deliberate role boundary:

- HTML is editable. One hex code lives in one place.
- HTML is diffable. Git history of an artifact is meaningful.
- HTML is accessible. Real text, screen-readable, indexable.
- HTML rasterizes deterministically. One shell command produces the PNG.

When a publication platform demands a PNG/JPG, you rasterize:

```bash
# macOS / Linux with Chromium or Chrome installed
chromium --headless --no-sandbox \
  --screenshot=output/social.png \
  --window-size=1080,1080 \
  examples/social-1080.html
```

That command is infrastructure (60%), not AI (10%). It runs from your terminal, your Makefile, or your CI. The worker doesn't do it.

---

## Five-minute setup

### 1. Fork or clone

```bash
git clone https://github.com/NFTYoginis/your-design-worker.git
cd your-design-worker
```

Or click "Fork" on GitHub if you want your own copy under your account.

### 2. Open in a Claude session

Either:

- **Claude Code** in the terminal: `cd` into the folder, run `claude`. The worker reads `CLAUDE.md` as its entry point.
- **Claude Project** (claude.ai): create a project, upload the folder. Same entry point.

### 3. Edit `reference/visual-system.md`

The repo ships with a working neutral default so the worked example dispatches as-is. Replace the default with your own visual system before serious use:

- **Palette** — named tokens with hex codes for every color you use.
- **Typography** — display + body + mono families, sizes, weights, tracking.
- **Grid** — column count, gutters, max-widths per artifact format.
- **Density** — default padding, line-heights, component conventions.
- **What this looks like / doesn't look like** — two paragraphs anchoring the aesthetic.

Five edits. The visual-system file walks you through them.

### 4. Write your first brief

Copy `briefs/_BRIEF-TEMPLATE.md` to `briefs/<today>-<slug>.md`. Fill the six sections:

1. What to produce (artifact list + pixel dimensions + format)
2. Audience
3. Format and constraints (copy text verbatim, asset references, accessibility floor)
4. Visual-system override (or default)
5. Success criteria
6. Refusal context (if relevant)

The brief should fit on one screen. If yours is sprawling, you're orchestrating in the wrong place — split it.

### 5. Dispatch

Paste this into the Claude session:

```
Read the brief at briefs/<your-filename>.md and execute.
```

That's the entire dispatch. The worker takes it from there: reads STATUS, reads the brief, reads the visual system, runs the pre-flight checklist, composes the artifact(s), self-verifies (dimensions, palette, contrast, zero external fetches), updates STATUS.

---

## File map

```
your-design-worker/
├── README.md             ← you are here
├── CLAUDE.md             ← worker entry point (first read every session)
├── CONTEXT.md            ← routing / load logic — the 30% orchestration made explicit
├── STATUS.md             ← first read, last write (you maintain)
├── identity.md           ← who the worker is, who it serves, what it does
├── rules.md              ← always / never / refusal gates / escalation
├── examples.md           ← worked example — study this to understand worker behavior
├── briefs/
│   ├── _BRIEF-TEMPLATE.md          ← copy this for each dispatch
│   └── 2026-05-14-example.md       ← paired with the worked example in examples.md
├── reference/
│   ├── icm-layer-model.md          ← 60-30-10, corrected definition, attributed to Van Clief
│   ├── dispatch-pattern.md         ← orchestrator-worker boundary, explained
│   └── visual-system.md            ← YOU EDIT THIS — palette + type + grid + density
├── examples/                        ← actual HTML artifacts from the worked example
│   ├── social-1080.html            ← 1080×1080 social card
│   ├── og-1200x675.html            ← 1200×675 OG card
│   └── preview.html                ← 1280×900 preview page
├── docs/
│   └── index.html                  ← Pages-ready landing page
├── LICENSE                          ← MIT
└── .gitignore
```

---

## What this worker doesn't do

The full list is in [rules.md](rules.md). Headline items:

- Doesn't orchestrate. Doesn't pick its own subjects, set its own deadlines, or dispatch other workers.
- Doesn't render third-party trademarked logos, mascots, or proprietary illustration styles without consent confirmation in the brief.
- Doesn't depict real, named individuals without consent confirmation.
- Doesn't fabricate text-on-asset claims — statistics, ratings, badges, customer counts. If a claim shows up as text on the artifact, the brief must ground it.
- Doesn't produce fake-UI assets (fake screenshots, fake notification overlays, fake review widgets).
- Doesn't compose against an unconfigured visual system. Empty `reference/visual-system.md` or unedited placeholder triggers a refusal.
- Doesn't produce imagery for high-harm domains without explicit operator authorization in the brief.
- Doesn't extend scope. One brief, the artifact set the brief names. No bonus variants.
- Doesn't fetch external assets at runtime. Self-contained means self-contained.

When any refusal gate fires, the worker writes `briefs/questions/<slug>-question.md` with verbatim quotes from the brief and the specific information needed to proceed. Then it stops.

---

## Push to GitHub + enable Pages

```bash
# 1. Initialize the repo (if you haven't already)
git init
git add .
git commit -m "Initial commit"

# 2. Create the remote and push (assumes gh CLI authenticated)
gh repo create <your-username>/your-design-worker --public --source=. --remote=origin --push

# 3. Enable GitHub Pages serving from /docs
gh api repos/<your-username>/your-design-worker/pages \
  -X POST \
  -f "source[branch]=main" \
  -f "source[path]=/docs"

# 4. Wait ~30-60 seconds, then verify
open https://<your-username>.github.io/your-design-worker/
```

If `gh` CLI isn't authenticated, use the GitHub web UI: Settings → Pages → Source = `main` branch, `/docs` folder → Save.

---

## The series

This repo is one of three in the operator-stack series. Same architecture, different domains:

- **[your-content-worker](https://github.com/NFTYoginis/your-content-worker)** — prose
- **your-design-worker** (this repo) — images, HTML previews, social variants
- **your-animation-worker** — voice-to-video MP4 via Remotion (coming next)

The animation repo's cross-link will be added here once it ships.

---

## Article series

The architecture is documented in:

- **Article 1** — "I burned 800,000 tokens on one daily routine. Here's the architecture that killed it." [Medium URL TBD]

Future articles will link from this section as they publish.

---

## License

MIT. See [LICENSE](LICENSE). Replace `<YOUR NAME OR HANDLE>` in the copyright line when you fork.

## Acknowledgments

- The 60-30-10 layer framework is from Jake Van Clief's ICM (Internal Coherence Maximization) teaching.
- The `docs/index.html` Pages-ready discipline is adapted from the existing public landing-page pattern in this maintainer's earlier worker repositories.

---

Last updated: 2026-05-14.
