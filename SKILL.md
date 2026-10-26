---
name: paep-project-manager
description: Review a PAEP project baseline, blockers and next actions when a project needs an evidence-backed engineering plan.
metadata:
  version: 1.1.0
  owner: paep-user
  category: project
---

# paep-project-manager

Read the project baseline and supplied task context. Identify the current goal, verified state, blockers, acceptance criteria and next action with an owner. Distinguish recorded progress from inference; do not invent percentages or successful tests. Produce a concise project review in the declared artifact. Suggest decision records and loop stages for the user to approve; this review does not mutate project state.

## Inputs

- `repository_path` (required, path): absolute project directory within configured PAEP roots. Read-only.
- `brief` (optional, string): additional non-sensitive review context, e.g. which decision or phase the review supports.

## Output

`artifacts/project-report.md` — evidence-backed review in the fixed sections below.

## Baseline reading order

Read in this order and stop expanding scope once the four layers are covered:

1. Authority pointer: the project's current-state record (e.g. `CURRENT_STATE.json` or equivalent) — note what it points to, not what older reports claim.
2. Pointed authority: the state/checkpoint file(s) the pointer names — status, revision, seals, recorded blockers.
3. Pending work records: newest status/work logs and open decision records for the reviewed scope.
4. Task context: the supplied brief and any referenced task files; read only what the brief references inside `repository_path`.

Never treat a historical PASS as current state; carry it forward only as a dated, sourced fact.

## Evidence levels

Tag every claim in the report with one of:

- `[recorded]` — present in a file read during this review; cite the path.
- `[derived]` — computed or inferred from recorded files; state the derivation.
- `[assumed]` — neither recorded nor derivable; state what would verify it.

A review whose key conclusions are `[assumed]` must say so in the summary; do not dress assumptions as findings.

## Fixed report sections

The artifact uses exactly these top-level sections, in this order:

```text
# Project review — <project> — <UTC date>
## Goal (as recorded)         — quoted or cited from authority files, [recorded]
## Verified state             — per area: claim + evidence level + source path
## Blockers                   — each with evidence, owner and what would clear it
## Acceptance criteria        — what evidence would count, per pending item
## Next action                — one primary action, owner, stop condition
## Decision requests          — items needing user approval, each with options
## Sources                    — every file read, with UTC read time
```

## Example invocation

```markdown
# Goal
Review the skills-console project state before the next implementation phase.

# Context
Authority pointer and current phase are recorded in the repository.

# Constraints
Read-only; no state mutation; cite every claim.

# Acceptance criteria
- Goal, state, blockers and next action each carry evidence levels and source paths.
- Decision requests are separable from findings.
```

## Combination with professional skills

For repository hygiene questions combine with `repo-maintainer` (housekeeping scope), for release questions with `paep-release-engineer`, and for process design output feed this review's next action into `paep-long-loop-manager`. This skill stays the state/baseline reviewer; do not duplicate their procedures here.

## Handoff convention

The report is self-contained for handoff: anyone picking it up needs only `repository_path`, the brief, and the report. Record the reading order used and any authority file that was unreadable; a missing authority file is a blocker, not a blank field.

## Execution boundary

Use the PAEP task/result protocol and the declared Markdown output in skill.yaml.
The runtime uses a reviewed external codex handoff. Writes, network,
shell commands, credentials and production mutations are outside this skill's
permissions. If evidence is missing, report it and stop; do not broaden access.
