# ClauDex-Coder

ClauDex Coder - When Claude and Codex code together

## What It Does

`claudex-coder` orchestrates a Claude plus Codex workflow that plans, reviews, implements, and re-reviews until quality gates pass.

For each run, it:

- Accepts a branch name plus a prompt.
- Uses the current branch when branch name is empty, or creates a new branch when a branch name is provided.
- Sends your request to Claude with `/plan-it`.
- Runs Codex `$plan-review`.
- Repeats plan updates with Claude `/plan-update` plus Codex re-review until `.ai/branches/<branch-slug>/plan-review.md` contains `ALL GOOD`.
- Runs implementation with Claude `/code-it`.
- Runs Codex `$code-review`.
- Repeats fixes with Claude `/code-fix` plus Codex re-review until `.ai/branches/<branch-slug>/code-review.md` contains `ALL GOOD`.
- Stops with a clear error if `git`, `gh`, `claude`, or `codex` is missing from `PATH`.
- Uses `MAX_TRIES` and `SLEEP_SECS` environment variables to control retry behavior.

- Author: Mike Lopez <e@mikelopez.com>
- Copyright (C) 2026 Mike Lopez <e@mikelopez.com>

## Requirements

- Linux only
- `bash`
- `git`
- [GitHub CLI (`gh`)](https://cli.github.com/)
- [Claude CLI](https://docs.anthropic.com/en/docs/claude-code)
- [Codex CLI](https://developers.openai.com/codex/cli/)

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/easterncoder/claudex-coder.git
cd claudex-coder
```

### 2. Install the launcher script

Choose one of the following:

```bash
# Option A: install globally
sudo install -m 0755 bin/claudex-coder /usr/local/bin/claudex-coder
```

```bash
# Option B: run from this repo directly
chmod +x bin/claudex-coder
```

### 3. Install skills

Install the contents of `skills/` into your preferred CLI skills directory:

```bash
# Codex
mkdir -p "$HOME/.codex/skills"
cp -R skills/. "$HOME/.codex/skills/"
```

```bash
# Claude
mkdir -p "$HOME/.claude/skills"
cp -R skills/. "$HOME/.claude/skills/"
```
You must install skills into both directories for this workflow to work.

## Setup Check

Verify required CLIs are available:

```bash
git --version
gh --version
claude --version
codex --version
```

## Usage

```bash
claudex-coder [branch-name] <prompt...>
```

To use the current branch, pass an empty string for `branch-name`.

Examples:

```bash
# Use current branch
claudex-coder "" "add caching to API responses"
```

```bash
# Create and use a new branch
claudex-coder feature/api-caching "add caching to API responses"
```

## License

GPL-2.0

## Forking and GPL-2.0 Compliance

If you fork or redistribute this project, GPL-2.0 requires that you:

- Keep copyright and license notices in place, including original author attribution.
- Include a copy of the GPL-2.0 license with your distribution.
- Mark modified files with clear change notices and dates.
- License derivative works under GPL-2.0 when distributed.
- Provide complete corresponding source code when distributing binaries or executables.
