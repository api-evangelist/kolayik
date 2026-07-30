---
name: Record time and expenses in Kolay İK
description: Log work or overtime timelogs for an employee, file expense/advance transactions against an expense category, and review both.
api: openapi/kolayik-public-api-openapi.yml
operations: [personList, timelogCreate, timelogList, timelogView, timelogDelete, expenseListCategories, transactionCreate, transactionList, transactionView]
generated: '2026-07-19'
method: generated
source: openapi/kolayik-public-api-openapi.yml
---

# Record time and expenses in Kolay İK

## Before you start

- Base URL `https://api.kolayik.com`, auth `Authorization: Bearer <TOKEN>`.
- No sandbox, no idempotency key. Every create is live and a blind retry duplicates the record.
- Deletes (`timelogDelete`, `transactionDelete`) are destructive — always confirm with a human and
  always read the record with the matching `...View` operation first.

## Time tracking

1. **Resolve the employee.** `personList` (`POST /v2/person/list`) → `personId`.
2. **Log time.** `timelogCreate` (`POST /v2/timelog/create`) with `startDate`, `endDate`
   (`YYYY-MM-DD HH:MM:SS`), `personId`, `status`, `description` and `type` (`work` or `overtime`).
3. **Review.** `timelogList` (`POST /v2/timelog/list`) filters on `personId`, `type`, `status`,
   `startDate`, `endDate`, and pages with `limit` / `page`, sorted via `sortType` / `sortOrder`.
   `timelogView` (`GET /v2/timelog/view/{id}`) reads one entry.
4. **Correct.** There is no timelog update operation — remove with `timelogDelete`
   (`DELETE /v2/timelog/delete/{id}`) and re-create.

## Expenses

1. **List valid categories.** `expenseListCategories` (`GET /v2/expense/list-categories`), filtered
   by `title` and `isEnable`. Use a returned category id — never invent one.
2. **File the transaction.** `transactionCreate` (`POST /v2/transaction/create`).
3. **Review.** `transactionList` (`POST /v2/transaction/list`) filters on `personId`, `type`
   (e.g. `advance-expense`), `status`, `paid`, `startDate`, `endDate` with `limit` / `page`.
   `transactionView` (`GET /v2/transaction/view/{id}`) reads one.

## Reading responses

`{"error": false, "data": {...}}` on success. Kolay documents no failure shape — surface non-200
responses verbatim.

## Related artifacts

- `conventions/kolayik-conventions.yml`
- `errors/kolayik-problem-types.yml`
