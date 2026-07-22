# Topic index

Use this table to choose references and search anchors. Coverage is intentionally incomplete and should grow from verified investigations.

| Topic or user phrase | Read first | Common anchors | Coverage |
|---|---|---|---|
| چند پرداختی، چند فاکتور، bulk payment assign, settlement preview | `engine.md`, then `backend.md` or `frontend.md`; add `database.md` for validation | `BatchInsertAndAssignAsync`, `PaymentAssignPreviewContext`, `PreviewKpiStore`, `PaymentAssign` | Verified Engine snapshot + historical cross-layer design |
| ثبت دریافتی، Payment insert | `frontend.md`, `backend.md`, `engine.md` | `PaymentDetailFormComponent`, `InsertWithEngineAsync`, `PaymentInsertCommand` | Historical analysis; verify current branches |
| تخصیص دریافتی به فاکتور | `frontend.md`, `backend.md`, `engine.md`, optionally `database.md` | `AssignModel`, `PaymentAssign`, `SalesDocMultiStepAssigner`, `AssignCalculator` | Historical analysis + current Engine anchors |
| KPI، اعتبار، credit cache | `engine.md`, `database.md` | `ICreditCache`, `PublicKpiRepository`, `PreviewKpiStore`, `CreditCacheManipulatorCollectionDoc` | Verified preview architecture; metadata remains environment-dependent |
| تخفیف وصول، collection discount | `engine.md`, `database.md` | `CollectionDiscountCalculation`, `UpdateSalesDocAfterAssignAggregator` | Historical Dev metadata; verify active rules |
| مجوز دریافت، Receive discrepancy | `engine.md`, then `backend.md` and `database.md` | `SalesReceiveHeaderCommandController`, `SalesReceiveDetailsManipulateCommand`, `SalesReceiveFlowCommandFactory` | Historical backlog dated 2026-07-20 |
| دیتابیس، SQL scenario، index، metadata | `database.md` | `salescollection.Payment`, `salescollection.PaymentAssign`, `SalesDocTypeStatusFlow` | Initial payment-focused coverage |
| دامنه تغییر، implementation plan | `project-map.md`, all affected layer files, `source-catalog.md` | endpoint/class named by user | Cross-layer workflow |

## Routing rule

- Prefer exact identifiers over Persian or English wording when searching.
- If a topic is absent, inspect current code and add only stable navigation or invariants after the user asks to enrich the skill.
- Do not load a historical document merely because it mentions the same broad domain; match the operation and entity first.
