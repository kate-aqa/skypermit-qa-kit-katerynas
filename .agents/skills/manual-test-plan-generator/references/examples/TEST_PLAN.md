# Manual QA Test Plan — SkyPermit Operator Authentication

## A. Overview
Validate combined registration, login and session-based access in this web application. Protect account integrity and operator isolation through risk-first manual testing. Sources: S1 API contract, Authentication & authorisation / Error semantics; S2 Frontend spec, Operator portal / Two contracts; S3 Data model, Operator; S4 Open questions §1 / Clarifications item 3 (paths in INPUT.md).

## B. Scope
Include registration, login, logout, profile identity, least privilege and immediate ownership regression across operator resources. Integration: Supabase Auth, profile persistence and session cookies. Exclude reviewer authentication redesign, privilege administration, password reset and full flight-rule validation. Compatibility coverage awaits an agreed matrix (F).

## C. Risk Assessment
- **High:** Cross-operator disclosure or writes compromise isolation; session/header ownership exceptions make enforcement complex.
- **High:** Partial registration or duplicate identities break account integrity; Auth/profile coordination introduces failure exposure.
- **High:** Incorrect session identity or self-selected privilege enables unauthorized actions; registration crosses identity and permission boundaries.
- **Medium:** Misleading login errors or stale header state obstruct access; UI/session synchronization creates regression exposure.

## D. Manual Testing Strategy
Start with registration/login/logout smoke, then functional, negative, integration and permission checks. Manually inspect browser requests, responses and profile outcomes; isolate operator checks from reviewer headers. Regress ownership filtering and header states. Explore repeated submission, navigation and interrupted registration; recovery acceptance remains provisional pending clarification. Apply equivalence classes and confirmed uniqueness rules; defer numeric boundary assertions until limits are agreed. Assess form feedback and navigation usability during exploration.

## E. Key Test Scenarios
- **High:** Register unique credentials/profile; verify signed-in identity, matching Auth/profile IDs, L1 and no privilege field (S1–S3).
- **High:** Log in as an existing operator; confirm session cookie, correct `/auth/me` profile and authenticated header (S1–S2).
- **High:** Log out; verify anonymous header and `/auth/me` returns 401; protected operator access requires authentication (S1–S2).
- **High:** As operator A, inspect lists and request B's resource without reviewer headers; lists remain A-scoped and access to B is 403 (S1).
- **High:** Attempt duplicate email/national ID and repeated registration; verify uniqueness. Investigate partial-creation/retry behavior; exact error and recovery outcomes need agreement (S1, S3).
- **High:** Attempt invalid credentials and client-supplied privilege/ownership changes; verify no unauthorized session or elevation. Exact credential-error presentation is unconfirmed (S1).
- **Medium:** Explore malformed/blank inputs, stale navigation and Auth interruption; record feedback, identity consistency and unresolved recovery expectations (S1–S3).

## F. Test Environment and Test Data
QA URL/build, browser/device matrix and Supabase test access are unconfirmed. Proposed data: synthetic unique profiles, two existing operators with distinct resources, duplicate email/national ID and invalid credentials. Use separate browser sessions; manual response/profile inspection access is required. Missing field limits block precise boundary data. No production identities are needed.

## G. Entry and Exit Criteria
**Entry:** Critical expectations agreed; environment stable; Auth, accounts, data and inspection access available; execution blockers resolved.

**Exit:** Critical scenarios executed with accepted outcomes; required functional/regression coverage complete; acceptance criteria satisfied; no unresolved release blockers; remaining risks accepted by the release decision owner; results and defects communicated.

## H. Assumptions and Open Questions
Assumption: immediate ownership regression belongs to authentication scope. Confirm matrix, environment, decision owner, password/field limits, session lifetime and registration rollback/error behavior. S4 leaves suspended-write status/body unresolved despite S1's 403 example; keep it outside settled authentication assertions. This plan does not establish release readiness.
