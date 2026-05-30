# Obsidian Lecture Notes Skill

A [Claude Skill](https://docs.claude.com) that turns raw lecture material (PDFs, slide decks, screenshots, or pasted text) into structured Obsidian notes — with Mermaid diagrams, wikilinks for Graph View, YAML frontmatter, flashcards, and a cheat sheet.

Built for university students who use Claude + Obsidian as their study stack. Packaged as a folder-format skill for **Claude Code**.

<!-- TODO: add a screenshot here of a generated note rendered in Obsidian -->

---

## Why this exists

University lectures arrive as PDFs and slide decks. Most note workflows fall into one of two failure modes:

1. **Paste slides into Obsidian** — fast, but useless for revision. No structure, no recall practice, no exam preparation.
2. **Manually restructure each lecture** — produces good notes, but takes hours per chapter.

This skill automates the second approach. Upload a lecture → get back an Obsidian-ready `.md` with:

- A consistent 6-block per-concept template (intro → diagram → table → memory hook → exam bullets → worked example)
- A diagram type picked from a decision table based on the actual concept type (not a default)
- Aggressive `[[wikilinks]]` so Obsidian Graph View becomes useful — recurring concepts become visually larger nodes (a free heuristic for "exam-likely")
- YAML frontmatter that feeds Obsidian Properties and Dataview queries
- A flashcard bank, mini cheat sheet, and self-test at the end

## Features

- **Subject-agnostic.** Detects subject from filename, course code, or content. Works for any university subject across any semester (CS, sciences, humanities, medicine, law) — not hardcoded to one curriculum.
- **15+ diagram types** mapped to concept types: `classDiagram`, `sequenceDiagram`, `stateDiagram-v2`, `flowchart`, `mindmap`, `erDiagram`, `gantt`, and more.
- **Content-type aware conventions** — rules trigger on what the content *is* (formulas, algorithms, taxonomies, timelines, schools of thought, case studies, cause-and-effect chains) rather than what the subject is named. Works for psychology, history, medicine, law as well as CS.
- **Strict YAML self-check** before delivery — prevents the common one-line-jam bug that breaks Obsidian Properties.
- **Anti-import rule** — when given a sample note from a different subject as a style reference, copies structure only, not diagrams or content. Stops cross-subject pollution.
- **Graph View optimization** — 8-20 wikilinks per note minimum, MOC backlinks, prev/next chaining between lectures.
- **Universal MOC template** with Dataview queries that auto-populate across subjects — one template works for every subject, no manual list maintenance.
- **Atomic concept notes (on-demand)** — after generating a lecture note, the skill offers to extract individual concept files for cross-lecture linking and Canvas use. Opt-in per lecture, no auto-pollution.
- **Router architecture** — main entry plus pluggable modes (`generate`, `lint`, `canvas`, `revision`). Currently only `generate` is fully implemented; the others are stubs reserved for upcoming releases.

## Requirements

- [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) installed
- [Obsidian](https://obsidian.md) for using the notes
- *Optional:* [Dataview plugin](https://github.com/blacksmithgu/obsidian-dataview) for auto-generated MOC pages

## Installation

Clone or copy this repo's skill folder into your Claude Code skills directory:

```bash
git clone https://github.com/huichiy/obsidian-lecture-notes-skill.git
mkdir -p ~/.claude/skills
cp -R obsidian-lecture-notes-skill ~/.claude/skills/lecture-notes
```

Or symlink the cloned repo if you want git updates to flow through automatically:

```bash
ln -s "$(pwd)/obsidian-lecture-notes-skill" ~/.claude/skills/lecture-notes
```

Claude Code auto-discovers skills under `~/.claude/skills/`. No restart needed — the next chat will pick it up.

To verify: ask Claude Code "list skills" or upload a lecture file and it should auto-trigger.

## Usage

In Claude Code, drop a lecture file (PDF, PPTX, screenshots, or pasted text) into the chat → the skill auto-triggers from the context.

If it doesn't auto-trigger, just say:

> Use the lecture-notes skill on this lecture.

The output is a single `.md` file dropped into your Obsidian vault. After delivery, the skill offers to extract atomic concept notes — say which ones (or `all` / `no`) to opt in per lecture.

### Other modes (planned)

The skill description also covers three additional modes that are stub-only in v1.1:

- **Lint** — say *"lint my OOAD notes"* / *"audit my vault"* to validate YAML, wikilinks, MOC backlinks, flashcard counts, etc.
- **Canvas** — say *"make a canvas for OOAD"* / *"knowledge graph for [subject]"* to emit a `.canvas` JSON map of the subject.
- **Revision pack** — say *"exam pack for OOAD Lec 3-7"* to consolidate multiple lectures into one revision document.

Currently each of these will pause and ask you to design the rules before running. Real implementations land in upcoming releases.

### Recommended vault structure

```
Vault/
├── [SUBJECT-CODE] - MOC.md           ← parent / Map of Content page
├── [SUBJECT-CODE] Lec 01 — Topic.md
├── [SUBJECT-CODE] Lec 02 — Topic.md
└── ...
```

A MOC (Map of Content) template is included in `templates/`. See the next section to set one up per subject.

### Set up a subject MOC (one-time per subject)

The repo includes a universal `templates/subject-MOC-template.md` that uses [Dataview](https://github.com/blacksmithgu/obsidian-dataview) queries to auto-populate as you add lecture notes. **One template works for every subject** — no need to duplicate logic per course.

1. Copy `subject-MOC-template.md` into your vault
2. Rename it to match your subject — e.g., `OOAD - MOC.md`, `DSF - MOC.md`. This exact format matters because every lecture note's footer links to `[[SUBJECT-CODE - MOC]]`.
3. Open the file, change `subject: SUBJECT_CODE` in the YAML to your actual subject code (e.g., `subject: OOAD`)
4. Replace `[Subject Full Name]` in the H1 heading
5. Save

From then on, every lecture note you save with matching `subject:` YAML automatically appears in the MOC. The template includes:

- **Auto-listed lectures table** (sorted by lecture number, with topic, exam weight, and status columns)
- **Exam-priority filter** — shows only lectures flagged `exam_weight: high`
- **Revision status tracker** — groups lectures by `draft` / `reviewed` / `complete`
- **Concept frequency view** — counts how often each `[[wikilink]]` appears across the subject's lectures. Concepts mentioned in 5+ lectures are almost certainly exam-important — this is the data equivalent of what Graph View shows visually.

If you don't use Dataview, the template still works as a manual hub page — just edit the lecture list by hand.

## Customization

This skill is designed to be forked. All rules live in `modes/generate.md`. Common customizations:

### Add a new content-type rule

Rules are keyed off **what the content is** (formulas, algorithms, timelines, schools of thought, case studies, etc.) rather than what subject the lecture belongs to. Open `modes/generate.md` → find `## Content-type conventions` → add your trigger and rule block following the same pattern. Examples already cover: formulas, algorithms, code, taxonomies, timelines, school comparisons, people contributions, cause-and-effect, case studies, definitions, processes, UML.

### Change the per-concept template

The default 6 blocks are: plain-English intro, diagram, compact table, memory hook, exam bullets, worked example. Edit `## Per-concept 6-block template` in `modes/generate.md` to add, remove, or reorder.

### Adjust YAML schema

Default fields: `subject`, `course_code`, `lecture`, `title`, `source`, `tags`, `aliases`, `related`, `exam_weight`, `status`. Add or remove based on what your Dataview queries need.

### Add a new mode

Create a new file under `modes/`, then add a routing row to `SKILL.md` so Claude knows when to dispatch to it.

## How it works

A Claude Skill is a folder containing a `SKILL.md` with YAML frontmatter (`name`, `description`) and markdown instructions. Claude Code reads the description at the start of every session — when a user's input matches what the description claims to handle, the body of the skill loads into context and Claude follows its instructions.

This skill uses a **router + modes** pattern. `SKILL.md` is a small dispatcher that detects which mode the user wants (generate / lint / canvas / revision) and then reads the corresponding file under `modes/`. Each mode file is a self-contained instruction set — so Claude only loads the rules relevant to the current request, and modes can be edited independently without affecting each other.

The active **GENERATE** mode instructs Claude to:

1. Detect the subject from the lecture material (filename, course code, or content)
2. Apply a strict output structure: YAML → title → TLDR → Lecture Map → mindmap → TOC → per-concept blocks → flashcards → footer
3. Pick diagrams from a decision table tied to concept types, not defaults
4. Apply content-type rules (formulas / algorithms / timelines / schools of thought / case studies / etc.)
5. Densely wikilink concepts so Obsidian Graph View becomes informative
6. Run a self-check before delivering the file
7. Offer atomic concept-note extraction after delivery (opt-in per lecture)

See `modes/generate.md` for the full instruction set.

## Project structure

```
obsidian-lecture-notes-skill/
├── SKILL.md                       ← router (mode detection)
├── modes/
│   ├── generate.md                ← lecture material → Obsidian note (active)
│   ├── lint.md                    ← health-check notes (stub)
│   ├── canvas.md                  ← generate .canvas knowledge map (stub)
│   └── revision.md                ← exam revision pack (stub)
├── templates/
│   └── subject-MOC-template.md
├── LICENSE
└── README.md
```

## Known limitations

- **Triggering is occasionally inconsistent.** If Claude responds without using the skill, say *"use the lecture-notes skill"* explicitly.
- **Mermaid syntax sometimes has subtle errors.** Verify diagrams render correctly by previewing in Obsidian before relying on them.
- **Large lectures (50+ slides) may need two passes** — process first half → second half → ask for a merged cheat sheet.
- **Subject detection can mis-fire** on lectures with no filename, course code, or title slide. Provide subject context when needed.
- **This release targets Claude Code.** The folder format works directly in Claude Code's skills directory. If you need a claude.ai-uploadable bundle (`.skill` file), you'll need to package it yourself — that workflow isn't included in this repo.
- **Lint, Canvas, and Revision modes are stubs.** They acknowledge your request and pause to design the rules — they don't actually run yet.

## Contributing

Issues and PRs welcome. If you fork this for your own subjects, feel free to share — the skill is built to be personalized.

## Credits

- Built on [Anthropic's Claude Skills](https://docs.claude.com)
- Diagrams via [Mermaid](https://mermaid.js.org)
- Inspired by [Nick Milo's PKM principles](https://www.linkingyourthinking.com) (Maps of Content, atomic notes)

## License

MIT
