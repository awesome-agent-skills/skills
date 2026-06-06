# Contributing to awesome-agent-skills

Thanks for wanting to contribute a skill! Here's how.

## What makes a good skill

- Solves a real, recurring problem
- Works across multiple AI tools (Claude Code, Codex, Cursor, etc.)
- Follows the [Agent Skills open standard](https://agentskills.io)
- Has a clear, specific description so agents know when to invoke it

## How to submit a skill

1. **Fork** this repo
2. **Copy** the [skill-template](./skill-template/) into `plugins/your-skill-name/`:
   ```bash
   cp -r skill-template plugins/your-skill-name
   ```
3. **Rename** the skills subdirectory to match your skill name:
   ```bash
   mv plugins/your-skill-name/skills/your-skill-name plugins/your-skill-name/skills/actual-name
   ```
4. **Edit** `plugins/your-skill-name/.claude-plugin/plugin.json`
5. **Write** your `SKILL.md` in `plugins/your-skill-name/skills/actual-name/`
6. **Add** an example session in `plugins/your-skill-name/skills/actual-name/examples/`
7. **Add** your skill to the `plugins` array in `.claude-plugin/marketplace.json`
8. **Update** the skills table in `README.md`
9. **Open a PR** with the title: `Add skill: your-skill-name`

## Plugin structure

```
plugins/your-skill-name/
├── .claude-plugin/
│   └── plugin.json          ← name, version, description, author
└── skills/
    └── your-skill-name/
        ├── SKILL.md          ← the skill instructions
        └── examples/         ← at least one example session
```

## SKILL.md rules

- `name`: lowercase letters, numbers, hyphens only — max 64 chars
- `description`: start with "Use when..." — max 1024 chars, no XML tags
- Body: keep under 500 lines

## PR checklist

- [ ] Plugin folder under `plugins/`
- [ ] `.claude-plugin/plugin.json` with name, version, description, author
- [ ] `SKILL.md` with valid frontmatter
- [ ] At least one example in `examples/`
- [ ] Entry added to `.claude-plugin/marketplace.json`
- [ ] README table updated
- [ ] Tested in at least one AI tool (mention which in the PR)

## Questions

Open an issue and tag it `question`.
