# Manual QA Test Plan — SkyPermit Data Model

## A. Overview
Validate seven entities in the Web application and supporting API through manual inspection. Sources: **DM** = `skypermit/docs/01-data-model.md`; **API** = `03-api-contract.md`; **SM** = `02-state-machine.md`; **BR** = `06-business-rules.md`; **Q** = `05-open-questions.md`; **CL** = `07-clarifications.md`, all under `skypermit/docs/`.

## B. Scope
Cover Operator/Auth identity, Pilot/Aircraft ownership, Zone references, FlightRequest persistence, ReviewAction decisions, and append-only AuditLog. Include derived values, validation, and lifecycle integration. Exclude exhaustive gate combinations, unrelated UI behavior, and unadopted CR-1 changes.

## C. Risk Assessment
- **High:** Wrong ownership or broken references expose another operator’s records or corrupt requests; multiple linked identities increase estimated likelihood.
- **High:** Stale derivations, shifted timestamps, or missing decision/audit records produce invalid authorisations or untraceable decisions; evaluation and persistence boundaries increase estimated likelihood.
- **Medium:** Invalid enum/text values disrupt registration or request handling; duplicate API/database validation increases estimated divergence likelihood.

## D. Manual Testing Strategy
Execute High scenarios first: functional smoke, permissions, manual integration and negative/boundary inspection. Follow with Medium validation and login/curfew regression (E2E-OP-009, 006–007). Investigate interrupted registration/submission and stale reads; clarify recovery expectations. Existing cases do not override requirements.

## E. Key Test Scenarios
- **High — Identity/ownership:** Register two operators; inspect Auth/profile IDs, L1 defaults, session-derived ownership, anonymous access (`401`), cross-owner access (`403`), and reviewer reads. In isolated disposable data, deleting an Auth user cascades its Operator row. [DM Operator; API Authentication]
- **High — Relationships/uniqueness:** Register linked records and create a DRAFT; retrieve matching IDs. Attempt duplicate operator email/national ID, pilot national ID, aircraft serial, and nonexistent references: constraints prevent invalid persistence; exact errors remain unconfirmed. [DM Entities]
- **High — Derived values:** Read pilot currency around 18 months and test submission with certification expiring before flight start. Test masses 0, 900/901, 3,500/3,501, 12,000/12,001, 30,000/30,001 g: C0–C3 boundaries hold; excess registration returns `422`. With approved fixture corrections, rereads recalculate without revisiting issued authorisations. [DM Derived; BR §§2–3]
- **High — Time/text:** Create and reread UTC windows; non-increasing endpoints and 2,001-character purposes cannot persist; 2,000 characters pass this constraint. Inspect API/database enforcement separately. Curfew evaluation uses zone-local time, including midnight crossings. [DM Time handling; BR §1.4]
- **High — Decisions:** Submit passing/failing drafts, approve/reject under review, then reread records. Gate rejection retains reason, first failing gate and citation; reviewer rejection requires a note and writes reviewer/action records. Illegal transitions return `409`. Rejection-field conflict remains open. [DM FlightRequest/ReviewAction; API Reviewer; SM]
- **High — Audit/reference protection:** Inspect creation/transition history, including nullable creation `from_state`, actor and timestamp; no AuditLog update/delete path exists. Zones expose reference reads without ordinary create/update flows. [DM AuditLog/Zone; API Reference data]
- **Medium — Enumerations:** Exercise documented values and invalid alternatives; invalid enums cannot persist. Do not infer gate rejection from INACTIVE pilots or RETIRED aircraft. [DM Enumerations; CL §2]

## F. Test Environment and Test Data
Unconfirmed: deployment, browser matrix, Supabase access, manual request tool, reviewer identity and fixture-correction access. Propose disposable accounts/records, read-only database inspection, seeded zones, and approved admin fixture preparation.

## G. Entry and Exit Criteria
**Entry:** Access, fixtures and dependencies available; scenario oracles agreed or explicitly deferred.

**Exit:** Critical scenarios and relevant regression executed successfully; agreed acceptance criteria met; release blockers resolved; residual risks accepted by a named owner; results/defects communicated.

## H. Assumptions and Open Questions
Provisional: DM defines scope; no execution/release approval claimed. Confirm release owner/blockers, correction access, and recovery oracles. Resolve DM’s gate-only `failing_gate` versus SM’s general REJECTED description; API suggests suspended `403`, while canonical Q §1 leaves response undefined. Inactive-zone citation remains open (Q §2). E2E-OP-008 contradicts first-failure order (BR Evaluation order). A4 pagination is approximate; no rendered preview inspected.
