# Contributing to awesome-agent-skills

Thanks for wanting to contribute a skill! Here's how.

## What makes a good skill

- Solves a real, recurring problem
- Works across multiple AI tools (Claude Code, Codex, Cursor, etc.)
- Follows the [Agent Skills open standard](https://agentskills.io)
- Has a clear, specific description so agents know when to invoke it

## How to submit a skill

1. **Fork** this repo
2. **Copy** the [skill-template](./skill-template/) into a new folder:
   ```bash
   cp -r skill-template your-skill-name
   ```
3. **Write** your `SKILL.md` following the template
4. **Add an example** in `your-skill-name/examples/` showing a real session
5. **Update** the skills table in `README.md`
6. **Open a PR** with the title: `Add skill: your-skill-name`

## SKILL.md rules

- `name`: lowercase letters, numbers, hyphens only — max 64 chars
- `description`: start with "Use when..." — max 1024 chars, no XML tags
- Body: keep under 500 lines
- No tool-specific code unless the skill clearly targets one tool

## PR checklist

- [ ] New folder named after the skill
- [ ] `SKILL.md` with valid frontmatter
- [ ] At least one example in `examples/`
- [ ] README table updated
- [ ] Tested in at least one AI tool (mention which one in the PR)

## Questions

Open an issue and tag it `question`.
