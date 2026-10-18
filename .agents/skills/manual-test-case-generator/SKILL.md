---
name: manual-test-case-generator
description: Generate and save one atomic, low-level manual QA test case from requirements, project documents, API specifications, feature descriptions, or a test plan. Use for an individual traceable manual check, including duplicate detection and repository-aware IDs, rather than broad test planning or application test execution.
---

# Atomic Manual Test Case Generator

Generate **one case per invocation** unless the user explicitly requests multiple. One specific action or condition → one primary, observable expected result. All cases are for manual execution. Do not produce test automation, frameworks, scripts, CI/CD integration, or automated implementation recommendations. Reading sources and validating documentation do not constitute application testing.

## Inspect inputs and conventions

Accept functional/technical requirements, PRDs, stories/acceptance criteria, API documentation, feature descriptions, existing manual cases, and test plans (including output from `manual-test-plan-generator`; invoking that skill is not required). Inputs may be pasted text, individual Markdown/TXT/PDF/DOCX documents, or directories of readable documents.

Inventory relevant files before reading a directory. Extract readable content with available tools; if a document or image-only page cannot be read reliably, report the limitation and request the essential excerpt. Do not claim unread evidence was analyzed. Do not install tools or modify requirements/application files as part of generation.

Read applicable repository guidance, templates, and relevant cases before asking questions. Identify the module, actor, testable condition, requirement authority, existing naming/storage conventions, and expected observation. Follow documented supersession rules; otherwise surface conflicting sources without choosing a winner by date alone. Existing cases, implementation and test plans aid coverage analysis but do not establish undocumented requirements. Source inspection may confirm a selector or current behavior; label observed behavior separately from approved requirements.

For a supplied plan, select one documented condition from a scenario and follow its references back to requirements where accessible. A plan's risk, assumption, or open question is not a confirmed product oracle. Never invent requirement IDs, messages, limits, status codes, roles or supported platforms.

## Focused setup and missing information

Reuse confirmed choices from the session/project. For a new project, ask small logical groups (up to three questions) only for unresolved decisions: required fields and format; ID/filename conventions; module directories and write destination. Recommend one Markdown file per case under `test-cases/<module>/`, with ID plus descriptive kebab-case slug. Where no convention exists, propose `{MODULE}-{POS|NEG|BND|INT|SEC|REG}-{NNN}` and confirm before adopting it. Do not impose this fallback on an established repository.

Before the first save, briefly summarize agreed conventions; no repeated interview is needed on later invocations. For this repository, the confirmed profile below already applies.

If the requested behavior is unspecified and several independent checks are possible, offer a short set of objectives and ask which one to generate. If a broad request has a clear primary objective, explain the suggested split, generate only that primary case, and leave other cases uncreated. If an equivalent case exists, identify it and recommend reuse/update; do not create a redundant case or update anything without explicit authorization.

When the expected result is missing or contradictory, ask a targeted clarification **before finalizing or saving**. Do not turn an assumption into a pass/fail rule. An explicitly stakeholder-confirmed assumption may be cited as such, with its provenance; unresolved oracles are not executable final cases.

## Atomic design and priority

Select the smallest reproducible condition with a supported observation. Setup may require several actions; it does not justify several independent objectives. If two outcomes could independently pass/fail, separate them. Supporting observations may explain the same outcome, but do not add a checklist of unrelated assertions. Do not use one bullet containing several independent checks to disguise a broad case.

Make prerequisites sufficient without depending on executing another case. State starting account/session/state, dependency access and required records directly. Use synthetic, identifiable data or documented data classes; do not embed real credentials. Define data only when needed. Avoid an existing account or form submission for a form-presence check.

Use minimal numbered steps, each with the action and input needed for a manual tester. Reuse verified selectors where the project requires them; never invent a selector for an element expected to be absent. Use the containing form's verified selector for an absence check. When a required selector is missing, report it instead of silently substituting CSS/DOM assumptions. Cite exact values only when confirmed. An unavailable environment is an execution prerequisite, not evidence of a passed test.

Choose techniques internally: equivalence partitioning for input classes; boundary analysis only for confirmed limits; decision tables for one selected condition combination; state transitions for one legal/illegal transition; use cases for a single actor action; error guessing and experience-based testing for relevant failure conditions with a defined oracle. Do not output theoretical explanations unless requested.

Assign High/Medium/Low by failure impact, exposure, likely defects, data integrity, permissions and dependency complexity. High covers critical flows, security enforcement, financial/data-loss and major integration failures. Medium covers important recoverable behavior and common validation; Low covers minor presentation or rare low-impact behavior. A UI field check is not automatically High merely because it belongs to authentication. Explain the specific importance briefly in Rationale; distinguish priority from defect severity.

## IDs, duplicates and safe saving

