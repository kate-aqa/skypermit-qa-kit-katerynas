### E2E-OP-017: Valid registration immediately signs in the new operator

**Module:** Authentication

**Priority:** High

**Doc ref:** `skypermit/docs/04-frontend-spec.md` ("Operator portal" — combined registration form logs the operator in immediately; "Two contracts to hold exactly" — authenticated header); `skypermit/docs/03-api-contract.md` ("Authentication & authorisation" — `POST /operators` signs the caller in with a session cookie; `GET /auth/me` returns the logged-in operator).

**Role(s):** Operator

**Preconditions:**
- A reachable SkyPermit test deployment with its Supabase Auth and database dependencies configured for registration and immediate sign-in.
- Browser is on the deployment's home page with no operator session.
- The synthetic email and national ID below are unused in the test deployment, including Auth users for the email. Record fresh values for each execution.

**Test Data:**
- Email: a unique synthetic address accepted by the test deployment, such as `qa.op017.<run-token>@example.com`; replace `<run-token>` with a unique execution token. Use an approved test mailbox domain if the environment requires one.
- Password: a new synthetic test password satisfying the deployment's configured policy; record it securely for this run. This case does not assert password-policy limits.
- Legal name: `QA Operator 017 <run-token>`.
- National ID: `QA017<run-token>` (synthetic and unique).
- Phone: `+380000000017`; address: `17 QA Test Street`; city: `Kyiv`; postal code: `01001`.

**Steps:**
1. Click the header's registration link (`operator-register-link`) to open the registration form (`operator-register-form`).
2. Enter the prepared email, password, legal name and national ID into `operator-register-email-input`, `operator-register-password-input`, `operator-register-legal-name-input` and `operator-register-national-id-input`, respectively.
3. Enter the prepared phone, address, city and postal code into `operator-register-phone-input`, `operator-register-address-line-input`, `operator-register-city-input` and `operator-register-postal-code-input`, respectively.
4. Click `operator-register-submit-button` once. After submission finishes, observe `operator-session-info` without performing a separate login.
5. In a second tab of the same browser session, open the same deployment's `/api/v1/auth/me` URL to inspect the server's current operator identity.

**Expected result:**
- Registration immediately establishes a session for the submitted operator: the authenticated header's `operator-session-info` shows the prepared legal name without a separate login. As the server-side observation of that same session, `GET /api/v1/auth/me` returns the logged-in operator profile with the submitted email and legal name, rather than an unauthenticated `401`.

**Rationale:** Registration is a critical entry point to the operator portal. High priority reflects the risk of creating an account that cannot immediately use its authenticated session. Checking the server-recognized identity alongside the header catches a client-only appearance of sign-in. Duplicate rejection, default authorisation level and field validation require separate cases.
