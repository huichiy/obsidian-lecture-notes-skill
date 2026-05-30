# Mode: CANVAS — Generate Obsidian Canvas map

⚠️ **This mode is a planned feature, not yet specified.**

If the user invokes CANVAS mode (by saying "make a canvas", "visual map for [subject]", "knowledge graph", "canvas for OOAD", etc.), respond:

> Canvas mode is planned but the specific layout rules haven't been defined yet. Want to design the canvas generator together now, or skip for this session?
>
> Planned behavior: read existing lecture notes + atomic concept notes in the subject → emit a `.canvas` JSON file with concept nodes (embedding `![[Concept Name]]`), connection edges (from wikilinks), and color coding by `exam_weight`.

Do not generate a `.canvas` file by guessing the JSON format. Wait for the user to confirm the design (node sizing, layout algorithm, color mapping, scope: per-lecture vs subject-level).
