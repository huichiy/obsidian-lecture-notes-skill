# Mode: TRACKER — Generate Obsidian Bases revision tracker

Create a `.base` file (Obsidian 1.9+ Bases database view) that pulls every lecture-note for a given subject into a live, sortable table. The user opens it like a dashboard to see exam-weight priorities, revision status, and progress.

## When to trigger

Triggers: `tracker`, `revision tracker`, `make a tracker`, `build a tracker for [subject]`, `进度表`, `复习追踪`, `追踪表`, `database view`, `dashboard for [subject]`.

If the user clearly wants a Bases-style table-of-lectures, route here. If they want a curated markdown landing page (links + intro paragraphs), they want a **MOC** — not a tracker. MOC and Tracker are complementary; this mode is for the latter.

## Pre-flight

1. **Verify Obsidian Bases is available.** Bases shipped with Obsidian 1.9 (mid-2025). The user must be on 1.9+. Ask once if their Obsidian version is older — `.base` files won't render on older versions.
2. **Resolve subject** from input. Ask if missing.
3. **Locate the subject's folder** in the vault. Use the subject code's matching folder if obvious (`DSF` → `DSF Note/`); else ask once.

## Inputs

- Subject code (e.g. `DSF`, `OOAD`).
- Vault path with the subject folder.
- The template file: this skill's `templates/subject-tracker-template.base` (in the repo).

## Generation steps

1. **Read the template** from the skill's `templates/subject-tracker-template.base`.
2. **Replace placeholders.** The template has `{{SUBJECT}}` markers — substitute every occurrence with the actual subject code (e.g. `DSF`). Subject values must match what appears in lecture-note YAML `subject:` fields — case-sensitive. If unsure, sample one lecture note's YAML first to confirm the casing.
3. **Write the file** to: `{vault_root}/{subject_folder}/{SUBJECT} - Tracker.base`
4. **Auto-create the parent folder** if missing.
5. **If the file already exists**, ask: *"Tracker already exists at {path}. Overwrite? (yes / no / save as new with timestamp)"*

## After writing

Close the turn with this 4-line summary:

```
✅ Saved {SUBJECT} - Tracker.base to {path}

Open it in Obsidian (requires v1.9+). You'll see 5 views:
  • All lectures · 🔥 Exam priority · 📝 Still to revise · ✅ Done · Cards
Edit the lecture's `status:` or `exam_weight:` YAML and the views update live.
```

## Optionally: cross-link to MOC

If a `{SUBJECT} - MOC.md` exists in the same folder, offer to add a one-line cross-reference:

> "Want me to add `→ [[{SUBJECT} - Tracker]]` to the top of {SUBJECT} - MOC.md so they cross-reference each other? (yes / no)"

If yes → insert immediately after the H1 heading. If no → skip cleanly.

## Boundaries

- **Don't modify the template file** — it's the source of truth in the repo.
- **Don't write to vault** without confirming overwrite if file exists.
- **Don't fabricate `subject:` casing** — sample a real lecture note's YAML before substituting to avoid mismatch (the filter is case-sensitive).
- The template targets `type: lecture-notes` only. Atomic concept notes (`type: concept`) are excluded from the tracker by design — they show up in Graph View / Canvas instead.
