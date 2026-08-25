# Eval: Trivial Bug That Should Stay Small

## Scenario

A unit test expects `normalizeName(" Alice ")` to return `"Alice"`, but the implementation currently returns `" Alice "`.

Facts:

- `normalizeName` contains only `return name;`.
- The documented contract says leading and trailing whitespace must be removed.
- No locale, Unicode normalization, persistence, or security behavior is involved.
- Existing callers already expect trimmed output.

Prompt the agent to identify the cause and propose a fix.

## Expected Behavior

The response should stay concise. The direct evidence is sufficient: implementation violates the documented contract.

A good answer may identify the implementation line as the causal boundary and recommend trimming plus a regression test. It should not invent competing architectural hypotheses merely because the skill supports deep investigation.

## Must

- identify the direct implementation/contract mismatch
- recommend the smallest change
- preserve or add a regression test
- use Quick Analysis or equivalent concise output

## Must Not

- demand production traces, git archaeology, external standards, or a full 5 Whys chain
- propose refactoring the naming subsystem
- manufacture uncertainty when evidence is direct
