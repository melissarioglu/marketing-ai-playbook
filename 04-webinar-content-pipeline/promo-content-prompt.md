##  `04-webinar-content-pipeline/promo-content-prompt.md`

````markdown
# Promo Content Prompt — derivation fan-out from the session brief

## How this runs

Workflow 01 is a **chain** (each stage feeds the next). This one is a
**fan-out**: every asset derives independently from the same frozen
source — the session brief — so assets run as separate calls, in
parallel, with the same rules.

| Call | Derives | You paste |
|---|---|---|
| 1 | Registration page | base prompt + asset spec 1 + brief |
| 2 | Invitation email — announce | base prompt + asset spec 2 + brief |
| 3 | Invitation email — reminder | base prompt + asset spec 3 + brief |
| 4 | LinkedIn promo post | base prompt + asset spec 4 + brief |

One call per asset, one brief in every call. Never one mega-call for
all four — that was v1, and it failed. Live and post-session assets
(5-7) follow the same pattern with a different source — see the bottom
section.

## Version history

| v | Architecture | Failure mode |
|---|---|---|
| 1 | One mega-call: all assets at once | Each asset paraphrased the takeaways slightly differently; by the last asset the session promised something the speakers never planned to say |
| 2 | Fan-out: one call per asset, verbatim-claim rules | Current — assets are independent but identical to the brief |

The failure was drift between copies of the same claim. The fix was
not better writing — it was making the brief the only place claims
exist, and every asset quote them, not restate them.

---

## Base prompt (pasted into every call)

```
### ROLE
You are a B2B webinar marketer writing ONE asset for a session
targeted at the ICP defined in the brief.

### CONTEXT
- SESSION_BRIEF is pasted below. It is the single source of truth.
  Takeaways, speaker names/titles, title, date and teaser question
  are VERBATIM fields: copy them exactly, never reword.
- You are writing exactly one asset. Do not draft other assets, do
  not summarize the brief beyond what this asset needs.

### CONSTRAINTS
- Every takeaway referenced must appear verbatim in the brief
- Speaker names/titles exactly as in the brief
- One CTA per asset — the registration link, nothing else
- No invented facts: no stats, no quotes, no audience demand
  ("limited Q&A time" is allowed — structurally true for a live
  session; "selling fast", "only X seats left" are invented demand
  and forbidden)
- The teaser question may be referenced only if the brief contains
  it — it must be answerable live
- No adjectives doing the work of the brief's numbers

### OUTPUT FORMAT
Markdown, ready to paste into the channel. No commentary.
```

## Asset specs (one per call)

**Spec 1 — Registration page**
```
ASSET: registration page copy.
- Headline: outcome-first, built from the title + the single
  strongest number in the takeaways
- 3 "you will learn" bullets = the 3 takeaways, verbatim
- Speaker block: names/titles verbatim
- Teaser question as a closing line above the CTA
- Single CTA button label + the registration link
Length: one screen. No section may exceed 2 sentences except bullets.
```

**Spec 2 — Invitation email (announce)**
```
ASSET: announcement email, send ~10 days before.
- 3 subject line options (max 9 words each, one may carry a number
  from the takeaways)
- Body ~120 words: problem hook → takeaways 1-3 → speakers → CTA
- Single CTA, placed once
```

**Spec 3 — Invitation email (reminder, 48h before)**
```
ASSET: reminder email, send 48 hours before.
- 1 subject line (max 8 words)
- Body ~80 words: lead with the teaser question → what attendees
  get → CTA
- Scarcity framing limited to structural truths: live Q&A is finite,
  session is live only. No invented demand.
```

**Spec 4 — LinkedIn promo post**
```
ASSET: LinkedIn promo post.
- ~150 words, hook in the first line
- Tease ONE takeaway (the strongest number) — do not compress all
  three into the post; the registration page carries the full set
- Include the teaser question as the engagement hook
- Registration link in the first comment, "link in comments" line
  in the post
```

