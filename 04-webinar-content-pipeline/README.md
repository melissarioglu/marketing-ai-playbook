## `04-webinar-content-pipeline/README.md`

````markdown
# 04 · Webinar Content Pipeline

## Problem
A webinar produced one live session and then died. Promo assets were
written from scratch each time, and the session itself — the most
expensive hour of thinking the team produced — was never reused.
Follow-up content shipped late or not at all, and titles, speaker
claims and takeaways drifted differently in every asset.

## Approach
Treat the webinar as a content system, not an event. One master
document feeds a derivation chain producing the full asset set:

| # | Asset | Phase |
|---|---|---|
| 1 | Registration page copy | pre |
| 2 | Invitation email — announce | pre |
| 3 | Invitation email — reminder (48h) | pre |
| 4 | LinkedIn promo post | pre |
| 5 | Run-of-show + discussion questions | live |
| 6 | Follow-up email — attendee + no-show variants | post |
| 7 | Recap post | post |

Same evidence-first contract as workflow 01: pre-session assets derive
from the **session brief** (single source of truth, written before
anything is promoted); post-session assets derive from the final
session notes/transcript. A takeaway that isn't in the source document
doesn't exist — and therefore can't drift between assets.

**Why derivation beats one mega-call:** the first version generated all
assets in a single prompt. Predictable failure — each asset paraphrased
the takeaways slightly differently, and by asset #6 the session promised
something the speakers never planned to say. The fix was the same as
everywhere else in this repo: narrow per-asset calls, verbatim-claim
rules, one source of truth.

## Measured impact
- 7 assets per session at near-zero incremental writing
- Follow-up assets shipped same-day instead of the following week

## Files

| File | What it is |
|---|---|
| `session-brief-template.md` | Input contract — the single source of truth for all pre-session assets |
| `promo-content-prompt.md` | Pre-session derivation prompt + acceptance criteria + troubleshooting (representative; post-session prompts follow the identical structure) |

## Acceptance criteria (assets are shippable when...)

- [ ] Every takeaway referenced appears **verbatim** in the session brief
- [ ] Speaker names/titles exactly as in the brief, every asset
- [ ] One CTA per asset — the registration link
- [ ] Zero invented stats, quotes or demand claims ("limited Q&A time"
      allowed; "selling fast" is not — check prompt rules)
- [ ] Each asset fits its channel (word counts in the prompt spec)

## Run it

~20 minutes — see [QUICKSTART.md](../QUICKSTART.md). Fill
`session-brief-template.md` first; the brief is the contract everything
else reads.

*All data in this folder is synthetic.*
````

---
