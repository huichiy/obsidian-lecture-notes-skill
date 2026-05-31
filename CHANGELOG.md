# Changelog

All notable changes to the lecture-notes skill. Format roughly follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.3] — 2026-05-31 (feature-complete milestone)

### Added
- **MOC auto-update in GENERATE** — After saving a lecture (and any chosen atomic notes), the skill locates `{SUBJECT} - MOC.md`, finds the lecture list section, plans an insertion in lecture-number order, shows a unified-diff preview, and only writes after explicit `yes`. Eliminates the most common LINT C4 finding ("lecture missing from MOC") for new lectures generated through the skill. Safe-by-design: skips MOCs with complex layouts (Dataview blocks, mixed lists), never touches custom user-curated sections, never overwrites.
- **Atomic note worked-example boost** — Atomic concept notes now include block 6 (worked example) from the source lecture's 6-block in addition to intro + diagram + key points. An atomic note opened in isolation now shows both *what it is* and *how it actually runs* — usable for cross-lecture revision without bouncing back to the lecture. Applies in both GENERATE's opt-in atomic extraction and EXTRACT-ATOMIC backfill.
- **Style Settings variable expansion** in `styles/concept-levels.css` — added pill text color, vertical padding, font size, font weight, letter spacing, border width / style / color, drop shadow (none / subtle / medium / strong), uppercase toggle. Existing color + radius + horizontal padding variables retained. All exposed in the Style Settings plugin UI.

### Changed
- `modes/generate.md`: new "After atomic extraction: offer MOC auto-update" section at the end; atomic-extraction body description updated to spell out the new 4-section structure (intro / diagram / key points / **worked example** / mentioned in / related concepts).
- `modes/extract-atomic.md`: extraction procedure step 2 now extracts blocks 1, 2, and 6 (was 1 and 2). Atomic note template gains a `## Worked example` section.

### Roadmap
- This release closes out the planned v1.3 scope (MOC auto-update + atomic quality + Style Settings expansion). v1.3 is the **feature-complete milestone**; beyond this the skill enters maintenance mode (bug fixes, new content-type rules as users hit new subjects, trigger-phrase additions). See `docs/roadmap.md` for the explicit out-of-scope list.

## [1.2.1] — 2026-05-31

### Added
- **Dotted weak-edge style** in Canvas via Advanced Canvas plugin `styleAttributes.path = "dotted"`. Visually separates `parent/containment` strong edges from `wikilink cross-reference` weak edges.
- Bases tracker template syntax fix: drop the `file.` prefix from frontmatter property references (Bases treats `file.*` as built-in File props, not YAML).

### Changed
- `modes/canvas.md`: spec out the dotted-line behaviour as part of CANVAS mode rules, with full JSON example.
- `README.md`: mark Advanced Canvas as the dotted-line provider.

## [1.2] — 2026-05-31

### Added
- **`modes/tracker.md`** — Obsidian Bases database view generator. Universal `.base` template (`templates/subject-tracker-template.base`); AI auto-substitutes the subject code based on user input.
- **`modes/extract-atomic.md`** — Backfill atomic concept notes from existing lecture notes without re-processing PDFs. Candidate detection filters trivial single-mention concepts.
- **CANVAS Exam Map variant** — Filters Knowledge Map to `exam_weight: high` only; adds bottom-row Confident / Weak / Notes annotation zones for drag-and-drop revision.
- **Interactive Lint auto-fix** — After delivering the lint report, offers `all` / `safe-only` / pick-by-number fixes with preview diff before applying.
- **`styles/concept-levels.css`** — Pill styling for `#level/1` (purple), `#level/2` (blue), `#level/3` (red) tags, with Chinese aliases (`#一级` / `#二级` / `#三级`). Includes `@settings` block for the Style Settings plugin.

### Changed
- Router (`SKILL.md`) description extended with TRACKER + EXTRACT-ATOMIC + Exam Map keywords.
- README: replaced "stubs" disclaimer with full mode reference table; added Recommended Plugins section.

## [1.1.1] — 2026-05-30

### Fixed
- **revision-mode formula extraction**: tightened regex to require explicit LaTeX markers (`\frac`, `\sum`, `_`, `^`, Greek letters, etc.) so prose patterns like `$80k but median = $58k` no longer get swept up as math.
- **canvas-mode pre-flight**: the `< 5 atomic` warning must actually stop and ask for user confirmation in chat instead of silently proceeding.
- **canvas-mode MOC-absent handling**: when no `{SUBJECT} - MOC.md` exists in scope, use a text placeholder node instead of failing.
- **generate-mode atomic parent rule**: `parent:` field must reference the **exact filename** of the just-saved lecture (with any `(new)` or other suffix), not a guessed cleaner version.

## [1.1] — 2026-05-30

### Added
- **Router + modes architecture**: `SKILL.md` becomes a 30-line dispatcher; rules live in `modes/{generate,lint,canvas,revision}.md`. Future modes added by creating a file + adding a router row.
- **LINT mode** (real implementation, not stub) — 12 health checks, scope auto-detection, 3-layer prev/next link check with git history support, chat report + optional save-to-file.
- **CANVAS mode** (real implementation) — Knowledge Map variant: hierarchical subject-level `.canvas` with MOC + lectures + atomic concepts, strong / weak edges, color coding by exam_weight.
- **REVISION mode** (real implementation) — Multi-lecture exam revision pack with TLDR roll-up, high-frequency concepts, merged cheat sheet, exam-likely list, formulas, combined self-test.
- **Content-type conventions** replace subject-keyed domain rules — works for humanities, medicine, history, law, in addition to CS/sciences.
- **Opt-in atomic concept-note extraction** at the end of GENERATE.

### Changed
- Skill packaging from single `.skill` bundle to **Claude Code folder format**.
- README: rewritten for Claude Code installation + expanded feature list.

## [1.0] — 2026-05-20

### Added
- Initial single-file `lecture-notes.skill` bundle for claude.ai upload.
- 6-block per-concept template, YAML frontmatter, Mermaid diagram selection by concept type, flashcard bank, mini cheat sheet, self-test, anti-import rule, subject-keyed domain conventions (OOAD / SE / Stats-DS-ML / Algo).
- Universal `subject-MOC-template.md` with Dataview queries.
