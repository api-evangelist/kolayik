# KolayIK

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