1. Search relevant module cases, then repository-wide case definitions and cross-module coverage for equivalent condition + action + expected result. Compare semantics, not only titles. Different data in the same equivalence class usually does not justify another case. Teaching examples are not live coverage, but their identifiers may still be reserved by local convention.
2. Inspect all case files, including multiple case headings in one file, and available local Git history/ID registries before assigning an ID. Include deleted/historically used IDs when visible. Allocate above the greatest known number in the applicable sequence; never fill a gap or reuse an ID. Do not fetch remote history merely for allocation. Report shallow/unavailable history or unreadable directories and qualify uniqueness accordingly. If a required registry cannot be inspected, use a clearly provisional ID in chat, explain the limitation, and do not claim repository-wide uniqueness or save a colliding final ID.
3. Immediately before saving, recheck the ID and destination for concurrent additions. If occupied, rescan and choose another unused ID; do not overwrite. Save each new case as its own Markdown file in the agreed module. Preserve all existing files. Overwriting any file requires explicit user approval; prior permission to generate a new case is not permission to replace one.
4. If writes are unavailable, provide the case and intended path, explicitly saying it was not saved. Follow environment approval requirements for protected directories; do not silently relocate files.

## Confirmed SkyPermit profile

Start each new case from [assets/manual-test-case-template.md](assets/manual-test-case-template.md) and follow [references/case-template.md](references/case-template.md) when drafting. Replace all placeholders, remove irrelevant optional fields, and adapt the step count to one verification before saving. This repository uses:

- Skill location: `.agents/skills/manual-test-case-generator/`; live cases: `test-cases/auth/`, `test-cases/flight-request/`, or another justified functional module. One case per new file; do not split or migrate legacy multi-case files.
- IDs: `E2E-<OP|REV|X>-<three digits>`, globally sequential within each prefix across modules. OP = Operator, REV = Reviewer, X = cross-role. File: `<ID>-<short-behavior-slug>.md`. Stop and agree an extension if the three-digit sequence is exhausted. E2E is the established manual-case identifier, not a request for a broad end-to-end case.
- Heading `### <ID>: <Title>` supplies mandatory ID and Title. Mandatory new-case fields: Module, Priority, Doc ref, Role(s), Steps, Expected result, Rationale. Include Preconditions, Test Data and Type only when useful; omit empty fields. Preserve the established relative order of legacy fields. Title describes the behavior, preferably within 90 characters.
- Doc refs: `skypermit/docs/...` with section/quoted-heading citations, resolved against read-only `../ct-gen-ai-skypermit`. Use verified stable `data-testid` values in UI steps. Read only relevant app files to confirm selectors; never modify that application.
- The local template requests UI and underlying observations; keep them about the same outcome and applicable to the check. A pure form check requires no invented server assertion. Operator sessions and reviewer `X-Reviewer-Id` headers are distinct mechanisms.
- Run existing documentation validation and rule-coverage commands from AGENTS.md after live-case changes. They inspect documentation, not the application. Report actual results and remaining gaps; do not claim a case assertion covers a rule merely because Doc ref cites it. The existing structural validator predates Module/Priority/Test Data/Type and may flag these fields, and its application-path resolution may differ from the documented override. Report such incompatibilities; do not strip agreed fields or modify validation scripts silently.

For another project, replace the profile with that project's confirmed conventions; retain atomicity, traceability, duplicate checks and safe saving.

## Worked example

For registration/session checks, or guidance on concrete test data and supporting observations, read [examples/E2E-OP-017-registration-starts-session.md](examples/E2E-OP-017-registration-starts-session.md). Use it as a pattern for one objective: valid registration immediately establishes the submitted operator's session. The authenticated header and manual `/api/v1/auth/me` inspection observe that same session; duplicate rejection, default privilege and field validation belong in separate cases.

Adapt its prerequisites, synthetic data, steps and observations to the current requirements. Do not copy its ID, assume its optional profile fields are always necessary, or treat example data as product validation rules. Reverify sources and selectors. This teaching copy mirrors the live `test-cases/auth/E2E-OP-017-registration-starts-session.md`; count the live case once for coverage and reserve 017 during allocation. Include this skill's examples when checking reserved IDs, without treating teaching copies as additional live cases.

## Quality gate and delivery

Before saving, review: one objective; clear behavior title; one measurable primary result; independently sufficient prerequisites; minimal executable steps; relevant data; correct priority; verified source and selectors; no duplicate; non-reused ID; correct file path/format; no undocumented oracle, empty fields, or testing automation content. Improve failed checks before delivery. If a source conflict prevents a valid expected result, stop at the clarification rather than saving a speculative case.

Run available documentation checks and inspect semantic correctness separately. Report missing tools or validator limitations honestly. The generation demonstration is not a claim of application execution.

Return the saved case path/ID, its single objective, validation results and material limitations. For usage and the worked example, see [README.md](README.md). For demonstration evidence and limitations, read [references/validation.md](references/validation.md) only when reviewing this skill.
