# Forever Hotel Management System - Central Final QA Deliverables

**Prepared:** 9 November 2026 | **Course:** SENG 34213, University of Kelaniya | **Format:** black and white.

These four files follow the course guideline Section 10.1 and consolidate **all six** subsystem repositories and companion shared services across the `forever-hotel` GitHub organization: Hotel Website (HW), Manager Dashboard (MAD), Front Desk (FDS), Food Ordering and Service Requests (FOSS), Kitchen Management (KMS), Worker Management (WKMS), plus shared platform repositories (`api-gateway`, `websocket-service`, `infra`).

## Required four PDFs (submit as listed)

- `testing/coverage-sprint8.pdf` - multi-subsystem test and scope/coverage evidence across all six subsystems and shared services.
- `testing/test-register.pdf` - cross-service test inventory and traceability limits (**307 recorded cases**: **264 suite-backed passing**, **43 unverified / not tested**).
- `security/owasp-checklist.pdf` - cross-service OWASP Top 10 (A01–A10) control findings, dependency audit status, and unresolved staging risks.
- `testing/performance-report.pdf` - executed local benchmark evidence (FDS 1,800-request read benchmark), verified load-testing harnesses (HW, MAD, API Gateway, FOSS k6 specifications), and staging execution protocol.

Each PDF contains a front consolidated interpretation and **bookmarks** for all **eight** top-level sections covering the overview, all six subsystems, and acceptance conditions. Every PDF is strictly monochrome (black / white / grey) without colored components.

## Interpretation and limitations

