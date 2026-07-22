# Backend reference

## Root

- Repository: `D:\GolrangProject\TotalSales\Gng.TotalSales.BE`
- API: `Gng.Sales.API`
- Services: `Gng.Sales.Service`
- DTOs and DB scripts: `Gng.Sales.Core`

## Existing payment endpoints

Historical source mapping:

| Operation | Backend endpoint | Existing input shape | Engine target |
|---|---|---|---|
| Insert payment | `Payment/InsertWithEngineAsync` | one `PaymentViewModel` | `PaymentCommandService/Insert` |
| Update payment | `Payment/UpdateWithEngineAsync` | one `PaymentViewModel` | `PaymentCommandService/Update` |
| Assignable payments | `Payment/GetAssignablePaymentAsync` | grid request | read-only stored procedure |
| Assign payment | `PaymentAssign/InsertWithEngineAsync` | one Payment ID + invoice IDs | `PaymentCommandService/Assign` |

Likely source files:

- `Gng.Sales.API\Controllers\salescollection\PaymentController.cs`
- `Gng.Sales.API\Controllers\salescollection\PaymentAssignController.cs`
- `Gng.Sales.Service\salescollection\PaymentService.cs`
- `Gng.Sales.Service\salescollection\PaymentAssignService.cs`
- `Gng.Sales.Core\DTOs\InsertPaymentAssignCommandInput.cs`
- `Gng.Sales.Service\Infrastructure\RuleEngine\SalesRuleEngineApiService.cs`

## Preview integration invariants

- Add explicit Backend DTOs and endpoints for bulk preview/commit instead of looping the legacy insert and assign endpoints.
- Preserve client correlation keys and structured Engine preview results through mapping.
- A preview path must not call legacy `PaymentController.InsertWithEngineAsync`; cheque customer-bank-account creation there can write before Engine execution.
- Decide whether `SalesExternalServiceLog` is an allowed technical write. The historical client writes it in `finally`; this conflicts with an absolute zero-write definition.
- Keep branch/customer/company authorization, existing payment-create and assign permissions, status-flow access, and read permissions. A new bulk permission must not bypass the old permissions.
- Pass cancellation, correlation, and idempotency data when the contract supports commit.

## Change checklist

1. Confirm the Engine route name and payload from current generated/client contracts.
2. Inspect serialization of `PreviewResult`; avoid flattening it to an opaque or lossy string when the UI needs structured rows.
3. Check pre-Engine writes, post-call logs, retries, and exception mapping.
4. Verify partial-success semantics in status codes and response models.
5. Add contract tests around correlation keys, amounts, warnings, and errors.
