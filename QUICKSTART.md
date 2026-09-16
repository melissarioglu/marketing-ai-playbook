# Quickstart: run your first workflow in ~15 minutes

No setup, no code, no API keys. You need any LLM chat interface
(Claude recommended) and this repo.

## Run workflow 01 (case-study generator)

Turns a raw customer-engagement document into a verified evidence block,
a master narrative, and 5 derived assets.

| Step |  Time | Do |
|------|-------|----|
|   1  | 2 min | Read the sample input: [01-case-study-generator/example-input.md](01-case-study-generator/example-input.md). Note the last line of the results section — it is deliberately vague. |
|   2  | 5 min | New chat: paste Stage 1 (Extraction) from [prompt-chain.md](01-case-study-generator/prompt-chain.md) + the full sample input |
|   3  | 2 min | New chat: paste Stage 1b (Verification) + Stage 1's JSON output + the sample input |
|   4  | 3 min | Fix flagged entries by hand → new chat: Stage 2 (Narrative) + the corrected JSON |
|   5  | 2 min | Same chat as Stage 2: paste Stage 3 (Derivation) |
|   6  | 1 min | Check the output against the acceptance criteria in [prompt-chain.md](01-case-study-generator/prompt-chain.md) |

## What you should see

- An evidence block where every metric carries a `source_line`
- The verification gate flagging the vague CTR line ("improved
  significantly" is not a stated figure) and excluding it
- A narrative where every number exists in the evidence block
- 5 derived assets, each ready to lift into its channel

If the gate caught the CTR line, the kit works — you just watched the
core idea run on synthetic data: **facts survive translation,
inventions don't.**

## Then run it on your own material

- Structure your input with
  [intake-template.md](01-case-study-generator/intake-template.md)
- Minimum bar before Stage 1: **5+ sourced metrics, 1+ verbatim quote**
- Keep the one rule of the chain: each stage is a separate call, and
  only structured output travels between stages

## The other workflows

| Workflow | How to start | First-run budget |
|---|---|---|
| 02 Market-entry playbook builder | Fill [intake-sheet.md](02-market-entry-playbook-builder/intake-sheet.md), freeze an evidence snapshot ([template](02-market-entry-playbook-builder/evidence-snapshot-template.md)), then follow [synthesis-steps.md](02-market-entry-playbook-builder/synthesis-steps.md) | Half a day |
| 03 Reporting agent | Put your data in the schema of [campaign-performance.csv](03-reporting-agent/data/campaign-performance.csv), set thresholds in [config.md](03-reporting-agent/config.md), run [narrative-generation-prompt.md](03-reporting-agent/narrative-generation-prompt.md) — generation and verification are two separate calls | ~30 min |
| 04 Webinar content pipeline | Fill [session-brief-template.md](04-webinar-content-pipeline/session-brief-template.md), then one call per asset using specs 1-4 in [promo-content-prompt.md](04-webinar-content-pipeline/promo-content-prompt.md) | ~20 min |

## Ground rules (all workflows)

1. Never paste confidential or client data into a tool your
   organization hasn't approved.
2. Every output passes a human gate before it ships.
3. If a claim can't be traced to the input, it doesn't ship.
