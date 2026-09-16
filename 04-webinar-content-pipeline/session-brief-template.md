# Session Brief — the single source of truth for all pre-session assets

Fill this BEFORE any promo asset is derived. The brief is the contract:
every asset reads it, and nothing outside it exists for promo purposes.

Two rules carry the whole file:

1. **A takeaway that isn't written here doesn't exist for promo assets.**
2. **Everything the promo promises, the live session delivers.** The
   teaser question is answered live; the takeaways are the speakers'
   actual claims.

## Fields

| Field | Entry |
|---|---|
| Working title (outcome-first): | |
| Date/time + timezone: | |
| Registration link (the only CTA): | |
| ICP (who this is for, one line): | |
| KPI target (registrations / attendance): | |

## Speakers — exactly as they should appear in assets

| Name | Title, Company |
|---|---|
| | |

Copied into every asset verbatim. A title misspelled here is
misspelled in seven assets.

## Core takeaways — max 3, verbatim

The only claims any pre-session asset may make about session content.
Write **claims, not topics** — claims carry numbers and verbs; topics
can't be quoted and can't be derived from.

| # | Takeaway (verbatim) |
|---|---|
| 1 | |
| 2 | |
| 3 | |

**Well-formed vs not:**

- ✅ "Campaign build cycles fell from ~3 weeks to 12 days by templating
  multi-format creative into one build pass"
- ❌ "A discussion about creative strategy" — a topic. Nothing can be
  derived from it except filler.

The verbatim rule is what keeps numbers identical across seven assets —
the same reason workflow 01 derives everything from the evidence block.

## Teaser question

One question the promo may tease. It must be answered live — if the
speakers can't commit to answering it, it can't be teased.

## Example brief (synthetic, filled)

| Field | Entry |
|---|---|
| Working title | Creative Is a Test Budget: Cutting Launch Cycles From 3 Weeks to 12 Days |
| Date/time | Thu 2025-11-20, 14:00 GST |
| Registration link | example.com/nova-webinar (fictional) |
| ICP | Growth and performance leads at MEA enterprise retail |
| KPI target | 400 registrations / 35% attendance |

| Speaker | Title, Company |
|---|---|
| Layla Haddad | VP Growth, Nova Apparel |
| Deniz Aral | Solutions Lead, Platform Co (fictional) |

| # | Takeaway (verbatim) |
|---|---|
| 1 | Campaign build cycles fell from ~3 weeks to 12 days by templating multi-format creative into one build pass |
| 2 | Output went from ~40 to 95 assets/month once budgets shifted toward winning creative inside the launch week |
| 3 | Blended ROAS moved 2.1 → 3.4 with no budget increase |

Teaser: "Which launch-week metric predicted ROAS three weeks out —
and why wasn't it CTR?"

## Minimum bar before running the derivation prompt

- 3 takeaways written as claims (not topics)
- All speakers confirmed, titles exact
- Date confirmed, registration link live
- Teaser question confirmed with the speakers

Below that, derivation will invent — see Troubleshooting #1 in
[promo-content-prompt.md](promo-content-prompt.md).

## If the brief changes after derivation started

Fix the brief first, mark it vN+1, re-run only the assets that quote
the changed field — same discipline as workflow 02's freeze log.
Assets derived from a stale brief are deleted, not patched.

## After the session

The final session notes/transcript replaces the brief as the source of
truth for post-session assets (#6 follow-up, #7 recap). The brief stays
frozen as the record of what was promised — which makes the recap
checkable: did the session deliver what the promo claimed? A takeaway
the speakers didn't say gets caught here, at the last gate, instead of
in a customer's memory.
