# The `.claude/` layer — same workflows, one command

Every workflow in this repo runs by hand: open any LLM chat, paste the prompts
in order, check the output against the acceptance criteria. That is the
default, and it is deliberate — see [QUICKSTART.md](../QUICKSTART.md).

This folder is the same four workflows wired up for [Claude
Code](https://code.claude.com/docs), so that cloning the repo and typing
`use the case-study-generator skill` runs the chain end to end. Still no code
and no API keys: skills and subagents are markdown files.

```
.claude/
├── skills/
│   ├── case-study-generator/SKILL.md      → workflow 01
│   ├── market-entry-playbook/SKILL.md     → workflow 02
│   ├── reporting-agent/SKILL.md           → workflow 03
│   └── webinar-pipeline/SKILL.md          → workflow 04
└── agents/
    └── evidence-verifier.md               → the verification gate
```

## Single source of truth

The skills do **not** restate the prompts. Each one reads the prompt file in its
workflow folder and orchestrates the stages around it. Edit the workflow folder;
the skill follows. Two copies of a prompt drift apart within a month.

## Why the verification gate is a subagent, not a step

Stage 1b asks whether the extracted metrics and quotes actually appear in the
source. Asking that question inside the same conversation that produced the
draft is a weak check: the context is already anchored on the text under review.

A subagent runs in its own context window with read-only tools. It sees the
source and the claims, and nothing else — not the reasoning that produced them.
That is what makes the gate structural rather than procedural, and it is why
"human validation gates on every output" is enforced by the architecture here
instead of by discipline.

The gate never rewrites anything. It returns `CLEAN` or a table of flagged
entries with a FIX or EXCLUDE verdict. A human decides what happens next.
