# ClauDex Coder

ClauDex Coder provides two Bash launchers for running a Claude-led plan and implementation loop with a second reviewer:

- `bin/claudex-coder` pairs Claude with Codex CLI.
- `bin/claursor-coder` pairs Claude with Cursor Agent.

- Author: Mike Lopez <e@mikelopez.com>
- Copyright (C) 2026 Mike Lopez <e@mikelopez.com>

## What It Does

Both launchers follow the same control flow:

1. Accept an optional branch name plus a prompt.
2. Use the current branch when the branch argument is empty, or create a new branch when one is provided.
3. Run Claude with `/plan-it`.
4. Review the generated plan.
5. Loop on Claude `/plan-update` plus re-review until `.ai/branches/<branch-slug>/plan-review.md` contains `ALL GOOD`.
6. Run Claude with `/code-it`.
7. Review the implementation.
8. Loop on Claude `/code-fix` plus re-review until `.ai/branches/<branch-slug>/code-review.md` contains `ALL GOOD`.
9. Stop early if required CLIs are missing from `PATH`.

The retry loop is controlled by:

- `MAX_TRIES` with a default of `20`
- `SLEEP_SECS` with a default of `0.2`

## Launcher Differences

### `claudex-coder`

- Requires `codex`
- Uses `codex --sandbox workspace-write -a never`
- Runs Codex reviews with `$plan-review` and `$code-review`
- Reuses the last Codex session with `resume --last` inside review loops

### `claursor-coder`

- Requires `cursor-agent`
- Uses `cursor-agent --trust --print --model gpt-5.3-codex-high`
- Runs Cursor reviews with `/plan-review` and `/code-review`
- Uses `--continue` for follow-up reviews inside review loops

## Requirements

Common requirements:

- Linux
- `bash`
- `git`
- [GitHub CLI (`gh`)](https://cli.github.com/)
- [Claude CLI](https://docs.anthropic.com/en/docs/claude-code)

Choose the reviewer CLI that matches the launcher you want to use:

- [Codex CLI](https://developers.openai.com/codex/cli/) for `bin/claudex-coder`
- `cursor-agent` for `bin/claursor-coder`

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/easterncoder/claudex-coder.git
cd claudex-coder
```

### 2. Install one or both launchers

```bash
sudo install -m 0755 bin/claudex-coder /usr/local/bin/claudex-coder
sudo install -m 0755 bin/claursor-coder /usr/local/bin/claursor-coder
```

If you prefer to run them from the repo directly:

```bash
chmod +x bin/claudex-coder
chmod +x bin/claursor-coder
```

### 3. Install the shared skills

This repository ships the workflow skills in `skills/`.

Install them for Claude:

```bash
mkdir -p "$HOME/.claude/skills"
cp -R skills/. "$HOME/.claude/skills/"
```

Install them for Codex if you use `claudex-coder`:

```bash
mkdir -p "$HOME/.codex/skills"
cp -R skills/. "$HOME/.codex/skills/"
```

If you use `claursor-coder`, make sure your `cursor-agent` setup exposes the same workflow commands from this repository:

- `/plan-it`
- `/plan-review`
- `/plan-update`
- `/code-it`
- `/code-review`
- `/code-fix`

## Setup Check

Verify the common CLIs:

```bash
git --version
gh --version
claude --version
```

Verify the reviewer CLI for the launcher you plan to run:

```bash
codex --version
```

```bash
cursor-agent --version
```

## Usage

Both launchers use the same argument shape:

```bash
<launcher> [branch-name] <prompt...>
```

To use the current branch, pass an empty string for `branch-name`.

Examples:

```bash
claudex-coder "" "add caching to API responses"
claudex-coder feature/api-caching "add caching to API responses"
```

```bash
claursor-coder "" "add caching to API responses"
claursor-coder feature/api-caching "add caching to API responses"
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
