# Atomic case template guidance

Use [the reusable Markdown template](../assets/manual-test-case-template.md) as the starting point for each new case. It is an output asset, not a live case or reserved ID.

- Replace every brace-delimited placeholder with confirmed, feature-specific content. Allocate the ID from the repository; never save a placeholder ID.
- The heading supplies mandatory ID and Title. Keep Module, Priority, Doc ref, Role(s), Steps, Expected result and Rationale for this repository.
- Remove Type, Preconditions or Test Data entirely when irrelevant. Do not retain empty fields, instructions or placeholder text in a finished case.
- Add or remove numbered steps to keep only the actions needed for one check. One step is sufficient when it fully describes the action; the two template steps are illustrative.
- Define prerequisites directly rather than depending on another case. Use synthetic data; include fresh-value instructions when uniqueness matters, without inventing product validation limits.
- Keep one primary result. UI and server observations may support the same outcome, as in [the registration-session example](../examples/E2E-OP-017-registration-starts-session.md); separate unrelated checks.
- Preserve the established relative order of Doc ref, Role(s), Preconditions, Steps, Expected result and Rationale. Use confirmed source sections and verified stable data-testid values for UI interactions.
- Save one case per `test-cases/<module>/<ID>-<short-behavior-slug>.md` file after duplicate and ID checks. Do not overwrite existing files without explicit approval.

For another repository, adapt to its confirmed conventions. Without additional local requirements, the general minimum is ID, Title, Module, Priority, Steps and Expected Result, with other fields included when relevant.
