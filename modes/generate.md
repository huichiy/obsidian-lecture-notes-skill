# Mode: GENERATE — Lecture material → Obsidian note

Convert raw lecture material into surgically clear Obsidian notes the user can revise from for exams. Do not summarize — *restructure and explain* so the user never has to re-open the slides.

## Subject identification

This skill works for **any university subject across any semester**. Identify the subject fresh each time from:

1. **Filename** (e.g., `Lec01_OOAD_Intro.pptx`, `CCP6224_Week2.pdf`)
2. **Course code** on the slides (e.g., `CCP6224`, `STAT3013`)
3. **Subject name** in slide headers/footers/title page
4. **Content/topic** if the above are absent

Derive a **short subject code** (typically 3-5 letters, e.g., `OOAD`, `ADA`, `STAT`, `PSY`, `HIST`) and use it consistently in YAML, tags, filenames, and wikilink prefixes throughout the note. If genuinely ambiguous, ask the user once: subject name + the short code they want to use.

## Output structure (in this exact order)

1. **YAML frontmatter** — see strict formatting rules below
2. **`# H1` title** — `[SUBJECT-CODE] Lec [N] — [Topic]`
3. **5-second TLDR** in a `> [!note]` callout
4. **Lecture Map** in a `> [!info]` callout (course code, source file, prereqs, related notes, prev/next)
5. **Opening `mindmap`** showing all concepts in the lecture
6. **Table of contents** with `[[#headings]]` links
7. **Per-concept blocks** (6-block template)
8. **Flashcard bank → Mini cheat sheet → Self-test**
9. **Footer** with MOC backlink + prev/next lecture links

## YAML frontmatter — STRICT formatting rules

Obsidian's Properties panel requires precise YAML. Violations break Properties, Dataview queries, and Graph View metadata. **Apply these rules without exception:**

- Open and close with `---` on their own lines.
- **Each key on its own line.** Never put multiple keys on one line.
- Lists use **block form** with one item per line prefixed by `- ` and 2-space indent:
  ```yaml
  tags:
    - subject-code
    - lecture-01
    - revision
  ```
- String values containing `:`, `&`, `#`, `[`, `]`, `{`, `}`, or quotes **must be quoted**: `title: "Intro to OOAD & Design"`
- Wikilinks in YAML lists **must be quoted**: `- "[[Concept Name]]"`
- Dates in `YYYY-MM-DD` format, no quotes.
- Boolean values are lowercase `true` / `false`, no quotes.

### YAML template (copy this structure literally, fill in values)

```yaml
---
subject: OOAD
course_code: CCP6224
lecture: 1
title: "Introduction to Object-Oriented Analysis & Design"
source: Lec01_Introduction.pptx
type: lecture-notes
created: 2026-05-20
tags:
  - ooad
  - lecture-01
  - revision
  - exam-prep
aliases:
  - "OOAD Lec 1"
  - "CCP6224 Lecture 1"
related:
  - "[[Object-Oriented Programming]]"
  - "[[UML]]"
exam_weight: medium
status: draft
---
```

`aliases` lets `[[OOAD Lec 1]]` and `[[CCP6224 Lecture 1]]` both resolve to the same note — useful for cross-referencing.

`exam_weight` is your estimate from lecturer emphasis, "important" flags, past-paper references, or repetition. Values: `low`, `medium`, `high`.

`status` values: `draft` (just generated), `reviewed` (user has read through), `complete` (fully revised).

## Lecture Map callout

After the TLDR, every note gets a navigation map. Format exactly like this:

```
> [!info] Lecture Map
> **Course:** CCP6224 · **Lecture:** 01 · **Source:** `Lec01_Introduction.pptx`
> **Prereq:** [[OOPDS]] (C++ fundamentals) · **Next:** [[OOAD Lec 02 — Use Cases]]
> **Related notes:** [[Object-Oriented Programming]] · [[Java]] · [[Design Patterns]] · [[UML]]
```

Fill in only the fields you have evidence for. Skip a field rather than inventing one.

## Wikilink rules — critical for Graph View

Obsidian's Graph View only shows what's linked. Sparse linking kills its usefulness.

- Wrap every key concept in `[[Concept Name]]` even if no file exists yet. Unresolved links still appear as nodes — when the same concept recurs across lectures, the node grows visually, signaling exam importance.
- Link to previous lectures when the topic builds on them: `as introduced in [[OOAD Lec 02 — Use Cases]]`.
- Link to the subject MOC at the bottom: `← [[SUBJECT-CODE - MOC]]`.
- Aim for **8–20 wikilinks per lecture note**.
- Format:
  - Concept files: `[[Concept Name]]` (Title Case)
  - Lecture files: `[[SUBJECT-CODE Lec N — Topic]]`
  - MOC files: `[[SUBJECT-CODE - MOC]]`

