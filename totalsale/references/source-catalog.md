# Source catalog

These documents are evidence and navigation aids, not automatically current truth.

## Prior SalesDistributor analyses

Root: `D:\GolrangProject\TotalSales\SalesDistributor`

| File | Best use | Status |
|---|---|---|
| `customer-payment-assignment-current-flow.md` | Full UI -> Backend -> Engine -> database trace for payment insert and assignment; preview leakage analysis | Historical analysis, includes Dev snapshot dated 2026-07-20 |
| `multi-payment-invoice-preview-implementation-plan.md` | Proposed contracts, simulation workspace, KPI overlay, side-effect policy, stale token, cross-layer scope, tests | Proposed design; not all phases are implemented |
| `multi-payment-settlement-engine-business-story.md` | Numerical allocation example, KPI failure, stale preview, current vs proposed Engine paths, acceptance criteria | Business story and proposed design |
| `sales-receive-engine-analysis-backlog.md` | Receive discrepancy gaps, database evidence, architectural decision, test matrix, affected Engine files | Historical backlog |
| `SalesDistributor-backlog.md` | Snapshot of broader backlog and priorities | Historical planning snapshot |

## Current Engine implementation evidence

Verified on 2026-07-22 at Engine commit `d10b1213b1` on branch `zendehdel/preview-bulk-paymentassign`:

- `PaymentCommandController.BatchInsertAndAssignAsync` has a dedicated preview path.
- Request-local payment/invoice allocation and KPI overlays exist.
- Preview avoids persistence and singleton KPI cache mutation through guarded execution and overlay stores.
- Partial success remains supported.

Always re-check the active branch because this evidence is branch-specific.

## Reading strategy

- Search headings first: `rg -n "^#{1,4} " <file>`.
- Read only the relevant section range.
- Prefer current code for implemented behavior and the documents for rationale, identified risks, and proposed work.
- When code and document disagree, report the disagreement and use code as the current implementation source unless runtime metadata proves otherwise.
