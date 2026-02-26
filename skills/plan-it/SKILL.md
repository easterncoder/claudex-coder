---
name: plan-it
description: Create or update a detailed execution plan for the `plan-it` command. Use when the user asks to run `plan-it`, draft a plan, or revise `.ai/branches/{branch-slug}/plan.md` scope. If the request is only to refresh `.ai/branches/{branch-slug}/context/*` without changing `.ai/branches/{branch-slug}/plan.md`, use `plan-context-update`.
---

# Plan It

## Goal

Write a clear, junior-ready plan to `.ai/branches/{branch-slug}/plan.md` and persist supporting context under `.ai/branches/{branch-slug}/context/*`.

## Branch Scope

1. Resolve `branch-slug` from the active git branch:
   - Run `git rev-parse --abbrev-ref HEAD`.
   - If the result is `HEAD`, use `detached-$(git rev-parse --short HEAD)`.
   - Replace `/` with `_` in the final value.
2. Use `.ai/branches/{branch-slug}` as the artifact root for this skill.
3. Do not write planning artifacts to top-level `.ai/*.md`.
4. Apply the shared guard in `.ai/references/default-branch-guard.md` before proceeding.

## Required Outputs

- `.ai/branches/{branch-slug}/plan.md`
- `.ai/branches/{branch-slug}/context/issues/*`
- `.ai/branches/{branch-slug}/context/prs/*`
- `.ai/branches/{branch-slug}/context/repos/*`

## Workflow

1. Run `.ai/references/default-branch-guard.md` and stop immediately if it blocks execution.
2. If scope is missing or the user runs `plan-it` without details, ask `What should I plan for?` and wait for the answer.
3. Confirm this is a planning task, not a context-only refresh task.
4. Read current task scope from the conversation and repository state.
5. Gather issue, PR, and repository context from GitHub when relevant.
6. Save detailed context artifacts into:
   - `.ai/branches/{branch-slug}/context/issues`
   - `.ai/branches/{branch-slug}/context/prs`
   - `.ai/branches/{branch-slug}/context/repos`
7. Use `.ai/templates/plan.md` as the structural baseline for `.ai/branches/{branch-slug}/plan.md`.
8. Fill `CONTEXT > ISSUES`, `PRS`, `REPOS`, and `NOTES` with bullet list items. If empty, write `- None`.
9. Fill implementation sections with explicit file paths, commands, validation steps, and acceptance criteria.
10. Remove ambiguity so a junior developer can execute without guessing.

## Failure Handling

- If `.ai/templates/plan.md` does not exist, create it from the standard template before writing `.ai/branches/{branch-slug}/plan.md`.
- If `.ai/branches/{branch-slug}` does not exist, create it.
- If `.ai/branches/{branch-slug}/plan.md` does not exist, create it before writing plan content.
- If `.ai/branches/{branch-slug}/context/issues`, `.ai/branches/{branch-slug}/context/prs`, or `.ai/branches/{branch-slug}/context/repos` do not exist, create them.
- If `.ai/branches/{branch-slug}/plan.md` is missing but legacy `.ai/plan.md` exists, copy legacy content once before writing.
- If GitHub data is unavailable, continue with local context and add `- Pending remote sync: <reason>` under `CONTEXT > NOTES`.
- If `.ai/references/default-branch-guard.md` blocks execution, do not write or update any planning files.

## Quality Bar

- Keep statements factual and specific.
- Prefer concrete steps over abstract guidance.
- Keep tasks ordered and verifiable.