## Tag conventions

Use in YAML (block form) and inline. Consistent tagging creates visual clusters when filtering Graph View.

- `[subject-code-lowercase]` — every note
- `lecture-[NN]` — every note (zero-padded, e.g., `lecture-01`)
- `concept/[concept-name-kebab]` — on each concept's heading line
- `exam-likely` — items flagged as exam-relevant
- `formula` — anywhere a formula appears
- `algorithm` — every algorithm
- `pattern` — design pattern, process model, methodology, school of thought
- `vague-in-lecture` — content flagged as uncertain

Inline tags use the `#` prefix; YAML tags do not.

## Per-concept 6-block template

Repeat for every distinct concept:

### 🧩 [[Concept Name]]
`#concept/concept-name-kebab` `#exam-likely` *(if applicable)*

1. **Plain-English intro** — 2-4 sentences, no jargon, like explaining to a smart friend.
2. **Diagram** — pick from the decision table below. Embed as ` ```mermaid ` block. If Mermaid genuinely doesn't fit, use SVG or a markdown table.
3. **Compact table** — `term | definition | example` or `attribute | value | why it matters`. Max ~8 rows; split if longer.
4. **Memory hook** — mnemonic, analogy, or one-line "the trick is…". The line the user remembers in the exam.
5. **Exam bullets** — 3-7 bullets phrased as exam answers. Match depth to mark allocation if implied (e.g., Pressman-style 1 mark ≈ 1 distinct point).
6. **Worked example** — one full traced example. Algorithm → trace table. Formula → real numbers. Historical event → cause/effect chain. Theory → concrete case.

## Diagram decision table — single source of truth

Pick by **concept type**, not by subject. Re-evaluate fresh every time.

| Concept type | Best diagram | Use when |
|---|---|---|
| Class structure, inheritance, associations | `classDiagram` | OO design subjects |
| Object interaction over time | `sequenceDiagram` | OO design, distributed systems |
| Object lifecycle / state changes | `stateDiagram-v2` | OO design, embedded systems, protocols |
| Use cases & actors | `flowchart` with actor shapes | OO design, requirements engineering |
| Entities & relationships | `erDiagram` | Databases, domain modeling, anatomy, taxonomy |
| Process / workflow / methodology phases | `flowchart LR` or `flowchart TD` | Software engineering, project management, scientific method |
| Topic landscape / concept overview | `mindmap` | **Any subject — use in opening section** |
| Project schedule / historical timeline | `gantt` | SE methodology, history, project management |
| ML / data pipeline | `flowchart LR` (each stage = node) | Data science, ML |
| Statistical / mathematical formula derivation | `$$ math $$` + small `flowchart` | Statistics, physics, economics |
| Algorithm logic, control flow | `flowchart TD` with **decision diamonds** | Algorithms, programming |
| Algorithm trace (step values) | **markdown trace table** | Algorithms |
| Big-O / complexity comparison | **markdown table** | Algorithms, data structures |
| Tree / graph / hash structure | `graph TD` or `graph LR` | Data structures, networks, family trees, org charts |
| Comparison of schools / theories / approaches | **markdown comparison table** | Psychology, philosophy, economics, history |
| Cause-and-effect / influence chain | `flowchart LR` with labeled arrows | History, sociology, medicine (etiology) |
| Linear process worth animating | inline SVG with `<animate>` | Any subject — pipelines, sorting passes |

Fallback: `flowchart TD` with the concept name as root.

## Anti-import rule — critical

When the user shares a sample note from a different subject/domain as a style reference:

- ✅ Copy the **structural template** (6-block format, flashcard structure, cheat sheet)
- ✅ Copy the **tone and density**
- ❌ DO NOT copy the **diagram types** from it
- ❌ DO NOT copy the **content or examples**

Diagram choice is made fresh from the decision table based on the NEW lecture's concepts. A statistics lecture does not get `sequenceDiagram` just because an OO sample had one. Violating this wastes the user's tokens and forces them to manually fact-check every diagram for relevance — defeating the point of the skill.

## Diagram quality rules

- Every major concept (one warranting the full 6-block template) gets a diagram — if no type genuinely fits, use a minimal `mindmap`. Minor definitions (single-term, single-sentence) don't need their own concept block; fold them into a related concept's compact table, or into a short glossary callout at the end of the section.
- Label every arrow and node — no orphan boxes.
- Keep node text ≤ 5 words; push detail into the table below the diagram.
- Pick the diagram type per concept from the decision table — don't default. If you find yourself using the same diagram type for 3+ consecutive concepts, pause and re-check the table for each one. Genuine repetition is fine (a UML-heavy lecture may have several `classDiagram` in a row, all justified), but verify each choice was deliberate, not automatic.

## Content-type conventions

Detect from the lecture content. A single lecture may trigger several of these at once — apply each rule where its trigger appears. These are **content-type** rules, not subject-type rules; they apply across any discipline.

**When content contains formulas / quantitative reasoning** (stats, physics, econ, ML, chemistry)
- Every formula gets a variable legend below it: `where x = …, μ = …`.
- Worked examples use real numbers, not just symbols.
- Distinguish **what** (definition) / **why** (intuition) / **how** (computation steps) / **when** (use case).

**When content contains algorithms / step-by-step procedures** (CS, biology protocols, lab methods, surgical steps)
- Pseudocode in ` ```pseudocode ` blocks, indented, with `// comments`.
- For computational algorithms: complexity table (best/avg/worst + space) + trace table with ≥ 3 iterations.
- Distinguish *correctness* from *efficiency* explicitly.

