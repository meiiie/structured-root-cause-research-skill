---
name: structured-root-cause-research
description: Evidence-based root-cause research for complex debugging, architecture reviews, security reviews, incidents, and high-stakes technical decisions. Use when the user asks to think carefully, find root cause, avoid patch fixes, test competing explanations, run 5 Whys, compare against best practices, map data/control/error/trust flows, assess evidence, or produce a verifiable recommendation.
---

# Structured Root-Cause Research

Use this skill to turn a vague or complex technical problem into a grounded decision. Favor evidence, falsifiable hypotheses, explicit uncertainty, causal boundaries, and verification over plausible stories or patch fixes.

Do not expose private chain-of-thought. Provide concise reasoning summaries through evidence ledgers, competing hypotheses, flow maps, root-cause chains, tradeoffs, and verification plans.

## Core Principle

Do not treat 5 Whys as the discovery engine. First establish evidence, map the relevant flow, identify where behavior first diverges from expectation, compare competing hypotheses, and run discriminating tests. Use 5 Whys only after the leading causal explanation is supported well enough to summarize.

## Workflow

### 1. Frame The Problem

State the decision this analysis should support:

- Observed symptom, question, or decision
- Expected behavior or target outcome
- Affected users, systems, or business goal
- Known constraints, deadlines, environment, versions, and risks
- Success criteria for a useful answer

If the task is small, keep this to one or two sentences.

### 2. Establish Expected vs Observed Behavior

Pin down the mismatch before explaining it:

- What should happen?
- What actually happens?
- Under which inputs, environments, versions, roles, or timing conditions?
- Is the symptom reproducible, intermittent, historical, or inferred from evidence?

Prefer a minimal reproduction or a precise observable symptom when possible.

### 3. Build An Evidence Ledger

Gather only evidence needed for the decision:

- Local evidence: code paths, configs, logs, tests, schemas, screenshots, traces, runtime behavior, deployment state, and relevant history
- External evidence: official docs, standards, reputable engineering writeups, and current product or platform behavior
- Dates, versions, environment details, and configuration precedence when facts may change
- Counterexamples, negative controls, and conflicting evidence

Separate direct observations from interpretations.

| Evidence | Source | Type | Supports | Contradicts | Reliability |
| --- | --- | --- | --- | --- | --- |
| ... | file/log/test/source | observation/inference | H1 | H2 | high/medium/low |

Prefer primary sources for correctness, security, compliance, or money-sensitive claims. Mark unsupported, indirect, stale, or single-source claims as lower confidence.

### 4. Map The Relevant Flow And Find The Causal Boundary

Map only the flows needed to explain the issue:

| Flow | Source | Control / Transform | Sink / Outcome | Failure Mode |
| --- | --- | --- | --- | --- |
| Data | User/API/input | Validation/transform | DB/output | Wrong state or leakage |
| Control | User action/job/event | Guard/policy | Side effect | Unexpected execution |
| Error | Exception/timeout | Fallback/retry | Log/UI/user | Silent failure or bad recovery |
| Trust | Actor/content/config | Permission boundary | Privileged action | Boundary bypass |

Trace from the symptom backward until you find the earliest incorrect observable state: the point where the system first changes from expected to unexpected behavior. Treat this as the current causal boundary, not automatically as the final root cause.

For security work, identify source, transforms, trust boundary, control, privileged sink, impact, prerequisites, authorization scope, and a negative control when feasible.

### 5. Generate Competing Hypotheses

Do not lock onto the first plausible explanation. List the smallest useful set of plausible causes.

| Hypothesis | Supporting Evidence | Contradicting Evidence | Prediction | Falsifier / Discriminating Test | Status |
| --- | --- | --- | --- | --- | --- |
| H1 | ... | ... | If true, ... | If false, ... | likely/possible/rejected/untested |

A good test should distinguish hypotheses, not merely produce more data.

Prefer the cheapest high-information test first. Reject or weaken a hypothesis when its expected observation does not occur.

### 6. Classify The Causal Structure

For incidents and recurring failures, distinguish roles instead of forcing one root cause:

- Trigger: event or condition that initiated the failure
- Proximate cause: immediate technical mechanism that produced the symptom
- Contributing factors: conditions that increased likelihood, severity, or duration
- Systemic/root cause: durable design, process, policy, or control weakness that allowed the failure class
- Detection gap: why the problem was not noticed earlier
- Recovery gap: why mitigation or recovery was slow or ineffective

Not every task needs every category. Use only those that improve the decision.

### 7. Summarize With Evidence-Based 5 Whys

Use 5 Whys as a compact causal summary after hypothesis testing, not as a private reasoning transcript:

```text
Symptom: [what is happening]
Why 1: [proximate cause, tied to evidence]
Why 2: [deeper system cause, tied to evidence]
Why 3: [design/process/control weakness, tied to evidence]
Root cause: [actionable durable cause]
Confidence: high | medium | low
Remaining uncertainty: [if material]
```

Stop when the cause is specific enough to change code, process, design, policy, positioning, or the next experiment. Do not force exactly five levels.

If multiple causes remain plausible, report them explicitly instead of manufacturing certainty.

### 8. Compare Against Better Practice

Use comparison only when it changes the decision:

| Aspect | Current | Better Practice | Gap | Evidence |
| --- | --- | --- | --- | --- |
| Design | ... | ... | ... | file/source |
| Reliability | ... | ... | ... | test/log/source |
| Security | ... | ... | ... | policy/CWE/OWASP/source |
| Maintainability | ... | ... | ... | code pattern/source |

Avoid vague "SOTA" or "industry best practice" claims. Name the standard, source, product pattern, or engineering practice being used for comparison.

