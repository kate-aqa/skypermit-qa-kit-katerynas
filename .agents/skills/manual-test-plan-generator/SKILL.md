---
name: manual-test-plan-generator
description: Generate concise, risk-based, one-page manual QA test plans from project documents, requirement directories, API specifications, existing cases, or feature descriptions. Use for manual test planning and release-readiness coverage, rather than detailed test-case suites or test execution.
---

# Manual Test Plan Generator

Produce a feature-specific English Markdown plan with sections A–H, targeting 450–600 words and approximately one A4 page. All testing activities must be manual. Never recommend automation frameworks, automated cases, CI/CD integration, or automation strategies. Document generation and document validation are allowed; do not execute application tests as part of planning.

## Inputs and activation

Use when asked to create or revise a manual QA test plan, including from PRDs, functional/technical specifications, stories and acceptance criteria, OpenAPI/Swagger, existing manual cases/checklists, or plain-language descriptions. Accept individual files or documentation directories, including Markdown, TXT, PDF, DOCX, and other readable formats. Preserve the user's scope and output destination; do not modify source requirements or the application.

## Analyze before interviewing

1. Read applicable repository guidance and reuse confirmed project context. For a directory, inventory relevant files first; inspect requirements, contracts, business rules, clarifications, and existing manual coverage. Avoid unrelated files and secrets.
2. Read text directly. Use available document readers/extractors for PDF/DOCX and other formats; inspect relevant tables and image-only pages when possible. If a file cannot be read reliably, identify it and the coverage limitation, ask for accessible content only if essential, and continue with readable evidence. Do not claim unread content was analyzed or silently install tooling.
3. Identify Web, Mobile, API, Backend, or Hybrid from evidence. Distinguish a web frontend with supporting APIs from multiple independently scoped clients; mark uncertain classifications as provisional. Extract actors, critical journeys, acceptance criteria, state changes, validation limits, integrations, data rules, access rules, and exclusions.
4. Build a compact working mapping of requirement → source section → risk → scenario. Consolidate duplicate requirements. Treat existing cases and implementation observations as supporting evidence, not authority over approved requirements. Record unavailable or stale sources.
5. Follow explicitly documented authority and supersession rules. Otherwise, do not choose a winner merely because a file is newer. Cite both sides of conflicts and explain their effect on coverage. Never invent features, limits, error codes, supported platforms, or acceptance thresholds.

## Focused interview

Ask only what materially affects scope or release readiness and is not already answered. Use small groups of up to three related questions; skip answered groups:

- **Scope and outcome:** target release/feature, actors, critical journey, exclusions, intended audience.
- **Execution context:** available environment, supported platforms/browsers/devices, accounts/data, dependency access and limitations.
- **Delivery and release:** output path/language, release-blocker definition, decision owner or constraints when relevant.

Recommend manual QA defaults as proposals, clearly separated from confirmed facts. Default to English Markdown, 450–600 words, risk-first execution, and an unconfirmed compatibility matrix. Do not invent a browser matrix or assume staging exists. Avoid making the user repeat supplied information.

When answers are needed, ask the smallest useful group, continue independent analysis, and wait for dependent decisions. If information is unavailable, the user requests an immediate draft, or remaining gaps can be recorded, generate a useful provisional plan with labeled assumptions. Do not silently convert an unanswered proposal into a confirmed requirement. Before drafting, briefly summarize confirmed scope, accepted defaults, and material assumptions; another approval round is unnecessary unless a consequential decision is unresolved.

## Risk and manual coverage decisions

Assess business impact and qualitative likelihood, using complexity, data integrity, permissions/security, integration dependencies, regression exposure, and user experience as evidence. Likelihood is an estimate, not a measured defect rate. Do not invent numeric scores.

- **High:** loss of a critical journey, unauthorized access, corrupt/duplicate business records, or a critical integration failure; prioritize even if likelihood is uncertain.
- **Medium:** recoverable failures or significant workflow/usability disruption with a workaround; complex changes increase priority.
- **Low:** limited-impact presentation or secondary behavior with low exposure.

