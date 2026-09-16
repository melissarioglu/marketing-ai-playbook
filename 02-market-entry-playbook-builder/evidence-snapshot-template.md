# Evidence Snapshot — the data freeze, as a concrete artifact

The snapshot is a table. That's the whole format.

What it solves: before the freeze, "what do we know about this market?"
lives in chat threads, docs, and people's heads — and every downstream
agent would quietly drift toward a different version of it. After the
freeze, there is exactly one version, with IDs, and every claim in the
final playbook traces to one.

## Rules

1. Freeze the file as `snapshot-vN.md`. From that moment it is
   **read-only**.
2. Every specialist agent cites item IDs (`#12`) — nothing else.
3. Any change → vN+1 → re-run **only the affected specialists** (see
   the freeze log format below).
4. A snapshot is never "complete" — it is frozen on a date, for a
   decision. Thin coverage is recorded as thin (see coverage check),
   not padded.

---

# EVIDENCE_SNAPSHOT v1 — {{MARKET}}

frozen {{DATE}} by {{NAME}} · scope per intake-sheet v{{N}}

| id | source_type | source_ref | date | data/claim | confidence |
|----|------------|-----------|------|-----------|------------|
| #1 | workspace doc | doc-114 | 2025-09 | GCC paid-social CPM band for enterprise retail | high |
| #2 | research note | note-31 | 2025-08 | Competitor A bundles {{CAPABILITY}} into base tier | medium |
| #3 | thread | th-882 | 2025-07 | KSA distribution partner preference shifts by region | low |
| #4 | | | | | |

## Field notes

- **source_type** — workspace doc / research note / thread / external
  citation. Everything must have entered the workspace through an
  approved channel before it lands here.
- **confidence** — high = multiple independent sources or first-party
  data · medium = single credible source · low = anecdotal or dated.
  Low-confidence items can be cited, but the section that leans on
  them ships with the caveat visible.
- **date** — of the source, not of the freeze. Stale data is visible
  data.

## Coverage check (run before specialist runs)

| section | snapshot items | label |
|---|---|---|
| market-funnel | #1, #4, #7 | full |
| gtm-channels | #5, #9 | partial |
| competitive | #2 | thin → expect open questions |
| pricing | #2 | thin → expect open questions |

Labels: `full` / `partial` / `thin`. A `thin` section ships with
[OPEN QUESTION: ...] lines — by design, not by failure. The label
travels into the section footer of the final playbook, so the reader
knows which sections are evidence-rich and which are directional.

## Freeze log

| version | change | re-ran |
|---|---|---|
| v1 | initial freeze | all specialists |
| v2 | added #14–16 (new dated pricing source for Competitor B) | pricing, competitive |

The freeze log is why hybrid orchestration stays honest: a snapshot
change doesn't silently propagate — it names exactly which agents'
outputs are now stale and must be regenerated.
