---
name: Sync payroll deductions and pay schedules
description: Model employee pay cadences and pull monthly ICHRA payroll deductions
  into the partner's payroll system.
api: openapi/thatch-health-platforms-openapi-original.yml
operations:
- postEmployersEmployerIdPaySchedules
- getEmployersEmployerIdPaySchedules
- patchEmployersEmployerIdPaySchedulesPayScheduleId
- deleteEmployersEmployerIdPaySchedulesPayScheduleId
- getDeductions
generated: '2026-07-21'
method: generated
---

# Sync payroll deductions and pay schedules

Authenticate with `Authorization: Bearer <YOUR_API_KEY>` against
`https://partners.thatchcloud.com/api/partners/v1`.

1. **Create pay schedules** for each employer with
   `postEmployersEmployerIdPaySchedules`
   (`POST /employers/{employer_id}/pay_schedules`) — frequency, reference pay
   date, and bank-closure strategy drive deduction calculations. Manage them with
   `getEmployersEmployerIdPaySchedules`,
   `patchEmployersEmployerIdPaySchedulesPayScheduleId`, and deactivate with
   `deleteEmployersEmployerIdPaySchedulesPayScheduleId`.
2. **Assign employees** to schedules via the `pay_schedules` array on the
   employee record.
3. **Pull deductions monthly** with `getDeductions` (`GET /deductions`), which
   requires `employer_id` and supports `periods[start_after]` /
   `periods[end_before]` ISO 8601 date filters. Deductions for the coming month
   are available five days before month end.
4. **Apply the amounts** in your payroll: each employee entry contains periods
   with `deductions[]` — integer minor-unit `amount` + `currency_code`, a `type`
   such as `s125_pretax`, and any `applied_correction`.

Rules: page with `page[number]`/`page[size]` (max 1,000); amounts are minor
units (1099 = $10.99); re-pull rather than cache long-term since corrections
appear as `applied_correction`.
