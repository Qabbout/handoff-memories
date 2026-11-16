---
name: handoff-memories
description: Write a handoff document for another agent (Claude Code or Codex) that carries over not only the work state but also the agent's memories, the project's AGENTS.md / CLAUDE.md context, and every specific instruction the user gave during the session. Use when the user asks for a handoff, to "pass this to a new agent", to continue in Codex or Claude, or runs /handoff-memories.
argument-hint: "What will the next session focus on, and which agent (Claude or Codex) will run it?"
disable-model-invocation: true
---

# Handoff with memories

A plain handoff tells the next agent what was done. This one also tells it **how the user wants the work done**: the rules,
preferences and decisions that live in the agent's memory, in the project's instruction files, and in the conversation
itself. A fresh agent that misses those repeats the mistakes the user already corrected.

Write the document to the OS temporary directory (`$TMPDIR`, else `/tmp`; `%TEMP%` on Windows), not the workspace, unless
the user names a place. Name it `handoff-<topic>-<YYYY-MM-DD>.md`.

## Gather first (read, don't guess)

1. **The conversation.** Scan the whole session for the user's own instructions: rules ("always", "never", "from now
   on", "don't"), approvals and their scope, decisions between options, corrections of the agent, preferences about
   format, tools, models, size, pace or tone. Quote the user's words briefly where the wording matters. Keep the latest
   version when an instruction changed, and say what it replaced.
2. **The agent's memory.**
   - Claude Code: the project's memory folder (the `MEMORY.md` index and the files it links, usually under
     `~/.claude/projects/<project>/memory/`) and any global `~/.claude/CLAUDE.md`.
   - Codex: `~/.codex/AGENTS.md`, `~/.codex/memories/` (or the memory tool's store) and any `codex.md`.
   Summarise the entries that apply to the next session; point to the file for the rest.
3. **Project instruction files.** `AGENTS.md`, `CLAUDE.md`, `.agents/AGENTS.md`, nested ones in the folders the next agent
   will touch, and files they import. Don't copy them: list each path with the two or three rules that matter most for
   the next task, and flag any rule the session overrode (with the user's words).
4. **Artifacts.** Plans, specs, briefs, tickets, PRs, branches, worktrees, reports. Reference them by path or URL; never
   paste their content.

## Write the document

Use these sections, in this order, and keep each to what the next agent needs:

1. **Goal and next step.** One paragraph: what the work is, where it stands, the very next action.
2. **State.** A table of the work items with their status and links (PRs, tickets, branches, worktrees). Note anything
   in flight (running jobs, other agents, pending CI) and what to check before touching it.
3. **User instructions from this session.** A numbered list, grouped (scope and approvals, workflow, quality rules,
   communication, tools and models). Each item: the rule, the user's words if short, and when it applies. Mark rules that
   override a memory or instruction file.
4. **Memories that apply.** The memory entries the next agent must follow, one line each, with the file they come from.
   Say which ones were updated or added in this session.
5. **Project context.** The instruction files to read first, each with its key rules (path + one line per rule).
6. **Decisions and open questions.** Decided (with who decided and when) vs still open (with the options). Never present
   an open question as decided.
7. **Gotchas.** Tool quirks, failure modes and workarounds learned in the session.
8. **Suggested skills.** The skills the next agent should load (names it can pass to its Skill tool), and why.
9. **For the other agent.** If the next agent is a different product (Claude → Codex or Codex → Claude): the
   capabilities it may lack (a model, a tool, session spawning) and what to do instead, or what to ask the user.

## Rules

- **Redact** secrets and personal data: API keys, tokens, passwords, OTPs, card or bank numbers, phone numbers, personal
  emails, home addresses. Keep work identities only where the task needs them (e.g. a reviewer's name), never contact
  details. Say "(redacted)" where something was removed.
- **Don't duplicate** content that lives in an artifact; link it.
- **Don't invent.** If something is unknown or unverified, say so.
- **Don't weaken a rule** while summarising it. If the user said "never", write "never".
- If the user passed arguments, tailor the document to that next focus and that agent.
- After writing, tell the user the path and the three things the next agent is most likely to get wrong without it.
