# Behavioral Evals

These scenarios test whether an agent using `structured-root-cause-research` behaves differently from a generic "find the root cause" prompt.

They are intentionally lightweight and human-reviewable. The goal is not to grade prose style; it is to detect investigation failures.

## Global Rubric

A strong response should:

- establish expected vs observed behavior before explaining the cause
- distinguish direct observations from inferences
- identify the earliest known divergence or explicitly state that it is not yet known
- consider a meaningful competing hypothesis when evidence permits one
- propose a discriminating test or falsifier rather than collecting confirmation-only evidence
- avoid claiming high confidence while a plausible competing explanation is untested
- separate proximate cause, contributing factors, and systemic/root cause when the scenario is multi-causal
- recommend the smallest durable change supported by evidence
- verify both the causal claim and the remediation

A weak response:

- jumps from the first error message to a fix
- treats correlation as causation
- runs 5 Whys on an untested assumption
- invents evidence or external standards
- recommends a broad rewrite without showing why it is necessary
- over-researches a trivial, directly evidenced bug

## Suggested Comparison

Run each scenario twice:

1. Baseline: `Analyze this problem and propose a fix.`
2. Skill: `Use $structured-root-cause-research to analyze this problem and propose the smallest durable fix.`

Compare the outputs against the scenario-specific rubric.
