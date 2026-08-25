# Structured Root-Cause Research Skill

Portable `SKILL.md` for evidence-based causal investigation in AI coding agents.

It helps Codex, Claude Code, and other file-based agents avoid patch fixes and plausible-but-unproven RCA stories by enforcing a lightweight investigation loop:

1. frame expected vs observed behavior
2. gather an evidence ledger
3. map the relevant data, control, error, and trust flow
4. locate the earliest divergence / causal boundary
5. generate competing hypotheses
6. test predictions and falsifiers
7. summarize the supported causal chain with 5 Whys
8. compare against named practices or standards
9. produce the smallest durable, verifiable change

This is not a "show chain-of-thought" prompt. It asks agents to provide concise reasoning summaries, assumptions, observations vs inferences, competing hypotheses, confidence levels, and verification plans.

## Why This Is Different

Many RCA prompts tell an agent to "think deeply" or run 5 Whys immediately. That can produce a coherent causal story without establishing that the story is actually true.

This skill treats 5 Whys as a **summary format after investigation**, not the discovery engine. The discovery loop is hypothesis-driven:

```text
Observed mismatch
  ↓
Flow + earliest divergence
  ↓
Competing hypotheses
  ↓
Prediction / falsifier
  ↓
Discriminating test
  ↓
Supported causal chain
  ↓
Durable fix + verification
```

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
Use $structured-root-cause-research to analyze why this bug keeps coming back. Test competing explanations and propose the smallest durable fix.
```

Other good triggers:

- "find the root cause"
- "do not patch around this"
- "test competing hypotheses"
- "find the earliest divergence"
- "run 5 Whys after validating the cause"
- "compare against best practices"
- "map the data/control/error flow"
- "analyze the trust boundary"

## When It Helps

- Complex bugs where quick patches keep failing
- Intermittent or environment-specific regressions
- Architecture reviews before a refactor
- Security reviews that need source-control-sink-impact clarity
- Incident analysis and postmortem preparation
- Reliability failures with multiple contributing factors
- High-impact technical decisions where evidence should drive scope

## Causal Model

For incidents and recurring failures, the skill distinguishes:

- **Trigger** — what initiated the failure
- **Proximate cause** — the immediate technical mechanism
- **Contributing factors** — conditions that increased probability, impact, or duration
- **Systemic/root cause** — the durable weakness that allowed the failure class
- **Detection gap** — why it was not noticed earlier
- **Recovery gap** — why mitigation was slow or ineffective

This prevents the common failure mode of forcing every incident into one simplistic root cause.

## Example Outputs

See:

- [examples/backend-bug.md](examples/backend-bug.md)
- [examples/security-review.md](examples/security-review.md)
- [examples/customer-offer-research.md](examples/customer-offer-research.md)

## Behavioral Evals

The repository includes lightweight evaluation scenarios under `evals/` covering:

- a misleading first hypothesis
- a multi-cause incident
- a security false positive
- a trivial bug that should not be over-researched

These are designed to test behavior, not only frontmatter or Markdown structure.

## Design Principles

- Evidence before recommendations
- Observations separate from inferences
- Competing hypotheses before causal certainty
- Falsification over confirmation-only research
- Earliest divergence before symptom patching
- Reasoning summary, not private chain-of-thought
- Smallest durable change over broad rewrites
- Named practices over vague "SOTA" claims
- Explicit confidence when evidence is incomplete
- Verify both the causal claim and the remediation

## Why This Exists

AI coding agents are good at generating fixes quickly. They are less reliable when a task requires disciplined investigation before action. A plausible explanation can sound convincing while still being wrong.

This skill gives the agent a compact operating procedure for behaving more like a careful investigator: establish the mismatch, find the causal boundary, test competing explanations, reject unsupported stories, and only then recommend and verify a durable change.

## License

MIT
