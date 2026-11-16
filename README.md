# handoff-memories

An agent skill for **Claude Code** and **Codex** that writes a handoff document for the next agent. Besides the work
state, it carries over what usually gets lost between sessions:

- the agent's **memories** (Claude's project memory, Codex's memories / AGENTS.md),
- the project's **AGENTS.md / CLAUDE.md** context,
- every **specific instruction** the user gave during the session (rules, approvals, decisions, corrections).

It redacts secrets and personal data, links artifacts instead of copying them, and notes what the next agent may lack when
it is a different product (Claude ↔ Codex).

## Install

```bash
# Claude Code
git clone https://github.com/Qabbout/handoff-memories ~/.claude/skills/handoff-memories
# Codex
git clone https://github.com/Qabbout/handoff-memories ~/.codex/skills/handoff-memories
```

Then run `/handoff-memories <what the next session will do, and which agent>`.

## Files

- `SKILL.md`: the skill (shared by both agents).
- `agents/openai.yaml`: Codex interface metadata.

Based on a simpler `handoff` skill; this version adds memories, instruction-file context and session instructions.

## License

MIT
