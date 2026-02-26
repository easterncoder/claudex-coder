---
name: plan-update
description: Update `.ai/branches/{branch-slug}/plan.md` for the `plan-update` command. Use when the user asks to run `plan-update` by applying feedback from `.ai/branches/{branch-slug}/plan-review.md`, handling `ALL GOOD`, and adding `CLARIFICATIONS` when needed.
---

# Plan Update

## Goal

Apply review feedback from `.ai/branches/{branch-slug}/plan-review.md` to `.ai/branches/{branch-slug}/plan.md` with zero ambiguity.

## Branch Scope

1. Resolve `branch-slug` from the active git branch:
   - Run `git rev-parse --abbrev-ref HEAD`.
   - If the result is `HEAD`, use `detached-$(git rev-parse --short HEAD)`.
   - Replace `/` with `_` in the final value.
2. Use `.ai/branches/{branch-slug}` as the artifact root for this skill.
3. Do not update top-level `.ai/plan.md` in this workflow.
4. Apply the shared guard in `.ai/references/default-branch-guard.md` before proceeding.

## Workflow

1. Run `.ai/references/default-branch-guard.md` and stop immediately if it blocks execution.
2. Read `.ai/branches/{branch-slug}/plan-review.md`.
3. If `.ai/branches/{branch-slug}/plan-review.md` is exactly `ALL GOOD`, respond that all is good and stop.
4. Read the current `.ai/branches/{branch-slug}/plan.md`.
5. Preserve the section structure from `.ai/templates/plan.md` while updating `.ai/branches/{branch-slug}/plan.md`.
6. If unresolved questions remain, add a `CLARIFICATIONS` section with precise questions.
7. Ensure the plan remains junior-ready with concrete steps, file targets, and validation criteria.

## Quality Bar

- Preserve valid existing plan content.
- Resolve every review finding or mark it in `CLARIFICATIONS`.
- Keep the result actionable and testable.

## Failure Handling

- If `.ai/templates/plan.md` does not exist, create it from the standard template before updating `.ai/branches/{branch-slug}/plan.md`.
- If `.ai/branches/{branch-slug}/plan-review.md` is missing but legacy `.ai/plan-review.md` exists, copy legacy content once before update.
- If `.ai/branches/{branch-slug}/plan.md` is missing but legacy `.ai/plan.md` exists, copy legacy content once before update.
- If `.ai/references/default-branch-guard.md` blocks execution, do not write or update any planning files.
