# KolayIK

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Kolay İK (Kolay Yazılım A.Ş.) is an Istanbul-based cloud HR / human-capital-management SaaS platform used by 4,500+ companies and 300,000+ employees, mostly in Türkiye. It covers personnel records (özlük), payroll (bordro), performance, shifts (vardiya), compensation review, applicant tracking, HR analytics, time and attendance (PDKS), leave, expenses and training.

Kolay publishes one official public REST API — the **Kolay Public API** (v2, `https://api.kolayik.com`) — documented as a public Postman collection at <https://apidocs.kolayik.com/>. It has 46 operations covering person, unit, leave, timelog, transaction, approval process, calendar, training, expense and payroll. Authentication is an HTTP bearer API token created in-product at <https://app.kolayik.com/settings/developer-settings>.

## Artifacts

| Artifact | Method |
|---|---|
| `postman/` — the official published Postman collection, verbatim | searched |
| `openapi/` — OpenAPI 3.1 converted faithfully from that collection | derived |
| `overlays/` — API Evangelist enhancements and gap notes | generated |
| `authentication/`, `conventions/`, `errors/`, `lifecycle/`, `data-model/` | searched / derived |
| `conformance/` — ISO 27001 / 27701 / 9001 and KVKK posture | searched |
| `security/` — domain security probe, vulnerability disclosure findings | probed |
| `changelog/`, `well-known/`, `packages/`, `cli/`, `mcp/`, `llms/` | searched / derived |
| `skills/`, `arazzo/`, `agentic-access/` | generated |

## Notable gaps

- No 4xx/5xx responses, JSON schemas, rate-limit policy or error-code registry are documented.
- **No sandbox or test mode** — every write hits live HR, payroll and personal data.
- No idempotency mechanism, no webhook or event surface, no OAuth scopes.
- No first-party SDKs or CLI; the `kolay-cli` package (and its `kolay-mcp` server) is community-maintained and explicitly not a Kolay product.
- No `/.well-known/security.txt` and no formal vulnerability disclosure policy, though a published information security policy and a security contact exist.

Backed by: 500-global — https://kolayik.com
