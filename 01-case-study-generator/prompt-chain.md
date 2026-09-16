## `01-case-study-generator/prompt-chain.md`

*(Önce küçük patch: dosya 6'daki Results listesinin sonuna şu satırı ekle — 5'li minimum bar ile örnek input'un tutarlılığı için)*

```markdown
- Time to first live campaign post-migration: 9 days (vs. ~5 weeks historical)
```

````markdown
# Prompt Chain: Case-Study Generator

**The one rule: each stage is a separate call. The structured output of
one stage is the only input allowed into the next — no free-text
carryover between stages.**

## How to run

| Stage | Prompt below | You also paste | New chat? |
|---|---|---|---|
| 1 Extraction | yes | raw input document | yes |
| 1b Verification | yes | Stage 1's JSON + the raw input | yes |
| — human fix | no | fix flagged entries by hand | — |
| 2 Narrative | yes | corrected JSON | yes |
| 3 Derivation | yes | (same chat as Stage 2) | no |

## Version history

| v | Architecture | Failure mode |
|---|---|---|
| 1 | Single mega-prompt | Dropped metrics, paraphrased quotes into inventions |
| 2 | Extraction + draft | Quotes still hallucinated under length pressure |
| 3 | Extraction + **verify** + draft + derive | Current — human verification gate added |

The version history is the point: the architecture was not designed in
one shot, it was driven by observed failures. v1 failed because one
prompt cannot hold extraction discipline and storytelling quality at
the same time — under length pressure, storytelling wins and facts lose.

---

## Stage 1 — Extraction

```
### ROLE
You are a marketing research analyst. Your only job is to extract
facts — not to write marketing copy.

### INPUT
A raw customer-engagement document, pasted after this prompt.

### TASK
Extract into exactly this structure:
{ company, industry, region, engagement, timeline, challenge, solution,
  results: [{ value, metric, time_period, source_line }],
  quotes: [{ text, speaker, role }],
  caveats }

### RULES
- Every metric must carry the exact source line it came from
- If a value is implied but not stated, exclude it
- If a field has no evidence, output null — never guess
- Preserve quotes verbatim, including imperfections

### OUTPUT FORMAT
JSON only. No commentary.
```

## Stage 1b — Verification gate

```
You will receive: (1) a JSON evidence block, (2) the source document
it was extracted from.

Compare every entry in the JSON against the source document:

1. results — does source_line literally appear in the source, and does
   it contain the stated value?
2. quotes — does the text appear verbatim in the source?

Output ONLY flagged entries, one per line, as:
FLAGGED | {field, index} | json_value | problem

If nothing is flagged, output the single word: CLEAN
```

Flagged entries are fixed by hand before anything moves forward. This
gate is what makes the chain safe for sales-facing material — and the
verification log it produces ships with every case study as an audit
trail.

## Stage 2 — Master narrative

```
### ROLE
B2B content writer for a {{INDUSTRY}} audience.

### CONTEXT
You receive a structured evidence block. It is the single source of
truth — do not use any knowledge not present in it.

### TASK
Write a master narrative following the template.md structure
(~600 words): outcome-first title with the headline metric, snapshot,
challenge, what we did, results, what's next.

### CONSTRAINTS
- Every number must appear in the evidence block
- Quotes verbatim, with speaker name and role
- If the story has a gap, write [NEEDED: ...] — do not paper over it
- No adjectives doing the work of numbers

### OUTPUT FORMAT
Markdown, one H2 per template section.
```

## Stage 3 — Derivation

```
From the evidence block and master narrative above, produce, each
under its own H2:
1. Web story intro — 120 words
2. Sales one-pager — 5 bullets
3. Deck slide speaker notes — 3 bullets
4. LinkedIn post — ~150 words, hook in the first line
5. Enablement snippet — 2 lines

### RULES
- Every claim traces to the evidence block, not the narrative alone
- Quotes verbatim with attribution
- One message per asset — do not compress the full story into each

### OUTPUT FORMAT
Markdown, one H2 per asset, ready to lift individually.
```

**Why derivation reads from the evidence block, not the narrative:**
the auditable layer is the source of truth for facts; the narrative is
only a storytelling reference. This is what keeps numbers identical
across all five formats.

---

## Acceptance criteria (output is shippable when...)

- [ ] Spot-check 3 numbers across assets: all appear in the evidence block
- [ ] Quotes verbatim, attributed, in every asset where used
- [ ] Zero unresolved [NEEDED: ...] in the shipped version
- [ ] Each derived asset fits its channel (word counts)
- [ ] Verification log archived with the case study (audit trail)

## Troubleshooting

| # | Failure mode | Symptom | Fix |
|---|---|---|---|
| 1 | Thin evidence block | Narrative invents connective tissue | Enforce the intake minimum (5+ sourced metrics, 1+ verbatim quote) before Stage 2 — abort and collect more input if below |
| 2 | Quote paraphrase | Quotes read "better" than the source | Never skip Stage 1b, even under deadline pressure — especially under deadline pressure |
| 3 | Derivation tone drift | All 5 assets read identical | Add a per-asset voice hint to Stage 3 (e.g. "one-pager: scannable, verb-first") |
| 4 | Metric drift between formats | Numbers differ across assets | Confirm Stage 3 reads the evidence block, not the narrative |
| 5 | Long transcript | Input exceeds the context window | Chunk the input, extract per chunk, merge the JSONs, verify the merged block against the full source |
````

