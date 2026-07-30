---
name: Check balances and file a leave request in Kolay İK
description: Look up an employee, read their leave balances, create a leave record over a date range with an optional replacement, and verify it.
api: openapi/kolayik-public-api-openapi.yml
operations: [personList, personLeaveStatus, leaveCreate, leaveView, leaveList, approvalProcessList]
generated: '2026-07-19'
method: generated
source: openapi/kolayik-public-api-openapi.yml
---

# Check balances and file a leave request in Kolay İK

## Before you start

- Base URL `https://api.kolayik.com`, auth `Authorization: Bearer <TOKEN>`.
- No sandbox exists — a created leave record is real and may trigger approval workflows and
  notifications. Confirm with a human before calling `leaveCreate`.
- No idempotency key: a retried `leaveCreate` books a second leave. Re-check with `leaveList`
  before retrying.

## Steps

1. **Find the person.** Call `personList` (`POST /v2/person/list`) and take the 32-character
   `personId`. Never construct an id.
2. **Read balances.** Call `personLeaveStatus` (`GET /v2/person/leave-status/{id}`) to see the
   employee's leave balances before booking anything. If the balance does not cover the request,
   stop and report it rather than booking.
3. **Check existing leave in the window.** Call `leaveList` (`GET /v2/leave/list`) with
   `personId`, `startDate` and `endDate` (`YYYY-MM-DD HH:MM:SS`), plus `status`, `limit` and
   `include_inactive_employees` as needed. Overlapping leave is a reason to stop and ask.
4. **Create the leave record.** Call `leaveCreate` (`POST /v2/leave/create`) with `startDate`,
   `endDate`, `personId`, `leaveTypeId`, and optionally `comment` and `replacementPersonId`. The
   `leaveTypeId` must come from the tenant's configured leave types — do not invent one.
5. **Verify.** Call `leaveView` (`GET /v2/leave/view/{leaveId}`) with the id returned in `data`.
6. **Understand routing.** Call `approvalProcessList` (`GET /v2/approval-process/list`) if you need
   to explain which approval workflow the request will enter.

## Conventions

- Dates are `YYYY-MM-DD`; datetimes are `YYYY-MM-DD HH:MM:SS`.
- List operations page with `limit` and `page`; `timelog`-style lists also take `sortType` and
  `sortOrder`.
- Every response is `{"error": false, "data": {...}}`.

## Related artifacts

- `conventions/kolayik-conventions.yml`
- `data-model/kolayik-data-model.yml`