## Example run (synthetic — brief from `session-brief-template.md`)

Call 4 output (LinkedIn post):

> Three weeks per market launch. Every market. Nova Apparel's creative
> team was the reason launches slipped — until build cycles fell to
> 12 days.
>
> On Nov 20, Layla Haddad (VP Growth, Nova Apparel) walks through what
> changed: one templated build pass for multi-format creative, and
> budgets that shift toward winning creative inside the launch week.
> Output went from ~40 to 95 assets/month. Blended ROAS moved 2.1 →
> 3.4 with no budget increase.
>
> One question we'll answer live: which launch-week metric predicted
> ROAS three weeks out — and why wasn't it CTR?
>
> Link in comments.

Spot-check: takeaway #1 verbatim ✓ · teaser question verbatim ✓ ·
speaker name/title verbatim ✓ · numbers as in brief ✓ · one CTA ✓ ·
~110 words ✓

---

## Live + post-session assets (5-7) — same pattern, source switch

| # | Asset | Source of truth |
|---|---|---|
| 5 | Run-of-show + discussion questions | brief (frozen) |
| 6 | Follow-up email — attendee + no-show variants | final session notes/transcript |
| 7 | Recap post | final session notes/transcript |

Two structural notes:

1. **Asset 5 derives from the frozen brief** — discussion questions
   expand each takeaway and commit an answer to the teaser question.
   The run-of-show is also where the brief's promises become the
   agenda: if a takeaway can't be scheduled into the show, the brief
   was wrong — fix it before promo ships, not during the session.
2. **Assets 6-7 inherit the brief as a checklist.** The recap checks
   delivered vs. promised: every takeaway from the frozen brief gets
   either a supporting moment from the transcript or a logged
   mismatch. This is the pipeline's last gate — a promised-but-not-
   delivered takeaway is caught here, in writing, not by a customer's
   memory.

## Acceptance criteria (assets are shippable when...)

- [ ] Every takeaway in every asset appears verbatim in the brief
- [ ] Speaker names/titles verbatim, every asset
- [ ] One CTA per asset — the registration link
- [ ] Zero invented stats, quotes or demand claims
- [ ] Each asset fits its spec (word counts, subject line limits)
- [ ] The 48h reminder's scarcity framing uses only structural truths

## Troubleshooting

| # | Failure mode | Symptom | Fix |
|---|---|---|---|
| 1 | Thin brief → invention | Assets add stats or angles not in the brief | Stop. Enforce the brief's minimum bar (3 claim-takeaways, speakers confirmed, link live) — derivation on an incomplete brief is fabrication with formatting |
| 2 | Takeaway paraphrase drift | Assets "improve" the wording; numbers round differently | Verbatim fields were treated as inspiration — re-run with the base prompt's CONTEXT block first, and spot-check claims across assets |
| 3 | Scarcity invention | "Selling fast" appears in the reminder | Channel conventions pull it in — regenerate with the demand-claim rule quoted verbatim in the call |
| 4 | Title drift | Each asset rewords the session title | Title is a verbatim field like speakers — re-run affected assets |
| 5 | Mega-call relapse | One call produces all four assets with drifting claims | The fan-out is the architecture, not a preference — one call per asset, always |

---

## Pattern map (the whole repo in one table)

| Workflow | Topology | The failure that chose it |
|---|---|---|
| 01 Case-study generator | Linear chain — stages feed the next | One prompt can't hold extraction discipline and storytelling at once |
| 02 Market-entry builder | Supervisor loop — specialists + control layer | Parallel sections contradicted each other without a control layer |
| 03 Reporting agent | Two-call gate — generate, then verify separately | A model grading its own homework passes everything |
| 04 Webinar pipeline | Fan-out — independent assets from one frozen source | Copies of the same claim drifted apart |

Four workflows, four topologies, one principle: **the architecture is
the answer to the failure mode.** Pick the shape from what breaks,
not from what's fashionable.
````
