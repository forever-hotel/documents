# Sprint 7 Retrospective

**Project:** Forever Hotel Integrated Hotel Management System  
**Course:** SENG 34213, University of Kelaniya  
**Focus:** Integration, service contracts and user workflows  
**Sprint dates:** 2026-10-07 to 2026-10-20 (project-board screenshot supplied by the team)  
**Prepared:** 2026-10-25 (Asia/Colombo)  
**Retrospective meeting date:** Not supplied; preparation date is not a meeting date  
**Attendees:** Attendance record not supplied; team confirmation required  
**Status:** Evidence-based retrospective draft; team discussion and approval not recorded

## Scope and evidence

This is a team-level reflection across the six business services and the API Gateway, Infrastructure and Documents repositories. It follows the guideline Appendix B.2 headings. Repository issues, selected source files, commit histories, recent CI metadata and central QA reports were reviewed. Tests, load runs and deployments were not executed during this document review.

The board screenshot confirms two-week windows for Sprints 5-7. The guideline Section 2.2 describes four-week development sprints. The team should record or confirm the reason for this scheduling difference with the supervisor. Work is attributed to a date window only where timestamps support it; a feature title or closed issue alone does not prove acceptance in that sprint.

## Went Well

1. **Integration issues became concrete and traceable.** The Gateway's Front Desk route-rewrite issue closed in this window. Front Desk records cover primary screens, live frontend/backend integration, realtime/accessibility work and audit/security hardening. This demonstrates why routing should be tested against actual backend paths rather than inferred from a common naming convention.

2. **Worker Management developed the event-driven task boundary.** The records cover KMS/FDS event consumption, escalation publication and authenticated frontend/backend integration. Source modules include task-event consumers and messaging infrastructure. Idempotent consumption and retry/dead-letter handling are valuable protections when a hotel event is delivered more than once.

3. **Hotel Website delivered important guest-facing foundations.** The date window includes room/guest entity mapping, registration, login, the room catalogue, Home promotion display, containerization and the switch to infra-owned schema handling. These pieces support the later booking journey, but they should not be confused with acceptance of the later payment and booking-management work.

4. **Manager Dashboard widened its management workflows.** Closure records include staff-account lifecycle work, promotion management, complaints, operational analytics and related screens. These are useful management capabilities, especially where the displayed state is grounded in the authoritative operational service.

5. **Food Ordering and Kitchen Management clarified data models and structure.** FOSS recorded contracts, session persistence/migration and frontend/security work. KMS recorded category/menu, allergen/dietary and order persistence plus frontend foundations. These are real prerequisites for ordering; DTOs and repository models alone do not establish a completed food-ordering journey.

## To Improve

1. **The team needs one accepted authentication contract.** The current Gateway and MAD guides say that authentication is owned by each subsystem, while the FDS central-auth contract remains proposed and its complete provider/session integration is unverified. These current records expose a boundary that needs agreement. Specify issuer, audience, staff identity, revocation, cookies, BFF responsibilities and Socket.IO authentication for the intended final architecture.

2. **Successful consumer tests do not prove a live producer-to-consumer path.** WKMS consumers exist, but the KMS order-ready contract is still documented as requiring coordination. FDS closeout records also distinguish broker availability from enabled application messaging. Review the complete checkout-to-cleaning and order-ready-to-delivery flows, including duplicate delivery and outage recovery.

3. **Different gateway branches encode different integration assumptions.** At the review snapshot the Gateway default configuration preserves the HW prefix, while the Website's PayHere guide describes stripping it. These are current integration risks observed after the sprint window, not a claim that a historical sprint test failed. Agree and test one release configuration before connecting the final services.

4. **Issue labels and closure cannot replace workflow acceptance.** Some Hotel Website Sprint 5 foundation tickets closed during this date window. Manager Dashboard's foundation-acceptance issue also closed here. Report that timing honestly, and retain evidence for acceptance criteria that depend on a provider, staging environment or another subsystem.

5. **Late quality work needs visible capacity.** The user confirmed that some work matching the guideline's Sprint 8 theme was filed under Sprint 7. Keep the original tickets and history, but record the actual release-hardening scope in the Sprint 8 work review. A phase label should help reviewers see what remains, rather than imply an unrecorded additional meeting took place.

## Action Items

These recommendations require team agreement and named owners.

| Action | Owner | Due |
| --- | --- | --- |
| Approve the final authentication/session/role contract across staff services and Gateway. | Authentication lead, FDS, MAD, WKMS and Gateway owners | Before authenticated system acceptance |
| Pin a shared Gateway configuration and test HTTP paths, cookies, callbacks and WebSocket forwarding. | Gateway owner and all service owners | Before the next integrated release candidate |
| Execute checkout-to-cleaning and order-ready-to-delivery with real isolated producer, broker and consumer fixtures. | FDS, KMS and WKMS owners | Before cross-service acceptance |
| Finish and verify FOSS guest authentication/session and KMS ordering/printing dependencies. | FOSS and KMS owners | Before claiming the complete guest-order journey |
| Classify existing quality/release tickets against the Sprint 8 theme without inventing historical dates. | Project coordinator and QA owners | Next release-planning review |
| Reconcile closed tickets with open acceptance gaps and attach exact CI/deployment evidence. | All subsystem owners | Before final delivery approval |

## Metrics

