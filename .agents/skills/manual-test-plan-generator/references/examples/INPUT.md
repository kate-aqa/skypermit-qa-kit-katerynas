# Example input: SkyPermit operator registration/login

## Planning request and interview outcome

Create a one-page manual QA plan for SkyPermit operator registration/login, including session/logout and immediate access-control regression. Use English Markdown, 450–600 words. No browser/device matrix or test environment has been provided; record these as unconfirmed. All testing is manual. Do not execute tests or change the application.

## Source-backed requirements snapshot

Sources are in the read-only sibling application `../ct-gen-ai-skypermit`. Citation aliases below retain this QA kit's `skypermit/docs/...` convention; resolve them against that sibling. Snapshot reviewed 2026-10-18.

- **S1:** `skypermit/docs/03-api-contract.md`, “Authentication & authorisation”, “Registry”, and “Error semantics”. API base `/api/v1`. `POST /operators` creates a Supabase Auth user and Operator profile in one request, then signs the caller in with a session cookie. `POST /auth/login` uses email/password and sets the session; `POST /auth/logout` clears it. `GET /auth/me` returns the operator profile or `401`. Registration starts at `L1`; privilege elevation is out-of-band. Operator-scoped endpoints derive ownership from the session rather than a submitted `operator_id`. Without a session, access is `401`; another operator's resource is `403` when no reviewer header is present. Single-resource reads and validation also allow any `X-Reviewer-Id`; this is an explicit exception. Reviewer identity remains a header, not operator login.
- **S2:** `skypermit/docs/04-frontend-spec.md`, “Operator portal” and “Two contracts to hold exactly”. `/register` combines email/password and Operator profile in one form. `authorisation_level` is absent from that form. `/login` authenticates operators. Header shows `operator-login-link` and `operator-register-link` logged out, or `operator-session-info` and `operator-logout-button` logged in. Stable `data-testid` is the UI selector contract; removed `operator-id-input` must not be reused.
- **S3:** `skypermit/docs/01-data-model.md`, “Entities → Operator”. Operator ID equals its Supabase Auth user ID. Email and national ID are unique. Profile fields include legal name, national ID, email, phone, address, city, postal code, authorisation level, registration date, and status. Field lengths and required/optional classification are not fully specified here.
- **S4:** `skypermit/docs/05-open-questions.md` §1 and `07-clarifications.md` item 3. Suspended-operator write denial has no settled status/body; this conflicts with the API contract's general `403` example. Do not use that example as a settled oracle for suspended-account handling. This does not redefine normal ownership-denial behavior.

## Coverage context and missing requirements

Existing manual login cases are in `test-cases/auth/E2E-OP-009-operator-login.md`; use as coverage context, not as authority over source contracts. The demonstration snapshot above is sufficient to reproduce the example without treating existing case expectations as new requirements.

No approved password length/complexity bounds, email verification/reset flow, lockout/rate policy, session lifetime, duplicate-registration error contract, failed-registration rollback/retry contract, supported browser matrix, QA URL, dependency failure procedure, or release decision owner was supplied. Do not invent these. Supabase Auth is a documented dependency; its availability in a QA environment is unconfirmed.
