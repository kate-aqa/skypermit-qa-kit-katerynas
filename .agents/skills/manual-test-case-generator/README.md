# Atomic Manual Test Case Generator

Creates one traceable manual test case per invocation. For this repository it preserves E2E-prefixed IDs, scans existing cases/history, checks duplicate objectives, and saves each new case separately under `test-cases/<module>/`.

## Invoke

```text
Use $manual-test-case-generator to create one atomic manual case
for [specific behavior] using ../ct-gen-ai-skypermit/docs.
```

You may supply a requirement file/directory, pasted feature description, API specification, existing cases, or a plan such as `TEST_PLAN.md`. Specify the target condition, module, data or environment when known. The skill asks only for missing decisions and blocks finalization if the expected result is not supported. It does not execute application tests.

If the skill is not listed yet, start a new session or explicitly ask Codex to read `.agents/skills/manual-test-case-generator/SKILL.md` and follow it. No global installation is needed.

## Conventions and examples

New cases use `E2E-<OP|REV|X>-NNN` and `<ID>-<short-behavior-slug>.md`, with one case per file. The heading supplies ID/Title; Module, Priority and the legacy documentation/role/steps/result/rationale fields are mandatory. Existing cases are preserved. Other repositories can confirm their own profile during setup.

The demonstration is [E2E-OP-016](../../../test-cases/auth/E2E-OP-016-registration-excludes-authorisation-level.md): opening the registration form reveals no authorisation-level control. This is one UI contract check; it does not establish server-side privilege enforcement. Duplicate-aware repeat invocation should point to this case instead of creating another.

For registration and session checks, the skill also uses your [E2E-OP-017 worked example](examples/E2E-OP-017-registration-starts-session.md). It shows reusable synthetic data and two observations of one session outcome. Its teaching copy matches the live case; it does not add another coverage item or make ID 017 available for reuse.

Use the [reusable Markdown template](assets/manual-test-case-template.md) to draft a case. Replace its placeholders, omit irrelevant optional fields and keep one primary expected result; the skill applies these rules when generating cases. See [template guidance](references/case-template.md) and [validation record](references/validation.md). No automated test code or helper scripts are included.