| Required metric | Evidence available |
| --- | --- |
| Issues planned | Not verified: no frozen planning baseline or accessible project-board export. |
| Issues completed against sprint commitment | Not verified: issue closure is not acceptance or proof of sprint assignment. |
| Velocity | Not verified: no agreed, completed story-point baseline. Hours and commits are not substituted. |
| CI pipeline pass rate | Not verified for this sprint: the review used a recent 30-run sample per repository, not the complete sprint run population. |
| Test coverage at sprint close | No matching whole-system sprint-close snapshot was verified. Later coverage is listed separately in review-sources.md. |

**Observed issue-closure events in this date window: 49.** The table below counts non-PR GitHub issues whose latest `closed_at` falls within the supplied dates, converted to Asia/Colombo time. It covers the nine supplied repositories only, includes duplicate/epic issues where present, and is not sprint velocity, acceptance, or a historical board reconstruction. Reopened history was not replayed.

| Repository | Issues with a closure timestamp in the window | Example source links |
| --- | --- | --- |
| hotel-website | 9 | [#36](https://github.com/forever-hotel/hotel-website/issues/36), [#35](https://github.com/forever-hotel/hotel-website/issues/35), [#33](https://github.com/forever-hotel/hotel-website/issues/33), [#29](https://github.com/forever-hotel/hotel-website/issues/29); remaining IDs in review-sources.md |
| manager-dashboard | 16 | [#63](https://github.com/forever-hotel/manager-dashboard/issues/63), [#61](https://github.com/forever-hotel/manager-dashboard/issues/61), [#59](https://github.com/forever-hotel/manager-dashboard/issues/59), [#56](https://github.com/forever-hotel/manager-dashboard/issues/56); remaining IDs in review-sources.md |
| front-desk | 5 | [#67](https://github.com/forever-hotel/front-desk/issues/67), [#65](https://github.com/forever-hotel/front-desk/issues/65), [#63](https://github.com/forever-hotel/front-desk/issues/63), [#59](https://github.com/forever-hotel/front-desk/issues/59); remaining IDs in review-sources.md |
| worker-management | 3 | [#46](https://github.com/forever-hotel/worker-management/issues/46), [#44](https://github.com/forever-hotel/worker-management/issues/44), [#43](https://github.com/forever-hotel/worker-management/issues/43) |
| food-ordering | 9 | [#31](https://github.com/forever-hotel/food-ordering/issues/31), [#28](https://github.com/forever-hotel/food-ordering/issues/28), [#25](https://github.com/forever-hotel/food-ordering/issues/25), [#23](https://github.com/forever-hotel/food-ordering/issues/23); remaining IDs in review-sources.md |
| kitchen-management | 6 | [#34](https://github.com/forever-hotel/kitchen-management/issues/34), [#31](https://github.com/forever-hotel/kitchen-management/issues/31), [#29](https://github.com/forever-hotel/kitchen-management/issues/29), [#27](https://github.com/forever-hotel/kitchen-management/issues/27); remaining IDs in review-sources.md |
| api-gateway | 1 | [#2](https://github.com/forever-hotel/api-gateway/issues/2) |
| infra | 0 | None observed |
| documents | 0 | None observed |

The complete closure-ID list and reviewed revisions are in [the source register](review-sources.md).

## Supporting evidence and team follow-up

- Routing and Front Desk: [api-gateway #2](https://github.com/forever-hotel/api-gateway/issues/2), [front-desk #63](https://github.com/forever-hotel/front-desk/issues/63), [front-desk #65](https://github.com/forever-hotel/front-desk/issues/65), [front-desk #67](https://github.com/forever-hotel/front-desk/issues/67).
- Events and worker integration: [worker-management #43](https://github.com/forever-hotel/worker-management/issues/43), [worker-management #44](https://github.com/forever-hotel/worker-management/issues/44), [worker-management #46](https://github.com/forever-hotel/worker-management/issues/46).
- Guest and kitchen foundations: [hotel-website #29](https://github.com/forever-hotel/hotel-website/issues/29), [hotel-website #33](https://github.com/forever-hotel/hotel-website/issues/33), [hotel-website #35](https://github.com/forever-hotel/hotel-website/issues/35), [food-ordering #23](https://github.com/forever-hotel/food-ordering/issues/23), [food-ordering #28](https://github.com/forever-hotel/food-ordering/issues/28), [kitchen-management #31](https://github.com/forever-hotel/kitchen-management/issues/31).
- Current integration cautions: [front-desk / docs/central-auth-integration-contract.md](https://github.com/forever-hotel/front-desk/blob/ca19caae9c69058e3bbf46d0e343758c230210f9/docs/central-auth-integration-contract.md), [api-gateway / config/routes.json](https://github.com/forever-hotel/api-gateway/blob/2727054cfd30b53e5f947ef5da17f407a358fba5/config/routes.json), [hotel-website / documents/api/payhere-checkout.md](https://github.com/forever-hotel/hotel-website/blob/3551505c8a053c08c6da51013826b831d0e7dfa5/documents/api/payhere-checkout.md), [worker-management / backend/src/realtime/README.md](https://github.com/forever-hotel/worker-management/blob/0f91f1c9d6390c80c9f12f82b834fee9bf374104/backend/src/realtime/README.md).

Confirm the actual meeting date and attendees, successful live integrations, deferred work, and action ownership. Current-source cautions above are explicitly later observations; they are not backdated meeting minutes.
