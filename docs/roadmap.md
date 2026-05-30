# Roadmap

Where the skill has been and where it's going.

## Current — v1.2.1 (feature-complete)

Six modes, fully implemented:

- **GENERATE** — lecture material → structured Obsidian note, with opt-in atomic concept extraction at the end.
- **LINT** — 12 health checks across YAML / wikilinks / MOC / diagrams / prev-next / naming, with interactive auto-fix flow.
- **CANVAS** — `Knowledge Map` (full overview) and `Exam Map` (filtered to exam-priority + revision drag-zones) variants. Advanced Canvas dotted weak edges.
- **REVISION** — multi-lecture exam revision pack: TLDR roll-up, high-frequency concepts, merged cheat sheet, exam-likely list, formulas, combined self-test.
- **TRACKER** — Obsidian Bases (`.base`) database view of all lectures in a subject. Five views: All / Exam Priority / To Revise / Done / Cards.
- **EXTRACT-ATOMIC** — backfill atomic concept notes from previously generated lecture notes. No PDF re-processing.

Plus:

- Universal MOC + Tracker templates (subject-substitutable).
- CSS snippet for `#level/1` / `#level/2` / `#level/3` concept tag pills (Style Settings compatible).
- Content-type conventions covering formulas / algorithms / code / taxonomies / timelines / schools-of-thought / case studies / people contributions / cause-and-effect / definitions / processes / UML.

## v1.3 — feature-complete milestone

Items still worth doing without breaking the architecture:

- **MOC auto-update** — When GENERATE finishes, append the new lecture to `{SUBJECT} - MOC.md` automatically (with a safe diff preview before writing, to avoid stomping user-curated sections).
- **Atomic-note quality boost** — Currently atomic notes carry the concept's plain-English intro + diagram. v1.3 will also include the worked example (block 6 of the 6-block template) for richer cross-lecture standalone use.
- **Style Settings expansion** — More CSS variables in the snippet: font family, pill border style, padding, shadow toggle.
- **Additional content-type rules** — As users hit lectures in subjects we haven't fully covered (e.g. linguistics, music theory), add rules.

## Beyond v1.3 — maintenance mode

The skill is intentionally **not** trying to grow into an unbounded tool. Features that are explicitly out of scope:

- ❌ **Live writing assistance** (compose new content from scratch) — there are general-purpose Claude prompts for that; lecture-notes is a transformer of given material.
- ❌ **Query mode / Ask-my-notes** — natural-language Q&A across the vault. Different skill (also: Obsidian search + Claude works for this without a custom skill).
- ❌ **AI-inferred pitfalls** — extracting "common mistakes" beyond the `> [!warning]` callouts already in source lectures. AI inference here is unreliable; better to let users mark warnings explicitly.
- ❌ **Auto-extraction of every concept on every lecture** — would pollute the vault. Opt-in extraction is the conscious choice.
- ❌ **Mathematical proofs / derivations** — out of scope; we extract and format what's in the source material.

If your needs hit an "out of scope" item, fork or write a sibling skill — the router + modes pattern is easy to extend independently.

## Versioning

Semantic-ish:

- **Major** (v1 → v2): if the SKILL.md description or any mode's I/O contract changes in a way that breaks existing scripted usage.
- **Minor** (v1.2 → v1.3): adding a new mode, expanding content-type rules, adding new template.
- **Patch** (v1.2 → v1.2.1): bug fixes, syntax corrections, new CSS, doc updates.
