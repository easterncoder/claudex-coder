---
name: plan-context-update
description: Refresh context artifacts for the `plan-context-update` command. Use when the user asks to run `plan-context-update` and only `.ai/branches/{branch-slug}/context/issues`, `.ai/branches/{branch-slug}/context/prs`, and `.ai/branches/{branch-slug}/context/repos` need updates. If `.ai/branches/{branch-slug}/plan.md` must be created or changed, use `plan-it`.
---

# Plan Context Update

## Goal

Refresh `.ai/branches/{branch-slug}/context/*` with accurate and current project context.

## Branch Scope

1. Resolve `branch-slug` from the active git branch:
   - Run `git rev-parse --abbrev-ref HEAD`.
   - If the result is `HEAD`, use `detached-$(git rev-parse --short HEAD)`.
   - Replace `/` with `_` in the final value.
2. Use `.ai/branches/{branch-slug}` as the artifact root for this skill.
3. Do not write context updates to top-level `.ai/context/*`.
4. Apply the shared guard in `.ai/references/default-branch-guard.md` before proceeding.

## Workflow

1. Run `.ai/references/default-branch-guard.md` and stop immediately if it blocks execution.
2. Inspect existing context files under `.ai/branches/{branch-slug}/context/issues`, `.ai/branches/{branch-slug}/context/prs`, and `.ai/branches/{branch-slug}/context/repos`.
3. Pull latest relevant GitHub data for the current task scope.
4. Update existing context files or add new ones where needed.
5. Keep each file focused, factual, and traceable to source data.
6. Preserve useful existing context that is still valid.
7. Do not modify `.ai/branches/{branch-slug}/plan.md` in this workflow.

## Output Rules

- Keep context grouped by folder purpose (`issues`, `prs`, `repos`).
- Use stable, readable filenames.
- Remove stale or contradictory statements when updating.

## Failure Handling

- If `.ai/branches/{branch-slug}/context/issues`, `.ai/branches/{branch-slug}/context/prs`, or `.ai/branches/{branch-slug}/context/repos` do not exist, create them.
- If branch-scoped context folders are missing but legacy `.ai/context/*` exists, copy legacy context once before updating.
- If GitHub data is unavailable, update from local data and write status notes in `.ai/branches/{branch-slug}/context/repos/_sync-status.md`.
- In `_sync-status.md`, add `- Pending remote sync: <reason>` entries for unresolved remote data.
- If `.ai/references/default-branch-guard.md` blocks execution, do not write or update any context files.
