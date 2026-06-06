# awesome-agent-skills

Open-source agent skills for Claude Code, OpenAI Codex, Cursor, Gemini CLI, and any tool that supports the [Agent Skills open standard](https://agentskills.io).

## Skills

| Skill | Description | Tools |
|-------|-------------|-------|
| [research-and-grill](./research-and-grill/) | Research any topic from the web, then get grilled on it with Socratic questions | Claude Code, Codex, Cursor |

## Installation

### Claude Code
```bash
cp -r research-and-grill ~/.claude/skills/
```

### OpenAI Codex
```bash
cp -r research-and-grill ~/.codex/skills/
```

### Any skill
```bash
# Clone the repo
git clone https://github.com/awesome-agent-skills/skills.git

# Copy the skill you want
cp -r skills/<skill-name> ~/.claude/skills/   # Claude Code
cp -r skills/<skill-name> ~/.codex/skills/    # Codex
```

## Usage

Once installed, invoke any skill by name:

```
/research-and-grill quantum computing
/research-and-grill --deep the French Revolution
```

## Contributing

We welcome new skills! See [CONTRIBUTING.md](./CONTRIBUTING.md) to get started.

Use the [skill-template](./skill-template/) as your starting point.

## Standards

All skills follow the [Agent Skills open standard](https://agentskills.io) — a cross-vendor specification that works across 35+ AI tools.

## License

MIT
