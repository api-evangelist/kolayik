---
name: Offboard (or rehire) an employee in Kolay İK
description: Terminate employment for a person in Kolay İK after reviewing their open leave, timelogs and files — and rehire a previously terminated person.
api: openapi/kolayik-public-api-openapi.yml
operations: [personList, personView, personLeaveStatus, leaveList, timelogList, personListFiles, personTerminate, personRehire]
generated: '2026-07-19'
method: generated
source: openapi/kolayik-public-api-openapi.yml
---

# Offboard (or rehire) an employee in Kolay İK

**This is the highest-consequence flow in the Kolay Public API.** `personTerminate` ends a real
person's employment record. There is no sandbox and no undo other than `personRehire`. Require
explicit human confirmation naming the employee and the termination date before calling it.

## Before you start

- Base URL `https://api.kolayik.com`, auth `Authorization: Bearer <TOKEN>`.
- The token's access type must be `owner` or `manager`; an `employee`-scoped token cannot terminate.

## Offboarding steps

1. **Resolve and confirm the person.** `personList` (`POST /v2/person/list`) then `personView`
   (`GET /v2/person/view/{id}`). Read the name, work email and start date back to the human and get
   confirmation that this is the right record.
2. **Review what is open.**
   - `personLeaveStatus` (`GET /v2/person/leave-status/{id}`) — remaining leave balances to settle.
   - `leaveList` (`GET /v2/leave/list`) with `personId` — future approved leave.
   - `timelogList` (`POST /v2/timelog/list`) with `personId` and `status=waiting` — unapproved time.
   - `personListFiles` (`GET /v2/person/list-files/{id}`) — özlük documents to retain.
3. **Terminate.** `personTerminate` (`POST /v2/person/terminate`). Report the result back rather than
   chaining further writes.

## Rehiring

Call `personRehire` (`POST /v2/person/rehire/{id}`) to restore a previously terminated person, then
verify with `personView`. Re-establish their unit assignment with `personUpdate`
(`PUT /v2/person/update`) if the org structure changed.

## Handling files

`personUploadFile` (`POST /v2/person/upload-file`) adds documents. `personDeleteFile`
(`DELETE /v2/person/delete-file/{fileId}`) and `personDeleteFolder`
(`DELETE /v2/person/delete-folder/{folderId}`) are irreversible — Turkish employment record-keeping
obligations may require retention, so never delete özlük files as part of offboarding without an
explicit instruction.

## Related artifacts

- `agentic-access/kolayik-agentic-access.yml` — per-operation execution contracts
- `authentication/kolayik-authentication.yml` — access types
