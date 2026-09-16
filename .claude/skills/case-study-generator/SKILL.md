---
name: case-study-generator
description: Turn raw customer-engagement material (interview transcript or account notes) into a verified evidence block, a master narrative, and five derived assets (web story, sales one-pager, deck notes, LinkedIn post, enablement snippet). Use when the user wants to write a customer case study, customer story, success story, or repurpose one into multiple formats.
---

# Case-Study Generator

Runs workflow 01 as a four-stage chain: **Extraction → Verification → Narrative → Derivation**.

Canonical prompts live in `01-case-study-generator/prompt-chain.md`. Read that
file and use its prompts verbatim — this skill orchestrates the chain, it does
not restate it. If the two ever disagree, `prompt-chain.md` wins.

## Before starting

Ask for the source document. If the user has none, point them at
`01-case-study-generator/intake-template.md` and stop.

Refuse to proceed past Stage 1 when the material has fewer than 5 sourced
metrics or no verbatim quote. Say what is missing and ask for it. A thin
evidence block is the single most common cause of an invented narrative
(prompt-chain.md, Troubleshooting #1).

## Chain

1. **Extraction** — apply Stage 1 to the source. Output the evidence block as
   JSON only. Write it to a file so the verifier can read it.
2. **Verification** — invoke the `evidence-verifier` subagent with the source
   path and the evidence-block path. Do not run this check yourself in this
   context; the separation is the point.
3. **Gate** — present flagged entries to the user and wait. EXCLUDE verdicts
   are dropped; FIX verdicts are corrected by the user, not by you. Never
   proceed on an unresolved flag, even when asked to move fast — say what is
   unresolved instead.
4. **Narrative** — apply Stage 2 against the corrected evidence block only.
   The source document is out of scope from here on. Gaps become
   `[NEEDED: ...]`.
5. **Derivation** — apply Stage 3. Every asset derives from the evidence
   block, not from the narrative. Give each asset its own voice hint so the
   five do not read identically.

## Finish

Walk the acceptance-criteria checklist in `prompt-chain.md` explicitly, item by
item, and report pass/fail per item. Then save the verification log next to the
outputs — it ships with the case study as the audit trail.

## Hard rules

- Never write a number that is not in the evidence block.
- Never smooth a quote. Imperfections are preserved.
- Never resolve a `[NEEDED: ...]` from your own knowledge.
