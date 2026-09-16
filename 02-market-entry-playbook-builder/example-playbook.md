## `02-market-entry-playbook-builder/example-playbook.md`

````markdown
# Example (synthetic) — GCC market-entry playbook, excerpt

**Market:** GCC enterprise retail (UAE, KSA) · Meridian Analytics
(fictional B2B SaaS) entering the region
**Inputs:** intake-sheet v1 · evidence snapshot **v1** (12 items, frozen)
**Pipeline:** chat-only mode · 4 specialists + supervisor · 1 rejection
loop total
**Output:** review-ready playbook (excerpt below) + audit trail
(snapshot, freeze log, rejection log)

The excerpt shows the three things that matter about this workflow's
output: **inline citations, visible coverage labels, and open questions
shipping as open questions.** Full playbook runs ~10-14 pages; what's
shown is the part that gets checked hardest.

---

## Supervisor log (excerpt, verbatim format)

| # | Section | Agent | Verdict | Detail |
|---|---|---|---|---|
| 1 | market-funnel | market-funnel | APPROVED | 6/6 claims cited; no overlaps |
| 2 | competitive | competitive | APPROVED (v2) | v1 rejected → guardrail; v2 clean |
| 3 | pricing | pricing | APPROVED (v2) | v1 rejected → guardrail 2; figure replaced by [OPEN QUESTION] |
| 4 | gtm-channels | gtm-channels | APPROVED | 1 overlap resolved (see #7) |

**Rejection record — pricing v1:**
```json
{
  "section": "pricing",
  "agent": "pricing",
  "status": "rejected",
  "reason": "guardrail 2: Competitor B price stated without a dated
    source line",
  "fix_request": "remove the figure or cite a snapshot item; if none
    exists, emit [OPEN QUESTION: ...] instead"
}
```

**The fix as shipped:** the pricing agent removed the invented figure
and emitted the open question. The v2 return cites only snapshot #2
and #9 — and the section is *thinner* than v1 but *shippable*, which
is the whole point: the pipeline is designed to produce honest partial
answers, not confident complete-looking ones.

---

## Section excerpt — Competitive (footer shows coverage label)

> ### Competitive
>
> **Landscape.** Three players hold recognizable positions in GCC
> enterprise retail. Competitor A bundles {{CAPABILITY}} into its base
> tier at a lower entry price point *(#2, 2025-08, medium confidence)*,
> which shapes the packaging recommendation in the Pricing section.
> Competitor B's current packaging is not covered by the snapshot —
> excluded from the positioning table rather than estimated.
>
> **[OPEN QUESTION]** No 2025 pricing or packaging data for Competitor B
> in snapshot v1 → positioning table covers A and C only.
>
> **Implication for entry.** Our entry angle (narrative input #2) lands
> against Competitor A's bundled tier; positioning, not price, carries
> the pitch. This leans on #3 — KSA distribution preferences shifting
> by region *(low confidence)* — so treat channel implications as
> directional.
>
> *Coverage: thin · evidence snapshot v1 · items #2, #3*

---

## Section excerpt — Pricing (thin section, shipped honestly)

> ### Pricing
>
> GCC paid-social CPM band for enterprise retail: snapshot #1
> *(2025-09, high confidence)* → acquisition cost floor is modelable.
> Competitor price points beyond Competitor A's base-tier bundle
> *(#2)*: not in snapshot.
>
> **[OPEN QUESTION]** Competitor B 2025 pricing — needed before final
> price positioning. Owner: competitive research, due before the
> playbook's next freeze (v2).
>
> *Coverage: thin · evidence snapshot v1 · items #1, #2*

---

## Executive summary (written last, from approved sections only)

> Meridian's GCC entry case rests on two evidence-backed facts and one
> named gap. Evidence: enterprise-retail CPM economics are favorable
> *(#1)*, and the closest incumbent bundles a comparable capability into
> its base tier *(#2)* — so entry competes on positioning, not price.
> Named gap: Competitor B's current packaging is unknown; the pricing
> recommendation is explicitly conditional on closing that gap before
> the next snapshot freeze.

No claim above exists outside an approved section — the assembly rule
holds, and it's checkable in one read.

---

## Pre-ship check against acceptance criteria

- [x] Spot-check 5 claims across 4 sections — all `snapshot_ref` values exist in v1
- [x] Rejection log clean (both v1 violations resolved in v2)
- [x] Both `thin` sections carry visible [OPEN QUESTION: ...] with owners
- [x] Executive summary contains no claim absent from approved sections
- [x] Every section footer carries its coverage label
- [x] Audit trail archived: snapshot v1 + freeze log + supervisor log

---

*Everything above is synthetic — Meridian Analytics, all snapshot
items, and all figures are fictional.*
````
