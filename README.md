# awesome-agent-skills

Open-source agent skills for Claude Code, OpenAI Codex, Cursor, Gemini CLI, and any tool that supports the [Agent Skills open standard](https://agentskills.io).

## Skills

| Skill | Description | Category |
|-------|-------------|----------|
| [research-and-grill](./plugins/research-and-grill/) | Research any topic from the web, then get grilled with Socratic questions | Learning |
| [pr-narrative](./plugins/pr-narrative/) | Generate a human PR title + description from git history. Creates or updates the PR automatically | Workflow |
| [pattern-enforcer](./plugins/pattern-enforcer/) | Scan codebase for existing patterns before implementing. Enforces consistency across UI, errors, tests, and naming | Workflow |

## Install

### Claude Code

Add the marketplace once, then install any skill:

```bash
claude plugin marketplace add awesome-agent-skills/skills
claude plugin install research-and-grill
claude plugin install pr-narrative
claude plugin install pattern-enforcer
```

### OpenAI Codex CLI

Skills are not yet in the official Codex marketplace. Install manually:

```bash
git clone https://github.com/awesome-agent-skills/skills.git
cp -r skills/plugins/research-and-grill ~/.codex/skills/
cp -r skills/plugins/pr-narrative ~/.codex/skills/
cp -r skills/plugins/pattern-enforcer ~/.codex/skills/
```

Once listed in the official Codex marketplace, you'll be able to install via `/plugins`.

### Cursor

```text
/add-plugin research-and-grill
```

Or search for the skill name in the Cursor plugin marketplace.

### Gemini CLI

```bash
gemini extensions install https://github.com/awesome-agent-skills/skills
```

## Usage

```
# Learn anything from the web + get Socratic quiz
/research-and-grill quantum computing
/research-and-grill --deep the French Revolution

# Generate a human PR title + description for current branch
/pr-narrative

# Update a specific PR
/pr-narrative 42

# Enforce codebase consistency before implementing
/pattern-enforcer add a dropdown to the settings page
/pattern-enforcer write a test for UserService.create
```

## Contributing

We welcome new skills! See [CONTRIBUTING.md](./CONTRIBUTING.md) to get started.

Use the [skill-template](./skill-template/) as your starting point.

## Standards

All skills follow the [Agent Skills open standard](https://agentskills.io) — a cross-vendor specification that works across 35+ AI tools.

## License

MIT
