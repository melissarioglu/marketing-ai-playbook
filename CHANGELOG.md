# Changelog

All notable changes to this repo. The repo itself follows the same
discipline its workflows preach: versioned, dated, with failure modes
recorded — not rewritten silently.

## [0.3.0] — Repair and Claude Code layer

### Fixed
- Every file was committed wrapped in its chat-transcript container: a
  filename heading plus a fenced code block. GitHub rendered the whole
  repo as code blocks, and `data/campaign-performance.csv` was not a
  parseable CSV at all. All 20 files unwrapped.
- `03-reporting-agent/README.md` "What good looks like" restated against
  the actual CSV: regional paid-social −19% WoW (314 → 255), GCC −37%,
  LEVANT +9.7% and therefore silent. The previous figures (−22%, −38%,
  "3 regions") did not match the data file.
- `01-case-study-generator/example-input.md` gained a fifth sourced
  metric, so the sample input clears the intake minimum it documents.
- Added the missing `01-case-study-generator/README.md`.
- Root README restored: what's inside, design principles, tools.

### Added
- `.claude/` — the four workflows as Claude Code skills, plus
  `evidence-verifier`, the verification gate as a read-only subagent
  running in its own context.

### Changed
- Impact figures stated as before/after durations rather than
  percentages with an unverifiable footnote.
- Workflow 04 scope stated honestly: specs 1-4 are published and
  runnable; assets 5-7 are documented as a pattern, not as copy-ready
  specs. The impact figure now counts only what a reader can run.

## [0.2.0] — Working kits release

### Added
- `QUICKSTART.md` — run workflow 01 end-to-end in ~15 minutes, zero setup
- Workflow 01: `intake-template.md`, `example-input.md` (with a
  deliberately vague metric line), full end-to-end `example-output.md`
  including the verification log
- Workflow 01: acceptance criteria and troubleshooting table
  (5 documented failure modes with fixes)
- Workflow 02: `intake-sheet.md` and `evidence-snapshot-template.md` —
  the data freeze is now a concrete, copyable artifact with a coverage
  check and a freeze log
- Workflow 03: `config.md` — thresholds separated from the prompt
  contract
- Workflow 04: `session-brief-template.md`

### Changed
- All prompts converted from inline quotes to copy-ready code blocks
- Workflow 02 impact metric restated as absolute durations
  (multi-week → same-day first draft, 2-3 days to final PDF)

### Why
v0.1 described the workflows. v0.2 makes them runnable by a stranger:
intake templates define the input, acceptance criteria define "done",
troubleshooting documents what actually breaks. The upgrade was driven
by one test: *can someone who has never met me run workflow 01 and
watch the verification gate catch the bad metric?*

## [0.1.0] — Initial release

- Four workflow folders: case-study generator, market-entry playbook
  builder, reporting agent, webinar content pipeline
- Prompt chains with version history and failure-mode notes
- Design principles and ground rules
