# Sprint 6 Retrospective

**Project:** Forever Hotel Integrated Hotel Management System  
**Course:** SENG 34213, University of Kelaniya  
**Focus:** Core capabilities, persistence and shared contracts  
**Sprint dates:** 2026-09-23 to 2026-10-06 (project-board screenshot supplied by the team)  
**Prepared:** 2026-10-25 (Asia/Colombo)  
**Retrospective meeting date:** Not supplied; preparation date is not a meeting date  
**Attendees:** Attendance record not supplied; team confirmation required  
**Status:** Evidence-based retrospective draft; team discussion and approval not recorded

## Scope and evidence

This is a team-level reflection across the six business services and the API Gateway, Infrastructure and Documents repositories. It follows the guideline Appendix B.2 headings. Repository issues, selected source files, commit histories, recent CI metadata and central QA reports were reviewed. Tests, load runs and deployments were not executed during this document review.

The board screenshot confirms two-week windows for Sprints 5-7. The guideline Section 2.2 describes four-week development sprints. The team should record or confirm the reason for this scheduling difference with the supervisor. Work is attributed to a date window only where timestamps support it; a feature title or closed issue alone does not prove acceptance in that sprint.

## Went Well

1. **Front Desk moved beyond the initial prototype into transactional hotel operations.** Closure records in this window cover reservation persistence, walk-in bookings, transactional check-in, room status, room reassignment, running folio and checkout core. Separating state changes and auditing into explicit workflows provides a stronger foundation than updating room or booking status directly from a screen.

2. **Worker Management expanded its operational and quality foundations.** The records cover worker authentication, task lifecycle, detail/shift APIs, escalation, gateway readiness, shared database integration and stronger frontend/backend coverage gates. Keeping queue, ownership and active-task limits on the server is a useful response to concurrent staff actions.

3. **The team documented important cross-service boundaries.** Documents recorded the subsystem repository ADR, database/realtime decisions, folder standards and KMS contract inventory. Food Ordering and Kitchen Management both recorded the FOSS-KMS ownership question. Naming the authoritative owner is valuable because it prevents two services from independently changing the same order or task state.

4. **Core development broadened across the remaining services.** Manager Dashboard recorded booking analytics and occupancy data work. Hotel Website closed its frontend scaffold, backend foundation, CI/configuration and API-contract documentation issues. Kitchen Management recorded its scaffold, configuration, Docker, health, CI/security and Jest-alignment work. The contribution was substantial, even though services were at different stages.

## To Improve

1. **Shared database responsibility was still fragmented.** The documents assign migration/ownership responsibilities, while service repositories contain local migrations, schema copies or manual setup directions. The Infrastructure snapshot remains an initial repository with no observed issue closures. This is a coordination risk, not proof of a live database failure. Agree one versioned migration path, deployment owner and disposable test fixture, then record any permitted service-local exceptions.

2. **Contracts arrived alongside features rather than ahead of all consumers.** The KMS inventory still contains proposed operations and details to freeze. Front Desk already had FOSS activation, printing, messaging and folio-related work in this period. Consumer implementations need approved paths, schemas, failure handling and idempotency rules before a contract can be called integrated.

3. **Sprint labels drifted from the actual calendar.** Several Hotel Website foundation issues assigned to Sprint 5 closed in the Sprint 6 date window. Preserve that carry-over instead of rewriting the historical plan. Scope should be rebalanced when foundational dependencies consume a later sprint.

4. **A feature's existence and its release acceptance require different evidence.** CI configuration, a PostgreSQL repository or a container definition establishes an available implementation. It does not alone establish a shared staging workflow. Each cross-service acceptance should identify the exact producer, consumer, configuration and observed result.

## Action Items

The following actions are proposed from the evidence review. They have not been represented as historical team commitments.

| Action | Owner | Due |
| --- | --- | --- |
| Publish a schema/version ownership matrix and a reviewed migration/deployment procedure. | Infrastructure/DB owner and service owners | Before the next shared schema change |
| Freeze FOSS-KMS, FDS-FOSS, FDS-WKMS and folio/printing contracts with examples and failure cases. | Affected producer and consumer owners | Before the next integration acceptance |
| Use an isolated database and broker fixture for cross-service regression checks. | Integration-test owner | Before shared workflow sign-off |
| Record the foundation carry-over and adjust remaining scope using the real board dates. | Project coordinator | Next sprint-planning review |
| Link each core workflow to requirements, tests, revision and acceptance outcome. | All subsystem owners | Before the next sprint review |

## Metrics

