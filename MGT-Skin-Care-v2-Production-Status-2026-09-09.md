# MGT Skin Care v2: production status
Audit completed September 9, 2026. Local validation ran September 8–9.

## Release position
The application is a connected local web preview, not a production release. The Sites project has no live URL and zero saved versions. The current API uses SQLite, reports demo mode off, and reports sign-in and AI unconfigured. The local production web build and API were restarted and respond successfully.

The estimated production window for the referral web portal is **4–6 weeks** after dedicated work and prerequisites begin. An illustrative September 9 start gives **October 7–21, 2026**. This is a planning estimate, not a commitment or a background work schedule. It assumes one dedicated full-stack engineer, part-time QA/operations, owner decisions within two business days, credentials/domain available during week 1, no redesign, and approved launch content on schedule. Delayed dependencies move these dates.

The baseline launch includes the referral directory, comparison and saved lists, preference-based Beauty & Style, account controls, support and approved informational content. Matching needs approved data or a clearly unavailable state. Live AI and subscriptions remain disabled until separately accepted. Adding both introduces an estimated 2–4 further weeks after their inputs are ready. Direct product commerce, fulfillment and an app-store mobile release require separate plans.

## Work completed in this checkpoint
- Restricted the admin overview at the API boundary. Viewer, catalog editor and SME roles receive content records but no support tickets, orders, partner operations, jobs, audit history, company settings or deletion requests. Compliance and superadmin retain operational access.
- Updated the operations screen to explain the restriction. Existing colors and page structure are preserved.
- Added integration tests for guest/customer denial, seven account role combinations, sensitive record filtering, forged identity headers, denied support writes and immediate access revocation on the next request.
- Added `pnpm verify:release`, a fail-fast sequence covering five build/type checks and twelve local suites. Source smoke tests use the installed TypeScript compiler in-process. Builds perform type checking.
- Corrected the API Dockerfile to call the AI gateway's existing typecheck script instead of a nonexistent build script.
- Expanded Docker context exclusions for development output, compiled output, logs and SQLite files. The container scaffold still needs an actual build and runtime exercise.

## End-to-end feature inventory
| Area | Implemented locally | Release gap |
|---|---|---|
| Navigation and theme | Dark theme, violet accents, responsive layout rules and customer/admin routes | Full current-page keyboard, mobile and cross-browser acceptance checks |
| Shop and Saved | 11 allowlisted retailers across 7 segments, search/filter state, comparison of up to 3, download, restoring share links, individual saves and atomic shortlist saves | Final directory review and production browser validation |
| Retailer responsibility | External storefront links, no live retailer price/stock data, legacy product purchase routes blocked | Commercial attribution or partner fees require separately authorized agreements |
| Beauty & Style | Six deterministic preference sections, consented profile saving, removal, multilingual notes and stale-edit detection | Browser interaction tests and finalized product copy. No live photo/color diagnosis |
| Skin Match / My Skin / Routine | Profile inputs, exclusions, rules pipeline, saved routines and simplification | Real reviewed products, ingredient rules and safe unavailable states if no match data |
| Replenishment | Saved reminder records and removal | Email/push delivery is not implemented |
| Learn / Coach | Knowledge drafting, independent approval, reviewed-excerpt retrieval and provider gateway | Approved editorial library, live provider setup, evaluation and failure handling |
| Account | Email OTP integration, secure cookie sessions, export and deletion-request intake | Live Supabase/email journey, deletion execution, retention policy and scheduled cleanup |
| Support | Requests, record ownership and privileged in-portal replies | Support staff/process and any external delivery integration |
| Administration | Verified sessions, drafting/approval roles, operations access filtering | Operator provisioning, complete product-management UI and operational tooling |
| Subscriptions | Consumer/vendor separation, monthly/annual paths, 14-day trial, checkout/portal/status webhooks | Approved pricing/benefits/terms, Stripe sandbox lifecycle and live reconciliation |
| Mobile / product commerce | Mobile scaffold preserved, legacy commerce disabled | Excluded from this web referral release |

A directory listing does not certify a partnership or retailer quality. Retailers remain responsible for their live prices, availability, billing, shipping and returns.

## Backend, data and security
The Express portal uses server-side sessions, request-origin and CSRF checks, rate limits, account/role checks and transactional record storage. Production startup rejects SQLite, demo mode and HTTP public origins. The new admin read restriction closes a specific excessive-access issue. It does not substitute for a complete security review.

SQLite is the development adapter. PostgreSQL uses a generic hub_records table and serialized transactions. Its adapter has not been exercised against a production database during this audit. Legacy SQL migrations must not be applied to this adapter without an explicit migration plan. Global transaction serialization, including some provider work, requires performance assessment before larger traffic.

