# ClauDex-Coder

ClauDex Coder - When Claude and Codex code together

## Requirements

- Linux only
- `bash`
- `git`
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
You can install into one or both of these directories based on your workflow.

## Setup Check

Verify required CLIs are available:

```bash
claude --version
codex --version
```

## Usage

```bash
claudex-coder [branch-name] <prompt...>
```

Examples:

```bash
# Use current branch
claudex-coder "add caching to API responses"
```

```bash
# Create and use a new branch
claudex-coder feature/api-caching "add caching to API responses"
```

## License

GPL-2.0
