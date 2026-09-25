# Generic Skill opt-in example

This is an example policy for optional task-specific Skills. Replace `example-skill` with a Skill available in your environment.

- Do not load or apply `example-skill` by default.
- Apply it when the user explicitly invokes it or clearly asks for its workflow.
- Keep activation scoped to the relevant task.
- Follow higher-priority instructions and existing safety and permission boundaries.
- Do not infer permission to use an external service from a Skill's name or description.
