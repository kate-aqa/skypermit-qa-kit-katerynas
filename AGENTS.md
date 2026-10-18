# Repository Guidelines

## Project Structure & Module Organization

This repository is a QA documentation kit for the read-only application `../ct-gen-ai-skypermit`, located at `/Users/katerynaantypenko/ Education/gen-ai-skypermit-qa-kit/ct-gen-ai-skypermit`.

The purpose of this kit is to generate and maintain manual test cases and test plans. Automated tests, automation frameworks, and API/UI automation suites are outside its scope. The QA scripts below validate documentation and report coverage; they do not execute application tests.

- `test-cases/`: manual case definitions grouped into `auth/` and `flight-request/` modules.
- `scripts/`: dependency-free Node.js utilities for case validation and business-rule coverage; see `scripts/README.md`.
- `api-automation/`: existing scaffold; do not extend it or implement automated tests as part of this kit.
- `.claude/`: test-design templates, worked examples, commands, and agent guidance. Live cases belong in `test-cases/`.

There is no application source, asset directory, package manifest, or CI configuration here. Use `../ct-gen-ai-skypermit` only as a read-only reference for requirements, contracts, business rules, and UI selectors. Do not modify the application project.

## Build, Test, and Development Commands

Run from the repository root with Node.js supporting `import.meta.dirname`:

```bash
node scripts/validate-cases.mjs --app ../ct-gen-ai-skypermit          # Validate case structure and references
node scripts/validate-cases.mjs --app ../ct-gen-ai-skypermit --json   # Machine-readable validation report
node scripts/rule-coverage.mjs --rules ../ct-gen-ai-skypermit/docs/06-business-rules.md          # Report asserted business-rule coverage
node scripts/rule-coverage.mjs --rules ../ct-gen-ai-skypermit/docs/06-business-rules.md --gaps   # List uncovered rules
```

No dependency installation, build, lint, or automated test-runner command is configured. The commands above explicitly point the documentation utilities at the read-only application because their default paths still target `../skypermit`.

## Coding Style & Naming Conventions

Match existing `.mjs` scripts: ES modules, two-space indentation, double-quoted strings, semicolons, and camelCase identifiers. No formatter or linter is configured.

Name case files like `E2E-OP-009-operator-login.md`. Use `E2E-<OP|REV|X>-<three digits>` IDs, unique across modules within each prefix. Follow `.claude/skills/e2e-test-design/templates.md`; omit empty Preconditions. Write Doc refs as `skypermit/docs/...` with section citations, retaining the existing citation convention while resolving references against `../ct-gen-ai-skypermit` via `--app`.

## Testing Guidelines

Generate test plans that define scope, scenarios, preconditions, test data, execution approach, and entry/exit criteria as appropriate. Keep deliverables focused on manual test cases and test plans; do not generate automated test code.

Run both QA scripts with the path overrides above after case changes and report remaining gaps. Coverage tracks rule citations asserted in Expected result; no percentage threshold is configured. Structural validation does not establish logical correctness: check expectations against the read-only application's contracts and business rules. UI steps must use stable `data-testid` selectors.

## Commit & Pull Request Guidelines

History contains only `init` and `Initial commit`; no established commit convention exists. Use concise, imperative subjects describing the change. In pull requests, explain the affected scenarios, cite case IDs or issues where applicable, and include validation results and known coverage gaps.
