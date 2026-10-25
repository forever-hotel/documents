# Retrospective Review Source Register

**Prepared:** 2026-10-25, Asia/Colombo. This register supports four retrospective drafts. It records the evidence reviewed, not a new test execution or a system acceptance sign-off.

## Method and limits

- Read-only review snapshots were cloned from the nine repositories supplied by the user. The selected branches/revisions below are fixed for these reports. No remote repository was modified.
- Root guides, selected source modules/controllers, schema/persistence structures, workflows, tests, changelogs, integration contracts and commit histories were inspected. This was a retrospective evidence review, not an exhaustive line-by-line audit of every file.
- GitHub REST issue/PR records, milestones, up to the most recent 30 Actions runs per repository, and published release lists were read. Pull requests were excluded from issue counts. There were no published releases in the observed release lists; this does not prove that no Git tags exist.
- Project-board fields could not be read because the available authentication lacks read:project permission. No permissions were changed. Sprint 5-7 dates come from the screenshot supplied in chat, interpreted in the project's 2026 context. Sprint 8 has no confirmed board dates.
- The team said some services already performed Sprint 8 quality/release work under Sprint 7. Sprint 8 is therefore a theme-based current work review, not a fabricated dated iteration.
- Report preparation dates are not meeting dates. Attendance, supervisor approval, original commitments, story points and historical sprint CI pass rates were not supplied or inferred.
- Issue closure counts use the current issue record's latest closed_at timestamp, converted to Asia/Colombo, within inclusive date boundaries. They do not replay reopened history or certify acceptance. Duplicates/epics may appear. Counts describe this nine-repository sample, not all possible organisation activity.
- Existing central PDFs contain historical snapshots. Current CI metadata is kept separate. No test suites, dependency scans, provider payments, load tests, database changes, deployments or live UAT were performed for these reports.

## Selected repository snapshots and issue states

All issue states are as observed during preparation, not as of an earlier sprint end. Snapshot links pin full commits; displayed SHA prefixes are for readability.

