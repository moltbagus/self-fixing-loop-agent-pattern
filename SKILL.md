---
name: self-fixing-loop-agent-pattern
description: Agentic AI self-fixing loop — 3-stage persistent memory pattern for eliminating repeated mistakes across sessions.
trigger: "agentic AI failures, repeated mistakes, memory layer design, self-improving agents"
created: 2026-06-30
tags: [agentic-ai, memory, self-improvement, llm, pattern]
---

# Self-Fixing Loop — 3-Stage Pattern

*Co-created by Claude and sirshibaninja at https://x.com/sirshibaninja*

Most agents fail not because of the LLM, but because of the **data layer**. They make the same mistake in session 2 that they made in session 1.

## The 3 Stages

### Stage 1 — Remember (Persistent Memory)
- Not just session memory — corrections must become **persistent rules**
- Write failures to a dedicated corrections file, not ephemeral session context
- Pattern: `failure → rule → persisted correction`

### Stage 2 — Trigger (Context Injection)
- On similar task start, **inject the correction rule** before acting
- Not reactive retrieval — proactive pre-loading
- Feed the rule into the system prompt or context window at decision time

### Stage 3 — Verify (Feedback Loop)
- After action, **verify against the rule**, not just outcome
- A correct action that violated the rule = still a failure (luck)
- Log verification pass/fail separately from outcome

## Why It Works

LLMs are stateless between sessions. Without persistent correction rules:
- Same failure reoccurs endlessly
- Session memory (short-term) doesn't transfer
- Corrections made in session 1 are forgotten by session 2

The loop closes by writing to a **durable store** (file/db), not session context.

## Implementation Notes

```
~/.hermes/corrections/       # Hermes Agent
~/.claude/corrections/       # Claude Code
~/.openclaw/corrections/     # OpenClaw
  <domain>/
    failures.jsonl           # raw failure log
    rules.md                 # synthesized rules from failures
    verify.json              # verification checklist
```

- Use JSONL for failure logs (append-only, no overwrites)
- Rules file: human + machine readable
- Load rules at agent startup, not on-demand
- Separate verification from outcome evaluation

## Anti-Patterns

- Storing corrections only in session memory
- Retrieving corrections only when failure occurs (should be proactive)
- Confusing lucky outcomes with correct reasoning
- Overwriting instead of appending to failure logs
