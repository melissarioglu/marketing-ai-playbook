# 03 · Reporting Agent

## Problem
Recurring performance reporting was a manual grind: export the data,
build the table, then hand-write "what happened and why it matters" —
hours per cycle, and commentary quality depended on who wrote it and
how tired they were. The numbers were reliable; the narrative was not.

## Approach
A data → narrative agent with a fixed contract:

**Structured data in → narrative report out → verification gate →
human approval**

Three design decisions:

1. **The data file is the only source of truth.** The agent reads the
   structured input and may not use any number not present in it. No
   memory, no estimates, no "industry context".
2. **Narrative sections have fixed jobs.** Headline → what changed →
   why it likely changed → what it means → recommended actions. Fixed
   structure makes cycles comparable and prevents vague "highlights"
   prose — every section either earns its place or is excluded.
3. **Verification runs as a separate call.** Generation and checking
   in one call lets the model grade its own homework. Two calls
   against the same data make "every number traceable" enforceable:
   any MISMATCH blocks the report before a human ever sees it.

## Measured impact
- Reporting cycle: hours of manual writing → minutes of review per cycle
- Commentary consistency: identical structure and standard every cycle,
  independent of who runs it

## Files

| File | What it is |
|---|---|
| `config.md` | Team-tunable thresholds (kept separate from the prompt contract) |
| `narrative-generation-prompt.md` | Full prompt + separate verification pass + synthetic example |
| `data/campaign-performance.csv` | Synthetic input — 2 weeks x 2 regions x 2 channels, one notable drop for the agent to find; schema note in header comments |

## What "good" looks like

The synthetic dataset contains one real signal: regional paid-social
leads down 19% WoW (314 → 255), driven by GCC (-37%), while organic
and LEVANT stay within threshold — LEVANT at +9.7%, excluded by the
config line, not by judgment. A correct run reports the GCC story,
labels the cause as hypothesis, cites only figures present in the CSV,
and stays silent about LEVANT. See `narrative-generation-prompt.md`
for the example output.

## Acceptance criteria (report is review-ready when...)

- [ ] Spot-check 3 report numbers against the CSV — all match
- [ ] Zero unlabelled hypotheses (every "why" carries the label)
- [ ] Each recommended action cites data present in the CSV
- [ ] Metrics within threshold are absent — no space-filling
- [ ] Verification pass returned all VERIFIED (any MISMATCH blocks)

## Run it

~30 minutes first run — see [QUICKSTART.md](../QUICKSTART.md). Schema
your data like `data/campaign-performance.csv`, set thresholds in
`config.md`, then generation and verification are two separate calls.

*All data in this folder is synthetic.*
