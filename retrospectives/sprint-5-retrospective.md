# Sprint 5 Retrospective

**Project:** Forever Hotel Integrated Hotel Management System  
**Course:** SENG 34213, University of Kelaniya  
**Focus:** Foundation and development environment  
**Sprint dates:** 2026-09-09 to 2026-09-22 (project-board screenshot supplied by the team)  
**Prepared:** 2026-10-25 (Asia/Colombo)  
**Retrospective meeting date:** Not supplied; preparation date is not a meeting date  
**Attendees:** Attendance record not supplied; team confirmation required  
**Status:** Evidence-based retrospective draft; team discussion and approval not recorded

## Scope and evidence

This is a team-level reflection across the six business services and the API Gateway, Infrastructure and Documents repositories. It follows the guideline Appendix B.2 headings. Repository issues, selected source files, commit histories, recent CI metadata and central QA reports were reviewed. Tests, load runs and deployments were not executed during this document review.

The board screenshot confirms two-week windows for Sprints 5-7. The guideline Section 2.2 describes four-week development sprints. The team should record or confirm the reason for this scheduling difference with the supervisor. Work is attributed to a date window only where timestamps support it; a feature title or closed issue alone does not prove acceptance in that sprint.

## Went Well

1. **The team established usable development foundations across several services.** Front Desk issues covered the backend scaffold, health endpoint, early booking search and its navigation/search screens. Worker Management recorded its scaffold, CI, PostgreSQL connectivity, task queue and atomic task-claim work. Having these foundations available allowed later features to build on consistent service boundaries.

2. **Quality work began alongside feature work.** Food Ordering closed its coding-standard, CI and dependency-remediation issues during this window. Worker Management also recorded CI work. This shows the value of treating build and test automation as development work rather than leaving it entirely to release preparation.

3. **Shared decisions started to acquire a written home.** The Documents repository recorded database review and the Ant Design standard. Hotel Website recorded its issue-board and branch-rules ticket. These records provided a basis for coordinating separately owned repositories.

4. **Early screens made the intended workflows concrete.** Worker task queue, My Tasks, task details and shift-dashboard issues, together with Front Desk booking-search screens, gave the team concrete interfaces to discuss. This was useful progress, provided screen completion was kept distinct from real backend and hotel-workflow acceptance.

## To Improve

1. **Foundation readiness was uneven across repositories.** Kitchen Management has no observed issue closures in this window, and its scaffold/setup closures appear later. Infrastructure has no observed closure events. These observations do not prove inactivity, but they show that one shared foundation milestone cannot be inferred from progress in the more active services. Next time, use a short common checklist with a runnable service, health contract, isolated test database and CI evidence for every owner.

2. **A prototype needs an explicit replacement plan.** Front Desk's early booking-search ticket explicitly used an in-memory repository; later persistence work was separate. That is a useful way to unblock UI work, but a prototype should be labelled and linked to its production persistence/integration dependency so that a demonstration is not mistaken for completed hotel functionality.

3. **Planning and closure records need to preserve the original commitment.** Current Hotel Website milestones show later-closed foundation tickets under Sprint 5. Those tickets cannot be counted as accepted in September simply because their current milestone says Sprint 5. Freeze each sprint's initial issue IDs, estimates and acceptance evidence, then record carry-over explicitly.

4. **Shared standards need more than a document or closed ticket.** Current ADRs include proposed decisions and pending review records. An architecture record should state the actual review outcome and its owner. A closed documentation issue should not silently convert a proposed decision into an accepted architecture.

## Action Items

These are recommendations from this reconstruction, not claims of historically agreed or completed actions. Owners are proposed roles; the team must assign named people and real due dates.

| Action | Owner | Due |
| --- | --- | --- |
| Record the actual sprint calendar, original issue commitment and carry-over policy. | Project coordinator | Next team review |
| Verify one setup/health/CI checklist for all six services, Gateway and Infrastructure. | Service owners and platform owner | Before the next integrated demonstration |
| Link every in-memory/demo adapter to its replacement and acceptance ticket. | Front Desk owner and other adapter owners | Before declaring persistence complete |
| Confirm ADR status and record peer/supervisor review where required. | Architecture/documentation owner | Before the next architecture-dependent change |
| Attach build, test and dependency-scan evidence to the exact reviewed revision. | CI owners | Before the next foundation acceptance review |

