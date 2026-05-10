---
name: structured-root-cause-research
description: Evidence-based root-cause research for complex debugging, architecture reviews, security reviews, incidents, product investigations, and high-stakes technical decisions. Use when the user asks to think carefully, find root cause, avoid patch fixes, run 5 Whys, compare against best practices, map data/control/error/trust flows, assess evidence, or produce a verifiable recommendation.
---

# Structured Root-Cause Research

Use this skill to turn a vague or complex problem into a grounded decision. Favor evidence, clear assumptions, confidence levels, and verification steps over long hidden reasoning.

Do not expose private chain-of-thought. Provide a concise reasoning summary through root-cause chains, evidence tables, flow maps, tradeoffs, and recommendations.

## Workflow

### 1. Frame The Problem

State the decision this analysis should support:

- Observed symptom, question, or decision
- Expected behavior or target outcome
- Affected users, systems, or business goal
- Known constraints, deadlines, and risks
- Success criteria for a useful answer

If the task is small, keep this to one or two sentences.

### 2. Build An Evidence Ledger

Gather only evidence needed for the decision:

- Local evidence: code paths, configs, logs, tests, schemas, screenshots, traces
- External evidence: official docs, standards, reputable engineering writeups, current customer or market evidence
- Dates, versions, and environment details when facts may change
- Counterexamples or conflicting evidence

Prefer primary sources for correctness, security, compliance, or money-sensitive claims. Mark unsupported or single-source claims as low confidence.

### 3. Run Evidence-Based 5 Whys

Use 5 Whys as a compact reasoning summary, not as a private reasoning transcript:

```text
Symptom: [what is happening]
Why 1: [immediate cause, tied to evidence]
Why 2: [deeper system cause, tied to evidence]
Why 3: [design/process/root cause, tied to evidence]
Root cause: [actionable cause]
Confidence: high | medium | low
```

Stop when the cause is specific enough to change code, process, design, positioning, or the next experiment. Do not force exactly five levels if fewer or more are warranted.

### 4. Map Relevant Flows

Map only the flows needed to explain the issue:

| Flow | Source | Control | Sink | Failure Mode |
| --- | --- | --- | --- | --- |
| Data | User/API/input | Validation/transform | DB/output | Wrong state or leakage |
| Control | User action/job/event | Guard/policy | Side effect | Unexpected execution |
| Error | Exception/timeout | Fallback/retry | Log/UI/user | Silent failure or bad recovery |
| Trust | Actor/content/config | Permission boundary | Privileged action | Boundary bypass |

For security work, identify source, control, sink, impact, and authorization scope. For customer or product research, replace sink with customer outcome.

### 5. Compare Against Better Practice

Use comparison only when it changes the decision:

| Aspect | Current | Better Practice | Gap | Evidence |
| --- | --- | --- | --- | --- |
| Design | ... | ... | ... | file/source |
| Reliability | ... | ... | ... | test/log/source |
| Security | ... | ... | ... | policy/CWE/OWASP/source |
| Maintainability | ... | ... | ... | code pattern/source |

Avoid vague "SOTA" claims. Name the standard, source, product pattern, or engineering practice being used for comparison.

### 6. Recommend The Smallest Durable Change

Recommend a solution or decision that addresses the root cause:

- State assumptions and tradeoffs
- Name what should not change
- Prefer existing project patterns and low-blast-radius changes
- Add verification proportional to risk
- Include rollback or follow-up when impact is high

## Output Formats

### Quick Analysis

```markdown
## Problem
[One-line problem statement]

## Root Cause
[2-4 sentence reasoning summary with confidence]

## Recommendation
[Specific action and verification]
```

### Deep Analysis

```markdown
## Problem
[Symptom, expected outcome, scope]

## Evidence
| Evidence | Source | Confidence |
| --- | --- | --- |
| ... | ... | high/medium/low |

## Root Cause Summary
Why 1: ...
Why 2: ...
Why 3: ...
Root cause: ...
Confidence: ...

## Flow Map
[Table or concise diagram]

## Comparison
| Aspect | Current | Better Practice | Gap |
| --- | --- | --- | --- |
| ... | ... | ... | ... |

## Recommendation
[Minimal durable solution]

## Verification Plan
- [ ] ...
- [ ] ...
```

## Quality Bar

- Do not jump from symptom to fix without evidence.
- Do not present private chain-of-thought; summarize reasoning.
- Do not use 5 Whys as a single-cause story when multiple causes are plausible.
- Do not use best-practice language without naming the source or pattern.
- Do not recommend broad rewrites when a smaller durable change works.
- Do not over-research low-risk tasks.
- Do not fabricate sources, benchmarks, customer demand, or security impact.
- Separate facts, inferences, and opinions.
- Preserve uncertainty when evidence is incomplete.

## Example Uses

### Backend Bug

```text
Problem: Hibernate fails on startup with "Not a managed type".
Root cause summary: A Spring Data JPA repository points at a domain model instead of a JPA entity, violating the clean architecture persistence boundary.
Recommendation: Change the JPA repository generic to the infrastructure entity, keep the domain repository port unchanged, and add or adjust a repository adapter test.
```

### Security Review

```text
Problem: A project config appears to trigger privileged behavior during a headless CLI run.
Evidence: Policy docs, local reproduction, negative control, fake canary only.
Root cause summary: Source-controlled config may cross a trust boundary before the expected control is applied.
Recommendation: Report only if the behavior contradicts documented restrictions and has a concrete authorized source-control-to-privileged-sink path.
```

### Customer/Offer Research

```text
Problem: A technical service offer is too broad to sell.
Root cause summary: The buyer cannot map "AI/security/full-stack" to a concrete pain, deliverable, price, or risk reduction.
Recommendation: Package one narrow offer with a sample report, lead scoring rubric, and a 10-lead outreach test.
```
