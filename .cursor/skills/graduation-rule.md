# Graduation rule

System rule. Do not invent a variant. Evaluate after every saved post-baseline test, and whenever the user reviews the active block.

**Inputs:** `tests` only (never `baseline`), and `primaryMetrics` (exactly two; each has `id`, `unit`, `threshold`). Curve side is system-owned: **left**. Do not ask the user to choose it.

**Rule:** The average of the latest 3 post-baseline tests must be less than each metric’s magnitude threshold. Curve must also be net left on each of those three tests. Offline may be left or right. Both metrics must pass.

`result` is magnitude (mean of `yards`). Signed curve is **calculated** from `shots`, not stored as `result`.

## Signed curve mean (per test)

For each shot: `L` → `+yards`, `R` → `−yards`, `none` → `0`.  
Signed curve mean = that sum ÷ `shots.length`.  
The test is **net left** only if signed curve mean **> 0**. Straight (`0`) and net right are not left.

## How to calculate

1. Count `tests`. The baseline is not in this list.
2. If fewer than 3: outcome is `more evidence needed`. State tests collected and tests required (3). Do not compute averages for graduation.
3. If 3 or more: take the latest 3, ordered by `date`, then `time` (nulls last), then `id`.
4. **Offline:** average `result.offline` on those three tests. Passes only if that average `<` the offline threshold. Side is ignored. Equality does not pass.
5. **Curve magnitude:** average `result.curve` on those three tests. Passes only if that average `<` the curve threshold. Equality does not pass.
6. **Curve side:** each of those three tests must be net left (signed curve mean `> 0`). If any of the three is not net left, curve fails.
7. If **offline** passes **and** **both** curve checks pass: `eligible to graduate`.
8. If either metric fails: `continue`.

Do not round in a way that turns a miss into a pass. Do not re-pool shots across tests. Do not use a signed average in place of `result.curve` for the size check (that would let hooks and fades cancel).

## What to show

Always name the outcome. Then:

- **more evidence needed:** collected vs required (3).
- **continue** or **eligible to graduate:** the three tests used (date + both magnitude results + units + each test’s net curve side: left / right / none), each metric’s magnitude average, each threshold, whether each of the three tests was net left, which metrics passed, and the exact rule.

Do not include the baseline in the three tests or the averages. You may show the baseline nearby as “where I started,” labelled as not part of the calculation.

## What this is not

Eligibility is not a status. Do not set the block to Graduated. Do not pause or close it. Do not claim practice caused the change, on-course transfer, or handicap equivalence. Do not classify shots as draw vs hook.
