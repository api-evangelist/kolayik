---
name: Onboard an employee in Kolay İK
description: Create a new person record in Kolay İK with their organizational unit assignment and compensation, then verify the record and assign onboarding training.
api: openapi/kolayik-public-api-openapi.yml
operations: [personShowAvailableDataFields, unitShowUnitTree, personCreate, personView, trainingList, personAssignTraining]
generated: '2026-07-19'
method: generated
source: openapi/kolayik-public-api-openapi.yml
---

# Onboard an employee in Kolay İK

Creates an employee (`Person`) in Kolay İK and completes the onboarding basics.

## Before you start

- Base URL is `https://api.kolayik.com`. Every path is under `/v2/`.
- Authenticate with `Authorization: Bearer <TOKEN>`. The token is created in-product at
  <https://app.kolayik.com/settings/developer-settings>.
- The token's Kolay access type bounds what you can do: `owner` has full read/write on all
  employees, `manager` is the same minus payment and tax details, `employee` sees only their own
  record. If a write fails, check the access type before retrying.
- **There is no sandbox.** Every write in this skill changes live HR and payroll data. Confirm with
  a human before any create, update, terminate or delete.
- There is no idempotency key. A retried `personCreate` will create a second person — never retry a
  write blindly; re-read with `personList` first.

## Steps

1. **Discover the custom fields this tenant uses.** Call `personShowAvailableDataFields`
   (`GET /v2/person/show-available-data-fields`). It returns the `fieldToken` values valid for
   `person.dataList`. Do not guess tokens.
2. **Read the organizational tree.** Call `unitShowUnitTree` (`GET /v2/unit/show-unit-tree`) and pick
   the exact `unitName` / `unitItemName` pairs (e.g. Şirket, Şube, Departman, Unvan) the new hire
   belongs to. If a unit item does not exist, create it with `unitCreateUnitItem`
   (`POST /v2/unit/create-unit-item`) — this changes the org chart, so confirm first.
3. **Create the person.** Call `personCreate` (`POST /v2/person/create`) with a `person` object.
   Only `firstName` and `lastName` are mandatory; every omitted field defaults to null. Respect the
   documented constraints:
   - `firstName` / `lastName` letters only.
   - `email` and `workEmail` must both be valid and must not be the same.
   - `mobilePhone` must be sent with `mobilePhoneCountryCode`; `workPhone` with
     `workPhoneCountryCode`.
   - `employmentStartDate`, `contractEndDate`, `birthday` in `YYYY-MM-DD`.
   - `contractType` is `definite` or `indefinite`; `contractEndDate` is mandatory when `definite`.
   - `idNumber` is the TCKN when `nationality` is `TR`.
   Attach `unitList` (with `default: true` on the active assignment) and `compensationList` if you
   have salary data. Use `options.sendWelcomeEmail` deliberately — it emails a real person.
4. **Verify.** Take `data.person.id` from the response and call `personView`
   (`GET /v2/person/view/{id}`) to confirm the stored record. Use `personViewSummary`
   (`GET /v2/person/view-summary/{id}`) when you only need the compact profile.
5. **Assign onboarding training.** Call `trainingList` (`GET /v2/training/list`) to find the course,
   then `personAssignTraining` (`POST /v2/person/assign-training`). Confirm with
   `personListTrainings` (`GET /v2/person/list-trainings/{id}`).

## Reading responses

Every documented response is `{"error": false, "data": { ... }}`. Treat `error` as the success flag
and read the payload from `data`. Kolay publishes no error-code registry and no 4xx/5xx examples, so
on a non-200 surface the raw status and body to the user rather than interpreting it.

## Related artifacts

- `conventions/kolayik-conventions.yml` — envelope, pagination, date formats
- `data-model/kolayik-data-model.yml` — entity graph and enumerations
- `authentication/kolayik-authentication.yml` — token model and access types
