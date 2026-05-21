# Obsidian Lecture Notes Skill

A [Claude Skill](https://docs.claude.com) that turns raw lecture material (PDFs, slide decks, screenshots, or pasted text) into structured Obsidian notes — with Mermaid diagrams, wikilinks for Graph View, YAML frontmatter, flashcards, and a cheat sheet.

Built for university students who use Claude + Obsidian as their study stack.

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

- **Subject-agnostic.** Detects subject from filename, course code, or content. Works for any university subject across any semester — not hardcoded to one curriculum.
- **15+ diagram types** mapped to concept types: `classDiagram`, `sequenceDiagram`, `stateDiagram-v2`, `flowchart`, `mindmap`, `erDiagram`, `gantt`, and more.
- **Domain-aware conventions** — UML rules for OO subjects, complexity tables for algorithms, formula legends for statistics, code blocks for ML.
- **Strict YAML self-check** before delivery — prevents the common one-line-jam bug that breaks Obsidian Properties.
- **Anti-import rule** — when given a sample note from a different subject as a style reference, copies structure only, not diagrams or content. Stops cross-subject pollution.
- **Graph View optimization** — 8-20 wikilinks per note minimum, MOC backlinks, prev/next chaining between lectures.

## Requirements

- A [Claude](https://claude.ai) account with Skills available (visible in the `+` menu when composing a message)
- [Obsidian](https://obsidian.md) for using the notes
- *Optional:* [Dataview plugin](https://github.com/blacksmithgu/obsidian-dataview) for auto-generated MOC pages

## Installation

1. Download `lecture-notes.skill` from this repo (click the file in the file list, then the **Download raw file** button).
2. In claude.ai, click **+** → **Skills** → **Add skill** → upload the file.
3. Confirm `lecture-notes` is toggled on in the Skills menu.

The skill is now available across every chat and project on your account.

## Usage

Start any chat → upload a lecture (PDF, PPTX, screenshots, or pasted text) → the skill auto-triggers from the context.

If it doesn't auto-trigger, just say:

> Use the lecture-notes skill on this lecture.

The output is a single `.md` file you download and drop into your Obsidian vault.

### Recommended vault structure

```
Vault/
├── [SUBJECT-CODE] - MOC.md           ← parent / Map of Content page
├── [SUBJECT-CODE] Lec 01 — Topic.md
├── [SUBJECT-CODE] Lec 02 — Topic.md
└── ...
```

A MOC (Map of Content) template is included in `templates/`.

## Customization

This skill is designed to be forked. Common customizations:

### Add a new domain

Currently the skill has conventions for four domain families:

- Object-Oriented Design / UML
- Software Engineering / Process / Methodology
- Statistics / Data Science / Machine Learning
- Algorithms / Data Structures / Programming

To add another (Networking, Discrete Math, Compilers, etc.), open `SKILL.md` → find `## Domain-specific conventions` → add your block following the same pattern.

### Change the per-concept template

The default 6 blocks are: plain-English intro, diagram, compact table, memory hook, exam bullets, worked example. Edit `## Per-concept 6-block template` in `SKILL.md` to add, remove, or reorder.

### Adjust YAML schema

Default fields: `subject`, `course_code`, `lecture`, `title`, `source`, `tags`, `aliases`, `related`, `exam_weight`, `status`. Add or remove based on what your Dataview queries need.

### Re-package after editing

```bash
# Requires Anthropic's skill-creator scripts
python -m scripts.package_skill ./lecture-notes ./dist
```

Then in claude.ai → **Manage skills** → delete the existing `lecture-notes` → **Add skill** → upload the new `.skill` file. Replacing instead of adding prevents duplicate-name confusion.

## How it works

A Claude Skill is a folder containing a `SKILL.md` file with YAML frontmatter (`name`, `description`) and markdown instructions. Claude reads the description at the start of every chat — when a user's input matches what the description claims to handle, the body of the skill loads into context and Claude follows its instructions.

This skill instructs Claude to:

1. Detect the subject from the lecture material (filename, course code, or content)
2. Apply a strict output structure: YAML → title → TLDR → Lecture Map → mindmap → TOC → per-concept blocks → flashcards → footer
3. Pick diagrams from a decision table tied to concept types, not defaults
4. Densely wikilink concepts so Obsidian Graph View becomes informative
5. Run a self-check before delivering the file

See `SKILL.md` for the full instruction set.

## Project structure

```
lecture-notes/
├── SKILL.md                       ← the skill itself
├── templates/
│   └── subject-MOC-template.md
├── examples/
│   └── example-output.md          ← <!-- TODO: add a sample generated note -->
├── lecture-notes.skill            ← packaged skill (release artifact)
└── README.md
```

## Known limitations

- **Triggering is occasionally inconsistent.** If Claude responds without using the skill, say *"use the lecture-notes skill"* explicitly.
- **Mermaid syntax sometimes has subtle errors.** Verify diagrams render correctly by previewing in Obsidian before relying on them.
- **Large lectures (50+ slides) may need two passes** — process first half → second half → ask for a merged cheat sheet.
- **Subject detection can mis-fire** on lectures with no filename, course code, or title slide. Provide subject context when needed.
- **Skill availability varies across Claude surfaces.** Available in claude.ai web/desktop/mobile, may not be available in all integrations.

## Contributing

Issues and PRs welcome. If you fork this for your own subjects, feel free to share — the skill is built to be personalized.

## Credits

- Built on [Anthropic's Claude Skills](https://docs.claude.com)
- Diagrams via [Mermaid](https://mermaid.js.org)
- Inspired by [Nick Milo's PKM principles](https://www.linkingyourthinking.com) (Maps of Content, atomic notes)

## License

MIT