The account export includes saved customer records. Style-profile removal executes immediately after confirmation. Account deletion currently records a pending request with a 30-day not-before date; it does not execute account erasure. A tested worker or operator procedure must cover application records, sessions, external identity, retention exceptions and recovery from partial failure. Automated session/record retention jobs and backup jobs are not installed.

## Deployment and source management
The prepared topology is Caddy HTTPS edge → Next.js web → Express API → PostgreSQL. Dockerfiles and Compose exist. Docker and psql executables were unavailable on this host, so container startup, database integration, production TLS and restore testing are unverified. A compatible production runtime and domain must be selected and exercised.

Sites project appgprj_6a9cb9f6a1e08191af5409cd113f054c returned current_live_url=null and latest_version_number=0. Registration is not deployment.

The public GitHub repository currently stores ZIP checkpoints rather than an extracted source tree. Those archives preserve source, assets and documentation but do not activate CI on that source. A future source checkout and release workflow are still required. Prior direct Git/connector authentication attempts did not establish a working source push. Browser archive publication remains available and authorized. Google Drive is a second checkpoint destination.

## Verification evidence
`pnpm verify:release` passed, exit code 0.

| Group | Passed checks |
|---|---|
| Build/type checks (5) | Domain build, shared build, AI gateway typecheck, API build, Next.js production build |
| API suites (5) | Referral portal, billing, coach, style, admin access |
| Domain/gateway suites (3) | Engine pipeline, scripted AI gateway routing, pricing logic |
| Web suites (4) | Component rendering, admin rendering, shop URL state, client recovery |

Local HTTP checks returned 200 for /shop and the web-to-API /api/hub/retailers route. API /readyz returned ok with storage sqlite. Tests use isolated in-memory data and provider mocks where applicable. Billing includes signature verification with a mocked Stripe client. The web rendering suites include earlier components, so they must not be described as full current-page browser E2E coverage.

Not performed: live Supabase/email, live Stripe sandbox, live AI providers, production PostgreSQL, container build/start, dependency security audit, penetration testing, load tests, complete browser/device/accessibility testing, production backup restore or deployment. There is no defensible overall completion percentage from these checks.

## Timeline and exit gates
| Window from Sep 9 | Work | Exit gate | Proposed accountable roles |
|---|---|---|---|
| Week 1, Sep 9–15 | Confirm referral launch scope, establish real source checkout and staging setup, provision domain and service access | Staging architecture, owners and credentials ready | Product owner, engineering, operations |
| Week 2, Sep 16–22 | Live sign-in, account lifecycle/deletion/retention, approved data and policy work | Account journey and deletion demonstration, launch content reviewed | Engineering, editorial and privacy reviewers |
| Weeks 3–4, Sep 23–Oct 6 | PostgreSQL and service integration, browser/device QA, failure recovery, backup restore and small-user pilot | Acceptance journeys pass, recovery demonstrated, pilot issues recorded | Engineering, QA, operations |
| Weeks 5–6, Oct 7–21 | Fix pilot issues, review release gates, gradual rollout and monitoring | Go/no-go decision, rollback ready, support coverage active | Product owner, QA, operations |

These are role assignments to make, not people already engaged. The estimate contains stabilization time and depends on timely review. Live AI and billing require separate provider/content or financial setup and acceptance before activation.

## Go/no-go criteria
1. The core shop → compare → save → restore journey passes on desktop and mobile browsers, including keyboard use, empty states and failed requests.
2. Live sign-in, session renewal/logout and customer/admin isolation pass.
3. Export, account deletion and retention are demonstrated with test accounts.
4. Production-like PostgreSQL, HTTPS staging and secure environment values are verified.
5. Approved company identity, policies and launch content are present.
6. Backup restoration and application rollback are rehearsed.
7. Monitoring, support ownership and a pilot review are in place, with no unresolved critical security defects.
8. Any enabled AI or billing feature has passed its own live-service acceptance checks.

## Evidence index
Primary sources: apps/api/src/portal/{server,security,store,style,retailers,ai,billing,catalog}.ts; apps/web/src/components; apps/api/test; apps/web/src/__smoke__; packages/domain and packages/ai-gateway; infra/portal; .openai/hosting.json; CURRENT-SCOPE.md; BUILD-CHECKPOINTS.md. Local test log: the current task's work/release-verification.log. This report supersedes older readiness statements when they conflict.

The presentation PDF is a visual slide export. An editable PPTX and this text report accompany it.
