# Edit Base Prompt

The standard prompt for applying an **approved** change to any prompt,
rule, or skill file. Reached only after `analysis-base-prompt.md` (and,
when prior analyses existed, `evaluation-base-prompt.md`) have already run
and the user has explicitly approved a specific change list.

## Golden rules

1. **No edit without a prior approved change list.**
2. **Scope discipline** — apply exactly the approved items, no
   opportunistic extra cleanup.
3. **Preserve intent, not just syntax.**
4. **Minimal diff** — change only what the approved item requires.
5. **Verify after editing** — the file still renders/parses correctly and
   its cross-references still resolve.

## Mandatory report header

```
# Edit — `{target_name}` v{n}
**Editor:** {ai_name} · **Run:** v{n} · **Date:** {ISO date}
**Target:** `{path}`
**Approved from:** `analysis/result/{target_name}/{ai_name}_v{n}.md`
**Approved items:** {list the exact finding IDs/titles signed off on}
```

## Standard edit entry format

```
### [{ORIGINAL-CATEGORY-ID}] Short finding title (as approved)
- Source finding: `analysis/result/{target_name}/{ai_name}_v{n}.md#{finding-id}`
- Change applied: precise description — before → after
- Verification: how it was confirmed the file is still valid
- Deviation from approval: NONE | {exact deviation and why}
```

## Process

1. Confirm the approval list is still current before editing.
2. Apply items one at a time, writing the report entry immediately after
   each.
3. Re-open the edited file after all items are applied and confirm scope
   and cross-references.
4. Flag any approved item that could not be applied as approved — don't
   silently apply a different version of it.

## Output

Write to `analysis/result/{target_name}/edit_{ai_name}_v{n}.md`. The
actual file edit lands at its real path — this report is the audit trail.