- There are **264 source-listed cases linked to passing CI suites** and **43 not tested/not verified** across **307 recorded register cases** (`33` HW + `58` MAD + `43` FDS + `54` FOSS + `82` KMS + `37` WKMS). Raw automated test execution across the organization now exceeds **5,300 passing automated checks**.
- Test and coverage snapshots are identified by exact repository, branch, commit SHA, and GitHub Actions run ID as of **9 November 2026** alongside committed subsystem QA deliverable baselines. **Do not average** heterogeneous coverage percentages across different tools or denominators.
- **Coverage highlights:**
  - **HW (`develop` @ `5e3cd6bf32`):** Backend Jest `88.38%` stmts / `82.10%` branches / `87.14%` funcs / `88.39%` lines (`460` passed); PostgreSQL integration `79` passed (`7` skipped); Frontend Node `58` passed (`98.20%` branches / `99.63%` lines on `53` instrumented tests); Browser E2E (`3` journeys) & Quality (`48` checks) pass; Staging tooling `22` passed (`96.08%` branches / `100%` lines).
  - **MAD (`development` @ `8b2c63f459`) & Shared Services:** Backend Jest `96.90%` stmts / `89.96%` branches / `90.02%` funcs / `96.90%` lines (`534` passed + `203` integration + `1` system); Frontend Vitest `92.90%` stmts / `89.28%` branches / `90.83%` funcs / `95.04%` lines (`341` passed + `8` Playwright E2E); all CI dependency audits pass (`0` high/critical vulnerabilities). Shared `api-gateway` (`39` passed, `95.27%` lines / `82.10%` branches; develop `@` `f12aad6cb4`, Runs `42647326526`, `42556026909`), `websocket-service` (`36` passed, `96.19%` lines / `86.88%` branches; development `@` `f12ef41ba9`, Runs `42650523420`, `42556069266`), and `infra` (`19` passed) all pass CI.
  - **FDS (`develop` @ `4b5ef57d4c`):** `949` passing CI tests (`531` BE unit [`93.43%` stmts / `84.25%` branches / `93.63%` lines], `56` PostgreSQL integration, `143` HTTP/Socket.IO E2E, `219` FE Jest [`88.51%` stmts / `80.09%` branches / `88.96%` lines]) and `0` npm audit vulnerabilities (`undici` `8.12.2`).
  - **FOSS (`develop` @ `1bdb9c9` / QA snapshot `9ac1e6f`):** Integrated live KMS menu and persistent food orders (PR #115) and authoritative stay expiry checks (PR #117). Backend Jest `82.97%` stmts / `76.64%` branches / `83.33%` lines (`558` passed; `9ac1e6f` snapshot `84.63%` stmts / `78.67%` branches / `84.91%` lines; Critical Business Logic Core `96.10%` stmts / `84.21%` branches / `96.40%` lines across `28` suites / `312` tests); Backend E2E `237` passed (`5` skipped); Frontend Node test runner `269` passed (`1` skipped) across `81` suites; CI Run `42644489101` passing.
  - **KMS (`develop` @ `0eaf5ab`):** Connected kitchen workflows to shared hotel services (PR #103) and transactional order-ready outbox (PR #101). Expanded from `76` to **`837` passing CI tests (`6` skipped)** across `144` passing suites (`393` BE unit, `58` BE E2E, `386` FE Jest) with `0` dependency vulnerabilities; backend test coverage enforced above 85% target at **`91.89%` stmts / `83.81%` branches / `85.53%` funcs / `94.00%` lines** (PR #106); augmented by `guest-kitchen.service.spec.ts` and `kitchen-events.service.spec.ts` (CI Run `42645080201`).
  - **WKMS (`develop` @ `1a33059c00`):** Expanded from `445` to **`649` passing CI tests (`43` skipped)** with enforced `>=80%` global branch and line gates: Backend Jest `88.72%` stmts / `85.45%` branches / `82.44%` funcs / `91.25%` lines (`400` passed); Frontend Jest `88.53%` stmts / `81.14%` branches / `91.50%` funcs / `89.74%` lines (`249` passed); supports KMS UUIDv5 food-order IDs (PR #74) and immutable images (PR #75).
- **Security & Performance:** All six security matrices remain **provisional** pending deployed staging TLS/WAF/penetration verification. FDS provides an executed local read-only HTTP benchmark (`15/15` cases passed across `1,800` measured requests, `0%` errors, max p95 `153.23 ms`, peak `1,016.85 req/s`); integrated multi-service 100/300-user staging load tests remain to be executed.
- Do not publish secrets, tokens, guest data, raw payment data, or shared database credentials.

## Source repositories (verified 9 November 2026)

- **HW**: https://github.com/forever-hotel/hotel-website - `develop` @ `5e3cd6bf323bc005e9efb86b5e09ba0e3015e727` (QA snapshot `63673ba704b4ae645a8f067e246309b6fb0f03d0`), CI date 2026-11-08 (Runs `42504096480`, `42504096452`, `42500427127`).
- **MAD**: https://github.com/forever-hotel/manager-dashboard - `development` @ `8b2c63f459a93c217eb7cb502a2500e80181aa6e`, CI date 2026-11-08 (Runs `42480400290`, `42504148805`).
- **Shared Services**:
  - https://github.com/forever-hotel/api-gateway - `develop` @ `f12aad6cb46f0436d2c4e0499ba3ae1345cf6c32`, CI date 2026-11-09 (Runs `42647326526`, `42556026909`).
  - https://github.com/forever-hotel/websocket-service - `development` @ `f12ef41ba9`, CI date 2026-11-09 (Runs `42650523420`, `42556069266`).
  - https://github.com/forever-hotel/infra - `development` @ `e3e17fcaac`, CI date 2026-11-08 (Run `42501357369`).
- **FDS**: https://github.com/forever-hotel/front-desk - `develop` @ `4b5ef57d4c4ce03c9ab2fe78f42b94eff96838cd` (QA branch `feature/84-fds-final-qa-evidence` @ `e590326` / `1065617` / `a6050dd`), CI date 2026-11-08 (Runs `42506217082`, `42506217056`).
- **FOSS**: https://github.com/forever-hotel/food-ordering - `develop` @ `1bdb9c97b5e1fc16bf896d8ca6b1a45f9fb63d5a` (QA snapshot `9ac1e6f3df9f93ee76be6db479dc8c4bf6529328`), CI date 2026-11-09 (Runs `42644489101`, `42572347542`, `42490527447`).
- **KMS**: https://github.com/forever-hotel/kitchen-management - `develop` @ `0eaf5ab9b071ae7e5e3478cf30a27fe819db35dc`, CI date 2026-11-09 (Runs `42645080201`, `42650340919`, `42555890536`, `42502180570`).
- **WKMS**: https://github.com/forever-hotel/worker-management - `develop` @ `1a33059c0005752ca709ca91ab11067dc6cff55a`, CI date 2026-11-08 (Runs `42504698971`, `42504970394`).
