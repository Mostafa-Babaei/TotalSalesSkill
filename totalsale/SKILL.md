---
name: totalsale
description: Analyze, explain, debug, review, implement, test, and database-validate the Golrang TotalSales system across frontend, backend, engine, and database. Use for TotalSales or SalesDistributor questions involving sales documents, payments, collection, payment assignment, settlement preview, KPI or credit cache, discounts and promotions, returns, receive or delivery discrepancies, inventory effects, APIs, Angular flows, SQL scenarios, cross-layer impact analysis, or implementation plans.
---

# TotalSales

Use this skill as a compact router into project-specific knowledge. Load only the references needed for the request, then verify mutable facts against the current source code.

## Locate the workspace

Use `D:\GolrangProject\TotalSales` as the default workspace when it exists. Read [project-map.md](references/project-map.md) for repository ownership and common search roots.

If the workspace is elsewhere, locate these markers before continuing:

- `Gng.TotalSales.UI`
- `Gng.TotalSales.BE`
- `Gng.TotalSales.Engines`
- `SalesDistributor`

## Route the request

1. Read [topic-index.md](references/topic-index.md) to identify the smallest relevant layer set.
2. Read only the matching layer references:
   - UI behavior, Angular state, forms, routes: [frontend.md](references/frontend.md)
   - API contracts, controllers, services, mapping: [backend.md](references/backend.md)
   - rules, state machines, commands, caches, side effects: [engine.md](references/engine.md)
   - tables, procedures, metadata, indexes, data scenarios: [database.md](references/database.md)
3. Read [source-catalog.md](references/source-catalog.md) only when prior analyses or historical decisions are relevant.
4. For a cross-layer change, read `project-map.md`, `topic-index.md`, and each affected layer file. Do not load every reference by default.

## Evidence workflow

Follow this order:

1. Treat the user's named file, endpoint, class, or scenario as the primary scope.
2. Use references to find likely files, invariants, and database objects.
3. Verify implementation-changing claims with `rg` and the current branch.
4. Verify database behavior with read-only SQL or checked-in DB scripts. Do not infer production metadata from code alone.
5. Label conclusions as one of:
   - `Verified`: confirmed in current code or current database evidence.
   - `Historical`: taken from a dated analysis or snapshot.
   - `Proposed`: a design that is not confirmed as implemented.

Never present dated Dev counts, metadata mappings, or branch-specific behavior as permanent facts.

## Change workflow

For implementation requests:

1. Check repository status and preserve unrelated user changes.
2. Trace the request from frontend to backend to engine to database, even if only one layer changes.
3. State invariants before editing. For preview and simulation flows, explicitly inspect persistence, singleton cache mutation, external notifiers, metadata cache, `AfterSave`, logging, and request-local cleanup.
4. Make the smallest compatible change in the owning repository.
5. Test in proportion to risk, including partial-success paths when all-or-nothing behavior is not required.
6. Report changed files, verified behavior, remaining cross-layer work, and any historical assumptions.

## Database safety

Default to read-only investigation. Use a transaction or disposable Dev fixture for write scenarios, and never run mutation scripts against a shared environment without explicit authorization. Read [database.md](references/database.md) before composing project-specific SQL.

## Grow the skill

When the user asks to add learned knowledge:

1. Record stable navigation and invariants, not a transcript of the investigation.
2. Put layer-specific facts in the matching reference and cross-layer routing in `topic-index.md`.
3. Add source paths, search anchors, verification date, and status (`Verified`, `Historical`, or `Proposed`).
4. Remove duplicated guidance and stale claims.
5. Re-run the skill validator after editing.

## Business learning, executive Q&A, and cross-chat continuity

This skill also provides a structured learning track for heads of sales who need business and technical mastery, not only implementation assistance.

For questions about **learning**, **roadmap**, **progress**, **managerial impact**, **sales document lifecycle**, **rules**, **discounts**, **warehouse/collection**, or **manufacturing vs distribution**:

1. Read [ROADMAP.md](../ROADMAP.md) and [PROGRESS.md](learning/PROGRESS.md) to determine what has been completed and what comes next.
2. Use [business-domain-map.md](references/business-domain-map.md) and the existing [topic-index.md](references/topic-index.md) to scope the answer.
3. For conceptual teaching, read [Lesson 01](learning/01-architecture-overview.md) and extend the lesson series progressively.
4. Follow [ASK-AND-UPDATE.md](learning/ASK-AND-UPDATE.md): business explanation → trace with confidence labels → side effects/risks → exercise.
5. When a learning step produces durable, verified knowledge, update the corresponding GitHub Issue checkbox and `learning/PROGRESS.md` in the same work session. Do not rely on conversation memory as the canonical progress store.
6. State exactly what is known from **current source** vs **historical source** vs **proposal/unknown**. Existing reference files are useful historical evidence, not unconditional current behavior.

**Repository security:** This GitHub repository is public. Read [SECURITY.md](../SECURITY.md). Do not copy organization-owned source from Azure DevOps or publish confidential backlog/code/runtime details here. For deep current-code analysis, use an approved private organization repository or authorized read-only Azure DevOps access.
