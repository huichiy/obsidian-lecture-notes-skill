# Workflow

How the lecture-notes skill fits into a study cycle, from first PDF to night-before-exam revision.

## The 5-stage study cycle

```
┌─────────────┐    ┌──────────┐    ┌────────────┐    ┌─────────────┐    ┌───────────────┐
│ Get lecture │ →  │ GENERATE │ →  │ Review +   │ →  │ Build out   │ →  │ Revision      │
│ material    │    │ note     │    │ atomic     │    │ Canvas +    │    │ pack +        │
│ (PDF/PPT)   │    │ + lint   │    │ extraction │    │ Tracker     │    │ self-test     │
└─────────────┘    └──────────┘    └────────────┘    └─────────────┘    └───────────────┘
```

### Stage 1 — Get lecture material

Drop the PDF / PPT / slide deck / screenshot into Claude Code chat. The skill auto-triggers on file upload. If it doesn't, type *"use the lecture-notes skill on this lecture."*

### Stage 2 — Generate the lecture note

The **GENERATE** mode produces a single `.md` file with:

- YAML frontmatter (subject, course code, exam_weight, status, tags)
- 5-second TLDR + Lecture Map callout
- Opening `mindmap` of all concepts
- Per-concept blocks (intro → diagram → table → memory hook → exam bullets → worked example)
- Flashcard bank (10–25), cheat sheet, self-test
- Footer with MOC backlink + prev/next lecture links

At the end, the skill **asks** whether to extract atomic concept notes. Reply with concept numbers (e.g. `1, 3, 6`), `all`, `recommended`, or `no`.

### Stage 3 — Optional: lint the new note

Say *"lint my OOAD notes"* to run 12 health checks (YAML, MOC backlink, wikilink count, flashcard count, diagrams, prev/next resolution, naming consistency, etc.) and get a markdown report. The report ends by offering **interactive auto-fix** for safe issues (`all` / `safe-only` / pick by number).

### Stage 4 — Build out the visual system

Once you have a few lectures + atomic concepts (≥ 5 atomic per subject):

- **Knowledge Map** — say *"make a canvas for OOAD"*. Subject-wide overview: MOC, all lectures, all atomic concepts, hierarchical layout, with cross-reference dotted edges between related concepts.
- **Tracker** — say *"make a tracker for OOAD"*. Generates an Obsidian Bases `.base` file: 5 views (all / exam priority / still to revise / done / cards). Update `status:` and `exam_weight:` in lecture YAMLs and the tracker reflects live.

### Stage 5 — Revision

- **Exam Map** — *"make an exam map for OOAD"*. Filtered Knowledge Map showing only `exam_weight: high`, with Confident / Weak / Notes drag-zones at the bottom. Use during revision to physically organize what you've mastered vs need to review.
- **Exam Revision Pack** — *"exam pack for OOAD Lec 3-7"* — consolidates multiple lectures into one document: merged TLDRs, high-frequency concepts, merged cheat sheet, all formulas, exam-likely concepts list, combined self-test. Open this the night before.

## Other entry points

- **Backfill atomic notes from old lectures** — *"extract atomic for OOAD"*. Reads existing `.md` lecture notes (no PDF re-processing) and generates atomic concept files retroactively.
- **Fix existing notes** — *"lint and fix my notes"* — runs lint + offers auto-fix in one go.

## Tips

- **YAML matters.** All four other modes (tracker, canvas, revision, lint) rely on accurate YAML. Don't edit YAML by hand carelessly — let lint catch issues.
- **Atomic notes are infrastructure.** Skipping atomic extraction is fine for one lecture, but a subject with no atomics gives a sparse canvas and no cross-lecture linking. Run `extract atomic` after the third or fourth lecture to bootstrap.
- **MOC is hand-curated.** The skill doesn't auto-update the MOC after every generate. Either update it yourself, or run lint periodically (the C4 check flags missing MOC entries).
- **Use Style Settings + CSS snippet** for `#level/1` / `#level/2` / `#level/3` concept tag pills — purely cosmetic but makes hierarchy visible at a glance.

## What this skill is NOT for

- **Live writing assistance** — it produces structured notes from given material, not blank-canvas composition.
- **Math derivations** — formulas are extracted; full mathematical proofs aren't generated.
- **Code project management** — for SE projects use a regular task tracker.
- **Replacing your own thinking** — the per-concept template and atomic note structure work best when you read them through and edit. AI is the first draft.
