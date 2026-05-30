# Design rationale

Why this skill is structured the way it is. Each section answers a "why" question that came up during design and gives the reasoning behind the chosen approach.

## Why router + modes?

**Alternative considered:** one big `SKILL.md` with all rules inline.

**Why rejected:** 800+ lines makes editing risky (changing canvas rules could accidentally touch generate rules), bloats context every invocation regardless of which mode the user wants, and resists future expansion.

**Chosen:** `SKILL.md` is a short router (30 lines) that detects the user's intent and dispatches to a self-contained file under `modes/`. Claude only loads the rules relevant to the current request. Each mode file is independently editable.

This is the "controller / fat models" pattern transplanted to skill design.

## Why content-type rules instead of subject-type rules?

**Alternative considered:** maintain per-subject conventions (OOAD has UML rules, Statistics has formula rules, etc.).

**Why rejected:** the skill needs to work across CS, sciences, humanities, medicine, law. Hardcoding subjects means every new subject = code change. Worse, subjects are arbitrary labels — what really determines diagram choice is **what the content is**, not what the course is called.

**Chosen:** rules trigger on content features — *"if the content contains formulas, then…"*, *"if the content contains a comparison of theories, then…"*. A psychology lecture about cognitive models triggers the same diagram rules as a CS lecture about ML algorithms when the structure is similar. A single lecture can trigger multiple content-type rules.

## Why opt-in atomic extraction?

**Alternative considered:** auto-extract atomic notes for every concept after every lecture.

**Why rejected:** vault pollution. A semester of 4 subjects × 14 lectures × ~10 concepts = 560 stub files. Most won't be cross-referenced. Graph view becomes noisy. Manual cleanup later.

**Chosen:** at the end of GENERATE, the skill **asks** which concepts (if any) to extract. User picks `all` / `recommended` / specific numbers / `no`. The same pattern in EXTRACT-ATOMIC mode (for backfilling): show a candidate list, let user pick.

## Why dotted weak edges in Canvas?

**Alternative considered:** all edges the same style; rely on color alone to distinguish.

**Why dotted is better:** strong edges (parent / containment) tell you the **organizational structure** — read them as "this concept comes from this lecture." Weak edges (wikilink cross-references) tell you the **conceptual network** — *"these concepts mention each other."* These are different relationships and deserve different visual weight.

Vanilla Obsidian Canvas can't render dotted edges, so we use the [Advanced Canvas](https://github.com/Developer-Mike/obsidian-advanced-canvas) plugin's `styleAttributes.path = "dotted"` field. Without the plugin, the attribute is silently ignored — graceful degradation.

## Why MOC and Tracker (Bases) coexist?

MOC = hand-curated index page. Free-form prose, "what is this subject about," manually maintained.
Tracker (Bases) = automatically generated database view of the same YAML metadata, filterable / sortable.

They serve different needs:
- **MOC** is what you read when *introducing yourself* to a subject.
- **Tracker** is what you read when *reviewing progress* — "how many lectures left to revise."

The skill generates both; the user uses each at appropriate times.

## Why exam_weight as YAML field instead of just tag?

**Alternative considered:** `#exam-likely` tag only.

**Why field is better:** Bases tracker filters and sorts on it directly. Canvas color-codes nodes by it. Both need a structured value (`high` / `medium` / `low`), not just presence-or-absence.

We keep the `#exam-likely` *tag* as well because tags are easier to filter in Obsidian's tag pane and search syntax. The two coexist: `exam_weight: high` is the data; `#exam-likely` is the affordance.

## Why anti-import rule?

**The bug it prevents:** user shows the skill a sample note from one subject (e.g. an OOAD note with `sequenceDiagram`) as a "style reference." Without the rule, the skill happily copies the diagram type into a new note for a totally different subject (e.g. a statistics lecture about variance), where `sequenceDiagram` makes no sense.

**The rule:** copy *structural template* (6-block layout, flashcard format, cheat sheet shape) from the sample. Re-pick diagrams from the decision table based on the **new** lecture's content. Always.

## Why "Z-Score" but "IQR" (not "Interquartile Range") for atomic note names?

**Alternative considered:** spell out all abbreviations canonically.

**Why short forms win:** the file becomes the wikilink target. `[[IQR]]` is how it appears in 90% of references in flashcards, cheat sheets, and worked examples. Long names like `[[Interquartile Range]]` get aliased — but then every author of every lecture needs to remember to use the alias. Easier to use the most common form as the canonical name and put the long form in `aliases:`.

## Why no auto-MOC-update (deferred to v1.3)?

MOC pages often have **custom user content** (intros, study notes, manually ordered link sections). Automated insertion has to handle ordering, formatting, not stomping over user comments — fragile. v1.2 chose to **flag** missing MOC entries via lint C4 rather than try to fix them. A safe auto-update is a v1.3 goal.

## Why folder-format skill (not `.skill` bundle)?

**Trade-off:** `.skill` bundles work on claude.ai web. Folder format works in Claude Code CLI.

**Why folder wins for this project:** the user uses Claude Code for daily study work. Easier git workflow on a folder than re-bundling per release. If the skill needs to work on claude.ai later, we can ship a `.skill` zip via Releases — but the source-of-truth is the folder.

## Why one repo for skill + templates + CSS?

**Alternative considered:** separate repos for `lecture-notes-skill`, `obsidian-lecture-templates`, `obsidian-lecture-styles`.

**Why one repo wins:** they version together. CSS depends on what tags the skill emits; templates depend on what YAML the skill produces; canvas paths depend on where atomic notes get saved. Splitting them creates version compatibility issues across N×N×N combinations. One repo, one version number, all dependencies in lockstep.

## Why so many trigger phrases (English + Chinese)?

The skill description influences whether Claude routes a user's request here. Broad trigger coverage = better routing. Chinese phrases (`复习包`, `画图`, `检查`, etc.) are included because the user works bilingually — adding ~30 chars of trigger keywords avoids missed routes.
