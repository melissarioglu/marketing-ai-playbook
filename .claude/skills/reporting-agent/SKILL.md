---
name: reporting-agent
description: Turn campaign or channel performance data into a short written narrative report — what moved beyond threshold, labelled hypotheses, and a capped set of recommended actions. Use when the user wants a performance report, weekly or monthly marketing reporting, campaign readout, or a data-to-narrative summary from a CSV.
---

# Reporting Agent

Runs workflow 03. Canonical prompt lives in
`03-reporting-agent/narrative-generation-prompt.md`; thresholds live in
`03-reporting-agent/config.md`. Read both. Config values are per-team and may
have been edited — always read config rather than assuming the defaults.

## Procedure

1. Read `config.md` for the notability threshold, length cap, action cap, and
   hypothesis-labelling rule.
2. Read the performance data in the schema of
   `03-reporting-agent/data/campaign-performance.csv`.
3. Compute period-over-period movement first, as arithmetic, before writing any
   prose. Do not let the narrative decide what moved.
4. Apply the narrative prompt. Metrics inside the threshold are **omitted** —
   not mentioned as stable, not used as filler. A short report is the correct
   output for a quiet period.
5. Label every causal statement as a hypothesis, inline, in the sentence that
   makes it. Correlation in a CSV is never a stated cause.
6. Cap recommended actions at the config value. Each must cite data present in
   the CSV.

## Verification

Invoke the `evidence-verifier` subagent with the CSV and the draft report. Any
MISMATCH blocks the report — do not deliver a report with an unresolved number.

## Acceptance criteria

Report pass/fail on each explicitly before delivering:

- [ ] Three spot-checked numbers match the CSV
- [ ] Zero unlabelled hypotheses
- [ ] Every recommended action cites CSV data
- [ ] Nothing within threshold appears in the report

## Hard rules

- Never compute a metric the CSV does not support, however reasonable the
  derivation looks.
- Never pad to length. The length figure in config is a cap, not a target.
