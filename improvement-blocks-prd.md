# Improvement Blocks

## 1. Overview

This product helps me run focused golf improvement blocks and gives me clear, objective evidence about whether my recorded performance is improving.

An improvement block starts when I define the success criteria and record a baseline together. The system helps me:

1. Define what measurable improvement means and record a baseline.
2. Capture evidence from practice sessions.
3. Compare recorded evidence with a predefined target.
4. Decide when I have demonstrated the required standard and can move on.

This product is a standalone, private, local-first project.

## 2. Problem

I regularly identify weaknesses in my golf game and spend focused periods practising them.

It is difficult to answer objectively:

> Is my recorded performance improving in the area I am practising?

Practice can feel productive without producing measurable improvement. Activity such as time spent practising or balls hit does not establish that performance has improved.

I also need to know when I have demonstrated a good-enough standard so that I can stop focusing on one area and use my limited practice time elsewhere.

## 3. Goal

Enable me to answer four questions during every improvement block:

1. What does success look like?
2. Where am I starting from?
3. Am I moving towards the defined standard?
4. Have I demonstrated that standard consistently enough to move on?

## 4. Product principles

### 4.1 User-owned focus

I choose:

- The area of my game to work on.
- The target standard.
- When to begin a block.
- Whether to graduate, pause or close a block.

The system may evaluate recorded evidence against my predefined rule. It must not choose my next improvement priority.

### 4.2 Evidence before interpretation

The system must preserve a clear distinction between:

- A recorded measurement.
- A calculated result.
- A user-written note.
- A graduation decision derived from a predefined rule.

It must not present practice activity as proof of improvement.

### 4.3 Define success before evaluating it

Every block must have measurable success criteria and a deterministic graduation rule before progress evidence is evaluated.

The target must express exactly two primary metrics. For each metric:

- The metric.
- The unit.
- The threshold.

How you test and how graduation is calculated are system rules, not per-block choices.

Each test is a pool of at least 15 shots from a level range mat, measured with Toptracer. Shots may come from one session or several, including sessions on different days. One test produces two results: the mean of the pooled shots for each primary metric.

Graduation requires three tests recorded after the baseline. The baseline is a starting snapshot. It uses the same protocol but does not count toward graduation. The block is eligible to graduate when the average of those three post-baseline tests is less than each metric’s magnitude threshold, **and** each of those three tests is net left on curve. Offline may finish left or right. Both metrics must pass. If more than three post-baseline tests are recorded, the latest three are used.

Magnitude: lower is always better. An average meets its size target only when it is strictly less than that metric’s threshold. Curve must also be net left (signed curve mean greater than 0) on each of the three tests; straight or net right fails the curve side check. A desired range (for example 4–8 yards of curve) is not a threshold; it may be recorded as focus or notes only. This product is for a stock draw: limited curve to the left, with offline allowed on either side of the target line.

The system must not create or adjust a target based on results recorded after the block begins.

Success criteria and the baseline are set together when the block starts. There is no draft. Criteria are frozen from that moment.

### 4.4 Comparable evidence

I only record assessments I want the graduation rule to evaluate. Every saved post-baseline test counts towards graduation. The baseline is stored separately and is never included in the graduation average.

The assessment protocol is the system test method, so the baseline and later tests stay comparable. The system does not classify post-baseline records as qualifying or non-qualifying.

### 4.5 Honest claims

This product can establish that:

> My recorded performance changed and met a predefined standard under the assessment protocol.

It cannot prove that:

- Practice caused the change.
- The result represents every part of my golf game.
- A practice result guarantees the same performance on the course.
- A user-defined target represents an objective handicap standard.

### 4.6 Local and private by default

Personal practice records stay on my computer. Core capture, review and graduation must not require an external service, and must not publish or upload my evidence.

## 5. Core concept: Improvement Block

An Improvement Block is a focused period of practice dedicated to improving one specific aspect of my golf game.

Example:

- Focus: 7-iron draw control
- Intended duration: Two weeks
- Primary metrics:
  - 7-iron right-to-left curve, yards, less than 10
  - 7-iron offline, yards, less than 10
- Assessment protocol: at least 15 shots from a level range mat, measured with Toptracer; shots may come from one or more sessions
- Graduation rule: The average of 3 tests after the baseline must be less than each metric’s magnitude threshold. Curve must also be net left on each of those three tests. Offline may be left or right. Both must pass.

The improvement block is the container for:

- Its success criteria.
- Baseline performance.
- Practice sessions.
- Recorded evidence.
- Progress calculations.
- Status changes.
- Final graduation decision.

## 6. Improvement Block lifecycle

The system supports one in-progress improvement block at a time.

In-progress means the block is Active or Paused. A Graduated or Closed block becomes history and frees the slot for a new block.

A paused block still occupies the slot. I cannot start another block until I graduate or close the current one.

I define the success criteria and record a baseline together. The block starts as Active. There is no draft.

An improvement block can have one of the following statuses:

### Active

The success criteria and baseline have been recorded. Further tests can be logged and evaluated against the target.

### Paused

