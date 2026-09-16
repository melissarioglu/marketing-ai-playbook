# Narrative Generation Prompt — generation and verification as two calls

## How to run

| Call | Prompt | You paste | New chat? |
|---|---|---|---|
| 1 — Generate | Generation prompt below | the CSV contents | yes |
| 2 — Verify | Verification prompt below | the report + the same CSV | **yes — never the same chat** |

Call 2 in the same chat as call 1 defeats the purpose: the model that
wrote the report will agree with it. Two chats against the same data
is what makes "every number traceable" enforceable instead of aspirational.

## Version history

| v | Architecture | Failure mode |
|---|---|---|
| 1 | Generate + self-check in one call | Reports graded their own homework; mismatches shipped |
| 2 | Generation call + separate verification call | Current — any MISMATCH blocks the report |

Same trajectory as workflows 01 and 02: the fix was not a better
prompt, it was splitting duties into separate calls.

---

## Call 1 — Generation prompt

```
### ROLE
You are a marketing performance analyst writing a recurring report for
marketing leadership. You translate data into decisions — you do not
decorate numbers.

### CONTEXT
- DATA below is the only source of truth. Every number you write must
  exist in DATA. You have no memory, no estimates, no industry context.
- CONFIG: notability threshold = beyond ±10% period-over-period;
  max 400 words; max 3 recommended actions; hypotheses labelled inline.
- Audience: leadership. They want signal, not a narration of the table.

### TASK
Write the report in exactly these sections:
1. HEADLINE — 1-2 sentences: the single most important movement
2. WHAT CHANGED — only metrics beyond the notability threshold,
   with values
3. WHY IT LIKELY CHANGED — hypothesis tied to data patterns only,
   each labelled "hypothesis"
4. WHAT IT MEANS — business implication, 2-3 sentences
5. RECOMMENDED ACTIONS — max 3, each with rationale citing DATA

### CONSTRAINTS
- No number that is not in DATA
- No adjectives doing the work of numbers ("strong performance") —
  state the number
- Hypotheses carry the word "hypothesis" — never stated as fact
- A metric within threshold is excluded — do not fill space
- One reporting period is one data point: if DATA covers a single
  period-over-period change, say so and do not extrapolate trends

### OUTPUT FORMAT
Markdown, one H2 per section, max 400 words.
```

## Call 2 — Verification prompt (separate chat)

```
You will receive: (1) a report, (2) the DATA it was written from.

For every number in the report, find the matching value(s) in DATA and
output one line per figure:
VERIFIED | "<report figure>" | <DATA row(s) or sum that produces it>

Arithmetic on DATA is allowed only if shown (e.g. column sums).
If a figure has no matching DATA value:
MISMATCH | "<report figure>" | not present in DATA

If any MISMATCH exists, end with: BLOCKED — report returns to revision.
If none: CLEAN — <n>/<n> figures verified.
```

Any MISMATCH blocks the report — fix and re-run both calls. The
verification output ships with the report as its audit trail.

---

## Example run (synthetic — `data/campaign-performance.csv`)

DATA covers W44 → W45: GCC and LEVANT paid-social plus regional
organic. One real signal sits in it; the rest is deliberately quiet.

### Call 1 output

> ## Headline
> Regional paid-social leads fell 19% WoW (314 → 255), driven by GCC
> (-37%).
>
> ## What changed
> - GCC paid-social leads: 190 → 119 (-37% WoW)
> - GCC CTR: 2.1% → 1.5% (-28.6%)
> - GCC conversion rate: 3.3% → 2.9% (-12.1%)
> - Regional paid-social total: 314 → 255 (-18.8%)
>
> ## Why it likely changed
> Hypothesis: creative fatigue in GCC — CTR fell 2.1% → 1.5% while
> spend was essentially unchanged (±2%), pointing at audience response
> rather than delivery volume. The smaller conversion decline
> (3.3% → 2.9%) suggests the landing experience is a secondary factor.
>
> ## What it means
> GCC is the region's largest paid-social market, so its decline moves
> the regional number by itself. At flat spend this is an efficiency
> problem, not a budget problem — restoring CTR matters more than
> adding budget.
>
> ## Recommended actions
> 1. Rotate GCC creative and re-measure CTR next cycle — rationale:
>    2.1% → 1.5% at flat spend is the primary anomaly (hypothesis:
>    fatigue).
> 2. Review the GCC landing/conversion path — rationale: conversion
>    3.3% → 2.9%, a secondary but real decline.
> 3. Treat as watch item, not incident — DATA covers one week-over-
>    week change; confirm the trend before budget or escalation moves.

*(~200 words. LEVANT appears nowhere: +9.7% is inside the ±10% threshold — excluded by config, not judgment. Organic -2.2%: same.)*

**One judgment call, made visible:** GCC spend (±2%) appears inside
the hypothesis sentence as the control for the CTR claim — it is
context for a beyond-threshold signal, not a listed change. If you
want that forbidden outright, add "spend moves only beyond threshold"
to CONSTRAINTS; the contract is yours to tighten.

### Call 2 output

```
VERIFIED | "314"       | sum(leads) paid-social W44: 190 + 124 = 314
VERIFIED | "255"       | sum(leads) paid-social W45: 119 + 136 = 255
VERIFIED | "19%"       | 255/314 - 1 = -18.8%
VERIFIED | "190 → 119" | GCC paid-social leads, W44 → W45 rows
VERIFIED | "37%"       | 119/190 - 1 = -37.4%
VERIFIED | "2.1% → 1.5%" | GCC ctr_pct, W44 → W45 rows
VERIFIED | "28.6%"     | 1.5/2.1 - 1
VERIFIED | "3.3% → 2.9%" | GCC conv_rate_pct, W44 → W45 rows
VERIFIED | "12.1%"     | 2.9/3.3 - 1
VERIFIED | "±2%"       | GCC spend_usd: 9,800 → 9,600
CLEAN — 10/10 figures verified.
```

The report is unblocked for human review, with the verification log
attached. A fabricated "CTR dropped 43%" — the number a tired writer
or a self-grading model might produce — would have died here as
MISMATCH, because 43% is CTR's *relative* misread of a figure DATA
states as 2.1 → 1.5.

---

## Acceptance criteria (report is review-ready when...)

- [ ] Call 2 ran in a separate chat and returned CLEAN
- [ ] Spot-check 3 report numbers against the CSV — all match
- [ ] Zero unlabelled hypotheses
- [ ] Each recommended action cites DATA
- [ ] Within-threshold metrics are absent — no space-filling

## Troubleshooting

| # | Failure mode | Symptom | Fix |
|---|---|---|---|
| 1 | Phantom number | Figure in report, nothing in CSV | The chat saw material beyond DATA — re-run with only the CSV in context |
| 2 | Self-grading | Verification "passed" in the generation chat | Enforce the two-chat rule; merge = v1 failure mode returns |
| 3 | Quiet-week padding | Commentary on within-threshold metrics | Quote the CONFIG block at the top of the call — the agent follows the prompt, so the prompt must carry the config |
| 4 | Double counting | Totals inflated | CSV contains a rollup row *and* its components for the same channel — schema rule: never both (see the schema note in the CSV) |
