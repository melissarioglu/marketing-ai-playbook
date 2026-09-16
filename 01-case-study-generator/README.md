# 01 · Case-Study Generator

## Problem
Customer case studies were our slowest content type: ~2 weeks each, with most
of that time lost translating one long-form story into five different formats
(web story, sales one-pager, deck slide, social post, enablement snippet) —
and losing metric accuracy in every translation.

## Approach
A four-stage prompt chain where each stage has one narrow job:

**Extraction → Verification → Draft → Derivation**

The key design decision: downstream formats are derived from a structured
*evidence block*, not from the narrative. The narrative is persuasive prose;
the evidence block is auditable fact. Every asset can be checked against the
auditable layer rather than against a paragraph that already interpreted it.

A single mega-prompt was tried first and failed predictably — it dropped
metrics and paraphrased quotes into inventions. The version history in
`prompt-chain.md` records what broke and what each revision fixed.

## Measured impact
- Production time per case study: ~2 weeks → ~4-5 working days
- The format set went from ~2 days of marginal work to near-zero

## Files

| File | What it is |
|---|---|
| `prompt-chain.md` | The full chain: prompts, version history, acceptance criteria, troubleshooting |
| `intake-template.md` | What Stage 1 should receive, and the minimum bar to proceed |
| `template.md` | Master narrative structure |
| `example-input.md` | Synthetic account notes — with one deliberately vague metric |
| `example-output.md` | End-to-end run, including the verification log |

## Run it

~15 minutes, zero setup — see [QUICKSTART.md](../QUICKSTART.md).

*All data in this folder is synthetic.*
