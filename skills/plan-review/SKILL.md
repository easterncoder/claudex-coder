---
name: plan-review
description: Review `.ai/branches/{branch-slug}/plan.md` for the `plan-review` command. Use when the user asks to run `plan-review`, then write review results to `.ai/branches/{branch-slug}/plan-review.md`, honoring `CLARIFICATIONS`, and write `ALL GOOD` when no issues remain.
---

# Plan Review

## Goal

Review `.ai/branches/{branch-slug}/plan.md` for correctness, completeness, and execution clarity for a junior developer.

## Branch Scope

1. Resolve `branch-slug` from the active git branch:
   - Run `git rev-parse --abbrev-ref HEAD`.
   - If the result is `HEAD`, use `detached-$(git rev-parse --short HEAD)`.
   - Replace `/` with `_` in the final value.
2. Use `.ai/branches/{branch-slug}` as the artifact root for this skill.
3. Do not write review output to top-level `.ai/plan-review.md`.
4. Apply the shared guard in `.ai/references/default-branch-guard.md` before proceeding.

## Workflow

1. Run `.ai/references/default-branch-guard.md` and stop immediately if it blocks execution.
2. Read `.ai/branches/{branch-slug}/plan.md`.
3. Check for a `CLARIFICATIONS` section and treat listed questions as open items.
4. Evaluate ambiguity, missing dependencies, ordering problems, and unverifiable steps.
5. Use `.ai/templates/plan-review.md` as the structural baseline for `.ai/branches/{branch-slug}/plan-review.md` when findings exist.
6. Write results to `.ai/branches/{branch-slug}/plan-review.md` using the output format below.
7. If the plan is fully acceptable, write exactly `ALL GOOD` to `.ai/branches/{branch-slug}/plan-review.md`.
8. If output is `ALL GOOD`, also state that all is good in the user-facing response.

## Output Format

- If there are no findings, write exactly `ALL GOOD`.
- Otherwise, list findings ordered by severity using:
`- severity: <critical|high|medium|low>; file: <path:line|N/A>; impact: <risk>; fix: <concrete action>`

## Failure Handling

- If `.ai/templates/plan-review.md` does not exist, create it from the standard template before writing `.ai/branches/{branch-slug}/plan-review.md`.
- If `.ai/branches/{branch-slug}/plan.md` is missing but legacy `.ai/plan.md` exists, copy legacy content once before review.
- If `.ai/references/default-branch-guard.md` blocks execution, do not write or update any review files.

## Review Focus

- Missing prerequisites
- Vague implementation steps
- Unclear acceptance criteria
- Risky sequencing
- Gaps between scope and plan steps
