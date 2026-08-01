---
name: Onboard an employer into ICHRA benefits
description: Create an employer in Thatch and run the embedded onboarding flow that
  gets them live on ICHRA health benefits.
api: openapi/thatch-health-platforms-openapi-original.yml
operations:
- postEmployers
- postEmployerOnboardingSessions
- getEmployersEmployerId
generated: '2026-07-21'
method: generated
---

# Onboard an employer into ICHRA benefits

Authenticate every request with `Authorization: Bearer <YOUR_API_KEY>` (keys are
`sk_`-prefixed, generated in the Thatch dashboard; partner access is provisioned
by platforms@thatch.com). Base URL: `https://partners.thatchcloud.com/api/partners/v1`.

1. **Create the employer** with `postEmployers` (`POST /employers`). Required
   details include email, name, business_type, EIN, address, and phone_number.
   Capture the returned `id` (`empl_...`).
2. **Create a secure onboarding session** with `postEmployerOnboardingSessions`
   (`POST /employer_onboarding_sessions`), passing `employer: <empl_id>`. The
   response contains a short-lived `claim_url` (note `expires_at`).
3. **Embed the onboarding iframe** in your product with `src = claim_url`. Thatch
   handles account creation, bank connection, allowance setup, and employee
   invites inside the iframe.
4. **Handle the redirect**: listen for a `THATCH_REDIRECT` postMessage from
   origin `https://app.thatch.com`, validate the `redirectUrl` (https only), and
   navigate the parent window to it.
5. **Track progress** with `getEmployersEmployerId` (`GET /employers/{employer_id}`)
   — `onboarding_status` and `lifecycle_events[]` reflect where the employer is.

Rules: there is no documented idempotency key — avoid blind retries of POSTs;
use `metadata` key-value pairs to correlate Thatch employers with your records.
The public spec documents only success responses; treat non-2xx defensively.
