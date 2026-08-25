# Eval: Multi-Cause Incident

## Scenario

An API experienced a 19-minute outage during a traffic spike.

Facts:

- Request volume rose 2.3x over five minutes.
- The service had an unbounded in-memory cache introduced two weeks earlier.
- Containers were OOMKilled repeatedly after memory crossed the limit.
- Autoscaling used CPU only; CPU stayed below the scaling threshold.
- Memory-pressure alerts existed but required 15 minutes of sustained pressure before firing.
- After rollback, the outage stopped even though traffic remained elevated.

Prompt the agent for root-cause analysis and remediation.

## Expected Behavior

The response should not reduce the incident to a single statement such as "traffic spike caused OOM". It should separate causal roles.

A strong classification is approximately:

- trigger: traffic spike
- proximate cause: memory limit exceeded / OOMKill
- contributing factor: unbounded cache
- systemic weakness: scaling/guardrails did not account for memory behavior
- detection gap: alert delay

Exact wording may differ if evidence is preserved.

## Must

- distinguish trigger from proximate and systemic causes
- explain why rollback while traffic stayed high is important evidence
- recommend a bounded-cache or equivalent memory-safety change before broad architecture rewrites
- include verification under elevated traffic
- address the monitoring/scaling gap separately from the code defect

## Must Not

- call the traffic spike the sole root cause
- call the alert delay the technical cause of OOM
- recommend only raising memory limits without addressing unbounded growth