### 9. Recommend The Smallest Durable Change

Recommend a solution or decision that addresses the supported causal mechanism:

- State assumptions and tradeoffs
- Name what should not change
- Prefer existing project patterns and low-blast-radius changes
- Explain why the change addresses the causal boundary or systemic weakness
- Add verification proportional to risk
- Include rollback or follow-up when impact is high

Do not recommend a fix merely because it removes the symptom in one reproduction.

### 10. Verify The Causal Claim And The Fix

A verification plan should test both:

1. Causal claim: the identified cause actually explains the failure.
2. Remediation: the proposed change removes the failure without unacceptable regression.

Prefer verification such as:

- Reproduction before, non-reproduction after
- Unit/integration/regression tests
- Negative controls
- Fault injection or failure-path tests when appropriate
- Metrics/logs/traces showing the expected state transition
- Security authorization tests across relevant roles or trust boundaries
- Rollback criteria for risky changes

## Investigation Heuristic

When working in a codebase or live system, use this loop:

1. Reproduce or precisely pin down the symptom.
2. Locate the first incorrect observable state.
3. Trace backward to the earliest divergence.
4. Identify the control or invariant expected to prevent it.
5. Check alternate paths, configuration, environment, version, and runtime differences.
6. Generate competing hypotheses.
7. Run the cheapest discriminating test.
8. Update confidence and reject unsupported explanations.
9. Only then recommend a durable change.

## Modes

### Diagnostic Mode

Use for bugs, incidents, security findings, reliability failures, and regressions where a causal failure chain exists.

### Decision / Research Mode

Use for architecture or product decisions where there may be no single root cause. Replace forced causality with:

```text
Hypothesis → Evidence → Counterevidence → Experiment / Decision Test
```

Do not "RCA-ify" an open-ended decision when the evidence only supports tradeoffs or multiple contributing factors.

## Output Formats

### Quick Analysis

Use when evidence is direct, blast radius is low, and few hypotheses remain.

```markdown
## Problem
[One-line expected vs observed mismatch]

## Root Cause
[2-4 sentence supported causal summary with confidence]

## Recommendation
[Specific action and verification]
```

### Deep Analysis

Use when multiple systems or trust boundaries are involved, evidence conflicts, impact is high, or confidence is below high.

```markdown
## Problem
[Symptom, expected outcome, scope]

## Evidence
| Evidence | Source | Type | Supports | Contradicts | Reliability |
| --- | --- | --- | --- | --- | --- |
| ... | ... | observation/inference | ... | ... | high/medium/low |

## Flow And Causal Boundary
[Table or concise diagram; identify earliest divergence]

## Competing Hypotheses
| Hypothesis | Evidence For | Evidence Against | Discriminating Test | Status |
| --- | --- | --- | --- | --- |
| ... | ... | ... | ... | ... |

## Causal Structure
- Trigger: ...
- Proximate cause: ...
- Contributing factors: ...
- Systemic/root cause: ...
- Detection/recovery gaps: ...

## Root Cause Summary
Why 1: ...
Why 2: ...
Why 3: ...
Root cause: ...
Confidence: ...
Remaining uncertainty: ...

## Comparison
| Aspect | Current | Better Practice | Gap |
| --- | --- | --- | --- |
| ... | ... | ... | ... |

## Recommendation
[Minimal durable solution]

## Verification Plan
- [ ] Test the causal claim
- [ ] Test the remediation
- [ ] Check regressions / negative controls
- [ ] Define rollback criteria when relevant
```

## Quality Bar

- Do not jump from symptom to fix without evidence.
- Do not present private chain-of-thought; summarize reasoning.
- Do not confuse correlation with causation.
- Do not treat the first plausible explanation as the root cause.
- Do not use 5 Whys as the primary discovery method when alternatives are plausible.
- Do not use a single-cause story when the evidence supports multiple causes.
- Do not call an observation an inference or an inference a fact.
- Do not claim high confidence when a meaningful competing hypothesis remains untested.
- Do not use best-practice language without naming the source or pattern.
- Do not recommend broad rewrites when a smaller durable change works.
- Do not over-research low-risk tasks.
- Do not fabricate sources, benchmarks, customer demand, security impact, or causal evidence.
- Preserve uncertainty when evidence is incomplete.

## Example Uses

### Backend Bug

```text
Problem: Hibernate fails on startup with "Not a managed type".
Investigation: Confirm the failing repository, inspect the class Hibernate actually manages, compare the domain and persistence types, and use a known managed entity as a discriminating control.
Root cause summary: The Spring Data repository points at a domain model instead of the infrastructure JPA entity, crossing the persistence boundary incorrectly.
Recommendation: Change the JPA repository generic to the infrastructure entity, keep the domain repository port unchanged, and add a repository adapter regression test.
```

### Security Review

```text
Problem: A project config appears to trigger privileged behavior during a headless CLI run.
Evidence: Policy docs, local reproduction, negative control, trust-boundary trace, fake canary only.
Investigation: Compare trusted vs untrusted project states and identify the first point where project-controlled data reaches a privileged sink without the documented control.
Root cause summary: Source-controlled config crosses a trust boundary before the expected authorization control is applied.
Recommendation: Report only if the behavior contradicts documented restrictions and has a concrete authorized source-to-control-to-sink-to-impact path.
```

### Architecture Decision

```text
Problem: A team is considering a broad persistence refactor.
Decision mode: Compare hypotheses about the real source of coupling, inspect dependency and change patterns, identify whether failures cluster at one boundary, and test whether a smaller adapter-level change removes the pressure to refactor.
Recommendation: Prefer the smallest architectural change supported by evidence; do not manufacture a root cause if the decision is primarily a tradeoff.
```
