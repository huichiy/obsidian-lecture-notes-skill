# Maintainer context for Claude Code

Read this if you're working **on the skill itself** (editing `modes/`, adjusting the router, updating templates). For "how to use the skill," see `docs/workflow.md`. For an example of a *vault-side* `CLAUDE.md` (what users put in their Obsidian vault), see `examples/vault-CLAUDE.md`.

## What this repo is

A **Claude Code skill** in folder format. Six modes (`generate` / `lint` / `canvas` / `revision` / `tracker` / `extract-atomic`) dispatched by a thin router (`SKILL.md`). Plus templates, CSS, and docs.

Production install path: `~/.claude/skills/lecture-notes/` (or a symlink to this clone).

## Architecture in 30 seconds

```
SKILL.md (router)
    │  detect mode from user input
    ▼
modes/{generate,lint,canvas,revision,tracker,extract-atomic}.md
    │  each is a self-contained instruction set
    ▼
output to user's vault (lecture .md / atomic .md / .canvas / .base)
```

The router's only job is dispatch. **Rules live in the mode files.** Adding a mode = create a new `modes/{name}.md` + add a routing row to `SKILL.md`. See `docs/design-rationale.md` § "Why router + modes?" for the reasoning.

## When editing

### Editing a mode file
- Each mode file should be **self-contained** — don't reference rules in other modes. If two modes need the same rule, duplicate it (it's instructions, not code; DRY-violation cost is low).
- Keep the **Boundaries** section at the bottom of each mode honest. Add to it as you discover edge cases.
- Mode files are user-facing prompts, not internal code. Write them in **plain language**, with concrete examples.

### Editing the router (`SKILL.md`)
- The `description:` field is **load-bearing** — it's what tells Claude when to route to this skill in the first place. Be specific. Long descriptions are OK; they only cost tokens once per session.
- Trigger lists in the mode-detection table can include **Chinese phrases** — the user works bilingually.
- Don't add complex routing logic. Keep it to "match phrase → read mode file."

### Editing templates
- `templates/subject-MOC-template.md` and `templates/subject-tracker-template.base` use `{{SUBJECT}}` as placeholder. The relevant mode does substitution.
- **Bases syntax** (`.base` files): YAML frontmatter properties are referenced **without** the `file.` prefix (e.g. `subject == "DSF"`, not `file.subject == "DSF"`). The `file.*` prefix is reserved for built-in File properties (`file.name`, `file.path`, `file.mtime`).

### Editing the CSS snippet
- Variables exposed via `@settings` block must be referenced as `var(--variable-name)` in CSS rules.
- Pill styling targets `a.tag[href="#level/N"]` selectors. Avoid changes that affect non-skill tags.

## Version conventions

- **Major** (v1 → v2): SKILL.md description changes or a mode's I/O contract breaks scripted usage.
- **Minor** (v1.2 → v1.3): new mode, expanded content-type rules, new template.
- **Patch** (v1.2 → v1.2.1): bug fixes, syntax corrections, CSS, docs.

After every release, update both `CHANGELOG.md` and `docs/roadmap.md` (move shipped items out of "next" into "current").

## Out-of-scope (don't add)

These were explicitly considered and rejected. Re-opening them needs strong evidence the original decision was wrong.

- ❌ Live writing assistance (compose new content from scratch — different use case).
- ❌ Query mode / ask-my-notes (separate skill territory).
- ❌ AI-inferred pitfalls (unreliable; user marks `> [!warning]` explicitly).
- ❌ Auto-extract every concept on every lecture (vault pollution; opt-in is intentional).
- ❌ Mathematical proofs / derivations (out of scope).

See `docs/roadmap.md` § "Beyond v1.3" for full reasoning.

## Testing the skill

There's no automated test suite. Validation is end-to-end:

1. Drop a PDF (or pasted text) into Claude Code with a clone of this repo symlinked at `~/.claude/skills/lecture-notes/`.
2. Verify GENERATE produces a `.md` matching `examples/sample-lecture-note.md`'s shape.
3. Try edge cases: lecture without course code, lecture with non-CS content (humanities, medicine), lecture with no clear concepts (review session).
4. After 3+ lectures exist, run LINT → CANVAS → REVISION → TRACKER → EXTRACT-ATOMIC in sequence; each should reach a usable output without errors.

When changing mode rules, **re-run the affected mode** on a representative input before committing. Mode rules are instructions to Claude, not code — bugs surface only at runtime.

## Commit conventions

- Co-authored commits: include `Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>` line at the end of the commit message (when Claude generated the change).
- Group related changes per commit. v1.2 was one big commit because the modes are interdependent; v1.2.1 split out a fix because it was independent.
- Don't push to `main` unless changes pass an end-to-end sanity check.

## Don't do these

- ❌ Don't add hidden state to modes (no "remember between invocations" — each invocation is fresh).
- ❌ Don't reference user-specific paths or subjects in mode files. The skill is subject-agnostic.
- ❌ Don't introduce dependencies on specific Obsidian plugins for **core** functionality. Advanced Canvas's dotted line is a *graceful enhancement* (degrades to plain cyan without it). Style Settings is *optional UI sugar*. Bases is **required only for the TRACKER mode**, which is documented.
- ❌ Don't add bash scripts or executable code to the skill folder. Skills are Markdown instructions to Claude, not programs.
