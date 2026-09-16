## 📄 Dosya 9/22 — `01-case-study-generator/example-output.md`

````markdown
# Example Output (synthetic) — full run on `example-input.md`

Chain: v3 · Input: Nova Apparel account notes (5 sourced metrics,
1 verbatim quote, 1 trap line)

This file shows what a correct run produces at every stage — use it to
calibrate your own runs. The interesting part is not the prose; it's
the verification log.

---

## Stage 1 — Evidence block

```json
{
  "company": "Nova Apparel",
  "industry": "DTC fashion retail",
  "region": "UAE, KSA",
  "engagement": "advertising platform deployment",
  "timeline": "Mar–Aug 2025",
  "challenge": "Creative production was the launch bottleneck; ~3-week
    build cycles per market launch, ~40 assets/month, creative treated
    as a fixed cost",
  "solution": "Migration onto the platform (Apr 2025): templated
    multi-format creative in one build pass; automated budget shifts
    toward winning creative within each launch week",
  "results": [
    {"value": "~3 weeks → 12 days", "metric": "campaign build cycle",
     "time_period": "per market launch",
     "source_line": "Campaign build cycle: ~3 weeks → 12 days per
       market launch"},
    {"value": "~40 → 95", "metric": "creative assets per month",
     "time_period": "first two quarters post-migration",
     "source_line": "Creative output: ~40 → 95 assets/month"},
    {"value": "2.1 → 3.4", "metric": "blended ROAS",
     "time_period": "across the deployment",
     "source_line": "Blended ROAS: 2.1 → 3.4 across the deployment"},
    {"value": "~18% lower", "metric": "CAC",
     "time_period": "by end of Q2",
     "source_line": "CAC: down ~18% by end of Q2"},
    {"value": "9 days (vs ~5 weeks historical)", "metric": "time to
     first live campaign post-migration",
     "time_period": "April 2025",
     "source_line": "Time to first live campaign post-migration: 9
       days (vs. ~5 weeks historical)"}
  ],
  "quotes": [
    {"text": "We stopped treating creative as a bottleneck and started
      treating it as a test budget.",
     "speaker": "Layla Haddad", "role": "VP Growth"}
  ],
  "caveats": [
    "All figures cover the first two quarters post-migration; longer
     trend not stated in source"
  ]
}
```

---

## Stage 1b — Verification log

```
FLAGGED | results[candidate, CTR] | "improved significantly" | no
  stated value or source_line in source document — implied claim
  only, excluded per Rule 2
CLEAN (v2, 5 results entries verified against source)
```

**The trap was caught.** The source's final line — "CTR improved
significantly after the first month" — contains no value and no
baseline. Stage 1 correctly refused to invent one; the gate logged the
exclusion. An incorrect run would have produced something like
"CTR improved 25%" — a fabricated number that would then propagate
into all five derived assets.

This log ships with the case study as its audit trail.

---

## Stage 2 — Master narrative (results section shown)

Full narrative runs ~600 words per template.md; the results section is
shown here because it's the part that gets checked hardest:

> ## Results
>
> | Metric | Before | After | Period |
> |---|---|---|---|
> | Campaign build cycle | ~3 weeks | 12 days | per market launch |
> | Time to first live campaign | ~5 weeks | 9 days | post-migration |
> | Creative assets per month | ~40 | 95 | first two quarters |
> | Blended ROAS | 2.1 | 3.4 | across deployment |
> | CAC | — | ~18% lower | by end of Q2 |
>
> As Layla Haddad, VP Growth, put it: "We stopped treating creative as
> a bottleneck and started treating it as a test budget."

Note what the narrative does **not** contain: any CTR claim. The gate
excluded it, so it appears nowhere downstream.

---

## Stage 3 — Derived asset: LinkedIn post (1 of 5)

> Nova Apparel's creative team used to be the reason launches slipped —
> three weeks per market, every market. Six months later: 12 days.
>
> What changed: one templated build pass for multi-format creative, and
> budgets that shift toward winning creative inside the launch week
> itself. Output went from ~40 to 95 assets a month. Blended ROAS went
> from 2.1 to 3.4.
>
> As their VP Growth Layla Haddad put it: "We stopped treating creative
> as a bottleneck and started treating it as a test budget."

*(Remaining 4 assets — web intro, one-pager, speaker notes, enablement
snippet — follow the same contract; omitted for brevity.)*

---

## Pre-ship check against acceptance criteria

- [x] Spot-check 3 numbers across assets — all trace to the evidence block
- [x] Quote verbatim and attributed in every asset where used
- [x] Zero unresolved [NEEDED: ...] (no gaps in this run)
- [x] Each asset fits its channel (post: ~100 words + link line)
- [x] Verification log archived above
````

