# Sprint 8 Retrospective

**Project:** Forever Hotel Integrated Hotel Management System  
**Course:** SENG 34213, University of Kelaniya  
**Focus:** Quality, security, performance and release readiness  
**Sprint dates:** Not established on the project board; no calendar dates assumed  
**Prepared:** 2026-10-25 (Asia/Colombo)  
**Retrospective meeting date:** Not supplied; preparation date is not a meeting date  
**Attendees:** Attendance record not supplied; team confirmation required  
**Status:** Evidence-based retrospective draft; team discussion and approval not recorded

## Scope and evidence

This is a team-level reflection across the six business services and the API Gateway, Infrastructure and Documents repositories. It follows the guideline Appendix B.2 headings. Repository issues, selected source files, commit histories, recent CI metadata and central QA reports were reviewed. Tests, load runs and deployments were not executed during this document review.

The board screenshot confirms two-week windows for Sprints 5-7. The guideline Section 2.2 describes four-week development sprints. The team should record or confirm the reason for this scheduling difference with the supervisor. Work is attributed to a date window only where timestamps support it; a feature title or closed issue alone does not prove acceptance in that sprint.

## How this report maps to the board

The course guideline names Sprint 8 as the final quality/release phase. The team confirmed that its project board currently ends at Sprint 7 and that some services, including Hotel Website, have already implemented work matching the Sprint 8 theme under Sprint 7 tickets.

This report therefore reviews **existing quality and release work by theme**, including records after 2026-10-20. It is not merely an empty future plan, and it is not evidence that a dated fourth sprint, a retrospective meeting or final system acceptance has occurred. The team must agree the final sprint/phase mapping with the supervisor and add any later dates and outcomes.

## Went Well

1. **Hotel Website implemented substantial quality and booking work.** The reviewed develop revision includes PayHere checkout/Brevo confirmation, guest booking reads and change controls, contract/integration tests, an automated visitor-to-booking journey, responsive/accessibility checks, security boundary checks and a booking load-test harness. Backend, frontend, container and performance-tooling workflows passed at that develop revision. Staging/deployment and release-documentation work also exists on other branches and in the local checkout; it is not automatically part of that reviewed develop revision.

2. **Front Desk added release-hardening evidence with explicit limits.** Backend, frontend and Docker CI passed at its reviewed develop revision. Its Plan 20 handoff differentiates image/smoke checks from actual Central Auth, authenticated realtime, application messaging and public HTTPS staging acceptance. That distinction helps the team target remaining integration work rather than repeat checks that already passed.

3. **Worker Management strengthened both runtime behaviour and verification.** Its reviewed develop CI passed, and its realtime documentation records local backend/frontend checks, authenticated event updates and narrower realtime coverage. Those local figures remain separate from the central historical coverage snapshot. This makes the basis of each quality claim easier to inspect.

4. **The Documents repository consolidated final deliverables.** Final-named SRS/SDS files and six-subsystem coverage, test-register, security and performance PDFs are present. The consolidated reports retain unverified results and heterogeneous test scopes. Reporting those gaps plainly is useful engineering evidence, even where it prevents a blanket release-ready claim.

## To Improve

1. **Release readiness must refer to one exact, coherent revision set.** The Website develop workflows passed, but a later documentation branch has a failed frontend workflow. Gateway main and MAD development have failed latest observed workflows. FOSS historical successful runs do not match its newly rebuilt develop baseline. KMS successful feature runs are not proof of a green run on its reviewed develop head. Preserve branch, SHA and run URL for each decision, then repeat the applicable checks for the final candidate.

2. **Quality gaps are uneven across services.** The central coverage report records strong measured scopes for HW, MAD, FDS and WKMS, lower measured FOSS backend coverage, no measured FOSS frontend coverage and no verified KMS coverage metrics. Do not average these into one platform percentage or use a narrow scope to claim whole-system coverage. Add missing instrumentation and close gaps in the actual final source scope.

3. **Security and performance require executed acceptance evidence.** The central security matrix retains open or scoped dependency findings. A passing production-only audit is not a clean full dependency tree. The Website load harness exists, but its staging acceptance run is explicitly pending; the consolidated report does not establish a common accepted performance benchmark. Run the approved workload with agreed targets and infrastructure monitoring before recording capacity as achieved.

4. **Closed tickets sometimes overstate final acceptance.** MAD release/UAT/staging tickets are closed, but its current README explicitly says automatic deployment and several related checks were removed. FDS issue #70 is closed while the closeout matrix still marks external integration/staging criteria partial or pending. Resolve this through acceptance review and accurate scope, not by treating issue status as approval.

5. **Shared operational ownership remains a release dependency.** The reviewed Infrastructure tree contains its initial README/font assets rather than the shared deployment/migration implementation described by its README. Its two issues remain open. The nine supplied repositories also do not include the separately mentioned tests repository. Assign actual ownership for shared staging, schema deployment, recovery, monitoring and cross-system test execution, and identify where that evidence will live.

## Cross-repository quality/release position

This is an evidence snapshot as prepared on 2026-10-25, not a final pass/fail certificate.