| Repository | Selected branch | Reviewed revision | Non-PR issues | Open | Closed |
| --- | --- | --- | --- | --- | --- |
| hotel-website | develop | [3551505c8a](https://github.com/forever-hotel/hotel-website/tree/3551505c8a053c08c6da51013826b831d0e7dfa5) | 46 | 29 | 17 |
| manager-dashboard | development | [75c40baa01](https://github.com/forever-hotel/manager-dashboard/tree/75c40baa01115412ffbfb3b073bed29c728ae9cd) | 64 | 7 | 57 |
| front-desk | develop | [ca19caae9c](https://github.com/forever-hotel/front-desk/tree/ca19caae9c69058e3bbf46d0e343758c230210f9) | 35 | 2 | 33 |
| worker-management | develop | [0f91f1c9d6](https://github.com/forever-hotel/worker-management/tree/0f91f1c9d6390c80c9f12f82b834fee9bf374104) | 26 | 0 | 26 |
| food-ordering | develop | [defae7b4a1](https://github.com/forever-hotel/food-ordering/tree/defae7b4a180f3f579c126781431125a3f5bd420) | 18 | 2 | 16 |
| kitchen-management | develop | [89087002ac](https://github.com/forever-hotel/kitchen-management/tree/89087002ac2df8f07b6140c00111979c75bc2799) | 19 | 2 | 17 |
| api-gateway | main | [2727054cfd](https://github.com/forever-hotel/api-gateway/tree/2727054cfd30b53e5f947ef5da17f407a358fba5) | 2 | 1 | 1 |
| infra | develop | [6e3f84d0d9](https://github.com/forever-hotel/infra/tree/6e3f84d0d9bf1d53af3971cf5da525eff74e6fb2) | 2 | 2 | 0 |
| documents | develop | [59334b6a2f](https://github.com/forever-hotel/documents/tree/59334b6a2f5535366f53c4042077c9ada3ab8d9b) | 10 | 0 | 10 |


Manager Dashboard's active development branch was reviewed explicitly; its default main branch points at an older revision. Website's selected develop snapshot is separate from later local/documentation/deployment branches. Gateway main is separate from its HW contract-test branch. The rebuilt FOSS develop baseline is separate from the historical CI revisions.

## Issue closure IDs by supplied sprint window

This is the complete count basis for the observed closure tables. A link may currently show a later updated title or state.

| Repository | Sprint 5: Sep 9-22 | Sprint 6: Sep 23-Oct 6 | Sprint 7: Oct 7-20 |
| --- | --- | --- | --- |
| hotel-website | [#5](https://github.com/forever-hotel/hotel-website/issues/5) | [#18](https://github.com/forever-hotel/hotel-website/issues/18), [#12](https://github.com/forever-hotel/hotel-website/issues/12), [#11](https://github.com/forever-hotel/hotel-website/issues/11), [#9](https://github.com/forever-hotel/hotel-website/issues/9), [#6](https://github.com/forever-hotel/hotel-website/issues/6) | [#36](https://github.com/forever-hotel/hotel-website/issues/36), [#35](https://github.com/forever-hotel/hotel-website/issues/35), [#33](https://github.com/forever-hotel/hotel-website/issues/33), [#29](https://github.com/forever-hotel/hotel-website/issues/29), [#27](https://github.com/forever-hotel/hotel-website/issues/27), [#25](https://github.com/forever-hotel/hotel-website/issues/25), [#21](https://github.com/forever-hotel/hotel-website/issues/21), [#24](https://github.com/forever-hotel/hotel-website/issues/24), [#20](https://github.com/forever-hotel/hotel-website/issues/20) |
| manager-dashboard | None | [#24](https://github.com/forever-hotel/manager-dashboard/issues/24), [#25](https://github.com/forever-hotel/manager-dashboard/issues/25) | [#63](https://github.com/forever-hotel/manager-dashboard/issues/63), [#61](https://github.com/forever-hotel/manager-dashboard/issues/61), [#59](https://github.com/forever-hotel/manager-dashboard/issues/59), [#56](https://github.com/forever-hotel/manager-dashboard/issues/56), [#54](https://github.com/forever-hotel/manager-dashboard/issues/54), [#53](https://github.com/forever-hotel/manager-dashboard/issues/53), [#50](https://github.com/forever-hotel/manager-dashboard/issues/50), [#48](https://github.com/forever-hotel/manager-dashboard/issues/48), [#46](https://github.com/forever-hotel/manager-dashboard/issues/46), [#45](https://github.com/forever-hotel/manager-dashboard/issues/45), [#43](https://github.com/forever-hotel/manager-dashboard/issues/43), [#39](https://github.com/forever-hotel/manager-dashboard/issues/39), [#38](https://github.com/forever-hotel/manager-dashboard/issues/38), [#36](https://github.com/forever-hotel/manager-dashboard/issues/36), [#31](https://github.com/forever-hotel/manager-dashboard/issues/31), [#29](https://github.com/forever-hotel/manager-dashboard/issues/29) |
| front-desk | [#17](https://github.com/forever-hotel/front-desk/issues/17), [#13](https://github.com/forever-hotel/front-desk/issues/13), [#12](https://github.com/forever-hotel/front-desk/issues/12), [#11](https://github.com/forever-hotel/front-desk/issues/11), [#10](https://github.com/forever-hotel/front-desk/issues/10), [#9](https://github.com/forever-hotel/front-desk/issues/9), [#7](https://github.com/forever-hotel/front-desk/issues/7), [#5](https://github.com/forever-hotel/front-desk/issues/5), [#3](https://github.com/forever-hotel/front-desk/issues/3), [#1](https://github.com/forever-hotel/front-desk/issues/1) | [#55](https://github.com/forever-hotel/front-desk/issues/55), [#53](https://github.com/forever-hotel/front-desk/issues/53), [#51](https://github.com/forever-hotel/front-desk/issues/51), [#49](https://github.com/forever-hotel/front-desk/issues/49), [#47](https://github.com/forever-hotel/front-desk/issues/47), [#45](https://github.com/forever-hotel/front-desk/issues/45), [#43](https://github.com/forever-hotel/front-desk/issues/43), [#41](https://github.com/forever-hotel/front-desk/issues/41), [#39](https://github.com/forever-hotel/front-desk/issues/39), [#37](https://github.com/forever-hotel/front-desk/issues/37), [#35](https://github.com/forever-hotel/front-desk/issues/35), [#33](https://github.com/forever-hotel/front-desk/issues/33), [#31](https://github.com/forever-hotel/front-desk/issues/31), [#29](https://github.com/forever-hotel/front-desk/issues/29), [#25](https://github.com/forever-hotel/front-desk/issues/25), [#23](https://github.com/forever-hotel/front-desk/issues/23), [#21](https://github.com/forever-hotel/front-desk/issues/21) | [#67](https://github.com/forever-hotel/front-desk/issues/67), [#65](https://github.com/forever-hotel/front-desk/issues/65), [#63](https://github.com/forever-hotel/front-desk/issues/63), [#59](https://github.com/forever-hotel/front-desk/issues/59), [#57](https://github.com/forever-hotel/front-desk/issues/57) |
| worker-management | [#23](https://github.com/forever-hotel/worker-management/issues/23), [#19](https://github.com/forever-hotel/worker-management/issues/19), [#18](https://github.com/forever-hotel/worker-management/issues/18), [#17](https://github.com/forever-hotel/worker-management/issues/17), [#15](https://github.com/forever-hotel/worker-management/issues/15), [#13](https://github.com/forever-hotel/worker-management/issues/13), [#8](https://github.com/forever-hotel/worker-management/issues/8), [#7](https://github.com/forever-hotel/worker-management/issues/7), [#5](https://github.com/forever-hotel/worker-management/issues/5), [#3](https://github.com/forever-hotel/worker-management/issues/3), [#1](https://github.com/forever-hotel/worker-management/issues/1) | [#39](https://github.com/forever-hotel/worker-management/issues/39), [#38](https://github.com/forever-hotel/worker-management/issues/38), [#37](https://github.com/forever-hotel/worker-management/issues/37), [#33](https://github.com/forever-hotel/worker-management/issues/33), [#31](https://github.com/forever-hotel/worker-management/issues/31), [#29](https://github.com/forever-hotel/worker-management/issues/29), [#27](https://github.com/forever-hotel/worker-management/issues/27), [#25](https://github.com/forever-hotel/worker-management/issues/25), [#11](https://github.com/forever-hotel/worker-management/issues/11), [#10](https://github.com/forever-hotel/worker-management/issues/10) | [#46](https://github.com/forever-hotel/worker-management/issues/46), [#44](https://github.com/forever-hotel/worker-management/issues/44), [#43](https://github.com/forever-hotel/worker-management/issues/43) |
| food-ordering | [#8](https://github.com/forever-hotel/food-ordering/issues/8), [#6](https://github.com/forever-hotel/food-ordering/issues/6), [#4](https://github.com/forever-hotel/food-ordering/issues/4) | [#12](https://github.com/forever-hotel/food-ordering/issues/12), [#10](https://github.com/forever-hotel/food-ordering/issues/10) | [#31](https://github.com/forever-hotel/food-ordering/issues/31), [#28](https://github.com/forever-hotel/food-ordering/issues/28), [#25](https://github.com/forever-hotel/food-ordering/issues/25), [#23](https://github.com/forever-hotel/food-ordering/issues/23), [#21](https://github.com/forever-hotel/food-ordering/issues/21), [#18](https://github.com/forever-hotel/food-ordering/issues/18), [#17](https://github.com/forever-hotel/food-ordering/issues/17), [#15](https://github.com/forever-hotel/food-ordering/issues/15), [#13](https://github.com/forever-hotel/food-ordering/issues/13) |
| kitchen-management | None | [#20](https://github.com/forever-hotel/kitchen-management/issues/20), [#19](https://github.com/forever-hotel/kitchen-management/issues/19), [#18](https://github.com/forever-hotel/kitchen-management/issues/18), [#16](https://github.com/forever-hotel/kitchen-management/issues/16), [#14](https://github.com/forever-hotel/kitchen-management/issues/14), [#12](https://github.com/forever-hotel/kitchen-management/issues/12), [#10](https://github.com/forever-hotel/kitchen-management/issues/10), [#7](https://github.com/forever-hotel/kitchen-management/issues/7), [#5](https://github.com/forever-hotel/kitchen-management/issues/5), [#3](https://github.com/forever-hotel/kitchen-management/issues/3), [#1](https://github.com/forever-hotel/kitchen-management/issues/1) | [#34](https://github.com/forever-hotel/kitchen-management/issues/34), [#31](https://github.com/forever-hotel/kitchen-management/issues/31), [#29](https://github.com/forever-hotel/kitchen-management/issues/29), [#27](https://github.com/forever-hotel/kitchen-management/issues/27), [#25](https://github.com/forever-hotel/kitchen-management/issues/25), [#23](https://github.com/forever-hotel/kitchen-management/issues/23) |
| api-gateway | None | None | [#2](https://github.com/forever-hotel/api-gateway/issues/2) |
| infra | None | None | None |
| documents | [#3](https://github.com/forever-hotel/documents/issues/3), [#1](https://github.com/forever-hotel/documents/issues/1) | [#13](https://github.com/forever-hotel/documents/issues/13), [#11](https://github.com/forever-hotel/documents/issues/11), [#9](https://github.com/forever-hotel/documents/issues/9), [#7](https://github.com/forever-hotel/documents/issues/7) | None |


## Selected recent CI observations

These are exact workflow-run metadata observations. Different branches and commits are intentionally separate. A success here does not establish coverage, staging acceptance, system integration or performance beyond the workflow's actual checks. Failure causes were not diagnosed from full logs during this review.

| Repository | Workflow / branch | Revision | Conclusion | Run |
| --- | --- | --- | --- | --- |
| hotel-website | Backend CI / docs/final-deliverables-reports | 9e58e8e7 | success | [40194853348](https://github.com/forever-hotel/hotel-website/actions/runs/40194853348) |
| hotel-website | Frontend CI / docs/final-deliverables-reports | 9e58e8e7 | failure | [40194853198](https://github.com/forever-hotel/hotel-website/actions/runs/40194853198) |
| hotel-website | Performance tooling / develop | 3551505c | success | [40193518100](https://github.com/forever-hotel/hotel-website/actions/runs/40193518100) |
| hotel-website | Backend CI / develop | 3551505c | success | [40193518080](https://github.com/forever-hotel/hotel-website/actions/runs/40193518080) |
| hotel-website | Frontend CI / develop | 3551505c | success | [40193518066](https://github.com/forever-hotel/hotel-website/actions/runs/40193518066) |
| hotel-website | Container checks / develop | 3551505c | success | [40193518058](https://github.com/forever-hotel/hotel-website/actions/runs/40193518058) |
| manager-dashboard | DDP quality gates / development | 75c40baa | failure | [40190478572](https://github.com/forever-hotel/manager-dashboard/actions/runs/40190478572) |
| manager-dashboard | DDP quality gates / development | bba65a07 | failure | [40137091073](https://github.com/forever-hotel/manager-dashboard/actions/runs/40137091073) |
| manager-dashboard | DDP quality gates / development | caef8822 | success | [39847903811](https://github.com/forever-hotel/manager-dashboard/actions/runs/39847903811) |
| front-desk | Backend CI Pipeline / develop | ca19caae | success | [40182596188](https://github.com/forever-hotel/front-desk/actions/runs/40182596188) |
| front-desk | Front Desk Docker CI / develop | ca19caae | success | [40182596184](https://github.com/forever-hotel/front-desk/actions/runs/40182596184) |
| front-desk | Frontend CI Pipeline / develop | ca19caae | success | [40182596177](https://github.com/forever-hotel/front-desk/actions/runs/40182596177) |
| worker-management | WKMS CI / develop | 0f91f1c9 | success | [40174879698](https://github.com/forever-hotel/worker-management/actions/runs/40174879698) |
| food-ordering | FOSS CI / feature/34-booking-room-verification | 343b7d00 | success | [38419260802](https://github.com/forever-hotel/food-ordering/actions/runs/38419260802) |
| food-ordering | FOSS CI / feature/34-booking-room-verification | 343b7d00 | success | [38417506181](https://github.com/forever-hotel/food-ordering/actions/runs/38417506181) |
| food-ordering | FOSS CI / develop | 17af0841 | success | [38381405941](https://github.com/forever-hotel/food-ordering/actions/runs/38381405941) |
| kitchen-management | KMS CI / feature/34-ant-design-frontend-foundation | 05804861 | success | [38539653626](https://github.com/forever-hotel/kitchen-management/actions/runs/38539653626) |
| kitchen-management | KMS CI / feature/33-kot-print-persistence | 72966779 | success | [38539644913](https://github.com/forever-hotel/kitchen-management/actions/runs/38539644913) |
| kitchen-management | KMS CI / feature/31-food-order-persistence | 930be4da | success | [38539637070](https://github.com/forever-hotel/kitchen-management/actions/runs/38539637070) |
| kitchen-management | KMS CI / feature/29-allergen-dietary-persistence | c60619ff | success | [38539628511](https://github.com/forever-hotel/kitchen-management/actions/runs/38539628511) |
| api-gateway | Gateway quality gates / main | 2727054c | failure | [40190484564](https://github.com/forever-hotel/api-gateway/actions/runs/40190484564) |
| api-gateway | Gateway checks / test/184-hotel-booking-gateway-contracts | 1bd396e7 | success | [40146585514](https://github.com/forever-hotel/api-gateway/actions/runs/40146585514) |


No Actions runs were observed for Infrastructure. Documents has review-assistant runs; these are not treated as application build/test gates. FOSS and KMS entries above include historical or feature runs and are not attributed to their selected release heads without a match.

## Primary implementation and documentation references

### hotel-website

- [hotel-website / issue.md](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/issue.md)
- [hotel-website / frontend/CHANGELOG.md](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/frontend/CHANGELOG.md)
- [hotel-website / backend/CHANGELOG.md](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/backend/CHANGELOG.md)
- [hotel-website / documents/api/payhere-checkout.md](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/documents/api/payhere-checkout.md)
- [hotel-website / backend/test/integration/booking-management.integration-spec.ts](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/backend/test/integration/booking-management.integration-spec.ts)
- [hotel-website / performance/README.md](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/performance/README.md)

### manager-dashboard

- [manager-dashboard / README.md](https://github.com/forever-hotel/manager-dashboard/blob/75c40baa01115412ffbfb3b073bed29c728ae9cd/README.md)
- [manager-dashboard / backend/src/app.module.ts](https://github.com/forever-hotel/manager-dashboard/blob/75c40baa01115412ffbfb3b073bed29c728ae9cd/backend/src/app.module.ts)
- [manager-dashboard / .github/workflows/ci.yml](https://github.com/forever-hotel/manager-dashboard/blob/75c40baa01115412ffbfb3b073bed29c728ae9cd/.github/workflows/ci.yml)

### front-desk

- [front-desk / README.md](https://github.com/forever-hotel/front-desk/blob/ca19caae9c69058e3bbf46d0e343758c230210f9/README.md)
- [front-desk / backend/src/app.module.ts](https://github.com/forever-hotel/front-desk/blob/ca19caae9c69058e3bbf46d0e343758c230210f9/backend/src/app.module.ts)
- [front-desk / docs/plan20-closeout-evidence.md](https://github.com/forever-hotel/front-desk/blob/ca19caae9c69058e3bbf46d0e343758c230210f9/docs/plan20-closeout-evidence.md)
- [front-desk / docs/central-auth-integration-contract.md](https://github.com/forever-hotel/front-desk/blob/ca19caae9c69058e3bbf46d0e343758c230210f9/docs/central-auth-integration-contract.md)

### worker-management

- [worker-management / CHANGELOG.md](https://github.com/forever-hotel/worker-management/blob/0f91f1c9d6390c80c9f12f82b834fee9bf374104/CHANGELOG.md)
- [worker-management / backend/src/tasks/task.module.ts](https://github.com/forever-hotel/worker-management/blob/0f91f1c9d6390c80c9f12f82b834fee9bf374104/backend/src/tasks/task.module.ts)
- [worker-management / backend/src/messaging/messaging.module.ts](https://github.com/forever-hotel/worker-management/blob/0f91f1c9d6390c80c9f12f82b834fee9bf374104/backend/src/messaging/messaging.module.ts)
- [worker-management / backend/src/realtime/README.md](https://github.com/forever-hotel/worker-management/blob/0f91f1c9d6390c80c9f12f82b834fee9bf374104/backend/src/realtime/README.md)

### food-ordering

- [food-ordering / README.md](https://github.com/forever-hotel/food-ordering/blob/defae7b4a180f3f579c126781431125a3f5bd420/README.md)
- [food-ordering / backend/src/app.module.ts](https://github.com/forever-hotel/food-ordering/blob/defae7b4a180f3f579c126781431125a3f5bd420/backend/src/app.module.ts)
- [food-ordering / backend/src/modules/guest-sessions/repositories/foss-session.repository.ts](https://github.com/forever-hotel/food-ordering/blob/defae7b4a180f3f579c126781431125a3f5bd420/backend/src/modules/guest-sessions/repositories/foss-session.repository.ts)
- [food-ordering / .github/workflows/ci.yml](https://github.com/forever-hotel/food-ordering/blob/defae7b4a180f3f579c126781431125a3f5bd420/.github/workflows/ci.yml)

### kitchen-management

- [kitchen-management / README.md](https://github.com/forever-hotel/kitchen-management/blob/89087002ac2df8f07b6140c00111979c75bc2799/README.md)
- [kitchen-management / backend/src/app.module.ts](https://github.com/forever-hotel/kitchen-management/blob/89087002ac2df8f07b6140c00111979c75bc2799/backend/src/app.module.ts)
- [kitchen-management / backend/src/modules/orders/repositories/food-order.repository.ts](https://github.com/forever-hotel/kitchen-management/blob/89087002ac2df8f07b6140c00111979c75bc2799/backend/src/modules/orders/repositories/food-order.repository.ts)
- [kitchen-management / .github/workflows/ci.yml](https://github.com/forever-hotel/kitchen-management/blob/89087002ac2df8f07b6140c00111979c75bc2799/.github/workflows/ci.yml)

### api-gateway

- [api-gateway / README.md](https://github.com/forever-hotel/api-gateway/blob/2727054cfd30b53e5f947ef5da17f407a358fba5/README.md)
- [api-gateway / config/routes.json](https://github.com/forever-hotel/api-gateway/blob/2727054cfd30b53e5f947ef5da17f407a358fba5/config/routes.json)
- [api-gateway / package.json](https://github.com/forever-hotel/api-gateway/blob/2727054cfd30b53e5f947ef5da17f407a358fba5/package.json)

### infra

- [infra / README.md](https://github.com/forever-hotel/infra/blob/6e3f84d0d9bf1d53af3971cf5da525eff74e6fb2/README.md)

### documents

- [documents / adr/ADR-001-subsystem-oriented-repository-architecture.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/adr/ADR-001-subsystem-oriented-repository-architecture.md)
- [documents / adr/ADR-003-database-schema-and-ownership.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/adr/ADR-003-database-schema-and-ownership.md)
- [documents / adr/ADR-004-realtime-communication-ownership.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/adr/ADR-004-realtime-communication-ownership.md)
- [documents / integration/front-desk-contract-inventory.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/integration/front-desk-contract-inventory.md)
- [documents / integration/kms-contract-inventory.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/integration/kms-contract-inventory.md)
- [documents / README.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/README.md)
- [documents / testing/coverage-sprint8.pdf](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/testing/coverage-sprint8.pdf)
- [documents / testing/performance-report.pdf](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/testing/performance-report.pdf)
- [documents / security/owasp-checklist.pdf](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/security/owasp-checklist.pdf)
- [documents / srs/srs-final.pdf](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/srs/srs-final.pdf)
- [documents / sds/sds-final.pdf](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/sds/sds-final.pdf)

## Guideline and dates

The user supplied SENG34213_System_Development_Project (1).pdf. Section 10.1 (printed page 40; PDF page 41) lists Retrospective Reports (all sprints), Markdown, documents/retrospectives/. Appendix B.2 (printed pages 45-46; PDF pages 46-47) provides the template and mentions GitHub Wiki. Section 2.2 (printed page 6) describes Sprints 5-8 as four-week phases. Keep the Markdown sources here and publish the agreed content to the Wiki when the team authorizes it; these drafts have not been published.

The screenshot supplied by the user gives Sprint 5 Sep 9-22, Sprint 6 Sep 23-Oct 6 and Sprint 7 Oct 7-20. The year 2026 follows the project/course context. The user confirmed Sprint 8 was not separately set up and that some relevant work was placed under Sprint 7. No Sprint 8 calendar interval has been invented.

## Review scope remaining

Confirm actual meeting records and action owners, provide the original planning/point baseline if available, reconcile the sprint-duration difference, and agree the Sprint 8 phase mapping. The separately mentioned tests repository was not in the nine supplied repository URLs and was not reviewed. Hosted production systems, private team conversations and client/supervisor approvals were not reviewed.
