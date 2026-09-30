# Improvement Blocks

A private, local-first tool for running focused golf improvement blocks and judging them against recorded evidence — not practice volume.

The product exists to answer four questions during every block:

1. What does success look like?
2. Where am I starting from?
3. Am I moving towards the defined standard?
4. Have I demonstrated that standard consistently enough to move on?

## Status

This repository currently holds the product definition. Implementation has not started.

The canonical specification is [`improvement-blocks-prd.md`](./improvement-blocks-prd.md). If this README and the PRD disagree, the PRD wins.

## How an improvement block works

An **improvement block** is a focused period of practice on one chosen part of the game. You pick the focus, both targets, and when to start, pause, graduate, or close. The system evaluates recorded tests against your predefined rule. It does not choose what to work on next.

Example:

| Field | Value |
| --- | --- |
| Focus | 7-iron draw control |
| Primary metrics | 7-iron right-to-left curve; 7-iron offline |
| Unit | Yards (both) |
| Graduation thresholds | curve less than 10 yards; offline less than 10 yards |
| Assessment | At least 15 shots from a level range mat, measured with Toptracer; shots may come from one or more sessions |
| Graduation rule | Average of the latest 3 post-baseline tests must be less than each metric’s magnitude threshold. Curve must also be net left on each of those three tests. Offline may be left or right. Both must pass. |

Only one block can be in progress at a time (`Active` or `Paused`). A `Graduated` or `Closed` block becomes history and frees the slot. There is no draft.

```text
Active  →  Graduated
        ↘  Paused (still occupies the slot)
        ↘  Closed without graduation
```

- **Active** — success criteria and a baseline test were recorded together. The block starts immediately. Further tests can be logged and the graduation rule evaluated.
- **Paused** — incomplete, not currently being worked on. Evidence is kept. The slot stays occupied.
- **Graduated** — you confirmed completion after the graduation rule was satisfied.
- **Closed** — you stopped without graduating. The system must not invent a reason.

Eligibility to graduate is a calculated result, not a status. The block stays `Active` until you graduate, pause, or close it.

## Assessment and graduation

Every saved test uses the same protocol:

- At least 15 complete shots
- From a level range mat
- Measured with Toptracer
- Shots may come from one session or several
- Saved `result` = mean of the pooled **yards** for both primary metrics (not the table AVG row, not a signed average)
- Each complete shot is stored with yards and side (`L`, `R`, or `none`)

Graduation requires three tests **after the baseline**. The baseline is a starting snapshot under the same protocol; it does not count toward those three. The block is eligible to graduate when the **average of the latest three post-baseline tests is strictly less than each metric’s magnitude threshold**, **and** each of those three tests is **net left on curve**. Offline may be left or right. Both must pass. Magnitude: lower is always better. This product is for a stock draw.

- Fewer than three post-baseline tests: more evidence is needed.
- More than three post-baseline tests: only the latest three are used.
- Recording the baseline with the success criteria starts the block as `Active`. After that, three further tests are required.
- Every saved post-baseline result counts. The system does not classify those tests as qualifying or non-qualifying.
- Success criteria freeze when the block starts. Changing the rule means graduating or closing the block and starting a new one.

The first version captures and evaluates evidence. It does not render charts or trends.

## Product principles

- **User-owned focus.** You choose the area, both targets, and the lifecycle decisions.
- **Evidence before interpretation.** A recorded measurement, a calculated result, a user-written note, and a graduation decision are distinct. Practice activity is not proof of improvement.
- **Define success before evaluating it.** Two metrics, each with unit and threshold, exist before progress is judged.
- **Comparable evidence.** The assessment protocol is a system rule, so tests stay comparable.
- **Honest claims.** The record can show that performance met a predefined standard under the protocol. It cannot prove that practice caused the change, or that a range result will hold on the course.
- **Local and private.** Personal practice records stay on this computer. Core capture, review, and graduation must not require an external service.

## Scope

**P0 — capture and evaluation**

Start a block with success criteria and a baseline together, log tests, pool shot rows from one or more Toptracer screenshots (minimum 15 complete shots, each with yards and side) with confirmation before save (baseline or later test), review evidence, evaluate the graduation rule, then graduate, pause, or close.

**P1 — history**

Open completed and closed blocks and review them as historical evidence. Later product rules must not silently rewrite them.

**Out of scope**

Round analysis, technical instruction, drill libraries, automatic practice plans, a general AI golf coach, swing-video analysis, and Hole19 integration.

## Privacy

Practice records are personal and local-first. Do not publish, upload, or send evidence to an external service as part of the core workflow. Screenshot capture should extract the structured result and avoid retaining the image after extraction.
