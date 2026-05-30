---
name: lecture-notes
description: Generate Obsidian-ready revision notes from any university lecture material (PDFs, PPTs, slide decks, screenshots, or pasted lecture text) — and manage the resulting note system. Use this skill whenever the user uploads lecture content, course slides, or class materials, mentions taking notes, processing a chapter or lecture, converting lecture material, or building revision notes for any course or subject. ALSO use this skill when the user asks to lint / health-check / audit existing notes, generate an Obsidian Canvas / visual map / knowledge graph for a subject, or create an exam revision pack / summary spanning multiple lectures. Works subject-agnostically across CS, sciences, humanities, medicine, law, and any other discipline.
---

# Lecture Notes Skill — Router

This skill has multiple modes. Detect which mode the user wants from their input, then read the corresponding mode file and follow its instructions in full.

## Mode detection

Match the user's request against these triggers, in order:

| Trigger | Mode | File to read |
|---|---|---|
| User uploaded lecture file(s) (PDF/PPT/PPTX/PNG/screenshots), OR mentioned a new lecture / chapter / class to process, OR pasted lecture text | **GENERATE** | `modes/generate.md` |
| "lint", "check", "audit", "health check", "validate", "rate", "find issues in my [subject] notes", "检查", "审一下", "体检", "评分" | **LINT** | `modes/lint.md` |
| "canvas", "visual map", "knowledge graph", "concept map", "画图", "知识图谱", "make a canvas / mindmap for [subject]" (no lecture file uploaded) | **CANVAS** | `modes/canvas.md` |
| "exam pack", "revision pack", "summarize Lec X to Y", "exam revision for [subject]", "consolidate lectures N-M", "复习包", "复习总结", "考前总结" | **REVISION** | `modes/revision.md` |

## Routing rules

- **One mode per invocation.** Don't try to combine modes in a single turn unless the user explicitly chains them ("generate this lecture **and then** lint it").
- **Read the mode file before acting.** Do not generate notes / canvas / revision packs from memory of what this router says — the rules live in the mode files.
- **If ambiguous**, ask the user once which mode they want. Don't guess.
- **If the user uploaded a lecture but also said "lint"**, ask which they meant — they're orthogonal operations.
- **GENERATE is the default** if a lecture file is present and no other mode keyword appears.

## After dispatching

Once you've read the mode file, follow it as if it were the entire skill. The router's job ends after the dispatch — do not re-consult this file mid-execution.
