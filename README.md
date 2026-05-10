# Structured Root-Cause Research Skill

Portable `SKILL.md` for evidence-based root-cause analysis in AI coding agents.

It helps Codex, Claude Code, and other file-based agents avoid patch fixes by forcing a lightweight research loop:

1. frame the problem
2. gather an evidence ledger
3. summarize root cause with evidence-based 5 Whys
4. map data, control, error, and trust flows
5. compare against named practices or standards
6. produce a verifiable recommendation

This is not a "show chain-of-thought" prompt. It asks agents to provide concise reasoning summaries, assumptions, evidence, confidence levels, and verification plans.

## Install

### Claude Code

```bash
mkdir -p ~/.claude/skills/structured-root-cause-research
cp SKILL.md ~/.claude/skills/structured-root-cause-research/SKILL.md
```

### Codex

```bash
mkdir -p ~/.codex/skills/structured-root-cause-research
cp SKILL.md ~/.codex/skills/structured-root-cause-research/SKILL.md
cp -r agents ~/.codex/skills/structured-root-cause-research/
```

### Project-local

```bash
mkdir -p .agents/skills/structured-root-cause-research
cp SKILL.md .agents/skills/structured-root-cause-research/SKILL.md
cp -r agents .agents/skills/structured-root-cause-research/
```

## Use

Ask your agent:

```text
Use $structured-root-cause-research to analyze why this bug keeps coming back and propose the smallest durable fix.
```

Other good triggers:

- "find the root cause"
- "do not patch around this"
- "run 5 Whys"
- "compare against best practices"
- "map the data/control/error flow"
- "analyze the trust boundary"

## When It Helps

- Complex bugs where a quick patch keeps failing
- Architecture reviews before a refactor
- Security reviews that need source-control-sink-impact clarity
- Incident analysis and postmortem prep
- Product or offer research where the next decision needs evidence

## Example Outputs

See:

- [examples/backend-bug.md](examples/backend-bug.md)
- [examples/security-review.md](examples/security-review.md)
- [examples/customer-offer-research.md](examples/customer-offer-research.md)

## Design Principles

- Evidence before recommendations
- Reasoning summary, not private chain-of-thought
- Smallest durable change over broad rewrites
- Named practices over vague "SOTA" claims
- Explicit confidence when evidence is incomplete

## Why This Exists

AI coding agents are good at generating fixes quickly. They are less reliable when the task needs disciplined investigation before action. This skill gives the agent a compact operating procedure for thinking like a careful engineer: gather evidence, identify the root cause, compare against better practice, and verify the outcome.

## License

MIT

