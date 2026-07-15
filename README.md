# handoff — Claude Code Skill

A Claude Code skill that captures your full session into a structured handoff document before context compacts — so nothing is lost when you restart.

## What it does

1. Collects everything from the current session: topics covered, decisions made, files changed, work in progress
2. Writes a timestamped markdown file to `~/handoffs/`
3. Tells you exactly where the file is and what to do next

Run it manually any time with `/handoff`, or wire it to fire automatically when context gets full (see Auto-trigger below). Resume any saved session with `/resume-handoff`.

---

## Prerequisites

Just [Claude Code](https://claude.ai/code). No MCP servers, no APIs, no extra setup.

```bash
npm install -g @anthropic-ai/claude-code
```

---

## Installation

### Option A — Global (available in every Claude Code session)

```bash
# Install both skills
curl -o ~/.claude/commands/handoff.md \
  https://raw.githubusercontent.com/priyankamishra11/handoff-plugin/main/.claude/commands/handoff.md

curl -o ~/.claude/commands/resume-handoff.md \
  https://raw.githubusercontent.com/priyankamishra11/handoff-plugin/main/.claude/commands/resume-handoff.md
```

### Option B — Project-level (only inside this repo)

```bash
git clone https://github.com/priyankamishra11/handoff-plugin.git
cd handoff-plugin
claude
```

---

## Usage

### Save a handoff

```
/handoff
```

No arguments. Reads the current session and writes a timestamped file to `~/handoffs/`.

**Output file:** `~/handoffs/handoff_YYYY-MM-DD_HH-MM.md`

Each file contains:
- What you were working on
- Everything covered in order
- Work in progress (exact state)
- Decisions made
- Files created or modified
- Exact next step
- Open questions / blockers

---

### Resume from a handoff

In a new Claude Code session:

```
/resume-handoff ~/handoffs/handoff_2026-07-16_10-30.md
```

Claude reads the file and shows you exactly where you left off. Say "go" to continue.

---

## Auto-trigger (recommended)

Add this section to your `CLAUDE.md` to make Claude create a handoff automatically when context is ~70% full:

```markdown
## Context Monitoring (non-negotiable)

When you notice the session context is getting large — you can sense this because
responses slow, /compact is about to trigger, or you start losing earlier conversation
details — do the following immediately, without waiting for me to ask:

1. Stop whatever you are doing and say:
   ⚠️ Context is almost full — Claude Code is about to compact this session automatically,
   which means earlier conversation will be summarised and some detail will be lost.
   Creating your handoff document now so nothing is lost...

2. Without any further input from me, read and execute every step in:
   ~/.claude/commands/handoff.md
   Follow it exactly as if the user had typed /handoff.

3. After the handoff doc is confirmed written, present the options block from
   Step 5 of handoff.md so the user knows what to do next.

Surface this warning and trigger as early as possible — when context is roughly
50% full — so the handoff doc captures the complete session, not a truncated one.
Do not wait until compaction starts. Do not ask permission. Act immediately.
```

---

## License

MIT