**When content contains code** (programming, ML)
- Use language-appropriate code blocks (` ```python `, ` ```java `, ` ```sql `, etc.).
- Comment the **why**, not the **what**.
- Pair "library API" content with a tiny runnable example, not just signatures.

**When content contains classifications / taxonomies / hierarchies** (biology, medicine, law, anatomy, OO inheritance)
- Use `erDiagram`, `classDiagram`, or nested lists depending on whether relationships are part-of, is-a, or attribute-of.
- Compact table with `category | criteria | example` for the leaves.

**When content contains historical timeline / chronological development** (history, evolution of theories, technology generations)
- `gantt` or `flowchart LR` with year labels.
- Highlight **turning points** with `⚠️` markers and explain *why* each turning point matters.

**When content contains comparison of schools / theories / approaches / paradigms** (psychology, philosophy, economics, methodology)
- Markdown comparison table: `school | proponents | core claim | strength | weakness | exam-ready example`.
- Show the **disagreements between them** explicitly — that's where exam questions come from.

**When content contains people / scholars / figures with contributions** (history, science history, philosophy)
- Table: `name | dates | key work / experiment | what it established`.
- Link contributions to the concepts they founded: `[[Wertheimer]] founded [[Gestalt Psychology]]`.

**When content contains cause-and-effect / influence chains** (history, medicine etiology, sociology, economics)
- `flowchart LR` with **labeled arrows** stating the causal mechanism, not just direction.
- Distinguish *necessary cause*, *sufficient cause*, and *correlated factor*.

**When content contains case studies / scenarios / clinical examples / legal cases** (medicine, law, business, ethics)
- Use a structured scenario block:
  - **Scenario:** [one-line situation]
  - **Key facts:** [bulleted, exam-relevant only]
  - **Analysis:** [framework applied]
  - **Conclusion / outcome:** [verdict + ratio]

**When content contains pure definitions / terminology** (any subject's foundational lectures)
- Compact table `term | definition | exam-ready phrasing | mnemonic`.
- One memory hook per term. Don't let definition-heavy sections turn into bullet swamps.

**When content contains process / methodology / framework with stages** (SE methodologies, scientific method, design thinking, clinical workflow)
- Comparison table with **when to use** and **risks/limitations** columns.
- Definitions before examples.
- Match answer depth to mark allocation if implied (e.g., Pressman-style 1 mark ≈ 1 distinct point).

**When content uses UML / formal notation** (OOAD, software engineering)
- UML-correct notation in `classDiagram`: `+` public, `-` private, `#` protected, `~` package.
- Sequence diagrams include activation bars and return arrows.
- Design principles (SOLID, GRASP, etc.): pair definition with *bad-code → good-code* mini example.

If a lecture's content doesn't match any rule above, apply the 6-block template plainly and pick diagrams from the decision table.

## End-of-note sections

### 📇 Flashcard bank
Collapsible callouts so they hide until clicked:

```
> [!question]- What is [term]?
> [Answer in one tight sentence — definition + key distinguishing feature]
```

10–25 cards per lecture. Cover every bolded term, formula, and diagram type. Each term should be a `[[wikilink]]` if it's a concept.

### 🎯 Mini cheat sheet
Compact one-liners — every definition / formula / rule on one line. Group by concept order. Designed to fit on one screen for last-minute review.

### 🧪 Self-test
3–5 exam-style questions, **no answers**. For active recall. Tag each: `Tests: [[Concept Name]]`.

### Footer
```
---
← [[SUBJECT-CODE - MOC]]
Previous: [[SUBJECT-CODE Lec N-1 — Topic]]
Next: [[SUBJECT-CODE Lec N+1 — Topic]]
```

## Quality bar

- No filler. Every line earns its place.
- Define every acronym on first use — `SDLC (Software Development Life Cycle)`.
- Concrete examples beat abstract description.
- Mark uncertainty with `> [!warning] Vague in lecture — verify with textbook.` Do not invent content.
- Don't paraphrase slide bullets — restructure into the per-concept template. 10 bullets often collapse into 1 table + 1 diagram.
- No essay paragraphs. Notes are for scanning.
- Emoji is functional, not decoration — only as section markers: 🧩 concept · 📇 flashcards · 🎯 cheat sheet · 🧪 self-test · ⚠️ exam-likely · 🔑 key insight.

## Execution order

When given a lecture:

1. Identify subject code + lecture number + topic
2. Write YAML frontmatter (verify each key on its own line before continuing)
3. H1 title
4. 5-second TLDR
5. Lecture Map callout
6. Opening `mindmap`
7. Table of contents
8. Per-concept blocks (6-part template each)
9. Flashcard bank → cheat sheet → self-test
10. Footer with MOC + prev/next links

If anything critical is unclear, ask one question first, then proceed.

## Output file

Save the final `.md` to `/mnt/user-data/outputs/` with filename `[SUBJECT-CODE]_Lec_[NN]_[topic-slug].md` (zero-padded lecture number, kebab-case topic), then present it for download.

## Self-check before delivering

Before saving the file, verify:

- [ ] YAML opens and closes with `---` on their own lines
- [ ] Every YAML key is on its own line (no two keys on one line)
- [ ] All YAML lists use block form (`- item` on indented lines)
- [ ] Wikilinks in YAML lists are quoted
- [ ] At least 8 wikilinks in the note body
- [ ] At least one diagram per concept
- [ ] Each diagram was picked from the decision table for its concept (not defaulted — if 3+ same type appear consecutively, each was a deliberate choice)
- [ ] Flashcard count is 10–25
- [ ] Footer includes MOC backlink

If any check fails, fix before delivering.

## After delivering: offer atomic concept notes (ask-on-demand)

Once the lecture note is saved and shown, **before ending your turn**, offer the user atomic concept note extraction.

Show a compact menu listing the major `[[Concept Name]]` wikilinks from the note (the ones that got their own 6-block, not minor glossary terms). Format:

```
✅ Saved: OOAD_Lec_03_class-diagram.md

Want atomic concept notes extracted for cross-lecture linking and Canvas use?
The lecture note already has the full detail — atomic notes are short standalone pages so each concept becomes its own Graph View node.

Concepts in this lecture:
  1. [[Class Diagram]]
  2. [[Association]]
  3. [[Multiplicity]]
  4. [[Inheritance]]
  5. [[Encapsulation]]

→ Reply with numbers (e.g. "1, 3, 4") to extract those
→ Reply "all" to extract all
→ Reply "no" or just continue with something else to skip
```

**If user picks some/all**: for each chosen concept, create a short atomic note (under `concepts/` in their vault if known, otherwise alongside the lecture note). Each atomic note contains:

- YAML with `type: concept`, `subject:`, `level:` (1/2/3 — your estimate based on how foundational it is), `parent:`, `tags:`
- 2-3 sentence definition (the plain-English intro from the lecture note's 6-block)
- The diagram for this concept (copied from the lecture note)
- A `## Mentioned in` section listing wikilinks back to lecture notes that reference this concept (currently just this lecture; user / future runs will add more)
- A `## Related concepts` section with sibling wikilinks

**`parent:` rules** — critical for cross-linking to work in Obsidian:

- Set `parent:` to a wikilink whose target is the **exact filename of the just-saved lecture note** (without the `.md` extension).
- Example: if the lecture note was saved as `DSF Lec 04 — Exploratory Data Analysis (new).md`, then atomic notes must use:
  `parent: "[[DSF Lec 04 — Exploratory Data Analysis (new)]]"`
- Do **NOT** invent a "cleaner" wikilink name (e.g. dropping the `(new)` suffix or matching an older filename). The parent wikilink must resolve to a file that actually exists in the vault — typically the file you just wrote.
- The `## Mentioned in` section follows the same rule: wikilinks point to the exact filename of the source lecture, not a guess.

Filename: `[Concept Name].md` (Title Case, as it appears in wikilinks).

**Do NOT auto-extract** without the user picking. Stub concept files pollute the vault.

**Do NOT redirect users to a separate `lecture-atomic` skill** — atomic note extraction lives inside the GENERATE mode.
