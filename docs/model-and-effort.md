# Model and effort policy

Codex starts with the model and reasoning effort the user has already selected. A task being large or complex does not by itself justify a confirmation round trip.

The default path is:

```text
Task → current model and effort → execute → verify in proportion to impact
```

Recommend a different configuration only when the current one is materially likely to reduce correctness, completion quality, or efficiency. Give one concise recommendation instead of a catalog of alternatives. Do not claim the picker or effort changed unless the environment confirms it.

When a configuration concern arises, separate it from task authorization. If reliable work can continue, proceed with the safe and useful parts while stating the limitation when it matters. Stop for the minimum user action only when the current configuration prevents reliable completion or when a decision genuinely belongs to the user.

Model and effort tradeoffs include quality, latency, cost, available tools, and the amount of verification the task needs. This policy does not rank specific models; availability and behavior change over time. It is not a model router and does not call another provider.