## Metrics

| Required metric | Evidence available |
| --- | --- |
| Issues planned | Not verified: no frozen planning baseline or accessible project-board export. |
| Issues completed against sprint commitment | Not verified: issue closure is not acceptance or proof of sprint assignment. |
| Velocity | Not verified: no agreed, completed story-point baseline. Hours and commits are not substituted. |
| CI pipeline pass rate | Not verified for this sprint: the review used a recent 30-run sample per repository, not the complete sprint run population. |
| Test coverage at sprint close | No matching whole-system sprint-close snapshot was verified. Later coverage is listed separately in review-sources.md. |

**Observed issue-closure events in this date window: 27.** The table below counts non-PR GitHub issues whose latest `closed_at` falls within the supplied dates, converted to Asia/Colombo time. It covers the nine supplied repositories only, includes duplicate/epic issues where present, and is not sprint velocity, acceptance, or a historical board reconstruction. Reopened history was not replayed.

| Repository | Issues with a closure timestamp in the window | Example source links |
| --- | --- | --- |
| hotel-website | 1 | [#5](https://github.com/forever-hotel/hotel-website/issues/5) |
| manager-dashboard | 0 | None observed |
| front-desk | 10 | [#17](https://github.com/forever-hotel/front-desk/issues/17), [#13](https://github.com/forever-hotel/front-desk/issues/13), [#12](https://github.com/forever-hotel/front-desk/issues/12), [#11](https://github.com/forever-hotel/front-desk/issues/11); remaining IDs in review-sources.md |
| worker-management | 11 | [#23](https://github.com/forever-hotel/worker-management/issues/23), [#19](https://github.com/forever-hotel/worker-management/issues/19), [#18](https://github.com/forever-hotel/worker-management/issues/18), [#17](https://github.com/forever-hotel/worker-management/issues/17); remaining IDs in review-sources.md |
| food-ordering | 3 | [#8](https://github.com/forever-hotel/food-ordering/issues/8), [#6](https://github.com/forever-hotel/food-ordering/issues/6), [#4](https://github.com/forever-hotel/food-ordering/issues/4) |
| kitchen-management | 0 | None observed |
| api-gateway | 0 | None observed |
| infra | 0 | None observed |
| documents | 2 | [#3](https://github.com/forever-hotel/documents/issues/3), [#1](https://github.com/forever-hotel/documents/issues/1) |

The complete closure-ID list and reviewed revisions are in [the source register](review-sources.md).

## Supporting evidence and team follow-up

- Foundation and UI work: [front-desk #1](https://github.com/forever-hotel/front-desk/issues/1), [front-desk #5](https://github.com/forever-hotel/front-desk/issues/5), [front-desk #13](https://github.com/forever-hotel/front-desk/issues/13), [worker-management #1](https://github.com/forever-hotel/worker-management/issues/1), [worker-management #3](https://github.com/forever-hotel/worker-management/issues/3), [worker-management #8](https://github.com/forever-hotel/worker-management/issues/8).
- Standards and planning: [food-ordering #4](https://github.com/forever-hotel/food-ordering/issues/4), [food-ordering #6](https://github.com/forever-hotel/food-ordering/issues/6), [documents #1](https://github.com/forever-hotel/documents/issues/1), [documents #3](https://github.com/forever-hotel/documents/issues/3), [hotel-website #5](https://github.com/forever-hotel/hotel-website/issues/5).
- Shared architecture status: [documents / adr/ADR-001-subsystem-oriented-repository-architecture.md](https://github.com/forever-hotel/documents/blob/59334b6a2f5535366f53c4042077c9ada3ab8d9b/adr/ADR-001-subsystem-oriented-repository-architecture.md).

At the team review, confirm meeting date/attendees, explain calendar differences, validate the observations, assign the action owners, and add the original planned/completed metrics if a dated board export exists. No retrospective meeting or supervisor approval is asserted by this report.
