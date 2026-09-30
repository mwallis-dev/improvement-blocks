# Evidence integrity

Apply on every save (baseline or later test).

- Fewer than 15 complete shots in the pool → create no record. State the count found.
- Either primary-metric value missing on the **saved** `result` → create no record.
- A complete shot requires `{ yards, side }` for both metrics. Missing side on a nonzero value → skip that row; do not guess `L` or `R`.
- Do not guess a value, carry one forward, invent a unit, or infer an assessment condition.
- Do not save one metric when the other is missing.
- Do not use a table `AVG` row as the saved result. Do not average session AVG rows together. Pool complete shot rows, then take the mean of `yards`.
- Store the `shots` list with per-shot yards and side. Do not drop side after the mean is confirmed.
- `result` is magnitude only. Graduation uses curve `side` only as defined in [graduation-rule.md](graduation-rule.md) (net left on each of the latest three tests). Do not use offline side in graduation.
- Notes are user-authored context only. Never convert a note into a measurement. A note cannot be saved on its own as a session.
- Do not overwrite a saved result without confirmation. After confirm, the new value is the record. No correction history.
- Every saved post-baseline result counts toward graduation. Do not classify those tests as qualifying or non-qualifying. The baseline is not a graduation test.