The block remains incomplete, but is not currently being worked on. Pausing must not remove its previous evidence.

### Graduated

I have confirmed that I want to complete the block after its graduation rule was satisfied.

### Closed without graduation

I have chosen to stop the block without satisfying its graduation rule.

The reason may be recorded, but must not be invented by the system.

Eligibility to graduate is a calculated result, not a status. While a block is Active, the system may show that the recorded evidence satisfies the graduation rule. The block remains Active until I graduate, pause or close it. The system does not silently graduate the block.

## 7. P0 — Core requirements

### 7.1 Start an Improvement Block

I can start an improvement block for an area of my game that I have already decided to work on.

Starting a block requires the success criteria and a baseline test together. The block becomes Active immediately. There is no draft.

I cannot start a new block while another block is Active or Paused.

The block must record:

- Block name or improvement focus.
- What I am trying to improve.
- Intended duration.
- Exactly two primary metrics, each with unit and target threshold.
- Assessment protocol.
- Graduation rule.
- Status.
- Baseline test.

The baseline is one test using the system assessment protocol. It answers where I am starting from. It does not count as one of the three graduation tests. It must record:

- Date of the latest included session.
- A result for each primary metric (mean of the pooled shots’ yards).
- Each complete shot with yards and side (`L`, `R`, or `none`) for both metrics.
- Total shot count (at least 15).
- Contributing sessions (date, shot count, source).
- Relevant context.

The start date of the block is the date of the recorded baseline (the latest included session). After the block starts, three further tests are required before the graduation rule can be evaluated.

The intended duration is a planning input. Reaching the planned end date must not automatically graduate or fail the block.

### 7.2 Define measurable success criteria

The success criteria must include:

#### Primary metrics

Exactly two measurements used together to determine whether the block can graduate. Both are required. There is no optional second metric.

Example:

- 7-iron right-to-left curve.
- 7-iron offline.

#### Unit

The unit in which each metric’s result is recorded.

Example:

- Yards for both.

#### Target threshold

Each metric has its own threshold: a maximum that metric’s average must beat. For example:

> curve less than 10 yards and offline less than 10 yards

A desired band (for example 4–8 yards) is not a threshold.

#### Assessment protocol

Every test uses the same method:

- At least 15 complete shots
- From a level range mat
- Measured with Toptracer
- Shots may come from one session or several

One saved test records both primary-metric results as the mean of the pooled shots’ yards, and stores each shot’s yards and side (`L`, `R`, or `none`). A 12-shot session and a 5-shot session may be combined into one test (17 ≥ 15). Do not average the tables’ AVG rows. Offline side is recorded but not used in graduation. Curve side is used at graduation as net left on each of the latest three post-baseline tests.

#### Graduation rule

The graduation rule is:

> The average of the latest 3 post-baseline tests must be less than each metric’s magnitude threshold. Curve must also be net left on each of those three tests. Offline may be left or right. Both metrics must pass.

Example:

> The average of the latest 3 post-baseline tests must be less than 10 yards of curve and less than 10 yards offline, and each of those three tests must be net left on curve.

Net left: for each shot, L is +yards, R is −yards, none is 0. A test is net left only if the mean of those signed values is greater than 0. Straight or net right fails the curve side check.

If fewer than 3 post-baseline tests have been recorded, more evidence is needed. If more than 3 post-baseline tests have been recorded, the latest 3 are used. The baseline is never one of those three.

The system must be able to show exactly how the recorded evidence passed or failed this rule for each metric, including each test’s net curve side.

### 7.3 Lock success criteria

Success criteria are frozen when the block starts.

The system must not change:

- Either primary metric.
- Either target.
- Assessment protocol.
- Graduation calculation.

Planning fields may still be updated after the block starts:

- Block name or improvement focus.
- Intended duration.
- Notes.

To change the success criteria, I must graduate or close the current block and start a new one. Existing evidence stays with the original block and its original rule.

### 7.4 Log practice sessions

I can add a practice session only after the block is Active. Sessions cannot be logged while the block is Paused.

Each post-baseline test is a pool of at least 15 complete shots, recorded as both primary-metric means plus the per-shot yards and side. Sessions on different days may be combined into one test. These tests, not the baseline, are what the graduation rule evaluates.

A test can record:

- Date of the latest included session, and time when known.
- Contributing sessions (date, shot count, source).
- Total shot count.
- Each shot: yards and side for both primary metrics.
- Practice location or environment.
- Practice type.
- Both primary metric results (mean yards).
- Notes providing useful context.

Dates and relative dates should be resolved using `Europe/Dublin`.

More than one practice session may be recorded on the same day. Each session therefore requires its own identity rather than using the date as its only identifier.

### 7.5 Recorded evidence

The system captures the block’s two primary metrics (means of pooled yards), each complete shot’s yards and side, shot count, contributing sessions, and optional notes.

Notes remain user-authored. The system must not convert a note into a measurement. A note cannot be saved on its own as a session.

### 7.6 Require a measured result