| Required metric | Evidence available |
| --- | --- |
| Issues planned | Not verified: no frozen planning baseline or accessible project-board export. |
| Issues completed against sprint commitment | Not verified: issue closure is not acceptance or proof of sprint assignment. |
| Velocity | Not verified: no agreed, completed story-point baseline. Hours and commits are not substituted. |
| CI pipeline pass rate | Not verified for this sprint: the review used a recent 30-run sample per repository, not the complete sprint run population. |
| Test coverage at sprint close | No matching whole-system sprint-close snapshot was verified. Later coverage is listed separately in review-sources.md. |

**Observed issue-closure events in this date window: 51.** The table below counts non-PR GitHub issues whose latest `closed_at` falls within the supplied dates, converted to Asia/Colombo time. It covers the nine supplied repositories only, includes duplicate/epic issues where present, and is not sprint velocity, acceptance, or a historical board reconstruction. Reopened history was not replayed.

| Repository | Issues with a closure timestamp in the window | Example source links |
| --- | --- | --- |
| hotel-website | 5 | [#18](https://github.com/forever-hotel/hotel-website/issues/18), [#12](https://github.com/forever-hotel/hotel-website/issues/12), [#11](https://github.com/forever-hotel/hotel-website/issues/11), [#9](https://github.com/forever-hotel/hotel-website/issues/9); remaining IDs in review-sources.md |
| manager-dashboard | 2 | [#24](https://github.com/forever-hotel/manager-dashboard/issues/24), [#25](https://github.com/forever-hotel/manager-dashboard/issues/25) |
| front-desk | 17 | [#55](https://github.com/forever-hotel/front-desk/issues/55), [#53](https://github.com/forever-hotel/front-desk/issues/53), [#51](https://github.com/forever-hotel/front-desk/issues/51), [#49](https://github.com/forever-hotel/front-desk/issues/49); remaining IDs in review-sources.md |
| worker-management | 10 | [#39](https://github.com/forever-hotel/worker-management/issues/39), [#38](https://github.com/forever-hotel/worker-management/issues/38), [#37](https://github.com/forever-hotel/worker-management/issues/37), [#33](https://github.com/forever-hotel/worker-management/issues/33); remaining IDs in review-sources.md |
| food-ordering | 2 | [#12](https://github.com/forever-hotel/food-ordering/issues/12), [#10](https://github.com/forever-hotel/food-ordering/issues/10) |
| kitchen-management | 11 | [#20](https://github.com/forever-hotel/kitchen-management/issues/20), [#19](https://github.com/forever-hotel/kitchen-management/issues/19), [#18](https://github.com/forever-hotel/kitchen-management/issues/18), [#16](https://github.com/forever-hotel/kitchen-management/issues/16); remaining IDs in review-sources.md |
| api-gateway | 0 | None observed |
| infra | 0 | None observed |
| documents | 4 | [#13](https://github.com/forever-hotel/documents/issues/13), [#11](https://github.com/forever-hotel/documents/issues/11), [#9](https://github.com/forever-hotel/documents/issues/9), [#7](https://github.com/forever-hotel/documents/issues/7) |

The complete closure-ID list and reviewed revisions are in [the source register](review-sources.md).

## Supporting evidence and team follow-up

- Front Desk core workflows: [front-desk #35](https://github.com/forever-hotel/front-desk/issues/35), [front-desk #39](https://github.com/forever-hotel/front-desk/issues/39), [front-desk #45](https://github.com/forever-hotel/front-desk/issues/45), [front-desk #47](https://github.com/forever-hotel/front-desk/issues/47), [front-desk #49](https://github.com/forever-hotel/front-desk/issues/49).
- Worker operations and quality: [worker-management #10](https://github.com/forever-hotel/worker-management/issues/10), [worker-management #27](https://github.com/forever-hotel/worker-management/issues/27), [worker-management #29](https://github.com/forever-hotel/worker-management/issues/29), [worker-management #31](https://github.com/forever-hotel/worker-management/issues/31), [worker-management #38](https://github.com/forever-hotel/worker-management/issues/38).
- Other foundations and core reads: [hotel-website #12](https://github.com/forever-hotel/hotel-website/issues/12), [manager-dashboard #24](https://github.com/forever-hotel/manager-dashboard/issues/24), [manager-dashboard #25](https://github.com/forever-hotel/manager-dashboard/issues/25), [kitchen-management #10](https://github.com/forever-hotel/kitchen-management/issues/10), [food-ordering #10](https://github.com/forever-hotel/food-ordering/issues/10).
- Shared boundaries: [documents / adr/ADR-003-database-schema-and-ownership.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/adr/ADR-003-database-schema-and-ownership.md), [documents / integration/kms-contract-inventory.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/integration/kms-contract-inventory.md), [documents / integration/front-desk-contract-inventory.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/integration/front-desk-contract-inventory.md).

The team should confirm the meeting record, whether the proposed contracts were accepted, which workflows actually reached a shared test environment, and the original sprint estimates. The source register preserves the observed closures without treating them as a complete acceptance register.
