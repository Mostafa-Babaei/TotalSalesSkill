# TotalSales Business & Engine Learning Roadmap

**Status date:** 2026-11-03 · **Repository:** `Mostafa-Babaei/TotalSalesSkill` · **Canonical progress:** [PROGRESS.md](totalsale/learning/PROGRESS.md)

The GitHub issues contain granular checkboxes; `PROGRESS.md` is the cross-chat checkpoint, kept in sync after each lesson. The training is evidence-led rather than a fixed eight-week deadline.

| Phase | Focus | Issue / checklist | Status | Main output |
|---|---|---|---|---|
| P0 | Source baseline and evidence rules | [#1](../../issues/1) | 🟡 In progress | reliable source map and current refs |
| P1 | Architecture and domain model | [#2](../../issues/2) | 🟡 Started | UI → BE → Engine → DB map |
| P2 | Sales documents and lifecycle | [#3](../../issues/3) | ⬜ Todo | document state/relationship map |
| P3 | Status flow, Rule Engine, Commands | [#4](../../issues/4) | ⬜ Todo | Rule Catalog and command sequence |
| P4 | Discounts, prices, recalculation | [#5](../../issues/5) | ⬜ Todo | discount decision tables |
| P5 | Inventory, credit, collection | [#6](../../issues/6) | ⬜ Todo | inventory & collection impact map |
| P6 | Returns, field service, SFA | [#7](../../issues/7) | ⬜ Todo | distribution / return flows |
| P7 | Tax invoices and integrations | [#8](../../issues/8) | ⬜ Todo | integration & failure map |
| P8 | Manufacturing vs distribution & management | [#9](../../issues/9) | ⬜ Todo | executive playbook |
| SEC | Source mirroring / access governance | [#10](../../issues/10) | 🔒 Blocked | private, authorized source access |

## Initial validated baseline

- The repository already contains `totalsale/SKILL.md` and layer references for frontend, backend, engine, database, project map and sources.
- `references/engine.md` documents a **historical, dated** payment/assignment preview implementation at a particular Engine branch and commit; it is **not proof of current production behavior**.
- No fresh full-code checkout, current implementation baseline, metadata validation or runtime verification has yet been completed in this roadmap session.
- The first architecture lesson has been drafted as an evidence-graded introduction, not a completed cross-layer code trace.

## Definition of done for each phase

- [ ] Understand the business semantics and exceptions; create a real scenario.
- [ ] Trace UI/BE/Engine/SQL as applicable; identify configuration/metadata.
- [ ] Cite exact source path + branch/commit + check date for implementation claims.
- [ ] Document side effects, rollback, risks and test gaps.
- [ ] Complete at least one teach-back exercise.
- [ ] Update the phase Issue, learning file and [PROGRESS.md](totalsale/learning/PROGRESS.md).

## Continue-work protocol

1. Read `totalsale/SKILL.md`, `ROADMAP.md`, `totalsale/learning/PROGRESS.md`.
2. Read the active Issue checklist before starting.
3. Identify whether code/DB evidence is currently accessible; use historical refs only with labels.
4. Deliver a short business explanation, actual technical trace when verified, and exercise.
5. Update the checkbox, source references and `PROGRESS.md` in the same turn.

**No unapproved source copying:** the repo is public. The Azure DevOps Features branch must not be uploaded here. See [SECURITY.md](SECURITY.md).
