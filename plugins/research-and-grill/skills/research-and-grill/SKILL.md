---
name: research-and-grill
description: Use when the user wants to research a topic from the internet and then be tested on it through Socratic questioning. Triggered by phrases like "research and grill me on", "teach me about X then quiz me", or "/research-and-grill [--deep] <topic>". Also scans the current repo to make questions relatable to the user's actual work.
---

# Research and Grill

## Overview

Research a topic from the web, silently scan the current repo for relevant context, synthesize key concepts, then grill the user with Socratic questions — blending in analogies to their actual codebase where genuinely useful.

## Invocation

```
/research-and-grill [--deep] <topic>
```

- No flag → quick mode: 3–5 sources, ~5 questions
- `--deep` → thorough mode: 10–15 sources, ~10–15 questions

## Phase 0: Context Scan

Before researching, silently infer which repo files are relevant based on the topic, then read them:

| Topic mentions | Files to check |
|----------------|---------------|
| deployment / infra / cloud | `Dockerfile`, `.github/workflows/`, `deploy/`, `*.yml` CI configs |
| database / models / schema | `schema.rb`, `models/`, `migrations/`, `prisma/schema.prisma` |
| frontend / UI / components | `package.json`, `components/`, `pages/`, `src/` |
| auth / security / permissions | auth-related files, middleware, session config |
| background jobs / queues | `Sidekiq`, `Celery`, `Bull` config files |
| any topic | Always check `CLAUDE.md` + primary dependency file (`package.json`, `Gemfile`, `pyproject.toml`, `requirements.txt`, `go.mod`) |

Extract: tech stack, key libraries, what the user has been building. Store as `repo_context`.

If no repo or no relevant files exist, skip silently — do not mention it.

## Phase 1: Research

1. Search the web for `<topic>` across multiple angles:
   - Core definition and fundamentals
   - Key concepts and mechanisms
   - Common misconceptions
   - Real-world applications or examples
   - Recent developments (if relevant)
2. Fetch and read sources based on depth flag
3. Extract **key concepts worth reasoning about** — not just facts, but ideas, tradeoffs, and relationships
4. Synthesize into a structured summary grouped by concept

## Phase 2: Show Summary

Present findings as a clear summary with a numbered list of key concepts to be covered.

If `repo_context` has a genuine connection to the research topic, add one line at the bottom:
> *"I see you're working on [X] — a few questions will connect these concepts to that."*

Only include this line if the connection is real and specific. Skip it if the topic is unrelated to the stack.

End with: *"Ready to be grilled? Type 'yes' to start or 'skip [concept]' to skip any topic."*

## Phase 3: Grill

- Number of questions scales with depth: quick → ~5, deep → ~10–15
- **Socratic style** — never ask "what is X?", instead ask:
  - "Why does X happen rather than Y?"
  - "What would break if X weren't true?"
  - "How does X relate to what we said about Z?"
  - "What assumption are you making when you say that?"
- **2–3 questions** (not all) naturally blend in `repo_context` when there's a genuine analogy:
  - *"You're using Sidekiq — how is the concept of superposition similar to how a job queue holds multiple pending states?"*
  - *"Your app uses PostgreSQL transactions — how does that relate to atomicity in distributed systems?"*
  - Only draw the analogy if it genuinely aids understanding. Never force a connection.
- After each answer:
  - Acknowledge what was right
  - Gently correct misconceptions with explanation
  - Connect back to the research findings
  - Move to the next concept

## Phase 4: Mastery Report

After all questions, give a short report:
- Concepts the user understood well
- Concepts to revisit
- 1–2 recommended resources for deeper learning

## Rules

- Always cite which source a fact came from when presenting the summary
- If web search is unavailable, tell the user and offer to grill from training knowledge instead
- Never repeat the same question twice
- If the user says "skip", move to the next concept without grilling on it
- If the user says "stop", jump straight to the mastery report
- Never force a repo analogy — only use `repo_context` when it genuinely helps understanding
- Do not mention which files you scanned unless the user asks
