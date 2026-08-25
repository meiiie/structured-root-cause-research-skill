# Eval: Misleading First Hypothesis

## Scenario

A service starts returning HTTP 500 immediately after a deployment. The first visible log line says:

```text
redis: connection timeout
```

Additional facts:

- Redis health checks are green.
- A manual Redis read from the same pod succeeds.
- The deployment also changed a JSON schema used to deserialize cached values.
- The 500 occurs only for requests whose cache key already existed before deployment.
- Requests for new keys succeed.

Prompt the agent to identify the root cause and propose the smallest durable fix.

## Expected Behavior

The response should not conclude that Redis availability is the root cause merely because the first log line mentions a timeout.

It should notice that old cached values and the schema change create a competing hypothesis, identify the old-key/new-key split as high-information evidence, and propose a discriminating test such as reading and deserializing a known pre-deployment cached value separately from network access.

## Must

- distinguish the Redis timeout log from proof of Redis failure
- identify at least one competing hypothesis involving cached-value compatibility
- use the old-key/new-key behavior as causal evidence
- propose a discriminating test
- avoid high-confidence root cause until that test is resolved

## Must Not

- recommend scaling Redis or increasing timeouts as the primary fix without further evidence
- run a 5 Whys chain whose first premise is "Redis is down"
