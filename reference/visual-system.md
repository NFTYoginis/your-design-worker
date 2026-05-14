# reference/visual-system.md

This is the file the worker reads every dispatch to know how to compose.

You (the reader who forked this repo) edit this file with your own visual rules. The worker uses whatever's here. The default below is a working starter — neutral, runnable as-is — so the worked example in `examples.md` produces real output the first time you dispatch. **Replace it with your own visual system once you've seen the worker behave.**

Marker for the operator: if the worker stops with "visual-system file is still showing the placeholder," it means this file matches the default below verbatim. Even small edits — your own palette codes, your own type family — flip the worker to "configured." If you want the default to count as configured for your fork, delete this marker block and the line "Replace with your own visual system…" above. The worker's gate checks for that line.

---

## Visual system — default working set

### Palette

Named colors with hex codes. Every color used in an artifact must appear here. If a brief references a color name, this file is the source of truth.

| Token | Hex | Use |
| --- | --- | --- |
| `--bg` | `#f6f5f0` | Default background — warm near-white |
| `--ink` | `#111111` | Body text, primary type, dark surfaces |
| `--muted` | `#6a6862` | Secondary text, captions, meta |
| `--accent-warm` | `#ff5436` | Primary accent — buttons, kickers, key callouts |
| `--accent-cool` | `#1f4dff` | Secondary accent — used sparingly for contrast or emphasis |
| `--rule` | `#d8d4c8` | Dividers, faint grid lines, table borders |
| `--surface` | `#ecebd6` | Soft surface for inset blocks, swatch chips |

Banned color usage: do NOT introduce hex codes outside this table without adding them to the table first. If a brief asks for a color that isn't here, the artifact uses the closest token in the table, or the worker writes a question.

### Typography

| Role | Family | Weight | Sizes |
| --- | --- | --- | --- |
| **Display** | system-ui, "Inter", "Helvetica Neue", Arial, sans-serif | 700 / 800 | 48-96px in hero, 32-48px in section headlines |
| **Body** | system-ui, "Inter", "Helvetica Neue", Arial, sans-serif | 400 / 500 | 15-18px paragraph, 14px caption |
| **Mono** | ui-monospace, "SF Mono", "JetBrains Mono", Menlo, monospace | 400 / 600 | 11-14px labels, 12-14px code |

Tracking: tight on display (`letter-spacing: -0.02em`), normal on body, loose on mono labels (`letter-spacing: 0.12em` uppercase).

Line-height: 1.0-1.1 on display, 1.45-1.6 on body, 1.55 on mono.

Real fonts only via system stack — no external `<link rel="stylesheet">` to Google Fonts or any CDN. Self-contained means self-contained.

### Grid

| Setting | Value |
| --- | --- |
| **Max content width** | 1080px on social square, 1200px on OG, 1280px on preview pages |
| **Columns** | 12, with 16-24px gutters |
| **Outer padding** | 64px on social/OG artifacts, 48-80px on preview pages |
| **Baseline grid** | 4px (everything snaps to multiples of 4) |

Composition rule: anchor each artifact's primary content to a 12-col grid; let secondary content (kicker labels, specimen captions) sit on the grid lines visibly. Make the grid feel intentional.

### Density

| Element | Rule |
| --- | --- |
| **Headlines** | Single sentence, ≤8 words for social, ≤12 words for OG. Period at end optional. |
| **Kicker** | Uppercase, mono, 12px, accent-warm. 1-3 words. |
| **Body paragraph** | ≤2 sentences in any one block. Longer prose belongs in a preview page, not a social card. |
| **CTA / URL** | One per artifact. Mono. Bottom of frame. |
| **Negative space** | Generous. If the artifact feels crowded, cut a line. |

### Components

| Component | Rule |
| --- | --- |
| **Swatch chip** | A 64×64 colored square with the hex code printed below in mono. Used in the hero of any "system" or "palette" artifact. |
| **Specimen label** | Mono, uppercase, 11px, accent-warm. Used above section headers as `01 ── WHAT IT IS` etc. |
| **Grid markers** | Faint vertical lines at column edges, rendered at 1px in `--rule`. Visible in hero band; fade in lower sections. |
| **Frame edge** | Hard corner. No drop shadow. No rounded edges above 4px. Cards and chips have sharp edges. |
| **Divider** | 1px horizontal rule in `--rule`. No double-rules, no decorative dividers. |

### What this visual system feels like

> Clean, intentional, slightly diagrammatic. Like a type-specimen catalog or a software design-system docs page. Reads as a designer's tool tray, not a marketing brochure. Generous whitespace; everything labeled; nothing decorative for its own sake.

### What this visual system does NOT feel like

> Gradient-heavy SaaS landing pages. Curve-everywhere "friendly tech" aesthetics. Stock-photo-driven lifestyle imagery. Glassmorphism. Emoji-as-icon usage. Neumorphism. Anything that reads as "designed by Canva."

If the composition starts drifting toward the second example, stop and re-read this file.

---

## TODO for the operator (edit these before serious dispatch)

- [ ] Replace the palette table with your brand's actual color tokens and hex codes.
- [ ] Replace the typography table with your brand's actual font choices (keep the system-stack pattern — no external font fetches).
- [ ] Adjust the grid rules to your brand's standard layouts.
- [ ] Adjust density rules to match your brand's tone (loose-and-spacious vs. dense-and-editorial).
- [ ] Replace the "What this looks like" / "What this doesn't" pair with your own.

Once those edits are made, this file is yours. The worker will compose against it on every dispatch.

## What NOT to put in this file

- Brand history, founder bio, company mission. (Different file: write a `reference/brand-context.md` if you need it.)
- Audience personas. (Different file: `reference/audience.md`.)
- Product imagery directory listings. (Different file: `reference/assets.md` with named filenames the worker can reference.)
- Voice and prose rules. (Different worker: `your-content-worker`.)

Visual system = how the artifact looks. Anything else lives in its own reference file.

---

Last updated: 2026-05-14 (default working set; replace with your own rules).
