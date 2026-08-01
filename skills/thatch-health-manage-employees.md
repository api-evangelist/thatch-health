---
name: Manage employees and dependents
description: Create and maintain employee records (with dependents) so Thatch can
  quote and enroll them accurately.
api: openapi/thatch-health-platforms-openapi-original.yml
operations:
- postEmployees
- getEmployees
- getEmployeesId
- patchEmployeesId
generated: '2026-07-21'
method: generated
---

# Manage employees and dependents

Authenticate with `Authorization: Bearer <YOUR_API_KEY>` against
`https://partners.thatchcloud.com/api/partners/v1`.

1. **Create each employee** with `postEmployees` (`POST /employees`), passing the
   `employer_id`, first/last name, `date_of_birth`, and five-digit `zip`
   (required). Send as much as you have — emails, `employment_subtype`,
   `pay_type`/`pay_rate`, `start_date` — because employee data drives the
   employer's quoting.
2. **Include dependents** where relevant: each object in `dependents[]` must have
   `relationship` and `date_of_birth`.
3. **Deduplicate against your payroll system** by setting `native_employee_id`
   (used when employees are first created via census upload and later via API).
4. **List and page** with `getEmployees` (`GET /employees`) using `page[number]`
   / `page[size]` (default 20, max 1,000); read the `pagination` object for
   totals and next page.
5. **Read or update** an individual record with `getEmployeesId` /
   `patchEmployeesId` (`GET|PATCH /employees/{id}`). Employees enrolled in plans
   also appear as Member objects (`member_id`).

Rules: use `metadata` for structured partner-side references (unset a key by
posting an empty string, all keys with `{}`); no idempotency key is documented,
so guard your own retries.
