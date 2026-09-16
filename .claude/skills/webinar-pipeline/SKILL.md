---
name: webinar-pipeline
description: Derive a full promotion and follow-up asset set from a single webinar or event session brief — promo copy, registration page text, recap, and repurposed content — with all claims bounded by the brief's stated takeaways. Use when the user wants webinar promotion, event content, session recaps, or to repurpose one session into multiple assets.
---

# Webinar Content Pipeline

Runs workflow 04. The canonical derivation prompt lives in
`04-webinar-content-pipeline/` — read the prompt file there and use it verbatim.

## Before starting

Require a completed `session-brief-template.md`. If the three core takeaways
are not written down, stop and collect them. This is not a formality: the
takeaways are the entire claim budget for every asset produced.

**A takeaway that is not in the brief does not exist.** You may not promise
anything on the promo assets that the session does not deliver — the credibility
cost lands on the person hosting, not on the copy.

## Procedure

1. Read the completed brief. Verify speaker names, titles, and company are
   spelled exactly as they should appear publicly; ask rather than guess.
2. Apply the derivation prompt to produce the asset set.
3. Give each asset its own job: promo creates the question, the registration
   page answers "why attend", the recap delivers substance, the repurposed
   content stands alone without the session. Do not compress all three
   takeaways into every asset.
4. The teased question must be one the brief marks as answered live.

## Verification

Invoke the `evidence-verifier` subagent with the brief and the asset set. Any
claim not traceable to a brief takeaway is flagged for removal.

## Hard rules

- No statistics, customer names, or product claims that are not in the brief.
- No "industry-leading" style adjectives — they are unverifiable by design.
- Speaker attribution is exact or absent.
