# Validation and demonstration

Reviewed 2026-10-18. These checks validate documentation; no application tests were executed.

## Demonstration request and agreed profile

Generate one atomic manual case verifying that the SkyPermit registration form has no authorisation-level field. The user confirmed repository-local storage beside the plan-generator skill, legacy E2E IDs/format with mandatory Module and Priority, and one new case per module file. Existing multi-case files remain intact.

Read the project guidance, legacy case template and live cases; inspected available local Git history, the frontend/API registration requirements, and read-only form/header source for selectors. The repository is not shallow. Observed OP IDs ranged through 015; allocated 016 above that maximum rather than filling gaps. Uniqueness is established against inspected working cases and available local history, not unseen remote branches.

## Generated artifact and semantic review

The case is test-cases/auth/E2E-OP-016-registration-excludes-authorisation-level.md, titled “Registration form excludes authorisation-level selection.”

- One condition: registration UI availability of a privilege control. One primary result: no authorisation-level input/selection control exists in the form. No registration submission, account creation, L1 persistence or privilege-tampering checks are combined with it.
- Two manual steps: open registration through the verified header link, then inspect the verified form container. Absence of a field does not require an invented selector for that field.
- Prerequisites specify a reachable deployment and signed-out browser; no prior case execution, seeded account or credentials are needed. The environment remains an execution prerequisite, not a claimed available deployment.
- The documented frontend and API sections explicitly exclude this field. Source inspection confirms operator-register-link and operator-register-form; no undocumented implementation detail is used as the expected behavior.
- Medium priority reflects a UI-contract check rather than proof of server-side access control. Optional Test Data is omitted because no input is required.
- Technique: one actor/use-case action and a requirement-backed presence/absence condition. Numeric boundaries and data partitions are irrelevant here and were not added.

Before creation, relevant case objectives were reviewed: existing auth cases cover login success/failure, unknown email, missing profile, anonymous access, logout and reload. None covers the selected registration-field condition. After creation, a repeat search finds E2E-OP-016 as the equivalent live case; the skill instructs reuse and no second save. This repeat check was reviewed in this session, not independently evaluated by another agent.

## Checks performed

The skill-creator initializer completed. Dependency-free checks passed for required frontmatter shape/name, name length/convention, description length, absence of unfinished scaffold markers, existing reference targets, required case fields, one case heading, title length, one primary result bullet, working-case ID uniqueness, no historical reuse of 016, source-file existence and selector values. The example contains no testing automation content. UI metadata was reviewed for quoted strings and the default automatic-selection policy. All linked resources were checked after this record was saved.

Manual semantic review, rather than structural counting alone, established that setup/actions support one observation. The skill also covers incomplete oracles, duplicate objectives, safe non-overwriting saves, inaccessible registries and input-format limitations. PDF/DOCX extraction and every fallback branch were not behaviorally exercised by this Markdown example.

## Tool limitations and known incompatibility

The bundled quick_validate.py was attempted but could not run because available Python lacks PyYAML (ModuleNotFoundError: yaml). Metadata was reviewed manually and with dependency-free checks; do not report this as a bundled-validator pass.

Node.js v24.22.0 is now available. Both required repository documentation utilities were rerun with the specified sibling-app overrides. `validate-cases.mjs --app ../ct-gen-ai-skypermit --json` examined 12 cases in 5 files and exited 1 with 22 issues: 16 docref-dead errors caused by source-path resolution and 6 field-unknown errors for the newer fields. `rule-coverage.mjs --rules ../ct-gen-ai-skypermit/docs/06-business-rules.md --gaps` reported 1/13 asserted and 12 uncovered. The missing-Node limitation is resolved; the validator incompatibilities remain. These UI/session cases add no asserted rule_reference coverage.

The executed validation confirms what static inspection found: that validate-cases.mjs rejects fields outside its older list, so Module, Priority and Type in the agreed new case format would be flagged. It also resolves cited skypermit/docs paths beneath the --app value, which does not match the documented sibling override. The skill reports these tool incompatibilities instead of stripping agreed fields or changing existing scripts. No helper scripts, application changes, validator changes or legacy-case migrations were made.

## Added registration-session example

The user supplied `examples/E2E-OP-017-registration-starts-session.md`. It is now linked directly from SKILL.md for registration/session generation and from README.md. Its content matches the live `test-cases/auth/E2E-OP-017-registration-starts-session.md` (byte-for-byte comparison passed). Both copies were preserved.

Manual review confirms one primary objective: valid registration immediately establishes the submitted operator's session. Header identity and the same-session `/auth/me` response are supporting observations of that outcome. The example separates duplicate rejection, default authorisation level and field validation. Its prerequisites and synthetic data are explicit; all referenced operator selectors exist in the read-only registration/header source. The skill instructs adapting this example, reverifying evidence, reserving ID 017 and counting the live case once. No application tests were executed.

Current uncovered rules: OH-1 §2.1, OH-1 §2.3, ZC-2026.1 §2 (AIRCRAFT and ZONE), OH-1 §4, ZC-2026.1 §4, TSD-4 §2.1, TSD-4 §2.2, TSD-4 §3, ZC-2026.1 §3, TSD-4 §5 and OH-1 §3. The coverage utility counts the two ZC-2026.1 §2 gate checks separately. PyYAML remains unavailable; this update does not claim a successful bundled skill-validator run.