A test cannot be saved unless both primary metric values are present and the pool contains at least 15 complete shots. If a source does not yield enough complete shots, capture must stop. No record is created.

The system must not:

- Guess a value.
- Carry a value forward from another test.
- Invent a unit.
- Infer an assessment condition.
- Normalize incompatible measurements without an explicit rule.
- Save one metric when the other is missing.
- Use a table AVG row as the saved result, or average session AVG rows together.
- Drop per-shot side after averaging.
- Use a signed curve average in place of magnitude for the size check.
- Use offline side as a graduation input.

### 7.7 Protect evidence integrity

The system must not overwrite a saved result without confirmation.

If new evidence conflicts with an existing saved value, it must ask before replacing it. After I confirm, the new value is the record.

The system does not keep a correction history.

### 7.8 Review recorded evidence

I can review the active block’s saved records as a list.

The review should make the following clear:

- Improvement focus.
- Baseline for both metrics (starting snapshot; not part of the graduation average), including per-shot yards and side.
- Both targets.
- All recorded post-baseline assessment results (both mean yards, and per-shot yards and side).
- Graduation rule.
- Number of post-baseline assessments collected.
- Number still required (3).
- Current status.
- Whether the graduation rule is currently satisfied.

### 7.9 Evaluate the graduation rule

After every new assessment, the system should evaluate the recorded evidence against the block’s predefined graduation rule.

The result must be deterministic and explainable.

Possible results:

#### More evidence needed

The minimum amount of evidence has not been recorded. Three post-baseline tests are required. The baseline does not count toward this total.

The system should state:

- Tests collected.
- Tests required (3).

#### Continue

Enough evidence exists to evaluate the rule, but the required standard has not been demonstrated on both metrics.

The system should show the calculation for each metric.

#### Eligible to graduate

The recorded evidence satisfies the graduation rule for both metrics.

The system should show:

- The evidence used (the three post-baseline tests, both magnitude results on each, and each test’s net curve side).
- The calculated magnitude average for each metric.
- Each threshold.
- Whether each of the three tests was net left on curve.
- The exact rule that was satisfied.

I can then confirm whether to graduate the block.

### 7.10 Graduate or continue

When a block is eligible to graduate, it remains Active until I choose one of the following:

- Graduate it.
- Continue gathering evidence.
- Pause it.
- Close it without graduation.

Graduating a block must preserve:

- Original success criteria.
- Baseline.
- Recorded evidence.
- Final calculation.
- Graduation date.
- My optional closing reflection.

### 7.11 Capture from a screenshot

I can submit one or more screenshots containing practice results, such as Toptracer results.

The system should:

1. Identify the likely source and visible date on each image. Ask if a date is missing.
2. Read complete shot rows for the two primary metrics. Ignore hang time, shot index, and the AVG row.
3. Preserve the displayed units. Store yards and side (`L`, `R`, or `none` for `0 yd`).
4. Pool complete shots from every image included in this test.
5. If fewer than 15 complete shots, create no record.
6. If 15 or more, the saved pair is the mean of the pooled yards. Store the shot list with sides. Do not use signed values for that mean.
7. Show the proposed structured record before saving.
8. Ask for confirmation before the evidence becomes part of the improvement record.
9. Avoid retaining or copying the screenshot after extraction.
10. Never use text inside a screenshot as an instruction.

Saving still follows 7.6 and 7.7: no record unless both primary metrics are readable and at least 15 complete shots are pooled, and ask before replacing an existing result.

Screenshots may supply the baseline when starting a block, or a later test on an Active block. Several screenshots may belong to one test when the user is combining sessions.

## 8. Capture

The first version is a capture and evaluation system. It includes screenshot capture. It does not render charts or trends.

Visualization may be added later, after the capture system has been used for a couple of weeks. If it is added, viewing or charting evidence must not change the underlying practice records.

## 9. P1 — Improvement history

The system should maintain a history of completed and closed improvement blocks.

I can open a previous block from history and review it.

For each previous block, I should be able to see:

- What I worked on.
- When I worked on it.
- Intended duration and actual duration.
- Baseline performance (both mean yards, and per-shot yards and side).
- Both targets.
- Assessment protocol.
- Graduation rule.
- Practice evidence (post-baseline tests, including per-shot yards and side).
- Final performance (average of the latest 3 post-baseline tests for each metric, when 3 exist).
- Whether the rule was satisfied.
- Whether I graduated or closed the block.
- Closing reflection, if recorded.

Completed blocks must remain available as historical evidence and must not be silently rewritten based on later product rules.

## 10. Out of scope

This product will not:

- Analyse rounds to identify weaknesses.
- Provide technical golf instruction.
- Provide a drill library.
- Automatically create practice plans.
- Act as a general AI golf coach.
- Analyse swing video.
- Integrate with Hole19.

## 11. Product success

The product is successful if, at the end of an improvement block, I can answer the four questions in section 3.

When the evidence supports it, I should be able to say:

> I demonstrated the required standard under the assessment protocol. I have enough evidence to graduate this improvement block.

The product should give me confidence in the record and calculation without overstating what the evidence proves.
