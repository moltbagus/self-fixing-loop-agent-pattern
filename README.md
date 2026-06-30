# Self-Fixing Loop Agent Pattern

*Co-created by Claude and sirshibaninja at https://x.com/sirshibaninja*

A skill for agentic AI systems that eliminates repeated mistakes across sessions using a 3-stage persistent memory pattern.

## The Problem

Most AI agents fail not because of the LLM, but because of the **data layer**. They make the same mistake in session 2 that they made in session 1.

LLMs are stateless between sessions. Without persistent correction rules:
- Same failure reoccurs endlessly
- Session memory (short-term) doesn't transfer
- Corrections made in session 1 are forgotten by session 2

## The 3 Stages

### Stage 1 — Remember (Persistent Memory)
Write failures to a dedicated corrections file, not ephemeral session context.

Pattern: `failure → rule → persisted correction`

### Stage 2 — Trigger (Context Injection)
On similar task start, inject the correction rule before acting — proactively, not on failure.

### Stage 3 — Verify (Feedback Loop)
After action, verify against the rule — not just outcome. A correct outcome that violated the rule = still a failure (luck).

## Directory Structure

```
~/.hermes/corrections/
  <domain>/
    failures.jsonl   # raw failure log (append-only)
    rules.md         # synthesized rules from failures
    verify.json      # verification checklist
```

## Usage

Install as a skill in Hermes Agent, Claude Code, or OpenClaw. On startup, load rules from the corrections directory into context before domain tasks. After any action, verify against loaded rules.

| Platform | Corrections Path |
|----------|-----------------|
| Hermes Agent | `~/.hermes/corrections/` |
| Claude Code | `~/.claude/corrections/` |
| OpenClaw | `~/.openclaw/corrections/` |


