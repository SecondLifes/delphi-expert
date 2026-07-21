# Analysis Base Prompt

The standard prompt for producing a five-lens analysis of a target — a
single file, a folder, a bulk/whole-project traversal, or the five lens
files themselves (a meta-review). Load this after the five-lens identity
(see `SKILL.md`) is already adopted.

## Determining the target

1. **A specific file or folder is named in the request** → analyze that
   directly. No traversal needed.
2. **No specific target named, but the request is "review this project"**
   → Bulk traversal mode (below).
3. **Neither** → ask what to analyze. Don't guess.

### Bulk traversal mode

This project's own `AGENTS.md` (and its per-tool equivalents —
`.claude/CLAUDE.md`, `.gemini/rules/project-rules.md`,
`.github/copilot-instructions.md`) already names and links every rule,
skill, and folder that matters. Start there and follow every reference it
makes, depth-first, writing one result file per target (see Output below)
before moving to the next.

## Golden rules

1. **No file gets read twice.** If a target was already covered earlier in
   the same session, reference the earlier result instead of re-reading.
2. **Sequential, not two-pass.** Fully analyze and write the result for one
   target before moving to the next. When the target is a folder, list and
   read its subdirectories explicitly — reading only a top-level `SKILL.md`
   or `README.md` is a critical failure.
3. **The five lens files, and this file, need no re-read** when it's their
   turn to be reviewed — both were already fully read during identity
   adoption.
4. **Independent by default.** The existence of another AI's (or your own
   earlier) result for the same target is not, on its own, a reason to
   read or be influenced by it — re-derive every analysis from the
   target's actual current content.
5. **Mandatory external verification.** A finding about a third-party
   library/tool/ecosystem's current behavior must be checked against a
   live, authoritative source before being stated as fact.
6. **Reality checks.** Prove a claimed file/structure exists by actually
   checking it (`ls`, `Test-Path`, opening the file) — a static read of a
   README claiming something exists is not an executed test.

## Finding categories

Ten categories. `CRITICAL` is a severity escalation, not a standalone
substance type — it always layers onto one of `BUG` / `ERROR` / `MISSING`.

| ID | Meaning |
|---|---|
| `OVERALL` | Short assessment of purpose, quality, strengths/weaknesses, whether material revision is needed. |
| `CRITICAL` | Incorrect/unsafe/materially misleading/data-loss-causing/core-function-breaking. Must also state its Underlying type. |
| `BUG` | Reproducible implementation/runtime/behavioral defect. |
| `ERROR` | Factually wrong content, invalid logic, incorrect instructions, misconfiguration. |
| `MISSING` | Content/behavior/capability explicitly promised or structurally expected, but absent. |
| `WARNING` | Works today but fragile/inconsistent/likely to degrade. No current defect. |
| `ADVISORY` | Optional clarity/maintainability/efficiency improvement. No verified defect. |
| `ADDITION` | Proposed new content/capability not currently in scope. |
| `REMOVAL`/`MERGE` | Redundant, duplicated, stale, or needlessly fragmented content. |
| `DISCARDED` | Findings refuted, unsupported, unverifiable, or duplicate — recorded, not reported as live. |

## Mandatory report header

```
# Analysis — `{target_name}` v{n}
**Reviewer:** {ai_name} · **Run:** v{n} · **Date:** {ISO date}
**Target:** `{path}` ({line count} lines) — verified
**Lenses applied:** all five | {subset and why}
**Mode:** {Analysis | Design | Correction | Optimization}
```

## Standard finding format

```
### [CATEGORY-ID] Short finding title
- Category: CRITICAL | BUG | ERROR | MISSING | WARNING | ADVISORY
- Lens(es): which of the five surfaced this
- Verification verdict: VERIFIED | PARTIALLY_VERIFIED
- Evidence type: executed/observed-this-session | static-review-based
- Location: file/section/line
- Finding: clear, exact description
- Evidence: concrete support
- Impact: practical consequence if left unfixed
- Recommendation: an actionable fix
```

## Process

1. **Locate and reproduce** every candidate finding against the real file.
2. **Verify against reality** — `VERIFIED`, `PARTIALLY_VERIFIED`, or route
   to `DISCARDED` if it doesn't survive contact with the actual file.
3. **Classify** every surviving finding into exactly one category.
4. **Deduplicate** repeated findings into one.
5. Don't invent findings to look thorough — state plainly when a target is
   already effective.

## Revision discipline

`ACCEPT`/`REJECT`/`DEFER` and `REMOVE`/`MERGE`/`KEEP`/`DEFER` are
proposals, not authorization to act. Don't merge, remove, or add content
without explicit user approval.

## Output

Write to `analysis/result/{target_name}/{ai_name}_v{n}.md` — increment
`{n}` on a re-run instead of overwriting. One result file per target per
run.
