# 02 · Market-Entry Playbook Builder

## Problem
Market-entry playbooks were multi-week manual cycles: pulling scattered
context from a workplace platform (docs, chat threads, workspace
knowledge), verifying it, then hand-writing GTM, competitive, funnel and
pricing sections — often by different people, each drifting from the
source data in a different direction.

## Approach
A supervisor-pattern multi-agent pipeline.

**Specialist agents** run on a frozen evidence snapshot:

| Agent | Owns |
|-------|------|
| research-extraction | pulls + stages raw market data from connected sources |
| market-funnel | market overview, sizing, funnel |
| gtm-channels | channel mix, launch sequence |
| competitive | competitor landscape |
| pricing | price positioning |

Each specialist's output travels **up to a supervisor agent**, which runs
control checks and assembles the final report section by section.

Three design decisions do the heavy lifting:

1. **Data freeze before drafting.** Research runs in staged commands; the
   raw data is then locked into an evidence snapshot. Every downstream
   agent may cite only the snapshot — no live-memory drift, and any
   snapshot change re-runs the affected specialists.
2. **Negative constraints as a separate layer.** Agents receive approved
   marketing narrative inputs *plus* an explicit "do not claim" list.
   When narrative and evidence conflict, evidence wins and the conflict
   is flagged — never silently resolved.
3. **Hybrid orchestration.** Steps the platform supports as true agents
   run as agents; the rest run as a manually sequenced command chain
   with identical prompts. Both modes produce the same artifacts.

## Output
A standard-format playbook, exported to PDF. The raw draft is generated
from the frozen evidence + drafting rules; the final version is produced
by merging the draft with the standard template.

## Measured impact
- Research-to-playbook cycle: multi-week manual research → same-day
  first draft, 2-3 days to final PDF
- Section consistency: all sections cite the same frozen evidence set

## Files

| File | What it is |
|---|---|
| `intake-sheet.md` | Input contract — scope, narrative inputs, do-not-claim list, section plan |
| `evidence-snapshot-template.md` | The data freeze as a concrete artifact: table format, coverage check, freeze log |
| `orchestrator-prompt.md` | Supervisor prompt + specialist brief template + guardrails + output contract |
| `synthesis-steps.md` | Staged pipeline: research → freeze → specialist runs → supervision → PDF |
| `example-playbook.md` | Synthetic example (excerpt) with supervisor log |

## Why this is the most complex workflow here

Workflow 01 chains prompts in a line. This one coordinates specialists,
enforces a shared evidence contract, and routes rejected work back —
the difference between a prompt chain and orchestration is the
supervisor's control loop, not the number of prompts.

## Run it

This workflow has a half-day first-run budget — see
[QUICKSTART.md](../QUICKSTART.md) for the entry sequence: intake sheet
first, then the snapshot freeze, then synthesis steps.

*All data in this folder is synthetic.*
