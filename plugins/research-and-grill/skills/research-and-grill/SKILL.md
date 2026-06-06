---
name: research-and-grill
description: Use when the user wants to research a topic from the web and be tested on it via Socratic questioning. Triggered by "research and grill me on X", "teach me then quiz me", or "/research-and-grill [--deep] <topic>". Scans current repo to make questions relatable to the user's codebase.
---

# Research and Grill

## Overview

Research a topic from the web, silently scan the current repo for relevant context, synthesize key concepts, then grill the user with Socratic questions — blending in analogies to their actual codebase where genuinely useful.

## Invocation

```
/research-and-grill [--deep] <topic>
```

- No flag → quick mode: fetch 3–5 pages, ~5 questions
- `--deep` → thorough mode: fetch 10–15 pages, ~10–15 questions

## Pre-flight: Topic Clarity Check

If the topic is a single generic word or fewer than 3 words with no clear focus (e.g. "databases", "security", "AI"), ask before proceeding:

> *"Are you interested in a specific aspect of [topic]? For example: [suggest 2–3 sub-angles]. Or type 'broad' to proceed with a general overview."*

If the topic is specific enough (e.g. "deploying Rails apps to EC2", "React Server Components"), proceed directly.

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

Use WebSearch and WebFetch:
- **Quick mode**: run 3–5 searches across different angles, fetch 3–5 source pages
- **Deep mode**: run 10–15 searches across different angles, fetch 10–15 source pages

Search angles:
- Core definition and fundamentals
- Key concepts and mechanisms
- Common misconceptions
- Real-world applications or examples
- Recent developments (if relevant)

Extract **key concepts worth reasoning about** — not just facts, but ideas, tradeoffs, and relationships. Synthesize into a structured summary grouped by concept.

## Phase 2: Show Summary

Present findings as a clear summary with a numbered list of key concepts to be covered.

If `repo_context` has a genuine connection to the research topic, add one line at the bottom:
> *"I see you're working on [X] — a few questions will connect these concepts to that."*

Only include this line if the connection is real and specific. Skip it if the topic is unrelated to the stack.

End with: *"Ready to be grilled? Type 'yes' to start. You can type 'skip [concept number]' at any point to skip a topic."*

## Phase 3: Grill

**Ask exactly one question at a time. Wait for the user's response before asking the next. Never batch questions.**

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
  - Ask the next question

## Phase 4: Mastery Report

After all questions (or when user says "stop"), give a short report:
- Concepts the user understood well
- Concepts to revisit
- 1–2 recommended resources for deeper learning

If fewer than 3 questions were answered, note the session was too short for a meaningful assessment. List all concepts as "not yet covered" with a recommended resource for each.

## Rules

- Always cite which source a fact came from when presenting the summary
- If web search is unavailable, tell the user and offer to grill from training knowledge instead
- Never repeat the same question twice
- If the user says "skip [concept]" before grilling starts, remove that concept from the question queue entirely
- If the user says "skip" during grilling, move to the next concept immediately
- If the user says "stop", jump straight to the mastery report
- Never force a repo analogy — only use `repo_context` when it genuinely helps understanding
- Do not mention which files you scanned unless the user asks
