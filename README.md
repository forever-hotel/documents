# Forever Hotel Management System - Central Final QA Deliverables

**Prepared:** 25 October 2026 | **Course:** SENG 34213, University of Kelaniya | **Format:** black and white.

These four files follow the course guideline Section 10.1 and consolidate **all six** supplied subsystem packages: Hotel Website (HW), Manager Dashboard (MAD), Front Desk (FDS), Food Ordering and Service Requests (FOSS), Kitchen Management (KMS), and Worker Management (WKMS).

## Required four PDFs (submit as listed)

- `testing/coverage-sprint8.pdf` - multi-subsystem test and scope/coverage evidence, followed by **all six full original coverage reports**.
- `testing/test-register.pdf` - cross-service test inventory and traceability limits, followed by **all six full original test-case PDFs**.
- `security/owasp-checklist.pdf` - cross-service OWASP findings and unresolved risks, followed by **all six full original security PDFs**.
- `testing/performance-report.pdf` - actual performance evidence gaps and next steps, followed by **all six full original performance PDFs**.

Each PDF contains a front consolidated interpretation and **bookmarks** for all six subsystems. Source PDFs are reproduced page-for-page without rewriting or omitting their entries. Every PDF is monochrome (black / white / grey) without colored components.

## Supporting records

- `testing/test-register-master.csv`: 295 original selected test rows combined into a normalized editable register. Every record stores source-case ID, original verbatim status, original row JSON and original CSV row number.
- `source-evidence/<SUBSYSTEM>/`: original README, coverage notes, security notes and source test-register CSV from each uploaded ZIP (unaltered).
- `SOURCE_MANIFEST.json`: source ZIP identifiers, cryptographic hashes, case counts and consolidated PDF assembly details.

## Interpretation and limitations

- There are **250 source-listed cases linked to passing CI suites** and **45 not tested/not verified**. This is *not* the platform's total test-execution count or a signed six-service acceptance report.
- Test and coverage snapshots differ in branch/date/commit. **Do not sum** FOSS historical `develop` and feature runs, infer one six-service release, or average heterogeneous coverage percentages.
- FOSS and KMS source CSVs do not provide an explicit SRS requirement-ID column; the normalized CSV does not invent identifiers.
- MAD's latest run passed reported tests but **failed overall** on backend high-severity dependency findings. KMS has no verified coverage metrics from the cited CI run. FOSS has measured results below the course's 80% coverage expectation.
- All six security matrices are **provisional**; no system-wide OWASP certificate or signed penetration test is implied.
- **There is no verified accepted common-system performance benchmark** (such as 100-concurrent-user API p95). MAD has a harness but no independently verified approved results. This performance PDF is an honest gap and plan, not proof that the threshold was met.
- Before final university submission, obtain final release SHAs, new verified CI results, comprehensive vulnerability disposition, measured isolated performance tests, approval/sign-off, and consolidated integration/UAT proof.
- Do not publish secrets, tokens, guest data, raw payment data, or shared database credentials.

## Source repositories

- **HW**: https://github.com/forever-hotel/hotel-website - snapshot: `pre-develop`, `606acf0674a18621639bce572ce3577af933cd63`, CI date 2026-10-22.
- **MAD**: https://github.com/forever-hotel/manager-dashboard - snapshot: `development`, `bba65a0762286d4b0ac902865d7a2dd1e683e9c7`, CI date 2026-10-24/25.
- **FDS**: https://github.com/forever-hotel/front-desk - snapshot: `develop`, `503465b7eb2c29b56cc2ec0d329d91d6b40853d4`, CI date 2026-10-24.
- **FOSS**: https://github.com/forever-hotel/food-ordering - snapshot: `develop (historical) + separate feature`, `17af08411eedbed6431211a834af14b9af9e4e46; 343b7d0018`, CI date 2026-10-13.
- **KMS**: https://github.com/forever-hotel/kitchen-management - snapshot: `develop`, `89087002ac2df8f07b6140c00111979c75bc2799`, CI date 2026-10-10.
- **WKMS**: https://github.com/forever-hotel/worker-management - snapshot: `develop`, `0f91f1c9d6390c80c9f12f82b834fee9bf374104`, CI date 2026-10-25.

## Original source PDF page totals (fully retained in the consolidated reports)

| Subsystem | Coverage | Test register | OWASP | Performance |
|---|---:|---:|---:|---:|
| HW | 2 | 19 | 4 | 3 |
| MAD | 2 | 31 | 3 | 2 |
| FDS | 2 | 24 | 3 | 2 |
| FOSS | 2 | 7 | 2 | 2 |
| KMS | 2 | 15 | 2 | 2 |
| WKMS | 2 | 21 | 4 | 3 |