| Repository | Reviewed position | Next acceptance evidence |
| --- | --- | --- |
| Hotel Website | develop backend/frontend/container/performance-tooling workflows passed; later docs-branch frontend run failed. Booking-quality code is present. | Final selected branch CI, actual staging smoke/rollback and accepted load results. |
| Manager Dashboard | Latest development workflow failed. Current guide removes automatic deployment and some earlier checks. | Diagnose the failing run and verify actual current acceptance scope. |
| Front Desk | develop backend/frontend/Docker workflows passed. Plan 20 still identifies external integration gaps. | Real auth/session, messaging/realtime and HTTPS staging workflow evidence. |
| Worker Management | develop CI passed; local realtime results are separately documented. | End-to-end producer/broker/consumer and deployed realtime evidence. |
| Food Ordering | Historical CI passed; rebuilt develop head is a different revision. Session/model foundations exist. | Fresh matching CI, missing guest-order functionality and sufficient measured coverage. |
| Kitchen Management | Selected feature CI runs passed. Reviewed develop implements persistence foundations. | Matching develop/release CI, coverage and complete order/printing integration. |
| API Gateway | Latest observed main quality workflow failed; HW-specific contract branch has passing runs. | Resolve final configuration mismatch and run the complete release contract suite. |
| Infrastructure | Initial tracked snapshot; no observed Actions runs; two open issues. | Reviewed shared deployment, schema and recovery implementation/evidence. |
| Documents | Final-named SRS/SDS and consolidated QA PDFs present. | Reconcile evidence to final revisions, obtain reviews and add agreed retrospective records. |

## Action Items

These actions are proposed from the current release review. The team has not yet confirmed named owners, deadlines or completion.

| Action | Owner | Due |
| --- | --- | --- |
| Agree how actual board iterations map to guideline Sprints 5-8; record dates without changing historical ticket evidence. | Project coordinator and supervisor liaison | Before retrospective submission |
| Freeze a release matrix of nine repository SHAs, configurations and required successful checks. | Release coordinator and all repository owners | Before the next release acceptance |
| Resolve failed Gateway/MAD/Website-branch checks and rerun FOSS/KMS on the selected current release heads. | Respective CI owners | Before release-candidate sign-off |
| Close FOSS/KMS coverage and functional gaps using isolated fixtures and linked acceptance tests. | FOSS/KMS and QA owners | Before claiming those requirements complete |
| Execute approved system integration, performance and security acceptance; retain actual outcomes. | QA, platform and subsystem owners | Before final demonstration approval |
| Verify deployed HTTPS, recovery/rollback, backups and monitoring against the agreed system environment. | Infrastructure/release owner | Before deployment acceptance |
| Reconcile QA PDFs and SRS/SDS status with final implementation and obtain real human review records. | Documentation owner and reviewers | Before final university submission |

## Metrics

| Required metric | Current evidence and limit |
| --- | --- |
| Issues planned | No separate Sprint 8 baseline is set up; some relevant tickets remain under Sprint 7. Not reconstructed as a numeric commitment. |
| Issues completed | Quality/release implementations and issue records exist, but no dated Sprint 8 acceptance population is established. No numeric sprint-completion claim. |
| Velocity | Not verifiable without an agreed Sprint 8 issue/point baseline. |
| CI pipeline pass rate | No full sprint denominator. Exact selected run results are linked in review-sources.md. |
| Test coverage | Historical per-service source scopes appear below; they are not one coordinated Sprint 8 release result. |

### Historical measured coverage used for reflection

Values below are copied from the existing central coverage report, which uses different commits and test scopes. They are not fresh measurements from this review, and later code may have different results.

| Measured scope | Lines | Branches | Qualification |
| --- | --- | --- | --- |
| HW backend | 91.82% | 85.17% | Historical pre-develop snapshot, predating complete booking/payment. |
| HW selected frontend TypeScript | 99.48% | 99.45% | Selected files; not whole frontend/UI coverage. |
| MAD backend / frontend | 97.08% / 96.02% | 90.69% / 91.25% | Separate measured scopes; cited overall run failed its audit gate. |
| FDS backend / frontend | 96.66% / 89.67% | 87.19% / 80.91% | Historical develop evidence. |
| FOSS backend develop / separate feature | 60.37% / 70.73% | 59.37% / 72.22% | Different historical revisions; below course expectation; not additive. |
| FOSS frontend | Not measured | Not measured | Coverage gap. |
| KMS backend / frontend | Not verified | Not verified | No measured metrics established by cited evidence. |
| WKMS backend / frontend | 94.80% / 94.90% | 88.19% / 85.55% | Historical source scopes; 33 backend tests skipped in cited evidence. |

## Supporting evidence and team follow-up

- Website: [hotel-website / frontend/CHANGELOG.md](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/frontend/CHANGELOG.md), [hotel-website / performance/README.md](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/performance/README.md), [hotel-website / backend/test/integration/booking-management.integration-spec.ts](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/backend/test/integration/booking-management.integration-spec.ts).
- Staff services: [front-desk / docs/plan20-closeout-evidence.md](https://github.com/forever-hotel/front-desk/blob/ca19caae9c69058e3bbf46d0e343758c230210f9/docs/plan20-closeout-evidence.md), [worker-management / backend/src/realtime/README.md](https://github.com/forever-hotel/worker-management/blob/0f91f1c9d6390c80c9f12f82b834fee9bf374104/backend/src/realtime/README.md), [manager-dashboard / README.md](https://github.com/forever-hotel/manager-dashboard/blob/75c40baa01115412ffbfb3b073bed29c728ae9cd/README.md).
- Central snapshots: [coverage report](../testing/coverage-sprint8.pdf), [security checklist](../security/owasp-checklist.pdf), [performance report](../testing/performance-report.pdf), [test register](../testing/test-register.pdf).
- Exact reviewed revisions, issue counts and CI run URLs: [source register](review-sources.md).

At the review, confirm the phase dates or explicitly approve this theme-based mapping, discuss the findings, assign owners and preserve the actual meeting record. Add real outcomes as acceptance checks complete. This report does not claim that every service has completed release hardening or that production readiness has been approved.
