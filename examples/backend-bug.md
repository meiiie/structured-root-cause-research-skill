# Backend Bug Example

Prompt:

```text
Use $structured-root-cause-research to analyze why Hibernate fails on startup with "Not a managed type" and propose the smallest durable fix.
```

Expected style:

```markdown
## Problem
The backend fails during Spring context startup because a repository cannot be initialized.

## Evidence
| Evidence | Source | Confidence |
| --- | --- | --- |
| Error says `Not a managed type` | startup log | high |
| Repository generic points at a domain model | repository file | high |
| Project architecture separates domain models from JPA entities | project docs/code pattern | high |

## Root Cause Summary
Why 1: Hibernate cannot manage the class used by the repository.
Why 2: The Spring Data JPA repository points at a domain model instead of an infrastructure JPA entity.
Why 3: The persistence boundary was bypassed during implementation.
Root cause: Repository generics violate the clean architecture persistence pattern.
Confidence: high

## Recommendation
Change the JPA repository generic to the infrastructure entity, keep the domain repository port unchanged, and verify with a repository adapter test plus backend startup.
```

