# awesome-agent-skills

Open-source agent skills for Claude Code, OpenAI Codex, Cursor, Gemini CLI, and any tool that supports the [Agent Skills open standard](https://agentskills.io).

## Install via CLI (recommended)

**Claude Code:**
```bash
claude plugin marketplace add awesome-agent-skills/skills
claude plugin install research-and-grill
```

**OpenAI Codex:**
```bash
codex plugin marketplace add awesome-agent-skills/skills
codex plugin install research-and-grill
```

## Skills

| Skill | Description | Category |
|-------|-------------|----------|
| [research-and-grill](./plugins/research-and-grill/) | Research any topic from the web, then get grilled with Socratic questions | Learning |

## Usage

Once installed, invoke by name:

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
