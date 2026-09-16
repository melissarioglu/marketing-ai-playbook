# Marketing AI Playbook

Open, generalized versions of AI workflows I run in production as a B2B
marketing operations lead (12+ years across TikTok, Google, and Getir,
METAP/MEA).

These are not prompts I collected — they are workflow designs I built to
solve real operational bottlenecks: slow case-study production, weeks-long
market-entry research, manual reporting, single-use webinar content.

**All company names, datasets, and outputs in this repo are synthetic.**
The methodologies are generalized from production workflows; no confidential
or client material is included. Impact figures below are my own before/after
observations from running these workflows in production — they are stated as
durations rather than percentages, because that is the form in which I can
describe them honestly outside the organizations where they ran.

## Use it, don't just read it

Every workflow ships as a working kit: intake template → copy-ready prompts →
acceptance criteria → troubleshooting. Start with
[QUICKSTART.md](QUICKSTART.md) — workflow 01 runs end to end in ~15 minutes
with no setup, no code, and no API keys.

## What's inside

| # | Workflow | AI pattern | Before → after |
|---|----------|-----------|----------------|
| 1 | [Case-study generator](01-case-study-generator/) | Prompt chaining + verification gate | ~2 weeks → ~4-5 working days per case study |
| 2 | [Market-entry playbook builder](02-market-entry-playbook-builder/) | Multi-step agent orchestration | Multi-week research → same-day first draft |
| 3 | [Reporting agent](03-reporting-agent/) | Data → narrative generation | Hours of writing → minutes of review per cycle |
| 4 | [Webinar content pipeline](04-webinar-content-pipeline/) | Content ops automation | 4 runnable assets per session, follow-ups same-day |

## Design principles

1. **Redesign the workflow around the AI — don't bolt AI onto the old
   process.** Each workflow started from "if I had this capability from day
   one, how would the process look?", not "which step can I automate?"
2. **Prompt structure is engineering, not writing.** Every prompt follows the
   same architecture — role → context → constraints → output format — iterated
   over 12+ months of production use, with the failures recorded.
3. **A verification gate on every output.** The model produces the draft; a
   separate call checks it against the source; a named human approves before
   anything ships. Generation and checking never share a call.
4. **No confidential or client data goes into a tool the organization hasn't
   approved.** Everything here runs on generalized or synthetic inputs.

## Or run it in Claude Code

Clone the repo and the workflows are available as skills:

```
use the case-study-generator skill
```

The chain, the verification gate, and the acceptance checklist run in order.
Still no code and no API keys — see [`.claude/README.md`](.claude/README.md)
for why the verification gate runs as a separate subagent.

## Tools

Claude · Claude Code · Lark
