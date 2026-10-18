# Manual Test Plan Generator

Repository-local Codex skill for concise manual QA planning. It creates documentation; it does not execute application tests.

## Invoke

From this repository, use:

```text
Use $manual-test-plan-generator to analyze ../ct-gen-ai-skypermit/docs
and create TEST_PLAN.md for operator registration/login.
Use English Markdown, 450–600 words; environment and browser matrix unconfirmed.
```

If it is not listed yet, start a new session or ask Codex to read `.agents/skills/manual-test-plan-generator/SKILL.md` and follow it explicitly. No global installation or repository configuration change is required.

Supply a file, directory, pasted feature description, or existing manual cases. Customize the feature/release, scope/exclusions, environment, approved browser/device matrix, language, and output path in the request. The skill reads evidence first, asks only material unanswered questions, summarizes facts/assumptions, and drafts the plan. For an immediate provisional draft, say so; unknowns remain visible.

## Example and checks

- [Example input](references/examples/INPUT.md): source-backed SkyPermit snapshot and interview context.
- [Example plan](references/examples/TEST_PLAN.md): the generated one-page demonstration.
- [Validation record](references/examples/VALIDATION.md): checks, findings and limitations.

A4 pagination depends on the Markdown renderer, font and margins; 450–600 words is a content budget rather than a verified print layout.
