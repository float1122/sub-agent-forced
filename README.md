# Sub-Agent Forced

A [Claude Code](https://claude.com/claude-code) skill that turns the main agent into a **pure orchestrator**: it never does real work itself, it only **decomposes, dispatches to subagents, independently verifies their output, and reports.**

The point isn't speed — it's a **clean main context** (the doing happens in subagents) and **sharp judgment** (you check results with your own hands instead of trusting reports).

## What it does

Under this skill the main agent is split into **verify vs produce**:

| ✅ Main agent MAY do | ❌ Main agent MUST delegate |
|---|---|
| One-line factual answers, clarifying questions | Writing or editing any code / file |
| Decompose & dispatch subagents | Designing architecture, schemas, APIs |
| **Verification only**: `grep`, build, test, read, diff | Brainstorming / spec / approach work |
| Synthesize subagent results into a report | Implementing a feature or fix |
| Decide what to delegate next | Writing tests |

### The four roles

1. **Decompose** — break the request into well-bounded units; independent → parallel, dependent → sequence.
2. **Dispatch** — one subagent per unit, each with a self-contained brief and a clear definition of done.
3. **Verify (with your own hands)** — confirm at least one load-bearing fact yourself. A green self-check, not a green self-report.
4. **Report & synthesize** — what was done, how it was verified, what's unconfirmed, what's next.

### The heart of it — the verification rule

A subagent saying *"done, tests pass, 0 errors"* is a **claim**, not a **fact**. Before reporting any unit as done, you independently confirm a load-bearing fact (run the build, run the tests, `grep` the change, read the file, diff the result). If your check disagrees with the report, the unit isn't done — re-dispatch with the discrepancy spelled out.

## Install

Claude Code discovers skills under `~/.claude/skills/`. Drop the folder in:

```bash
git clone https://github.com/jangfolk/sub-agent-forced.git
mkdir -p ~/.claude/skills/sub-agent-forced
cp sub-agent-forced/SKILL.md ~/.claude/skills/sub-agent-forced/SKILL.md
```

Then invoke it via the `Skill` tool (or `/sub-agent-forced` if your setup exposes it as a command). Once active, the main agent will delegate everything and only verify + report.

## License

[MIT](LICENSE)
