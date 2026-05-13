# Skills

A personal collection of agentic skills by Walid KINI, compatible with [Claude Code](https://claude.ai/code) and [Codex CLI](https://developers.openai.com/codex/cli).

## Installation

Clone this repository:

```bash
git clone https://github.com/walkidni/skills
```

Then symlink the skills you want to use.

### Claude Code

```bash
ln -s /path/to/skills/<skill-name> ~/.claude/skills/<skill-name>
```

### Codex CLI

```bash
ln -s /path/to/skills/<skill-name> ~/.codex/skills/<skill-name>
```

Symlinks are supported natively by both tools — changes take effect immediately after a `git pull`.

## Updating

```bash
cd /path/to/skills && git pull
```

## Skills

| Skill | Description |
|-------|-------------|
| [`sequential-task-execution`](./sequential-task-execution/SKILL.md) | Execute approved feature plans one task at a time with TDD, spec review, optional code-quality review, and history tracking. Supports Full Mode and Lite Mode. |
