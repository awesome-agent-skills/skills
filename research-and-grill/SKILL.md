---
name: research-and-grill
description: Use when the user wants to research a topic from the internet and then be tested on it through Socratic questioning. Triggered by phrases like "research and grill me on", "teach me about X then quiz me", or "/research-and-grill [--deep] <topic>".
---

# Research and Grill

## Overview

Research a topic from the web, synthesize the key concepts, show a summary, then grill the user with Socratic questions scaled to how much was researched.

## Invocation

```
/research-and-grill [--deep] <topic>
```

- No flag → quick mode: 3–5 sources, ~5 questions
- `--deep` → thorough mode: 10–15 sources, ~10–15 questions

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

Present the findings to the user as a clear summary with a numbered list of the key concepts that will be covered in the grilling.

End with: *"Ready to be grilled? Type 'yes' to start or 'skip [concept]' to skip any topic."*

## Phase 3: Grill

- Number of questions scales with depth: quick → ~5, deep → ~10–15
- **Socratic style** — never ask "what is X?", instead ask:
  - "Why does X happen rather than Y?"
  - "What would break if X weren't true?"
  - "How does X relate to what we said about Z?"
  - "What assumption are you making when you say that?"
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
- If web search is unavailable, tell the user and offer to grill from your training knowledge instead
- Never repeat the same question twice
- If the user says "skip", move to the next concept without grilling on it
- If the user says "stop", jump straight to the mastery report
