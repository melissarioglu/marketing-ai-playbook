##`02-market-entry-playbook-builder/synthesis-steps.md`

````markdown
# Synthesis Pipeline: research → freeze → specialists → supervision → PDF

The whole pipeline, with human gates marked. Commands are written for
chat-only mode (one chat per step); in native-agent mode the same
stages run as agents with the same prompts.

```
Stage A  Research (staged commands)          ── human gate ──┐
Stage B  Data freeze (snapshot vN)           ◄── freeze owner┘
Stage C  Specialist runs (4-5 in parallel)
Stage D  Supervision (control loop)          ── human gate ──┐
Stage E  Raw draft (from snapshot + rules)   ◄── supervisor┘
Stage F  Final assembly → PDF                ◄── named human approval
```

Nothing enters Stage B without a human gate, and nothing leaves
Stage F without a named approval. The machine does the middle.

---

## Stage A — Research (staged commands)

Three commands, in order, each in the same chat. Staged — not one
"research this market" mega-request — because the mega-request returns
an unreadable wall; staging keeps every artifact reviewable.

**A1 — Scope pull**
```
Research the market defined below. Use ONLY the connected workspace
sources available to you (docs, knowledge base, relevant conversation
threads). For each relevant item you find, return one row:
{source_ref, source_type, date, what it says in one line}.

MARKET SCOPE (paste from intake-sheet §A):
{{SCOPE}}
TIME HORIZON: {{HORIZON}}

Do not analyze. Do not summarize. Inventory only.
```

**A2 — Relevance filter**
```
Review the inventory above. Keep only items inside the market scope.
For each kept item add: relevance (high/medium/low) and what question
about the market entry it helps answer. Output the filtered list only.
```

**A3 — Extraction**
```
From the filtered list, extract the concrete data points needed for
these sections: market & funnel, GTM & channels, competitive, pricing.
One row per data point: {source_ref, date, data/claim, confidence
(high = multiple independent sources or first-party; medium = single
credible source; low = anecdotal or dated)}.
Flag any section where you found fewer than 3 data points.
```

**Human gate:** review the extraction sheet. Delete weak rows, fix
confidence grades, then move to freeze. This is the cheapest moment
to catch bad data — and the last one before it becomes load-bearing.

## Stage B — Data freeze

Paste the reviewed extraction sheet into
[evidence-snapshot-template.md](evidence-snapshot-template.md),
run the coverage check, freeze as `snapshot-vN.md`.

**From this moment the snapshot is read-only.** Every downstream
citation is an item ID. Need a change? vN+1 + freeze log entry +
re-run only the affected specialists (the template's freeze log shows
how).

## Stage C — Specialist runs

Briefs from [orchestrator-prompt.md](orchestrator-prompt.md):
`market-funnel`, `gtm-channels`, `competitive`, `pricing` — in
parallel, each with **snapshot vN + narrative inputs (§B) + negative
constraints (§C)**. No other inputs. `research-extraction` has already
done its job in Stage A.

Chat-only mode: one chat per specialist. Keep the chats separate —
cross-reading between specialists is how overlap and contradiction
creep in. The supervisor handles coherence in Stage D; specialists
stay narrow.

## Stage D — Supervision

Paste all specialist returns into the supervisor prompt
([orchestrator-prompt.md](orchestrator-prompt.md)). It runs the four
control duties — citation, guardrail, overlap, gap — per return.

Rejected returns go back to their specialist with the `fix_request`.
Loop until all sections are APPROVED or explicitly shipping as
`thin` with [OPEN QUESTION: ...] lines. A rejected return that can't
be fixed from the snapshot becomes an open question — that is a valid
terminal state, not a failure.

**Human gate:** skim the supervisor's rejection log. If everything
was approved on the first pass, be suspicious — check one section
yourself.

## Stage E — Raw draft

```
Assemble the approved sections in SECTION_PLAN order into a raw
document. Do not rewrite section content. Your only edits:
- remove duplicate claims per the overlap resolutions
- add each section's coverage label as a footer
- write the executive summary LAST, from approved sections only
- append a consolidated Risks & Open Questions section from every
  section's open_questions list

Output: full raw draft in markdown.
```

## Stage F — Final assembly → PDF

Merge the raw draft into the pre-loaded standard format template
(headers, branding, TOC) → review-ready playbook.

**Named human approves → PDF export.** The approval is a name and a
date on the cover page, not a vibe. This playbook feeds real entry
decisions; the audit trail (snapshot version, freeze log, rejection
log) ships alongside it.

---

## Acceptance criteria (playbook is review-ready when...)

- [ ] Every factual claim in every section carries a snapshot_ref that
      exists in snapshot vN (spot-check 5 across different sections)
- [ ] Zero guardrail violations open (rejection log clean or resolved)
- [ ] Every `thin` section carries visible [OPEN QUESTION: ...] lines —
      no silently confident gaps
- [ ] Executive summary contains no claim absent from an approved section
- [ ] Each section footer has its coverage label
- [ ] Snapshot version, freeze log and rejection log archived with the PDF

## Troubleshooting

| # | Failure mode | Symptom | Fix |
|---|--------------|---------|-----|
| 1 | Snapshot drift | Sections cite facts not in the snapshot | The specialist chat saw pre-freeze material — re-run it with only the snapshot in context |
| 2 | Constraint erosion | Same claim class violates the list in two sections | The do-not-claim list was paraphrased per agent — re-pass intake-sheet §C verbatim to all |
| 3 | Overlap ping-pong | Two sections keep trading the same claim | Supervisor assigns a single owning agent in the registry; strike from the other permanently |
| 4 | Section freeze | One `thin` section blocks the whole assembly | Ship it as thin with open questions — the pipeline is designed for partial evidence, that's what labels are for |
| 5 | Snapshot vN+1 confusion | Old-citation sections after a re-freeze | Freeze log names affected specialists — re-run only those; others' citations stay valid |
````

