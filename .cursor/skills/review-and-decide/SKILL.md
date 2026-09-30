---
name: review-and-decide
description: Reviews the in-progress improvement block as a list with an explained graduation result, then applies a user decision to graduate, pause, resume, close, or continue. Use when the user wants to review the block, see progress, check eligibility, graduate, pause, resume, close, keep going, or open a previous block from history.
---

# Review and decide

Operator workflow for PRD 7.8–7.10. Render evidence first. Change status only if the user asked and confirmed. Eligibility is not a status.

Read [block-store.md](../block-store.md) and [graduation-rule.md](../graduation-rule.md) before reading or writing.

## Guardrails

- Do not log tests. That is `capture-test`.
- Do not start a block. That is `start-improvement-block`.
- Do not silently graduate. Eligible still means `Active` until they confirm.
- Do not invent a close reason or a closing reflection.
- Do not change either primary metric, unit, threshold, protocol, or graduation rule. To change those, they must graduate or close, then start a new block.
- Do not claim practice caused the change, on-course transfer, or handicap equivalence.
- No charts.

## Routing

1. **In-progress block present** (`data/current-block.json`): show the review. Then apply a decision only if requested.
2. **No in-progress block**, user asks about a previous block: read `data/history/` only. Do not rewrite history files.
3. **No in-progress block**, user asks to review/graduate/pause/close: say the slot is empty. They can start a new block.

## Review (always first)

Show this list. Keep measurement, calculation, note, and decision visually distinct. Label the baseline as not part of the graduation average.

```markdown
# Active block

**Focus:** …
**Improving:** …
**Status:** Active | Paused
**Intended duration:** …
**Primary metrics:**
1. … — … — less than …
2. … — … — less than …
**Protocol:** at least 15 shots, level range mat, Toptracer; shots may come from one or more sessions
**Graduation rule:** average of the latest 3 post-baseline tests must be less than each metric’s magnitude threshold; curve must also be net left on each of those three tests; offline may be left or right; both must pass

## Baseline (starting snapshot; not in the average)
- Date (latest session):
- Shot count:
- Sessions:
- Metric 1 result (mean yards):
- Metric 2 result (mean yards):
- Net curve (this test): left | right | none
- Shots: (session date, curve yards+side, offline yards+side) …

## Post-baseline tests
- (date, shot count, both mean results, net curve) … or none
- Shots for each test: (session date, curve yards+side, offline yards+side) …

## Graduation
(from graduation-rule.md: outcome + collected/required, or three tests + each average + each threshold + rule)
```

Planning edits allowed here after confirm: name/focus, intended duration, notes. Frozen fields stay frozen.

## Decisions (only on request)

Propose the change, wait for confirm, then write.

| User asks | Allowed when | Effect |
| --- | --- | --- |
| Continue / keep gathering | Any | No status change |
| Pause | `Active` | Status `Paused`. Evidence kept. Slot still occupied |
| Resume | `Paused` | Status `Active` |
| Graduate | Outcome is `eligible to graduate` | See below |
| Close | Any | See below |

**Graduate.** Refuse if not eligible; show the calculation. On confirm: set `status` `Graduated`, `endDate` and `graduationDate` (`Europe/Dublin`), `finalCalculation` from the rule, optional `closingReflection`. Move the file to `data/history/<id>.json`. Delete `data/current-block.json`. Preserve original criteria, baseline, and all tests.

**Close.** On confirm: set `status` `Closed`, `endDate`, `finalCalculation`, `closeReason` only if they gave one. Optional reflection. Same move-to-history as graduate. Closing is allowed even if eligible; that is not graduating.

**Continue** means do nothing.

After graduate or close, the slot is free for a new block. Do not choose what they should work on next.
