# Security Review Example

Prompt:

```text
Use $structured-root-cause-research to analyze this potential CLI trust-boundary issue. Keep it authorized, evidence-based, and report only if impact is concrete.
```

Expected style:

```markdown
## Problem
A source-controlled project config appears to trigger privileged behavior during a headless CLI run.

## Evidence
| Evidence | Source | Confidence |
| --- | --- | --- |
| Behavior reproduces with fake canary only | local PoC | high |
| Negative control blocks behavior under expected mode | local control | medium |
| Product docs describe the relevant restriction | official docs | high |

## Flow Map
| Flow | Source | Control | Sink | Failure Mode |
| --- | --- | --- | --- | --- |
| Trust | source-controlled config | permission boundary | local command/tool execution | config crosses boundary before control applies |

## Recommendation
Submit only if the behavior contradicts documented restrictions and shows concrete source-to-sink impact inside the authorized scope. Otherwise keep it as an internal research note.
```

