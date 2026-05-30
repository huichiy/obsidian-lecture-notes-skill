# Examples

Sample outputs from the lecture-notes skill, used to illustrate what each mode produces.

## Files

| File | Mode | What it is |
|---|---|---|
| `sample-lecture-note.md` | GENERATE | A real lecture note for "Exploratory Data Analysis" — shows the full 6-block per-concept template, YAML schema, 13 Mermaid diagrams, 25 flashcards, cheat sheet, self-test. |
| `sample-atomic-concept.md` | GENERATE atomic-extraction step | A `type: concept` atomic note for "Box Plot" — short definition, single diagram, `parent:` pointing back to the lecture, `Mentioned in` + `Related concepts` backlinks. |
| `sample-canvas.canvas` | CANVAS (Exam Map variant) | A `.canvas` JSON file — illustrative skeleton with text-type nodes (in real vault these are `file:` nodes pointing into the vault). Shows MOC → lecture → atomic structure, dotted weak edges between cross-referencing concepts, and bottom-row Confident/Weak zones. |

## What's NOT in here

- Tracker (`.base`) — see `templates/subject-tracker-template.base` in the repo root.
- MOC template — see `templates/subject-MOC-template.md`.
- CSS snippet — see `styles/concept-levels.css`.

## Notes

- Filenames in the sample canvas use `text:` nodes so the file is openable standalone without missing-file errors. In real generated canvases, lecture and atomic nodes are `type: "file"` with paths into your vault.
- The sample lecture note references prev/next lectures (`DSF Lec 03` / `DSF Lec 05`) that don't exist in this examples folder — that's intentional, showing the link format. In a real vault these would resolve.
