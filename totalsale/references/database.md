# Database reference

## Contents

- Locations and ownership
- Core payment objects
- Investigation workflow
- Reusable read-only scenarios
- Write-scenario rule

## Locations and ownership

- Checked-in SQL root: `D:\GolrangProject\TotalSales\Gng.TotalSales.BE\Gng.Sales.Core\DBScripts`
- Payment scripts: `salescollection\Stored Procedures`
- Payment indexed view: `salescollection\Views\vw_Indexed_PaymentAssignAmount.sql`
- Runtime rule and status-flow behavior is metadata-driven; inspect the target environment when exact active behavior matters.

## Core payment objects

- `[salescollection].[Payment]`
- `[salescollection].[PaymentAssign]`
- `[salescollection].[vw_Indexed_PaymentAssignAmount]`
- `usp_GetAssignablePayment.sql` and refactored variants
- `usp_PaymentGetAll.sql`, `usp_PaymentAssignGetAll.sql`
- `usp_PaymentAssignInsert.sql`, `usp_PaymentAssignValidate.sql`

Verified entity/SQL fields include:

- `Payment`: `Id`, `PaymentAmount`, `SalesDocAssignAmount`, `CreditorCustomerId`, `PayerCustomerId`, `CollectionDocTypeStatusFlowId`, `FinalizeStatusCode`, `OutOfAssignable`, row/state fields.
- `PaymentAssign`: `PaymentId`, `SalesDocCategoryTypeId`, `SalesDocTypeId`, `SalesDocHeaderId`, `PaymentAssignAmount`, row/state fields.
- The indexed view aggregates active rows where `IsDeleted=0` and `StateCode=1` by category and sales document header.

Historical Dev snapshot dated 2026-07-20 reported `RowVersion` on both entities and no dedicated `PaymentAssign` index on `PaymentId` or `(SalesDocCategoryTypeId, SalesDocHeaderId)`. Re-check schema and execution plans before recommending an index.

## Investigation workflow

1. Identify the company, branch, customer, document category, status flow, and environment.
2. Read checked-in SQL and EF mappings to learn expected names.
3. Query metadata and data with `READ UNCOMMITTED` only when dirty reads are acceptable for diagnosis; otherwise use the environment's safe read policy.
4. Capture query date/environment and label counts as snapshots.
5. Compare denormalized totals with active detail rows.
6. Use actual execution plans before proposing indexes.

## Reusable read-only scenarios

Adapt filters to a narrow customer/payment set before execution.

### DB-PAY-001 — payment assignment reconciliation

Purpose: detect over-allocation and disagreement between `Payment.SalesDocAssignAmount` and active detail rows.

```sql
SELECT
    p.Id,
    p.PaymentAmount,
    p.SalesDocAssignAmount,
    SUM(CASE WHEN pa.StateCode = 1 AND pa.IsDeleted = 0
             THEN pa.PaymentAssignAmount ELSE 0 END) AS ActiveAssignAmount
FROM salescollection.Payment AS p WITH (READUNCOMMITTED)
LEFT JOIN salescollection.PaymentAssign AS pa WITH (READUNCOMMITTED)
    ON pa.PaymentId = p.Id
WHERE p.Id IN (/* narrow payment ids */)
GROUP BY p.Id, p.PaymentAmount, p.SalesDocAssignAmount;
```

Check `ActiveAssignAmount <= PaymentAmount` and equality with the stored aggregate unless the current business model documents an exception.

### DB-PAY-002 — many-to-many evidence

Purpose: find payments assigned to multiple invoices and invoices receiving multiple payments.

```sql
SELECT pa.PaymentId, COUNT(DISTINCT pa.SalesDocHeaderId) AS InvoiceCount
FROM salescollection.PaymentAssign AS pa WITH (READUNCOMMITTED)
WHERE pa.StateCode = 1 AND pa.IsDeleted = 0
GROUP BY pa.PaymentId
HAVING COUNT(DISTINCT pa.SalesDocHeaderId) > 1;

SELECT pa.SalesDocCategoryTypeId, pa.SalesDocHeaderId,
       COUNT(DISTINCT pa.PaymentId) AS PaymentCount
FROM salescollection.PaymentAssign AS pa WITH (READUNCOMMITTED)
WHERE pa.StateCode = 1 AND pa.IsDeleted = 0
GROUP BY pa.SalesDocCategoryTypeId, pa.SalesDocHeaderId
HAVING COUNT(DISTINCT pa.PaymentId) > 1;
```

### DB-PAY-003 — preview zero-write proof

Purpose: prove that preview leaves business data unchanged.

1. Capture targeted row counts and aggregates for Payment, PaymentAssign, invoice discount/sharing, and affected sales document rows.
2. Execute one preview request with unique correlation/client keys.
3. Repeat the same queries in the same environment.
4. Assert no new business rows and no changed row versions or aggregates.
5. Inspect technical logs separately because policy may allow or forbid them.
6. Verify cache/KPI safety through Engine tests or observability; SQL alone cannot prove in-memory isolation.

### DB-PAY-004 — status-flow and rule metadata

Purpose: explain why a payment is assignable or why a rule executed.

- Resolve the payment's collection document type and current status flow.
- Inspect active primitive flow/mapping rows and user access.
- Inspect active assignment rule mappings and priorities for the exact company/branch context.
- Record IDs and effective dates; do not generalize one environment's mappings.

## Write-scenario rule

For test data creation, use a disposable Dev database or an explicit transaction with a reviewed rollback path. Never turn these read scenarios into production cleanup scripts without separate authorization and backups.
