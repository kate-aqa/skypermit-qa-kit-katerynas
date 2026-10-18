### E2E-OP-016: Registration form excludes authorisation-level selection

**Module:** Authentication

**Priority:** Medium

**Type:** Functional

**Doc ref:** `skypermit/docs/04-frontend-spec.md` ("Operator portal") — `authorisation_level` is not a registration-form field; `skypermit/docs/03-api-contract.md` ("Authentication & authorisation") — privilege elevation is not self-service.

**Role(s):** Operator

**Preconditions:**
- A reachable SkyPermit test deployment with the browser on its home page, signed out of any operator account.

**Steps:**
1. Click the header's registration link (`operator-register-link`) to open `/register`.
2. Inspect all fields in the registration form (`operator-register-form`) without submitting it.

**Expected result:**
- The registration form contains no input or selection control for `authorisation_level`.

**Rationale:** Guards the documented registration interface against accidental exposure of an administrative setting. Medium priority reflects the UI contract; server-side privilege enforcement requires a separate check.
