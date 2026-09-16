## `03-reporting-agent/config.md`

````markdown
# Reporting Config — team-tunable thresholds

The prompt in `narrative-generation-prompt.md` is the **contract** —
it defines how the agent thinks. This file holds the **knobs** — the
numbers your team tunes per reporting cadence. Keep them separate:
contracts change rarely and deliberately; knobs change per team and
per quarter.

## Thresholds

| Parameter | Default | Matches prompt | What it controls |
|---|---|---|---|
| Notability threshold | beyond ±10% period-over-period | ✅ | Which metrics earn a "What changed" entry |
| Report length | max 400 words | ✅ | Hard ceiling — signal density |
| Max recommended actions | 3 | ✅ | Forces prioritization; an action list is a decision list |
| Hypothesis labelling | required, inline | ✅ | Every causal claim carries the word "hypothesis" |
| Verification | separate call; any MISMATCH blocks | ✅ | The gate — non-negotiable (see below) |

## What's tunable vs. what's not

**Tunable:** the four numbers above. A weekly ops report might tighten
notability to ±15% to cut noise; a monthly leadership report might
loosen to ±5% and raise the word cap.

**Not tunable — these are the contract:**

1. **The data file is the only source of truth.** No number outside
   the CSV, ever.
2. **Verification is a separate call.** Merging generation and checking
   back into one call re-creates the failure the split exists to
   prevent: the model grading its own homework.
3. **Hypotheses are labelled, never stated as fact.**
4. **A named human approves before the report ships.**

If you change a threshold here, update the matching line in the
prompt's CONSTRAINTS section in the same sitting. A config that
disagrees with its prompt is worse than no config — the agent will
follow the prompt, and the reviewer will expect the config.

## Cadence variants (examples)

|       Variant      | Notability | Length    | Actions |
|--------------------|------------|-----------|---------|
| Weekly ops         | ±15% WoW   | 300 words |       3 |
| Monthly leadership | ±5% MoM    | 400 words |       3 |
| Quarterly review   | ±3% QoQ    | 600 words |       5 |

Same contract, different knobs — the report stays comparable across
cycles because the structure never moves.
````
