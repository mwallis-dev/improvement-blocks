---
name: start-improvement-block
description: Starts an improvement block by recording success criteria and a baseline snapshot together, then freezing both. Use when the user wants to start a block, begin an improvement block, set a target with a baseline, record a baseline to open a block, or use a screenshot as the baseline for a new block.
---

# Start an improvement block

Operator workflow for PRD 7.1–7.3. One sitting: criteria + baseline → `Active`. No draft.

Read [block-store.md](../block-store.md) and [evidence-integrity.md](../evidence-integrity.md) before writing. If an image is attached, also read [screenshot-capture.md](../screenshot-capture.md).

## Guardrails

- Do not choose the focus, either metric, or either threshold. Ask.
- Do not suggest what to work on next.
- Do not create a block without a baseline test.
- Do not create a block with only one primary metric. Exactly two are required.
- Do not create a block with fewer than 15 complete baseline shots.
- Do not turn a desired range (for example 4–8 yards) into a threshold. Ask for a single upper bound per metric.
- Do not ask the user to invent the assessment protocol or graduation rule. Stamp the system values from the store doc.
- Do not adjust a threshold to fit the baseline result.

## Workflow

1. **Check the slot.** If `data/current-block.json` exists with status `Active` or `Paused`, refuse. Name the current focus and status. Tell the user to graduate or close it before starting another. Stop.

2. **Collect user-owned fields.** All required:
   - Block name or improvement focus
   - What they are trying to improve
   - Intended duration (planning only; does not pass or fail the block)
   - Primary metric 1: name, unit, threshold (a number that metric’s average must beat: strictly less than)
   - Primary metric 2: name, unit, threshold (same rule)

   Assign stable `id` values that match `result` keys (for example `curve` and `offline`).

3. **Collect the baseline test** (same protocol as every later test: at least 15 shots, level range mat, Toptracer; shots may come from one or more sessions). Required:
   - `shots` (each with `sessionDate` and `{ yards, side }` for both metrics)
   - Result for metric 1 and result for metric 2 (means of `shots[].*.yards`)
   - `shotCount` (`shots.length`, ≥ 15)
   - `sessions` (each with date, shotCount, source)
   - Relevant context (optional)

   Units must match the frozen units. Both results are required.

   **Typed:** accept only if they supply the shot list with yards and side for both metrics (not two averages alone). Compute `result` from those shots. Do not invent sides.

   **Screenshot:** follow [screenshot-capture.md](../screenshot-capture.md). Pool complete shot rows (yards and side) from every image they included in this baseline. Do not use `AVG` cells. If dates are missing, ask. Baseline `date` and block `startDate` = latest included session. If complete shots &lt; 15, stop. Create no block.

4. **Propose, then wait.** Show the structured record below. Do not write files until the user confirms.

5. **On confirm, write `data/current-block.json`.** Create `data/` if needed. Status `Active`. `startDate` = baseline `date`. Write the baseline on `baseline` only. Leave `tests` as `[]`. Criteria are frozen from this moment.

6. **After save, report state — do not graduate.** Baseline is the starting snapshot, not test 1 of 3. Outcome: more evidence needed (0 post-baseline tests collected, 3 required). The block stays `Active`.

## Proposed record

```markdown
# Start improvement block

**Focus:** …
**Improving:** …
**Intended duration:** …
**Primary metrics (frozen):**
1. … — … — less than …
2. … — … — less than …
**Protocol (system):** at least 15 shots, level range mat, Toptracer; shots may come from one or more sessions
**Graduation rule (system):** average of the latest 3 post-baseline tests must be less than each metric’s magnitude threshold; curve must also be net left on each of those three tests; offline may be left or right; both must pass
**Status after save:** Active (frozen)

## Baseline (starting snapshot; not one of the 3 graduation tests)
- Date (latest session):
- Shot count:
- Sessions: (date, shots, source) …
- Metric 1 result (mean yards):
- Metric 2 result (mean yards):
- Net curve (this test): left | right | none
- Shots: (session date, curve yards+side, offline yards+side) …
- Context:

Confirm to start this block?
```

## After it exists

This skill does not edit an Active or Paused block. Planning-field edits, pause, graduate, and close belong to the review skill. Later tests belong to the capture skill.
