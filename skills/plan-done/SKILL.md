---
name: plan-done
description: Delete the current branch artifact folder for the `plan-done` command. Use when the user wants to reset branch planning artifacts under `.ai/branches/{branch-slug}`.
---

# Plan Done

## Goal

Reset planning workflow artifacts so work can restart from a clean state.

## Branch Scope

1. Resolve `branch-slug` from the active git branch:
   - Run `git rev-parse --abbrev-ref HEAD`.
   - If the result is `HEAD`, use `detached-$(git rev-parse --short HEAD)`.
   - Replace `/` with `_` in the final value.
2. Use `.ai/branches/{branch-slug}` as the only cleanup target.

## Targets To Clear

- `.ai/branches/{branch-slug}`

## Workflow

1. Resolve the target folder `.ai/branches/{branch-slug}`.
2. If the folder exists, delete the entire folder recursively.
3. Report that the folder was removed.
4. If the folder does not exist, respond with `Nothing to clear`.

## Safety Rules

- Only remove `.ai/branches/{branch-slug}`.
- Never remove `.ai/templates/*`.
- Never remove `.ai/references/*`.
- Never remove files outside `.ai/`.
- Do not create a commit in this workflow.

## Failure Handling

- If the repository is not a git repo, stop and ask the user for the branch slug.
- If delete fails, report the error and stop.
- If target folder does not exist, return `Nothing to clear`.
