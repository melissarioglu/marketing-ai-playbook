---
name: market-entry-playbook
description: Build a market-entry playbook from a frozen evidence snapshot — market and funnel, GTM and channels, competitive, pricing, risks — using multi-step agent orchestration with per-section source citation. Use when the user wants market-entry research, a country or segment expansion plan, a GTM playbook, or competitive and pricing analysis for a new market.
---

# Market-Entry Playbook Builder

Runs workflow 02. Canonical prompts live in
`02-market-entry-playbook-builder/orchestrator-prompt.md` and
`synthesis-steps.md`. Read both before starting; use them verbatim.

## The non-negotiable order

Scope → **freeze** → specialists → synthesis. Research that continues after the
freeze is the failure mode this workflow exists to prevent: sections stop
agreeing with each other because each one saw different data.

1. **Scope.** Have the user fill `intake-sheet.md`. The do-not-claim list in
   section C is binding on every later step. Anything outside the sheet is out
   of scope.
2. **Freeze.** Build the evidence snapshot with
   `evidence-snapshot-template.md` and save it as `snapshot-v1.md`. From that
   moment treat it as read-only. Run the coverage check: a section marked
   `thin` is expected to ship open questions, not guesses.
3. **Specialists.** Run one pass per section. Each pass reads the snapshot and
   nothing else, and cites snapshot item IDs (`#3`) inline for every claim.
   Anything the snapshot does not cover becomes `[OPEN QUESTION: ...]`.
4. **Synthesis.** Assemble sections per the section plan in intake-sheet D.
   Do not introduce claims during synthesis — synthesis reorders and connects,
   it does not add evidence.

## Verification

Before delivery, invoke the `evidence-verifier` subagent with the snapshot and
the assembled playbook. Every cited ID must resolve to a real snapshot row and
support the claim made. Uncited claims are flagged for removal.

## Changing the snapshot

New data does not get pasted into a running build. Bump the snapshot to `vN+1`,
record the change in the freeze log, and re-run only the affected specialist
sections. Say which sections you re-ran.

## Hard rules

- No market-size, share, or pricing figure without a dated snapshot row.
- No momentum adjectives ("fast-growing", "underserved") without a snapshot
  reference — this is the most common way this workflow drifts into fiction.
- Never fill an `[OPEN QUESTION: ...]` from your own knowledge.