Explain each selected risk in feature-specific terms; do not manufacture a Low risk to populate all categories. Execute High scenarios first. Use High/Medium/Low consistently for risks and scenario priorities; distinguish coverage priority from defect severity.

Select only relevant manual activities: functional and smoke for critical paths; targeted regression for affected existing behavior; exploratory charters for uncertainty and usability; manual integration checks for component/service boundaries; negative and boundary checks for documented rules; compatibility for a confirmed matrix; security-oriented permission/data checks; recovery for relevant interruptions. For unknown recovery expectations, propose an investigation and clarification rather than a fabricated pass condition. Include manually assessable non-functional risks where supported; do not promise load capacity or penetration assurance from a manual plan.

Adapt to architecture: Web uses browser sessions and UI/API effects; Mobile uses documented device/OS and lifecycle behavior; API uses human-issued requests and response/data inspection; Backend uses human-triggered jobs and observable outcomes; Hybrid covers documented handoffs. Tools and environments must be available or explicitly proposed, never assumed.

Apply design techniques internally:

- Equivalence partitioning and boundary value analysis for input classes and documented limits. For missing limits, flag the gap rather than invent a boundary.
- Decision tables for interacting conditions/permissions; state transitions for sessions and workflow legality; use cases for actor journeys.
- Error guessing, experience-based testing, and time-boxed exploratory testing for retries, interruptions, stale state, and confusing feedback, where relevant. Do not invent a timebox duration.

Choose a compact set of high-value scenarios (usually 5–8), each with an action and observable expected outcome or an explicit unresolved oracle. Preserve source references and critical negative coverage when compressing. Do not expand into numbered UI steps or a detailed test suite unless separately requested. If UI steps are requested, follow the project's selector convention; in this QA kit use verified stable `data-testid` values.

## Draft and check

Use [references/plan-template.md](references/plan-template.md) when drafting. Keep A–H visible; use “Not applicable” with a reason only where supported, and “Unconfirmed” for missing facts. Avoid repeating environments in Scope and Environment. Use short source labels and section citations to keep claims traceable.

Before delivery:

- Verify each expected behavior against a readable source or confirmed answer. Separate facts, planning recommendations, assumptions, and open questions.
- Check that critical journeys, high risks, meaningful negative/boundary checks, dependencies, and regression exposure map to scenarios. Do not claim exhaustive coverage.
- Ensure entry/exit criteria are observable: required data/access available, critical scenarios executed with accepted outcomes, relevant regression complete, blockers resolved, acceptance criteria satisfied, residual risks accepted, results communicated. No invented percentages or defect thresholds.
- Check all eight sections, risk/scenario priorities, concise actionable wording, and exclusively manual testing activities. Planning is not evidence that testing passed or release approval was granted.
- Count words and trim repetition toward 450–600. Use compact headings/bullets and at most one small table where helpful. Aim for A4 at 10–11 pt with reasonable margins; actual pagination depends on the renderer. If rendered preview is available, inspect it without shrinking text to force a fit. Without a preview, explicitly describe pagination as approximate.

## Usage and examples

Invoke with `$manual-test-plan-generator` plus a feature description, file, or directory, optionally specifying scope, environment, language, and output path. Example: “Use $manual-test-plan-generator on ../ct-gen-ai-skypermit/docs for operator registration/login; write TEST_PLAN.md; browser matrix unconfirmed.” If the skill is not yet listed, explicitly ask Codex to read this `SKILL.md`; automatic discovery may require a new session.

Read [references/examples/INPUT.md](references/examples/INPUT.md) and [references/examples/TEST_PLAN.md](references/examples/TEST_PLAN.md) only for a worked SkyPermit example, not as defaults for other projects. The example input is a source-backed snapshot, not an authoritative replacement for live project documents. [references/examples/VALIDATION.md](references/examples/VALIDATION.md) records the demonstration's checks and limits.
