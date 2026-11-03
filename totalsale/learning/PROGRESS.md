# TotalSales Learning Progress — canonical checkpoint

**Updated:** 2026-11-03 · **Overall:** groundwork created; no full business domain signed off.

## Current phase
- **P0** source/evidence mapping: **in progress** ([Issue #1](../../../issues/1)).
- **P1** architecture/domain model: **started** ([Issue #2](../../../issues/2)).
- **Next Action:** Verify current repository branches/commits in UI, BE and Engine using permitted read-only access, then trace one sales document state transition through all layers. If access is not available, continue concept training while explicitly marking all code-level assertions unverified.

## Completed checkpoints
- [x] Found existing `Mostafa-Babaei/TotalSalesSkill` and read its `totalsale/SKILL.md`.
- [x] Reviewed existing `project-map.md`, `topic-index.md`, `engine.md`, `backend.md`, `database.md`, and `source-catalog.md`.
- [x] Chose to extend the existing skill, not replace or duplicate it.
- [x] Created 9 learning-phase/issues plus separate source-access security issue.
- [x] Published architecture primer and answer/evidence workflow.
- [x] Confirmed destination GitHub repo is public; blocked code mirroring pending authorization/private destination.

## Not yet completed
- [ ] Full source-level analysis of sales document transitions in current `Features` or deployed revision.
- [ ] Rule selection, execution ordering, transaction boundaries validated in current code.
- [ ] Discount pipeline validated in current code.
- [ ] Manufacturing/distributor backlog comparison grounded in current configured behavior.
- [ ] End-to-end source/metadata proof or hands-on lesson assessment.

## Source of truth / evidence ledger
| Evidence | Grade | Scope and date | Note |
|---|---|---|---|
| `totalsale/references/project-map.md` | Historical | pre-existing project map | file roots only; verify paths |
| `totalsale/references/engine.md` | Historical verified snapshot | 2026-07-22, specific preview branch/commit | payment preview only, not every rule |
| `totalsale/references/backend.md` | Historical analysis | prior service integration | current API can differ |
| `totalsale/references/database.md` | Historical / examples | payment model and read-only SQL | metadata is environment-dependent |
| `totalsale/learning/01-architecture-overview.md` | Learning explanation | 2026-11-03 | concepts distinguished from real evidence |

## How future chats resume

Read this exact file, follow **Next Action**, then update this file and the appropriate Issue after delivering a concrete learning checkpoint. Do not mark a phase complete without source evidence + teaching exercise + test/impact notes. Avoid treating elapsed days as progress.

## Session log
- 2026-11-03 — Roadmap created; existing skill inspected; architecture lesson drafted; public-repo source copying blocked. P0 and P1 in progress.
