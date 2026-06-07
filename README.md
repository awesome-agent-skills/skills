# awesome-agent-skills

Open-source agent skills for Claude Code, OpenAI Codex, Cursor, Gemini CLI, and any tool that supports the [Agent Skills open standard](https://agentskills.io).

## Skills

| Skill | Description | Category |
|-------|-------------|----------|
| [research-and-grill](./plugins/research-and-grill/) | Research any topic from the web, then get grilled with Socratic questions | Learning |
| [pr-narrative](./plugins/pr-narrative/) | Generate a human PR title + description from git history. Creates or updates the PR automatically | Workflow |
| [pattern-enforcer](./plugins/pattern-enforcer/) | Scan codebase for existing patterns before implementing. Enforces consistency across UI, errors, tests, and naming | Workflow |
| [react-conventions](./plugins/react-conventions/) | Build React data fetching, mutations, and forms the right way. Matches existing approach, prescribes React Query + RHF/Zod only in a vacuum | Frontend |
| [ui-completeness](./plugins/ui-completeness/) | Verify a React component after building it — the four data states (loading, error, empty, success) plus responsive issues | Frontend |

## Install

### Claude Code

Add the marketplace once, then install any skill:

```bash
claude plugin marketplace add awesome-agent-skills/skills
claude plugin install research-and-grill
claude plugin install pr-narrative
claude plugin install pattern-enforcer
claude plugin install react-conventions
claude plugin install ui-completeness
```

### OpenAI Codex CLI

```bash
codex plugin marketplace add awesome-agent-skills/skills
codex plugin install research-and-grill
codex plugin install pr-narrative
codex plugin install pattern-enforcer
codex plugin install react-conventions
codex plugin install ui-completeness
```

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

# Build React data/forms the right way (matches your stack)
/react-conventions fetch the user profile for the settings page
/react-conventions build a form to edit the user's bio

# Verify a React component covers all its states + responsive
/ui-completeness UserList
```

## Contributing

We welcome new skills! See [CONTRIBUTING.md](./CONTRIBUTING.md) to get started.

Use the [skill-template](./skill-template/) as your starting point.

## Standards

All skills follow the [Agent Skills open standard](https://agentskills.io) — a cross-vendor specification that works across 35+ AI tools.

## License

MIT
