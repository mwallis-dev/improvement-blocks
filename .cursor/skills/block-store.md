# Block store

Local JSON files under `data/`. Gitignored. Never commit, upload, or publish.

| Path | Contents |
| --- | --- |
| `data/current-block.json` | The one in-progress block (`Active` or `Paused`), or file absent |
| `data/history/<block-id>.json` | Graduated or Closed blocks (P1) |

Read `data/current-block.json` to detect an in-progress block. If the file is missing, the slot is free.

## In-progress block shape

```json
{
  "id": "blk_2026-08-28_a1",
  "status": "Active",
  "name": "7-iron draw control",
  "improving": "Reduce hooking; keep a controlled draw",
  "intendedDuration": "Two weeks",
  "primaryMetrics": [
    { "id": "curve", "name": "7-iron right-to-left curve", "unit": "yards", "threshold": 10 },
    { "id": "offline", "name": "7-iron offline", "unit": "yards", "threshold": 10 }
  ],
  "assessmentProtocol": "At least 15 shots from a level range mat, measured with Toptracer. Shots may come from one or more sessions.",
  "graduationRule": "The average of the latest 3 post-baseline tests must be less than each metric’s magnitude threshold. Curve must also be net left on each of those three tests. Offline may be left or right. Both metrics must pass.",
  "startDate": "2026-08-28",
  "baseline": {
    "id": "tst_2026-08-28_1",
    "date": "2026-08-28",
    "time": null,
    "result": { "curve": 11.6, "offline": 12.9 },
    "shotCount": 17,
    "sessions": [
      { "date": "2026-08-26", "shotCount": 12, "source": "Toptracer" },
      { "date": "2026-08-28", "shotCount": 5, "source": "Toptracer" }
    ],
    "shots": [
      { "sessionDate": "2026-08-26", "curve": { "yards": 1, "side": "L" }, "offline": { "yards": 8, "side": "R" } }
    ],
    "location": null,
    "practiceType": null,
    "notes": null
  },
  "tests": [],
  "notes": null
}
```

`primaryMetrics` is exactly two items. Each `id` is the key on `result`. Each `threshold` is a number in that metric’s `unit` (magnitude). Graduation is eligible when the average of the latest 3 entries in `tests` is strictly less than each threshold, **and** each of those three tests is net left on curve. Offline side is ignored. Derive net left from `shots` as in [graduation-rule.md](graduation-rule.md).

`baseline` is the starting snapshot. Never include it in `tests` or in the graduation average.

`assessmentProtocol` and `graduationRule` are system-owned. Copy the strings above. Do not invent variants.

## Test shape

The baseline and each post-baseline test use this shape. Store the baseline only on `baseline`. Append post-baseline tests only to `tests`.

```json
{
  "id": "tst_2026-08-28_1",
  "date": "2026-08-28",
  "time": null,
  "result": { "curve": 11.6, "offline": 12.9 },
  "shotCount": 17,
  "sessions": [
    { "date": "2026-08-26", "shotCount": 12, "source": "Toptracer" },
    { "date": "2026-08-28", "shotCount": 5, "source": "Toptracer" }
  ],
  "shots": [
    { "sessionDate": "2026-08-26", "curve": { "yards": 1, "side": "L" }, "offline": { "yards": 8, "side": "R" } }
  ],
  "location": null,
  "practiceType": null,
  "notes": null
}
```

- `date` is `YYYY-MM-DD` in `Europe/Dublin` and is the date of the **latest** session in `sessions`. Relative dates resolve in that timezone. `startDate` equals baseline `date`.
- `id` must be unique. Date alone is not an identity. More than one test may share a date.
- `result` is required and must include a number for every `primaryMetrics[].id`. It is the mean of `shots[].*.yards` for that metric, not a table `AVG` cell and not a signed/net average. Graduation uses `result` for **size**. Curve **side** is derived from `shots[].curve` at evaluation time.
- `shotCount` is `shots.length`. Must be ≥ 15. Do not save if it is lower.
- `sessions` lists each contributing session (`date`, `shotCount`, `source`). A single-session test still has one entry. Sum of session `shotCount` values equals `shotCount`.
- `shots` is required. Each complete shot stores both metrics as `{ "yards", "side" }`. `side` is `L`, `R`, or `none` (`0` yards → `none`; nonzero yards must be `L` or `R`). `sessionDate` matches the session that produced the shot. Do not drop side after averaging.
- Units live on `primaryMetrics`. Do not store a separate test-level unit.
- `notes` is optional user text. Never derive `result` from notes.

At start, `tests` is `[]`. After start, three post-baseline tests are required before the rule can be evaluated (0 of 3 collected).

## Status values

`Active` | `Paused` | `Graduated` | `Closed`

No `Draft`. A new block is written as `Active` with `baseline` set and `tests` empty.

## History shape

On graduate or close, write the full block to `data/history/<block-id>.json` and delete `data/current-block.json`. Never rewrite a history file later.

Add these fields when ending:

```json
{
  "status": "Graduated",
  "endDate": "2026-09-11",
  "graduationDate": "2026-09-11",
  "finalCalculation": {
    "testsUsed": [],
    "byMetric": [
      { "id": "curve", "average": 9.2, "threshold": 10, "passed": true },
      { "id": "offline", "average": 8.0, "threshold": 10, "passed": true }
    ],
    "ruleSatisfied": true
  },
  "closingReflection": null,
  "closeReason": null
}
```

- `Graduated`: set `graduationDate` (Europe/Dublin). `closeReason` is null. `closingReflection` is optional user text.
- `Closed`: set `closeReason` only if the user supplied one. Never invent a reason. `graduationDate` is null. `ruleSatisfied` is whether the rule was satisfied at close, not a substitute for graduating.
- `finalCalculation` uses the same method as [graduation-rule.md](graduation-rule.md). If fewer than 3 post-baseline tests exist, `testsUsed` is `[]`, each `average` is null, each `passed` is false, `ruleSatisfied` is false.
