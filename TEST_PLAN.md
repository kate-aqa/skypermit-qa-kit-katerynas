# Manual QA Test Plan — SkyPermit

## A. Overview
Assess the Web flight-authorisation journey through manual UI/API actions and persistence inspection. Sources under `skypermit/docs/`: **DM** `01-data-model.md`, **SM** `02-state-machine.md`, **API** `03-api-contract.md`, **UI** `04-frontend-spec.md`, **Q** `05-open-questions.md`, **BR** `06-business-rules.md`, **CL** `07-clarifications.md`. CL governs its correction; Q tracks unresolved decisions.

## B. Scope
Include registration/sessions, operator isolation, pilot/aircraft registries, zones, draft/check/submit, reviewer queue/decisions, cancellation, maintenance completion, and audit integrity. Exclude CR-1 proposals, superseded thresholds, load-capacity and penetration assurance.

## C. Risk Assessment
- **High:** Session/ownership failures expose another operator’s records; API enforcement across multiple resources increases estimated likelihood.
- **High:** Incorrect gate order, thresholds, local-time conversion or overlap logic permits invalid flights; interacting rules increase estimated likelihood.
- **High:** Invalid transitions or lost decision/audit records corrupt authorisations and traceability; multiple write paths increase estimated likelihood.
- **Medium:** Misleading picker eligibility, filtering or feedback disrupts recoverable workflows; UI/API interpretation creates divergence risk.

## D. Manual Testing Strategy
Run High smoke, permission, integration and boundary scenarios first; then Medium UI regression. Issue requests manually; inspect responses and permitted records. Use equivalence classes, boundaries and state transitions. Explore retries, interruptions and stale queues; clarify recovery oracles. Reuse login/curfew cases after review; E2E-OP-008 conflicts with first-failure order.

## E. Key Test Scenarios
- **High — Identity:** Register/login/reload/logout: profile ID matches Auth identity, registration starts L1, session persists until logout. Anonymous operator access returns `401`, cross-owner access `403`; reviewer access follows the header contract. [DM Operator; API Authentication]
- **High — Registries:** Create/retrieve owned pilots/aircraft; reject duplicate unique identifiers and invalid references. Check derived currency at 18 months, bands at 900/901, 3,500/3,501, 12,000/12,001 and 30,000/30,001 g; excess registration returns `422`. [DM Entities/Derived; BR §§2–3]
- **High — Draft/check/submit:** Create DRAFT; repeated validate leaves state unchanged. Submit passing data reaches UNDER_REVIEW; multiple failures identify the first PILOT→AIRCRAFT→ZONE→TIME_WINDOW failure with its citation; rejection details persist. [API Flight requests/Gate pipeline; DM FlightRequest]
- **High — Eligibility:** Exercise rating/class combinations, certification expiring before flight start, permitted bands, ceiling equality/excess, authorisation levels and inactive zones. Independent failures map to documented gates; INACTIVE/RETIRED alone add no gate rejection. [BR §§1.1–1.3; CL §§1–4]
- **High — Scheduling:** Test both sides of 6-hour/60-day and inclusive 15–240-minute limits, UTC/local midnight curfews, amended KBP/ODS 23:00–05:00, overlapping versus touching windows, and changed schedules between check/submit. Only APPROVED/UNDER_REVIEW same-aircraft requests block overlap. [BR §1.4]
- **High — Decisions/lifecycle:** Approve/reject under review; rejection requires note, missing reviewer header returns `400`. Inspect ReviewAction and append-only transition logs. Check approved cancellation at/beyond the two-hour deadline, terminal-state `409`, and completion only through maintenance. [SM Legal transitions; API Reviewer; DM AuditLog; BR §5]
- **Medium — UI/data feedback:** Confirm owned pickers retain ineligible combinations, queue sorting/state filters work, rejection citations remain visible, and 2,000/2,001-character purposes respect the cap. [UI Operator portal/Reviewer queue; DM Time handling]

## F. Test Environment and Test Data
Unconfirmed: running deployment, browser matrix, request client, database inspection and maintenance access. Propose disposable operators, admin-prepared L2/L3/inactive-zone fixtures, reviewer headers, seeded six-zone catalogue and controlled windows. Confirm Supabase Auth, schema migrations and seed readiness without modifying the reference project.

## G. Entry and Exit Criteria
**Entry:** Environment, dependencies, accounts, fixtures and observation access available; expected outcomes agreed or explicitly deferred.

**Exit:** High scenarios and agreed regression pass; acceptance criteria satisfied; blockers resolved; named owner accepts residual risks; results/defects communicated.

## H. Assumptions and Open Questions
Provisional: whole application; release/owner unconfirmed. Resolve suspended response (API suggests `403`; Q §1 leaves it undefined), inactive-zone citation (Q §2), REJECTED metadata (SM versus DM), and all-gate display versus short-circuiting (UI versus API). External originals were unavailable. No execution claimed; A4 pagination approximate, without rendered preview.
